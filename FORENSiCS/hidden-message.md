# 🕵️ Challenge Writeup – Hidden Data in `suspicious.png`
![image](https://github.com/user-attachments/assets/40265087-b668-4222-b700-cafa9f767412)

## 🧩 Challenge Description
> We are given a suspicious image file (`suspicious.png`). We suspect it may contain a hidden message or flag. Our goal is to extract any hidden information from this image.

---

## 🔎 Step 1: Initial File Inspection
We started by analyzing the image in a hex editor:

- Magic bytes confirmed it's a **valid PNG file**:
  ```
  89 50 4E 47 0D 0A 1A 0A   => .PNG....
  ```

- Visually, the image appears as a **color gradient**, which is often a cover for steganography.

---

## 🛠️ Step 2: LSB Extraction with Python

### 🔧 Script: `extract_lsb.py`
VIết file py check ảnh vì nghi ngờ nó là giấu bằng kỹ thuật LSB (Least Significant Bit steganography)
```python
from PIL import Image

# Load the image
img = Image.open("suspicious.png")
pixels = list(img.getdata())

# Prepare bit buffer
bits = ""

# Extract LSBs from RGB values
for pixel in pixels:
    for channel in pixel[:3]:  # Ignore alpha channel if present
        bits += str(channel & 1)

# Group bits into bytes
message = ""
for i in range(0, len(bits), 8):
    byte = bits[i:i+8]
    if len(byte) < 8:
        break
    char = chr(int(byte, 2))
    message += char
    if "}" in message:  # Likely end of a flag
        break

print("🔓 Hidden message:", message)
```

---

## 🧪 Step 3: Running the Script

![image](https://github.com/user-attachments/assets/022213f4-9eb7-4c48-b8bd-7ee32cbaf119)

```

> ✅ **Flag found: `tjctf{steganography_is_fun}`**

---

## 📌 Notes

- LSB steganography is effective because changing the least significant bit of color channels usually does **not significantly alter** the visible image.
- This method works best on **lossless formats** like PNG. It won’t work reliably with JPEG (which uses lossy compression).

---

## 🧠 Lessons Learned

- Always suspect LSB if an image looks too normal or has gradients.
- Tools like `stegsolve`, `zsteg`, or custom scripts with Pillow can help in CTFs.
- Knowing file format structure helps validate whether the file is tampered or not.
