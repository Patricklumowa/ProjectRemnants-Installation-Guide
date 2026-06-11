# Project Remnants Installation Guide

This guide has three installation methods in one place:

1. Windows installer installation
2. Windows manual installation
3. Linux installation

Use the Windows installer first if you are on Windows. Use manual installation only if the installer cannot work for your setup.

## Before You Start

Close Project Zomboid before installing.

Make sure Project Remnants is installed from Steam Workshop or placed in your mods folder. The installer files should be inside the Project Remnants mod folder, in a folder named `root`.

The important files are:

```text
install_project_remnants.ps1
NPCFW.jar
```

They should be in the same folder.

For the Steam Workshop version, the folder is usually:

```text
<SteamLibrary>\steamapps\workshop\content\108600\3738362476\mods\ProjectRemnants\root
```

Example:

```text
C:\Program Files (x86)\Steam\steamapps\workshop\content\108600\3738362476\mods\ProjectRemnants\root
```

After installing, enable Project Remnants in the Project Zomboid Mods menu, then restart the game.

## 1. Windows Installer Installation

This is the recommended Windows method.

1. Open the Project Remnants `root` folder.
2. Find `install_project_remnants.ps1`.
3. Right-click `install_project_remnants.ps1`.
4. Click `Run with PowerShell`.
5. Wait for the installer to finish.
6. If it asks for your Project Zomboid install folder, paste the folder that contains `ProjectZomboid64.json`.
7. Restart Project Zomboid.

The installer will:

- find `NPCFW.jar`;
- find your Project Zomboid install folder;
- copy `NPCFW.jar` into the game root;
- back up `ProjectZomboid64.json`;
- patch `ProjectZomboid64.json`;
- validate the result.

The backup will be saved beside the original file with a name like:

```text
ProjectZomboid64.json.ProjectRemnantsBackup.20260604-235538
```

### If The Installer Closes Immediately

If the PowerShell window opens and closes instantly, run it manually so you can see the error message.

1. Open the Project Remnants `root` folder.
2. Click the address bar at the top of File Explorer.
3. Type:

```text
powershell
```

4. Press Enter. PowerShell should open inside the `root` folder.
5. Run:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\install_project_remnants.ps1
```

If it still cannot find Project Zomboid, run it with the game path:

```powershell
.\install_project_remnants.ps1 -ProjectZomboidPath "C:\Program Files (x86)\Steam\steamapps\common\ProjectZomboid"
```

Change the path if your game is installed in another Steam library.

### Common Installer Errors

If PowerShell says scripts are disabled:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\install_project_remnants.ps1
```

If Windows says the file is blocked:

1. Right-click `install_project_remnants.ps1`.
2. Click `Properties`.
3. If there is an `Unblock` checkbox, enable it.
4. Click `Apply`.
5. Run the installer again.

If the installer says `NPCFW.jar` is missing:

1. Make sure you are running the script from the `root` folder.
2. Make sure `NPCFW.jar` is in the same folder as `install_project_remnants.ps1`.
3. Re-download or reinstall Project Remnants if the jar is missing.

If the game fails before the game window opens and mentions this:

```text
Could not find agent library instrument
Module java.instrument may be missing from runtime image
```

Run the installer again. If it still happens, use the manual installation below and make sure the JRE DLL files are copied correctly.

## 2. Windows Manual Installation

Use this only if the Windows installer does not work.

### Step 1: Open The Game Folder

1. Open Steam.
2. Right-click Project Zomboid.
3. Click `Manage`.
4. Click `Browse local files`.

This opens the main Project Zomboid game folder. This folder contains `ProjectZomboid64.json`.

### Step 2: Copy The Required DLL Files

Open this folder:

```text
jre64\bin
```

Copy these files:

```text
instrument.dll
java.dll
jli.dll
vcruntime140.dll
api-ms-win-crt-convert-l1-1-0.dll
api-ms-win-crt-heap-l1-1-0.dll
api-ms-win-crt-runtime-l1-1-0.dll
api-ms-win-crt-stdio-l1-1-0.dll
api-ms-win-crt-string-l1-1-0.dll
msvcp140.dll
ucrtbase.dll
vcruntime140_1.dll
```

Paste them into the main Project Zomboid game folder.

Then open:

```text
jre64\bin\server
```

Copy this file:

```text
jvm.dll
```

Paste it into the main Project Zomboid game folder.

### Step 3: Back Up `ProjectZomboid64.json`

In the main Project Zomboid game folder, find:

```text
ProjectZomboid64.json
```

Make a backup copy of it before editing.

Example backup name:

```text
ProjectZomboid64.json.backup
```

### Step 4: Replace The JSON Contents

Open `ProjectZomboid64.json` with a text editor such as Notepad, Notepad++, or VS Code.

Do not use Microsoft Word.

Replace the entire file with this:

