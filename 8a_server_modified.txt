"""
Emotion Detection Flask Application

Entry point for the Emotion Detector web service that analyzes
emotional sentiment in text using the Watson API.
"""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask('Emotion Detector')
@app.route("/emotionDetector")
def sent_analyzer():
    """Call Watson API to detect emotions in provided text."""
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)
    if response['dominant_emotion'] == 'None':
        return "Invalid text! Please try again!."

    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']
    # Return a formatted string with the sentiment label and score
    return (f"For the given statement, the system response is 'anger': {anger},"
            f" 'disgust': {disgust}, 'fear': {fear},"
            f" 'joy': {joy} and 'sadness': {sadness}. The dominant emotion is {dominant_emotion}")

@app.route("/")
def render_index_page():
    """Render Index Page"""
    return render_template('index.html')

app.run(host="0.0.0.0", port=5000)
