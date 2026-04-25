# Ludo Master: The Student & Developer Handbook

This handbook is the final entry point for any student or new developer. It explains how to navigate this large project and how to handle its massive source files.

---

## 1. How to Read Large Files (3000+ Lines)
In this project, `MainActivity.java` is very large. To understand it easily as a student, you should look at it in **Blocks**:

### A. The "Regions" Strategy
The code is organized into specific functional areas:
1.  **Variables (Top)**: All IDs and settings are at the top.
2.  **Inner Classes (Middle)**: The `Piece`, `Dice`, and `Player` classes are defined *inside* the main class to keep them connected.
3.  **Engine Logic (Center)**: Look for `move()` and `checkDeath()`.
4.  **UI Setup (Bottom)**: Most of the layout-related code like `initViews()` is grouped at the end.

### B. IDE Tools for Students
If you are using **Android Studio**:
- Press `Ctrl + F12` (or Cmd + F12 on Mac) to see the **File Structure**. This lets you jump straight to any method without scrolling.
- Use **Code Folding**: Click the small minus (`-`) icons next to method names to "collapse" code you aren't working on.

---

## 2. Folder Structure: Where is everything?
For a student, the Android folder structure can be confusing. Here is the simple version:

| Folder Path | What is inside? |
| :--- | :--- |
| `app/src/main/java/` | **The Brain**: All the Java logic (`.java` files). |
| `app/src/main/res/drawable/` | **The Graphics**: Every image, token, and button icon. |
| `app/src/main/res/layout/` | **The Design**: The XML files that define how the screen looks. |
| `app/src/main/res/raw/` | **The Sound**: All the music and SFX (`.mp3`). |
| `app/src/main/res/anim/` | **The Movement**: The XML files that define how things blink or spin. |

---

## 3. Documentation Guide: Which file should I read?
We have created 7 detailed manuals. Here is which one to use depending on your goal:

- **"I want to change the game's rules"**  
  👉 Read [**LINE_BY_LINE_EXPLANATION.md**](file:///e:/LudoMaster/ProjectDocs/LINE_BY_LINE_EXPLANATION.md)
- **"I want to change the images or sounds"**  
  👉 Read [**ASSET_MAPPING.md**](file:///e:/LudoMaster/ProjectDocs/ASSET_MAPPING.md)
- **"I want to build and install the app"**  
  👉 Read [**MAIN_GUIDE.md**](file:///e:/LudoMaster/ProjectDocs/MAIN_GUIDE.md)
- **"I want to understand every single variable and math formula"**  
  👉 Read [**MASTER_CODE_EXPLANATION.md**](file:///e:/LudoMaster/ProjectDocs/MASTER_CODE_EXPLANATION.md)
- **"I am a advanced developer looking for a method list"**  
  👉 Read [**ULTIMATE_ENGINE_INDEX.md**](file:///e:/LudoMaster/ProjectDocs/ULTIMATE_ENGINE_INDEX.md)

---

## 4. Tips for New Users
- **Start with the Splash**: Look at `SplashActivity` first to see how the app starts.
- **Trace the Dice**: To understand the game flow, follow the code from the `roll()` method in the `Dice` class.
- **Don't Scroll, Search**: Use `Ctrl + F` to find specific words like "winner" or "kill" to see how those features work.

---
*Created to make learning LudoMaster easy for everyone.*
