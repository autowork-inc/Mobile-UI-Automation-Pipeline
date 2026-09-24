# Mobile UI Automation Pipeline

## Overview
This is an Android automation orchestrator I built using Python, ADB, and UIAutomator2. I needed a reliable way to automate interactions and upload media within a complex mobile app that doesn't provide a public API and constantly changes its UI layout. Due to NDA, the exact interaction scripts are private, but here is a technical overview of how the pipeline works

## Key Features
The system reads task payloads from a local Excel file using openpyxl and executes them directly on a physical Android device. Since relying purely on Android XML nodes often fails, I built a custom vision core using OpenCV and EasyOCR. This allows the script to literally read the screen and tap on text targets. If the app throws an completely unexpected screen that neither the UI nodes nor OCR can handle, the script triggers a rescue module: it takes a screenshot and sends it to an AI Vision API, which analyzes the context and returns the exact coordinates to click to dismiss the obstacle
Because mobile networks and apps are inherently flaky, I had to build in a lot of resilience. I wrote a network manager module that physically interacts with Android's system settings via ADB to toggle Airplane mode, hard-reset Wi-Fi, and cycle VPN connections to test under different network conditions. There is also a file management module that handles pushing media to the device, clearing OS-level thumbnail caches, and using SQL commands to register the new files directly into the Android MediaStore database so the target app sees them instantly

## Tech Stack
**Python, UIAutomator2, ADB, OpenCV, EasyOCR, Gemini Vision API, openpyxl, pyotp**
