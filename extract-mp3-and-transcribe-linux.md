# Extract MP3 and Transcribe Audio on Linux

**Disclaimer: Use at your own risk.**

This guide shows how to create three simple custom commands on a Fedora-based Linux system:

- `mp3` extracts audio from video files
- `whisper` transcribes audio into text locally
- `mic` extracts only microphone audio from OBS recordings

Everything installs into your home directory to keep your system clean and easy to maintain.

---

# Overview

After setup, you will be able to run:

```bash
mp3 video.mp4
whisper audio.mp3
mic recording.mkv
```

---

# Part 1: Create the mp3 Command

## What this does

This creates a custom command called `mp3`.

Example:

```bash
mp3 your-video.mp4
```

Output:

```bash
your-video.mp3
```

---

## Step 1: Install ffmpeg

Install ffmpeg:

```bash
sudo dnf install ffmpeg
```

This installs the audio and video conversion tool.

---

## Step 2: Create your personal commands folder

Create a personal scripts folder:

```bash
mkdir -p ~/bin
```

This folder will store your custom commands.

---

## Step 3: Create the mp3 script

Open the file in KWrite:

```bash
kwrite ~/bin/mp3
```

Paste this:

```bash
#!/bin/bash

input="$1"
output="${input%.*}.mp3"

ffmpeg -i "$input" -vn -acodec libmp3lame -ab 192k "$output"
```

Save and close.

Make it executable:

```bash
chmod +x ~/bin/mp3
```

---

## Step 4: Ensure ~/bin is in your PATH

Check your PATH:

```bash
echo $PATH
```

If you do not see `/home/yourusername/bin`, add it:

```bash
kwrite ~/.bashrc
```

Add this line at the bottom:

```bash
export PATH=$HOME/bin:$PATH
```

Apply the change:

```bash
source ~/.bashrc
```

Test:

```bash
mp3 video.mp4
```

---

# Part 2: Install Whisper.cpp

Whisper.cpp runs speech-to-text locally on your computer.

---

## Step 1: Create projects folder

```bash
mkdir -p ~/projects
cd ~/projects
```

---

## Step 2: Download Whisper.cpp

```bash
git clone https://github.com/ggerganov/whisper.cpp
cd whisper.cpp
```

Create build folder:

```bash
mkdir build
cd build
```

---

## Step 3: Build Whisper.cpp

```bash
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --config Release
```

This creates the whisper executable.

---

## Step 4: Download the model

Download medium English model:

```bash
../models/download-ggml-model.sh medium
```

This is about 1.5 GB.

---

# Part 3: Create the whisper Command

Open the script:

```bash
kwrite ~/bin/whisper
```

Paste:

```bash
#!/bin/bash

~/projects/whisper.cpp/build/bin/whisper-cli \
  -m ~/projects/whisper.cpp/models/ggml-medium.bin \
  -f "$1"
```

Make executable:

```bash
chmod +x ~/bin/whisper
```

Usage:

```bash
whisper audio.mp3
```

The transcription appears in the terminal.

---

# Part 4: Create the mic Command

This extracts only microphone audio from OBS recordings.

OBS typically stores mic audio on track 2.

---

## Step 1: Create the script

```bash
kwrite ~/bin/mic
```

Paste:

```bash
#!/bin/bash

input="$1"
base="${input%.*}"
output="${base}.mic.mp3"

ffmpeg -i "$input" -map 0:a:1 -acodec libmp3lame -ab 192k "$output"
```

Save and close.

---

## Step 2: Make executable

```bash
chmod +x ~/bin/mic
```

---

## Step 3: Use it

Example:

```bash
mic "2025-07-09 17-49-24.mkv"
```

Output:

```bash
2025-07-09 17-49-24.mic.mp3
```

---

# Recommended Workflow

Best workflow for video creation:

1. Record video in OBS
2. Edit video
3. Export final video
4. Extract audio from final video:

```bash
mp3 final-video.mp4
```

5. Transcribe:

```bash
whisper final-video.mp3
```

This ensures transcription matches your published content.

---

# Folder Structure

After setup, your home folder will contain:

```
~/bin/
├── mp3
├── mic
└── whisper

~/projects/
└── whisper.cpp/
```

---

# Summary

You now have three powerful custom commands:

Extract audio:

```bash
mp3 video.mp4
```

Extract microphone audio:

```bash
mic recording.mkv
```

Transcribe audio locally:

```bash
whisper audio.mp3
```

Everything runs locally, securely, and privately.

No cloud services required.
