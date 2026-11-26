# Vocalize: real-time ASL translator

Vocalize turns American Sign Language fingerspelling into text and speech in the browser. Hold a sign up to your webcam; MediaPipe finds your hand, a TensorFlow model reads the letter, and the app builds words, autocorrects them and can read the sentence aloud.

## Features

- Live webcam recognition of ASL letters with MediaPipe hand landmarks
- Letter-to-word assembly with spell-check autocorrect
- Text-to-speech for the finished sentence
- Optional sentence clean-up and chat assistant powered by Claude
- Model stats page for the trained classifier

## Tech stack

| Part | Tools |
| --- | --- |
| Frontend | HTML, CSS, JavaScript, MediaPipe Tasks |
| Backend | Python, Flask, Flask-CORS |
| Model | TensorFlow / Keras CNN trained on hand images |
| Language | Anthropic API, pyspellchecker, AIML fallback chatbot |
| Hosting | Vercel (serverless Python in `api/`) |

## Getting started

```bash
git clone https://github.com/rajivagarwal221/ASL---Vocalize.git
cd ASL---Vocalize/ASL/asl-sign-recognition
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt tensorflow opencv-python mediapipe numpy
cp backend/.env.example backend/.env                 # add ANTHROPIC_API_KEY
python backend/app.py                                # http://localhost:5000
```

The trained model is not in git. Put `best_asl_model.keras` and `labels.npy` in `ASL/asl-sign-recognition/models/`, or train your own:

```bash
python training/preprocess_data.py
python training/train_model.py
```

## Project structure

```text
ASL/asl-sign-recognition/
  frontend/     web app (index.html, ai_logic.js)
  backend/      Flask server for local use (app.py, predictor.py, chatbot.py)
  api/          same logic packaged for Vercel serverless (index.py)
  training/     preprocessing and model training scripts
  notebooks/    data prep, training and prediction notebooks
  vercel.json   routes /api/* to the Python function, everything else to frontend/
```

The `frontend/`, `backend/` and `docs/` folders at the repository root hold a separate TradingAgents experiment (a FastAPI + React "war room" for multi-agent trading analysis) and are not part of Vocalize.

## Security

Keep `ANTHROPIC_API_KEY` in `.env` (ignored by git) or in Vercel's environment variables. Never hard-code it.
