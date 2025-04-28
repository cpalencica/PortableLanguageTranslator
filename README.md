# Portable Language Translator - Engineering Addendum

This document serves as knowledge transfer guide for any potential future teams working on the Portable Language Translator project. It outlines the current state of the project, technical challenges, implementation details and known issues to assist in understanding and further devloping this system.

## Project Overview
The Portable Language Translator is a wearable device that enables real-time translation between spoken languages (currently English, Spanish, Korean) and basic American Sign Langauge (ASL) gestures. The system uses a Raspberry Pi 5, camera, microphone, touch screen and speakers housed in a 3D-printed enclosure with shoulder straps for portability.

## Core Functionality
- **Speech-to-Speech Translation:** Translates between English, Spanish and Korean using Google Cloud APIs
- **ASL Gesture Recognition:** Recognizes 5 ASL gestures ("Hello", "Thank You", "Bathroom", "Help", "Yes") and converts them into speech
- **Two-way Communication:** Enables conversation between ASL users and non-signers

## Repository Structure
This repository is organized into three main folders:
- **sw(Software):** Contains all the software-related code, scripts and documentation. A detailed README_SOFTWARE.md specific to the software components can be found inside this folder.
- **hw(Hardware):** Contains all the hardware-related files, schematics and documentation. The hw folder includes its own README_HARDWARE.md with hardware-specific details.
- **fw(Firmware):** Contains firmware code and documentation. The fw folder also has a dedicated README_FIRMWARE.md explaining the firmware components.

Each subfolder provides its own README that describes the specific components and instructions that relate to that part of the Portable Language Translator.

## Technical Architecture
### Hardware Components
- Raspberry Pi 5
- USB 2.0 Camera
- USB Microphone
- USB Speaker
- Touch Screen
- 5000 mAh battery pack
- 3D-printed enclosure

### Language Translation Pipeline
1. **Audio Capture:** Using USB microphone and WebRTC VAD for speech detection
2. **Speech-to-Text:** Google Cloud Speech-to-Text API
3. **Translation:** Google Cloud Translation API
4. **Text-to-Speech:** Google Cloud Text-to-Speech API
5. **Audio Output:** Through USB speakers

### ASL Recognition Pipeline
1. **Video Capture:** Using USB camera with OpenCV
2. **Hand Landmark Detection:** Using MediaPipe
3. **Gesture Classification:** LSTM model converted to TFLite
4. **Speech Synthesis:** Outputs recognized gesture as spoken words

### User Interface
- Built with PyQt5
- Three main tabs: Translation, WiFi Connection, Settings
- Volume controls and mode toggle (physical buttons)

## Getting Started
### Prerequisites
- Internet Connection (required for cloud based translation APIs)
- Python 3.9+ environment
- Power Source for the Raspberry Pi 5 (exterior battery pack)

### Initial Setup
1. **Power on:** Press the power button on the battery pack once
2. **Connect to WiFi:** Navigate to WiFi tab and connect to a network
3. **Configure Settings:** Set your preferred language options in the Settings tab
4. **Start Translation:** Return to the Translation tab to begin using the device

### Operating Modes
- **Speech Translation Mode**: Speak and the device will detect the language being spoken and translate bidirectionally with the base language set in the Settings tab.
- **ASL Mode:** Press the button on the left side to switch modes. The camera feed will display and detect ASL gestures.

## Technical Gotchas and Known Issues
### Critical Information
- **Internet Dependency:** The speech translation functionality requires an internet connection to use Google Cloud APIs. ASL recognition works offline but with limited vocabulary.
- **Battery Life Constraints:** The device runs for approximately 3.125 hours on a full charge.
- **ASL Recognition Limitations:** The current implementation only recognizes 5 gestures with 80%+ accuracy. Optimal detection distance is 0.67 to 1 meter from camera and the performance degrades significantly past this range.
- **Speech Recognition Limitations:** Works best 0.5-1 meters from microphone and converstational volume is required for reliable operation. Background noise can impact performance.
- **Heat Management:** The Raspberry Pi 5 can get hot during extended use.
- **Water/Moisture Vulnerability:** The device is not waterproof or water-resistant. Keep away from liquids.

### Development Challenges
When extending this project, be aware of these challenges we encountered:
- **LSTM Model Optimization:** Converting the TensorFlow model to TFLite required balancing accuracy with performance. Further optimization might be possible.
- **Latency Management:** Speech translation introduces variable latency (0.34-1.90 seconds) depending on phrase length. ASL recognition has approximately 2.5 seconds of latency.
- **Model Training:** During our second prototype testing, the ASL recognition model was trained on a limited dataset. Ensure there is a significant amount of data for higher recognition accuracy.

### Future Improvements
Based on our experience, these areas would benefit from further development:
- **Expanded ASL Vocabulary:** The current system only recognizes 5 gestures. Training the model on more gestures would increase utility.
- **Offline Translation:** Implementing an on-device translation library would eliminate the need for an internet connection.
- **Environmental Robustness:** Adding more noise cancellation for the microphone and improving camera performance in varied lighting conditions.

## Debugging Tips
### Common Issues and Solutions
- **Device Won't Power On:** Check battery charge level on the external battery pack and verify connections between battery and Raspberry Pi.
- **No Audio Output:** Check volume settings and verify speaker connection.
- **Camera Not Working:** Check USB connection.
- **Slow Performance:** Check CPU temperature and verify proper cooling for the Raspberry Pi 5. 

