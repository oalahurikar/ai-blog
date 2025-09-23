
### Opensource Models
- https://github.com/coqui-ai/TTS
- https://huggingface.co/tasks/text-to-speech


Examples to generate audio using python

Option 1: Using Pre-trained Models and APIs
This is the simplest and quickest way to get started.
- **Libraries:**
    - **gTTS:** A straightforward library that uses Google's TTS API.
    - **pyttsx3:** A cross-platform library that works offline.
    - **Picovoice Orca:** Provides high-quality voices with a smaller footprint.

```python
from gtts import gTTS

text = "Hello, this is a text-to-speech example."
tts = gTTS(text=text, lang='en')
tts.save("output.mp3")
```

Option 2: Cloud services

```python
import openai

response = openai.audio.speech.create(
    model="tts-1",
    voice="alloy",
    input="This is an example using OpenAI's TTS."
)

with open("output.mp3", "wb") as f:
    f.write(response.content)
```
Option 3: Building a Custom Model
This involves training your own TTS model, which requires a significant amount of data and computational resources.

- **Libraries:**
    - **TensorFlow:** A popular deep learning framework.
    - **PyTorch:** Another powerful deep learning framework.
    - **Tacotron 2:** A well-known TTS model architecture.
    - **WaveNet:** A neural network for generating audio waveforms.

- **Steps:**
    1. **Gather Data:** Collect a large dataset of paired text and audio recordings.
    2. **Preprocess Data:** Clean the text, extract features from the audio, and align the text with the audio.
    3. **Train Model:** Use a deep learning framework to train your TTS model on the preprocessed data.
    4. **Inference:** Deploy your trained model to convert new text into speech.


Option 4:
```python
from pydub import AudioSegment
from pydub.generators import Sine

# Text-to-speech section
intro_text = """
Welcome to this short meditation practice. Begin by sitting comfortably, with your back straight 
and your hands resting gently on your lap. Close your eyes, and take a deep breath in through your nose, 
and slowly exhale through your mouth.
"""

# Using silent audio as a placeholder for the 2-minute meditation duration
pause = AudioSegment.silent(duration=120000)  # 2 minutes of silence

# End of meditation message
end_text = "This concludes your meditation. When you're ready, gently open your eyes."

# Generate audio segments with TTS
intro_audio = AudioSegment.silent(duration=1000)  # Placeholder to simulate TTS audio
end_audio = AudioSegment.silent(duration=1000)    # Placeholder to simulate TTS audio

# Combine intro, 2-minute pause, and end audio
meditation_audio = intro_audio + pause + end_audio

# Save the audio file
output_file = "/mnt/data/meditation_practice_with_pause.mp3"
meditation_audio.export(output_file, format="mp3")

output_file
```