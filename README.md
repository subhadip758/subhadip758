from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput
from kivy.uix.label import Label
from kivy.uix.dropdown import DropDown
from kivy.uix.spinner import Spinner
from datetime import datetime

# Mood tracking app
class MoodSyncApp(App):
    def build(self):
        self.mood_data = []
        self.coping_strategies = {
            "Happy": "Keep up the great mood! Engage in your favorite activities.",
            "Sad": "Try to talk to a friend or practice deep breathing.",
            "Angry": "Take a short walk or practice mindfulness.",
            "Stressed": "Try meditation or write about your stress."
        }
        
        # Main layout of the app
        layout = BoxLayout(orientation='vertical', padding=10, spacing=10)
        
        # Label to display app title
        title_label = Label(text="MoodSync - Mood Tracker", font_size=24, size_hint=(1, 0.1))
        layout.add_widget(title_label)

        # Spinner (dropdown) for selecting the mood
        self.mood_spinner = Spinner(
            text="Select your mood",
            values=("Happy", "Sad", "Angry", "Stressed"),
            size_hint=(None, None),
            size=(200, 44),
        )
        layout.add_widget(self.mood_spinner)
        
        # Text input for additional notes
        self.note_input = TextInput(
            hint_text="Additional notes (optional)",
            size_hint=(1, 0.2),
            multiline=True
        )
        layout.add_widget(self.note_input)
        
        # Submit button to log mood and add to history
        submit_button = Button(text="Log Mood", size_hint=(1, 0.1), on_press=self.log_mood)
        layout.add_widget(submit_button)
        
        # Label to display coping strategy based on mood
        self.coping_label = Label(text="Coping Strategy will appear here", size_hint=(1, 0.1))
        layout.add_widget(self.coping_label)

        # Display logged moods
        self.history_label = Label(text="Mood History: None", size_hint=(1, 0.2))
        layout.add_widget(self.history_label)
        
        return layout

    def log_mood(self, instance):
        mood = self.mood_spinner.text
        notes = self.note_input.text
        timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        
        # Log the mood entry
        self.mood_data.append({'mood': mood, 'timestamp': timestamp, 'notes': notes})
        
        # Display the coping strategy
        self.coping_label.text = f"Coping Strategy: {self.coping_strategies.get(mood, 'No strategy available')}"
        
        # Update mood history
        history = "\n".join([f"{entry['timestamp']}: {entry['mood']} - {entry['notes']}" for entry in self.mood_data])
        self.history_label.text = f"Mood History:\n{history}"

        # Clear the note input field
        self.note_input.text = ""

if __name__ == '__main__':
    MoodSyncApp().run()