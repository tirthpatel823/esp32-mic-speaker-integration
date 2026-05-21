# ESP32 Real-Time Audio Streaming System

Using just a microphone, a speaker, and an ESP32, this project creates a complete real-time audio communication pipeline where the microcontroller can listen, process audio, and play responses back through a speaker. It demonstrates how an ESP32 can be used for applications such as voice assistants, intercom systems, sound-triggered automation, and live audio streaming.

The system combines an ESP32, a MAX98357 audio amplifier, and a Flask + WebSocket server to establish a microphone-to-server-to-speaker loop with support for downloadable audio recordings.

---

# Features

- Real-time microphone audio capture using ESP32  
- Audio streaming through WebSockets  
- Speaker playback using MAX98357 amplifier  
- Server-side WAV file generation and storage  
- Download recorded audio files from the server  
- Low-latency two-way audio pipeline  
- Simple architecture for learning audio streaming systems  

---

# Hardware

1. ESP32 Development Board  
2. INMP441 (or equivalent I2S digital microphone)  
3. MAX98357A (or equivalent I2S DAC / power amplifier)  
4. Full-range speaker (4–8Ω, 3–5W recommended)  
5. Power Supply  
   - ESP32: 5V via USB or regulated 5V / 3.3V supply  
   - MAX98357A: 5V recommended (can run on 3.3V with lower output)  
6. Breadboard and jumper wires  

---

# Software Components

## ESP32 Firmware (C++ / Arduino)

Handles:
- Real-time audio capture  
- Audio streaming to the server  
- Playback through the speaker  
- Communication and flow control  

## Server Backend (Python / Flask + Async WebSocket)

Handles:
- Receiving audio streams from ESP32  
- WAV file creation and storage  
- Streaming audio back to ESP32  
- Audio download endpoint  

---

# Key Design Choices

## Sample Format: 16kHz, 16-bit Mono

A 16kHz sample rate provides a good balance between voice intelligibility and bandwidth usage for real-time audio streaming. Using 16-bit mono audio keeps the data lightweight while maintaining clear voice quality and allowing a direct pathway into WAV file generation without additional conversion.

---

## Streaming in Small Chunks

Audio data is transmitted in small chunks to:
- Avoid large memory spikes on the ESP32  
- Reduce buffering overhead  
- Enable smoother playback with minimal latency  

This approach keeps the system responsive even with limited microcontroller memory.

---

## Circular Buffer with Watermarks

A circular audio buffer is used to manage incoming audio streams efficiently.

### Benefits
- High and low watermarks detect when incoming data is too fast or too slow  
- Prevents buffer overflows and underruns  
- Maintains smooth playback continuity  

### Flow Control
Pause and resume messages are used for flow control, ensuring the playback buffer remains within safe operating levels and reducing audio artifacts.

---

## Stereo Duplication for Playback

The MAX98357 expects stereo audio frames. Since the microphone input is mono, the audio samples are duplicated across both left and right channels during playback.

### Advantages
- Reduces CPU processing complexity  
- Keeps playback timing simple  
- Maintains compatibility with the MAX98357 amplifier  

---

# Project Workflow

1. ESP32 captures audio from the microphone  
2. Audio is streamed to the Flask server using WebSockets  
3. Server processes and stores the audio as a WAV file  
4. Audio can be streamed back to the ESP32  
5. MAX98357 amplifier outputs the audio through the speaker  
6. Recorded audio files can be downloaded from the server  

---

# Applications

- Voice Assistants  
- Smart Intercom Systems  
- Audio Monitoring Systems  
- IoT Sound Processing Projects  
- Real-Time Audio Communication  
- Educational DSP and Networking Projects  

---

# Tech Stack

- ESP32 Arduino Framework (C++)  
- Python  
- Flask  
- Async WebSockets  
- I2S Audio Communication  

---

# Future Improvements

- Noise suppression  
- Echo cancellation  
- Speech-to-text integration  
- Wake-word detection  
- AI voice assistant support  
- Cloud audio processing  
- Mobile/Web dashboard integration  
