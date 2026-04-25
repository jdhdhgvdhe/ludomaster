# Ludo Master: Complete Project Guide

Welcome to the **Ludo Master** documentation. This folder contains a comprehensive breakdown of the entire project, including its code logic, file structure, asset mapping, and build instructions.

## 1. Project Overview
Ludo Master is a professional Android game built using **Java**. It features a modern Material Design UI, custom animations, and multiple game modes including vs Computer, Local Multiplayer, and Team Play.

## 2. Technology Stack & Language
- **Primary Language**: Java (Android SDK)
- **Build System**: Gradle with Kotlin DSL (`build.gradle.kts`)
- **Min SDK**: API 24 (Android 7.0)
- **Target SDK**: API 34 (Android 14)
- **Libraries**:
    - **Glide**: Used for high-performance GIF and image rendering.
    - **Material Design**: For sleek UI components.
    - **MediaPlayer**: For all in-game sound effects and background music.

## 3. How to Build the Application

Follow these steps to build and run the app from source:

### A. Environment Setup
1.  Install **Android Studio** (latest version recommended).
2.  Ensure you have **JDK 17** configured in your IDE.

### B. Project Import
1.  Launch Android Studio and select **Open**.
2.  Navigate to the `e:\LudoMaster` directory and click **OK**.
3.  Wait for the **Gradle Sync** to finish.

### C. Building the APK
1.  Go to the top menu: `Build > Build Bundle(s) / APK(s) > Build APK(s)`.
2.  The generated APK will be located in `app/build/outputs/apk/debug/`.

### D. Running on Device
1.  Connect your Android phone via USB (with Developer Options & USB Debugging enabled).
2.  Click the green **Run** button (or press `Shift + F10`).

---

## 4. Documentation Index
- [**ASSET_MAPPING.md**](file:///e:/LudoMaster/ProjectDocs/ASSET_MAPPING.md): Detailed explanation of every image, sound, and animation file and where it is used in the code.
- [**CODE_STRUCTURE.md**](file:///e:/LudoMaster/ProjectDocs/CODE_STRUCTURE.md): Explanation of the Java classes (`MainActivity`, `HomeActivity`, etc.) and the logic flow.

---
*Created by Antigravity AI for the LudoMaster Project.*
