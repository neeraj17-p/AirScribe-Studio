# AirScribe 🎙️⚡

> **iQOO Hackathon 2026 Submission | Track: Productivity**
> 
> AirScribe is an on-device, voice-first ambient productivity assistant that transforms fleeting thoughts into structured tasks and pushes them directly to your desktop workspace without breaking your flow.

[Demo Video] - TBA

## ⚠️ The Problem
Professionals and students face constant context-switching. Capturing a quick task or meeting note requires looking away from the laptop, opening a mobile app, typing, and manually transferring that data later. Cloud-based voice apps introduce latency and privacy concerns for sensitive work data. 

## 💡 The Solution
AirScribe runs completely **offline and on-device**. You speak into your phone, and an on-device Small Language Model (SLM) cleans the transcription and extracts actionable items. Using the **iQOO Office Kit**, these formatted tasks instantly pop up on your laptop's clipboard or local filesystem, ready to paste into VS Code, Notion, or Jira.

## ✨ Key Features
*   **Zero-Latency Voice Capture:** Uses Android's native offline `SpeechRecognizer` for instant audio-to-text conversion.
*   **Private On-Device AI:** Integrates Google's MediaPipe LLM Inference API running a quantized SLM (Gemma 2B) locally on the phone to extract tasks and format markdown.
*   **Invisible Cross-Device Sync:** Leverages iQOO Office Kit's shared clipboard and file-drop capabilities to teleport formatted text from the phone's brain directly to the desktop cursor.

## 🏗️ System Architecture
1.  **Input:** User taps record (or floating widget) and speaks.
2.  **ASR Pipeline:** Audio → `SpeechRecognizer` → Raw Text.
3.  **NLU Pipeline:** Raw Text → `MediaPipe LLM (Prompt: Extract action items)` → Markdown Text.
4.  **Handoff:** Markdown Text → Android System Clipboard & `AirScribe_Sync.md`.
5.  **Office Kit Bridge:** Phone Clipboard syncs to Windows/Mac → User presses `Ctrl+V` on desktop.

## 🛠️ Tech Stack & Architecture

### 1. Core Application (Frontend & Background)
*   **Language:** Kotlin
*   **UI Toolkit:** Jetpack Compose (Material 3, dark mode supported)
*   **Architecture:** MVVM (Model-View-ViewModel) for cleanly separating AI inference logic from UI state
*   **Async Processing:** Kotlin Coroutines & WorkManager (ensures voice processing finishes even if the app is minimized)

### 2. On-Device AI & NLU (iQOO Snapdragon NPU)
*   **Speech-to-Text (ASR):** Android Native `SpeechRecognizer` API (Offline Mode) for zero-latency, completely private audio transcription.
*   **Local LLM (Task Extraction):** Llama 3.2 1B (Quantized w4a16) or Phi-3 Mini.
*   **Hardware Acceleration:** Deployed via **Qualcomm AI Hub** targeting the Snapdragon Hexagon NPU. This ensures high-speed token generation and minimal battery drain on the iQOO hackathon device.
*   **Prompting Strategy:** Hardcoded system instruction that strictly forces the model to output Markdown-formatted tasks (`- [ ] Task`).

### 3. Desktop Bridge (iQOO Office Kit Integration)
*   **Clipboard Handoff:** Android `ClipboardManager` API. Once the NPU formats the text, it is pushed to the system clipboard, which iQOO Office Kit automatically syncs to the connected PC for instant `Ctrl+V` pasting.
*   **File Sync:** Android `MediaStore` / `java.io.File` API. Saves long meeting summaries as `.md` files directly to local storage, which Office Kit instantly mirrors to the desktop workspace.

### 4. Design & Prototyping
*   **Figma:** Cross-platform user flow mapping.
    *   *Mobile UI:* Voice recording interface, waveform animations, and NPU processing status indicators.
    *   *Desktop UI:* Office Kit notification mockups and auto-populated VS Code/Notion screens for the final pitch deck.

### 5. Version Control & Demo
*   **VCS:** Git & GitHub
*   **Hardware Setup:** iQOO Loaner Device + Host Machine (Windows/Mac) running iQOO Office Kit.

## 🚀 Local Setup & Installation

### Prerequisites
*   Android Studio (Latest Version)
*   Physical iQOO Android Device (Android 11+)
*   iQOO Office Kit installed and paired on your host Windows/Mac machine.

### Build Instructions - TBA

## 👥 Team
*   **Neeraj Piralkar** 
*   **Tanmay Patil** 

