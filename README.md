mport speech_recognition as sr
import webbrowser
import pyttsx3

# Initialize text-to-speech engine
engine = pyttsx3.init()

# Set Female Voice
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)  # Change index if needed

# Function to speak
def speak(text):
    """Converts text to speech using pyttsx3 with a female voice."""
    engine.say(text)
    engine.runAndWait()

# Function to process voice commands
def process_command(command):
    """Executes commands based on recognized speech."""
    command = command.strip().lower()
    print(f"DEBUG: Recognized command → '{command}'")  # Debugging print

    if "open google" in command:
        speak("Opening Google")
        webbrowser.open_new_tab("https://google.com")
    elif "open facebook" in command:
        speak("Opening Facebook")
        webbrowser.open_new_tab("https://facebook.com")
    elif "youtube" in command or "utube" in command or "you tube" in command:
        speak("Opening YouTube")
        webbrowser.open_new_tab("https://youtube.com")
    elif "open linkedin" in command:
        speak("Opening LinkedIn")
        webbrowser.open_new_tab("https://linkedin.com")
    else:
        speak("Sorry, I didn't understand that command.")

# Main function
if __name__ == "__main__":
    speak("Voice Assistant Activated.")

    while True:
        try:
            with sr.Microphone() as source:
                r = sr.Recognizer()
                r.adjust_for_ambient_noise(source, duration=1)  # Reduce background noise
                print("Listening for command...")
                audio = r.listen(source, timeout=8, phrase_time_limit=12)  # Increased timeout

            try:
                command = r.recognize_google(audio)  # Google Speech API
                print(f"Command received: {command}")  # Debugging
                process_command(command)
            except sr.UnknownValueError:
                print("Sorry, I could not understand that.")
            except sr.RequestError as e:
                print(f"Speech Recognition API error: {e}")

        except sr.UnknownValueError:
            print("Could not understand the audio. Please try again.")
        except sr.RequestError as e:
            print(f"API request failed: {e}")
        except Exception as e:
            print(f"Error: {e}")
