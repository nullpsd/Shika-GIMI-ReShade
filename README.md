**🌌 Automated Installation Guide for Shika + GIMI + ReShade**

**📦 Required Files**

1. **Shika** — [Download](https://github.com/shika-hub/shika-releases/releases)
1. **XXMI Launcher (Portable Version)** — [Download](https://github.com/SpectrumQT/XXMI-Launcher/releases)
1. **ReShade** — [Download](https://reshade.me/)
1. **XXMI ReShade Add-on (zip)** — [Download](https://gamebanana.com/tools/18082)
1. **500 ml of coffee** (optional but helpful ☕)

This setup was personally designed and improved by [@null.psd](https://github.com/nullpsd) to **automate** integration—regardless of where your game is located!

-----
**🧩 Two Setup Options**

**Option 1: Fully Ready-to-Use**

💡 Easiest and fastest:
📥 [Download the working setup](https://github.com/nullpsd/Shika-GIMI-ReShade) and just set your game folder path in XXMI Launcher settings, ReShade Shader/Texture folders in settings (Home key - Settings). 

💡 Download the archive, which contains 6 parts. Open GENSH.part01.rar and extract it.
The most important thing is that all files end up inside a folder named GENSH.

**Option 2: Manual Setup**

📚 Learn the process and configure everything yourself—educational, but takes longer.

-----
**🗂️ Step-by-Step Manual Setup**

**1. Create a Folder**

Create a folder anywhere and name it GENSH (or anything you prefer).

-----
**2. Download & Setup XXMI**

1. Inside GENSH, create a folder: XXMI
1. Extract contents of XXMI-Launcher-Portable.zip into ```GENSH/XXMI```
1. Run XXMI Launcher.exe from ```GENSH/XXMI/Resources/Bin```
1. In the launcher:
   1. Select **Genshin Impact** under "Select Games To Mod"
   1. Click the game icon (top-left), then gear icon (top-right)
   1. Set Game Folder to your Genshin folder (e.g. ```D:/Games/HoYoPlay/games/Genshin Impact game```)
1. Back on the main screen, click the 3 dots → **Repair GIMI**
1. Wait until it finishes. A new GIMI folder appears inside XXMI
-----
**3. Setup Shika**

1. In GENSH, create folder shika
1. Place launcher.exe from Shika release into ```GENSH/shika```
1. From ```GENSH/XXMI/Resources/Bin```, create a shortcut of XXMI Launcher.exe and move it into the GENSH root
1. Run launcher.exe from shika, select GenshinImpact.exe path, then close it
-----
**4. Setup ReShade**

1. Download **ReShade with Add-on Support** and xxmi\_reshade\_add-on.zip
1. Create: ```GENSH/XXMI/ReShade```
1. Extract into that folder:
   1. inject.exe
   1. XXMI-ReShade-Extension.vbs
   1. ReShade.ini
1. Do **NOT** use the included reshade64.dll (it’s outdated)
1. On Desktop, create a temp folder, copy GenshinImpact.exe into it
1. Run ReShade\_Setup, choose copied .exe, select **DirectX 10/11/12**
1. Enable *all* effects by double-clicking group names
1. Click Next → Skip addons → Finish
1. Copy the new reshade-shaders folder to ```GENSH/XXMI/ReShade```
1. Create folder ```GENSH/XXMI/ReShade/presets```
1. Open ReShade\_Setup.exe with WinRAR, extract ReShade64.dll, move it to ```GENSH/XXMI/ReShade```
-----
**5. Configure XXMI for ReShade & Shika**

Open XXMI Launcher → Settings → **Advanced**
Enable:
   1. ✅ Run Pre-Launch
   1. ✅ Wait until it ends
In Pre-Launch field, paste:
   
  ``` ..\..\ReShade\XXMI-ReShade-Extension.vbs gi ```

Enable Custom Launch → Method: Hook
Paste into the field:

  ``` 'start ..\..\shika" launcher.exe' ```

-----
**6. Create BAT Files for Switching Modes**

**6.1 Create two files inside GENSH:**

- Genshin Impact on.bat
- Genshin Impact off.bat

**6.2 Paste into Genshin Impact off.bat:**

```
@echo off
setlocal enabledelayedexpansion
:: Base BAT Dir
set "baseDir=%~dp0"
:: XXMI Config path
set "inputFile=%baseDir%..\GENSH\XXMI\XXMI Launcher Config.json"
set "outputFile=%temp%\xxmi_tmp.json"
:: Replace XXMI config
> "%outputFile%" (
    for /f "usebackq delims=" %%A in ("%inputFile%") do (
        set "line=%%A"
        set "line=!line:"custom_launch_enabled": true="custom_launch_enabled": false!"
        echo !line!
    )
)
:: Rewrite original config file
move /Y "%outputFile%" "%inputFile%" >nul
:: Startup XXMI
cd /d "%baseDir%"..\GENSH\XXMI\Resources\Bin
start /B "" "XXMI Launcher.exe" --nogui --xxmi GIMI

```
**6.3 Paste into Genshin Impact on.bat:**

```
@echo off
setlocal enabledelayedexpansion
:: Base BAT Dir 
set "baseDir=%~dp0"
:: XXMI Config path
set "inputFile=%baseDir%..\GENSH\XXMI\XXMI Launcher Config.json"
set "outputFile=%temp%\xxmi_tmp.json"
:: Replace XXMI config
> "%outputFile%" (
    for /f "usebackq delims=" %%A in ("%inputFile%") do (
        set "line=%%A"
        set "line=!line:"custom_launch_enabled": false="custom_launch_enabled": true!"
        echo !line!
    )
)
:: Rewrite original config file
move /Y "%outputFile%" "%inputFile%" >nul
:: Startup XXMI
cd /d "%baseDir%"..\GENSH\XXMI\Resources\Bin
start /B "" "XXMI Launcher.exe" --nogui --xxmi GIMI
:: startup shika launcher.exe
cd /d "%baseDir%..\GENSH\shika"
start /B "" "launcher.exe"

```

-----
**7. Launching the Game**

1. Run .bat files as Administrator
1. Press **Home** in-game to open ReShade, skip tutorial
1. Go to **Settings** tab and set:
   1. Shader path:
      ```D:\GENSH\XXMI\ReShade\reshade-shaders\Shaders\\*\*```
   1. Texture path:
      ```D:\GENSH\XXMI\ReShade\reshade-shaders\Textures\\*\*```
1. Set hotkey to toggle effects
1. Place your ReShade presets in ```GENSH\XXMI\ReShade\presets```
1. In-game, click the large blue bar in ReShade and select your preset folder
-----
**✅ You're Done!**

- You can delete the GIMI Quick Start file from desktop
- For convenience, create shortcuts to the .bat files and enable **Run as Administrator**

⚠️ With ReShade enabled, the game may freeze on exit via Alt+F4 or normal menu
💡 **Solution:** Use Task Manager to close or enable “Fast Exit” in Shika → Settings → Misc

-----
**🗂️ Final Folder Structure**

GENSH\

├── shika\

│   ├── config.json

│   ├── imgui.ini

│   └── launcher.exe

├── XXMI\

│   ├── Backups

│   ├── GIMI

│   ├── Locale

│   ├── ReShade\

│   │   ├── presets

│   │   ├── reshade-shaders

│   │   ├── inject.exe

│   │   ├── ReShade.ini

│   │   ├── Reshade64.dll

│   │   └── XXMI-ReShade-Extension.vbs

│   ├── Resources

│   ├── Themes

│   └── XXMI Launcher Config

├── Genshin Impact on.bat

├── Genshin Impact off.bat

└── XXMI Launcher.exe (shortcut)

-----
**👤 Credits**

Guide & Automation by [@null.psd](https://github.com/nullpsd)

