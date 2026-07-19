# Installing TML Flow On Windows

This guide explains how to install, open, update, and remove the completed TML Flow desktop app. The packaged release currently supports 64-bit Windows and does not require Node.js, npm, or an internet connection for normal use.

Last reviewed: July 18, 2026.

## Table of contents

- What You Need
- Install The App
- Open TML Flow
- Optional: Verify The Download
- Update TML Flow
- Uninstall TML Flow
- Troubleshooting
  - Windows Blocks The Installer
  - The App Does Not Appear After Installation
  - A Project File Does Not Open By Double-Clicking
  - Normal Features Seem To Require Wi-Fi
- Build The Installer From The Source Folder (developer)
  - 1. Prepare The Windows Computer
  - 2. Install The Project Dependencies
  - 3. Build The Windows Installer
  - 4. Run The Generated Installer

---

## What You Need

Use the Windows installer whose name follows this pattern:

```
TML Flow-<version>-win-x64.exe
```
You only need the `.exe` file to install the app.

> The current installer is an unsigned external-test build. Install it only when it came from a source you trust. Windows may display an unknown-publisher or SmartScreen warning.

## Install The App

1. Double-click `TML Flow-<version>-win-x64.exe`.
2. If Windows displays a security confirmation, verify that this is the installer you expected. For a trusted unsigned test build, choose **More info**, then **Run anyway** if that option is available. A managed computer may require an administrator to approve the app.
3. Follow the TML Flow Setup wizard.
4. Choose the installation folder, or keep the suggested location.
5. Finish the wizard and allow it to launch TML Flow.

The installer adds TML Flow to the Start menu, creates a desktop shortcut, and registers `.tmlbundle` and `.tmlproj` project files with Windows where the operating system permits it.

## Open TML Flow

After installation, open the app in any of these ways:

- Double-click the **TML Flow** desktop shortcut.
- Open the Start menu, search for **TML Flow**, and select it.
- Double-click a saved `.tmlbundle` or `.tmlproj` project file.

On the TML Flow home screen, choose **Create Project** to start a new project or **Open Project** to open an existing project. Use **File > Save As** to save new work as a `.tmlbundle` file.

The app can run without Wi-Fi for normal modeling, project open/save, bundled Bible and map data, PDFs, exports, and built-in documentation. ESV lookup and external web links still require an internet connection.

## Optional: Verify The Download

If the release includes `SHA256SUMS.txt`, you can check that the installer was copied or downloaded without alteration. Open PowerShell in the folder containing the installer and run:

```powershell
Get-FileHash ".\TML Flow-<version>-win-x64.exe" -Algorithm SHA256
```

Compare the displayed hash with the installer entry in `SHA256SUMS.txt`. They must match exactly. A mismatch means you should not run that copy of the installer.

## Update TML Flow

1. Save and close any open projects.
2. Run the newer `TML Flow-<version>-win-x64.exe` installer.
3. Follow the setup wizard, then launch TML Flow.
4. Open an existing `.tmlbundle` and confirm that it loads before deleting the older installer.

Installing a newer version does not intentionally remove your project files or saved app data. Keep separate backups of important `.tmlbundle` files before an update.

## Uninstall TML Flow

1. Open **Windows Settings > Apps > Installed apps**.
2. Find **TML Flow**.
3. Open its menu, choose **Uninstall**, and confirm.

You can also remove it through **Control Panel > Programs and Features**. The uninstaller removes the installed application but leaves user-created project files and app data in place.

## Troubleshooting

### Windows Blocks The Installer

The current external-test build is unsigned, so SmartScreen, Device Guard, or your organization's security policy may block it. Confirm that the installer came from a trusted source. If **Run anyway** is unavailable, ask the computer's administrator to approve it; do not disable organizational security controls.

### The App Does Not Appear After Installation

Search for **TML Flow** in the Start menu. If it is missing, run the installer again and complete every setup page. You can also use Windows Settings to check whether TML Flow appears under **Installed apps**.

### A Project File Does Not Open By Double-Clicking

Open TML Flow first, then use **File > Open** and select the `.tmlbundle` or `.tmlproj` file. Windows may require you to choose TML Flow manually the first time a file association is used.

### Normal Features Seem To Require Wi-Fi

Restart TML Flow and try the action again. Normal project editing, bundled data, PDF tools, exports, and help are packaged with the app. Only ESV lookup and external web links are expected to require a connection.

## Build The Installer From The Source Folder (developer)

This section is only for someone who has the TML Flow source code and needs to create the installer. End users can skip it.

### 1. Prepare The Windows Computer

Install a current Node.js/npm development environment, then open a terminal in the TML Flow source folder.

### 2. Install The Project Dependencies

```powershell
npm install
```

### 3. Build The Windows Installer

```powershell
npm run desktop:dist:win
```

### 4. Run The Generated Installer

Open this file and follow the setup wizard:

```
release\TML Flow-<version>-win-x64.exe
