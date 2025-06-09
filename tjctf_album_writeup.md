
# TJCtf Easter Egg Writeup: Hidden Audio in Album Cover

## Challenge: `forensics/album-cover`

> "I heard there's a cool easter egg in the new TJCtf album cover."

---

## Step 1: Provided Files

- `albumcover.png`: a grayscale image
- `abcover.py`: a Python script that generates `albumcover.png` from a WAV file

```python
# Key snippet from abcover.py
with wave.open('flag.wav', 'rb') as w:
    frames = np.frombuffer(w.readframes(w.getnframes()), dtype=np.int16)
arr = np.array(frames)
img = arr.reshape((441, 444))
img = (img + 32767) / 65535 * 255
img = img.astype(np.uint8)
Image.fromarray(img).save('albumcover.png')
```

**Insight**: File này đọc file .wav và in ra file albymcover.png

---

## Step 2: Reversing the Image to Recover Audio

Dùng gpt đảo ngược code:

```python
from PIL import Image
import numpy as np
import wave

img = Image.open("albumcover.png").convert('L')
arr = np.array(img)
arr = arr.astype(np.float32) / 255 * 65535 - 32767
arr = arr.astype(np.int16)
audio = arr.flatten()

with wave.open("recovered_flag.wav", "w") as w:
    w.setnchannels(1)
    w.setsampwidth(2)
    w.setframerate(44100)
    w.writeframes(audio.tobytes())
```

This gave us back a WAV file named `recovered_flag.wav`.

---

## Step 3: Spectrogram Analysis
![alt text](image.png)

Dùng gpt phân tích bằng phương pháp Spectrogram.

## Step 4: Flag Extraction
Upon enhancing the spectrogram contrast and viewing manually, we discovered the hidden message:

```
tjctf{THIS-EASTER-EGG-IS-PRETTY-COOL}
```

---

## ✅ Flag:
```txt
tjctf{THIS-EASTER-EGG-IS-PRETTY-COOL}
```

