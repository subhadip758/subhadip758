// --- FRONTEND: React + Tailwind ---
// src/App.jsx
import React, { useEffect, useState } from "react";
import axios from "axios";

function App() {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState("");

  const fetchTasks = async () => {
    const res = await axios.get("http://localhost:5000/api/tasks");
    setTasks(res.data);
  };

  useEffect(() => {
    fetchTasks();
  }, []);

  const addTask = async () => {
    if (!newTask) return;
    await axios.post("http://localhost:5000/api/tasks", { title: newTask });
    setNewTask("");
    fetchTasks();
  };

  const toggleComplete = async (id) => {
    await axios.patch(`http://localhost:5000/api/tasks/${id}`);
    fetchTasks();
  };

  const deleteTask = async (id) => {
    await axios.delete(`http://localhost:5000/api/tasks/${id}`);
    fetchTasks();
  };

  return (
    <div className="max-w-xl mx-auto mt-10 p-4">
      <h1 className="text-3xl font-bold mb-4">Task Manager</h1>
      <div className="flex gap-2 mb-4">
        <input
          type="text"
          className="flex-1 border rounded px-2 py-1"
          value={newTask}
          onChange={(e) => setNewTask(e.target.value)}
          placeholder="Enter task"
        />
        <button onClick={addTask} className="bg-blue-500 text-white px-4 rounded">
          Add
        </button>
      </div>
      <ul>
        {tasks.map((task) => (
          <li
            key={task._id}
            className="flex justify-between items-center p-2 border-b"
          >
            <span
              className={`flex-1 ${task.completed ? "line-through text-gray-500" : ""}`}
            >
              {task.title}
            </span>
            <button
              onClick={() => toggleComplete(task._id)}
              className="text-green-600 mx-2"
            >
              {task.completed ? "Undo" : "Done"}
            </button>
            <button
              onClick={() => deleteTask(task._id)}
              className="text-red-600"
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
