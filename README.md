# Talking-robot.
Here is the code import cohere
import speech_recognition as sr
import pyttsx3

# Step 1: Speech Recognition
def get_audio_input():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Speak something...")
        audio = recognizer.listen(source)
    try:
        print("Recognizing...")
        text = recognizer.recognize_google(audio)
        print(f" You said: {text}")
        return text
    except sr.UnknownValueError:
        return "Sorry, I could not understand the audio."
    except sr.RequestError:
        return "Could not request results. Check your internet connection."

# Step 2: Send to Cohere LLM
def generate_response(prompt):
    co = cohere.Client("r5R7pYgTP4VCflQ3Kv4Mbqtj5uYFzvqVZRFYLHGT")
    response = co.generate(
        model='command-r',  # or 'command-light' etc
        prompt=prompt,
        max_tokens=100,
        temperature=0.7
    )
    reply = response.generations[0].text.strip()
    print(f"Response: {reply}")
    return reply

# Step 3: Convert Response to Audio
def speak_text(text):
    engine = pyttsx3.init()
    engine.say(text)
    engine.runAndWait()

# Main flow
if __name__ == "__main__":
    user_input = get_audio_input()
    if user_input.startswith("Sorry") or user_input.startswith("Could not"):
        speak_text(user_input)
    else:
        response = generate_response(user_input)
        speak_text(response)
 