```json
{
    "mainClass": "zombie/gameStates/MainScreenState",
    "classpath": [
        "projectzomboid.jar",
        "<LOCATION_OF_YOUR_STEAM_LIBRARY>/steamapps/workshop/content/108600/3738362476/mods/ProjectRemnants/NPCFW.jar"
    ],
    "vmArgs": [
        "-Djava.library.path=win64/;.;jre64/bin;jre64/bin/server",
        "-javaagent:<LOCATION_OF_YOUR_STEAM_LIBRARY>/steamapps/workshop/content/108600/3738362476/mods/ProjectRemnants/NPCFW.jar",
        "-Djava.awt.headless=true",
        "-Xmx6144m",
        "-Dzomboid.steam=1",
        "-Dzomboid.znetlog=1",
        "-XX:-CreateCoredumpOnCrash",
        "-XX:-OmitStackTraceInFastThrow"
    ],
    "windows": {
        "6.1": {
            "vmArgs": [
                "-XX:+UseG1GC"
            ]
        },
        "10.0.17134": {
            "vmArgs": [
                "-XX:+UseZGC"
            ]
        }
    }
}
```

Replace this part:

```text
<LOCATION_OF_YOUR_STEAM_LIBRARY>
```

With your real Steam library path.

Example:

```text
C:/Program Files (x86)/Steam
```

So the jar path becomes:

```text
C:/Program Files (x86)/Steam/steamapps/workshop/content/108600/3738362476/mods/ProjectRemnants/NPCFW.jar
```

The `-Xmx6144m` value means Project Zomboid can use up to about 6 GB of RAM.

You can increase it if you want:

```text
8192m = 8 GB
10240m = 10 GB
12288m = 12 GB
```

Do not lower it below `6144m`.

### Step 5: Start The Game

1. Start Project Zomboid.
2. Enable Project Remnants in the Mods menu.
3. Restart Project Zomboid.
4. Play.

Project Zomboid is bad at reloading Java-side mod changes while already running, so always restart after enabling the mod.

## 3. Linux Installation

Linux installer source:

```text
https://github.com/Mahad-Ibrahim/Installer_Project_Remnants_Linux-
```

The Linux installer is a Bash script named:

```text
scriptForLinux.sh
```

### Requirements

You need `jq`.

Install it with your distro package manager if it is missing.

Debian, Ubuntu, Linux Mint:

```bash
sudo apt install jq
```

Fedora:

```bash
sudo dnf install jq
```

Arch:

```bash
sudo pacman -S jq
```

Do not run the Project Remnants installer itself with `sudo`.

### Step 1: Open The Linux Mod Folder

For native Steam, the Project Remnants folder is usually:

```text
~/.local/share/Steam/steamapps/workshop/content/108600/3738362476/mods/ProjectRemnants
```

For Flatpak Steam, it is usually:

```text
~/.var/app/com.valvesoftware.Steam/.local/share/Steam/steamapps/workshop/content/108600/3738362476/mods/ProjectRemnants
```

Open a terminal in that folder.

The folder must contain:

```text
NPCFW.jar
```

### Step 2: Download The Script

Run:

```bash
curl -L -o scriptForLinux.sh https://raw.githubusercontent.com/Mahad-Ibrahim/Installer_Project_Remnants_Linux-/main/scriptForLinux.sh
```

If you do not want to use `curl`, download `scriptForLinux.sh` from the Linux installer repository and place it in the Project Remnants folder.

### Step 3: If You Use A Custom Steam Library

If Project Zomboid is not in the normal Steam or Flatpak location, edit `scriptForLinux.sh` before running it.

Find this line:

```bash
GAME_PATH=""
```

Put the path to the folder that contains `ProjectZomboid64.json` between the quotes.

Example:

```bash
GAME_PATH="/mnt/games/SteamLibrary/steamapps/common/ProjectZomboid"
```

To find the path:

1. Open the Project Zomboid install folder.
2. Open a terminal there.
3. Run:

```bash
pwd
```

4. Copy the output into `GAME_PATH`.

### Step 4: Run The Script

Run:

```bash
chmod +x scriptForLinux.sh
./scriptForLinux.sh
```

You should see:

```text
Success! Project Remnants is installed.
```

### Linux Troubleshooting

If the script says `jq` is missing, install `jq`, then run the script again.

If the script says `NPCFW.jar` is missing, make sure you are running it from the Project Remnants mod folder.

If the script says Project Zomboid was not found, set `GAME_PATH` manually in `scriptForLinux.sh`.

If the game does not start after installation, restore the backup JSON created by the installer, then check that:

- `NPCFW.jar` was copied into the Project Zomboid game folder;
- `ProjectZomboid64.json` contains `-javaagent:NPCFW.jar`;
- Project Remnants is enabled in the Mods menu;
- the game was restarted after enabling the mod.

