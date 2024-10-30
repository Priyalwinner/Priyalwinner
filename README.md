pip install transformers streamlit
# Import Libraries
from transformers import pipeline
import streamlit as st

# Load Hugging Face’s Emotion Analysis Model
emotion_analyzer = pipeline("text-classification", model="j-hartmann/emotion-english-distilroberta-base", return_all_scores=True)

# Streamlit App Setup
st.title("Emotion Translator App")
st.write("Detect emotions from text and get visual cues for better understanding.")

# User Input
user_input = st.text_input("Enter text to analyze emotions:")

# Define a function to interpret emotion and give visual feedback
def interpret_emotion(emotions):
    emotion_scores = {emotion['label']: emotion['score'] for emotion in emotions[0]}
    dominant_emotion = max(emotion_scores, key=emotion_scores.get)

    # Display emotion and color feedback
   st.write(f"Detected Emotion: **{dominant_emotion}**")
    if dominant_emotion == "joy":
        st.markdown("<div style='background-color:yellow;padding:10px;'>🙂 Joy Detected! Bright and happy.</div>", unsafe_allow_html=True)
    elif dominant_emotion == "anger":
        st.markdown("<div style='background-color:red;padding:10px;'>😡 Anger Detected! Take a deep breath.</div>", unsafe_allow_html=True)
    elif dominant_emotion == "sadness":
        st.markdown("<div style='background-color:blue;padding:10px;'>😢 Sadness Detected. Offering support.</div>", unsafe_allow_html=True)
    elif dominant_emotion == "fear":
        st.markdown("<div style='background-color:gray;padding:10px;'>😨 Fear Detected. Stay calm.</div>", unsafe_allow_html=True)
    else:
        st.markdown("<div style='background-color:green;padding:10px;'>🤔 Neutral or Mixed Emotion Detected.</div>", unsafe_allow_html=True)

# Process input and display emotion
if user_input:
    emotions = emotion_analyzer(user_input)
    interpret_emotion(emotions)

