# Benign In-App Keylogger Implementation

**Purpose:** To implement a benign, in-app keystroke logger using Python. This experiment focuses on the ethical considerations of logging, such as capturing keystrokes only when the application window has focus and only after obtaining explicit user consent.

### Prerequisites

1.  **Python:** You need Python installed. This script uses the `tkinter` library, which is part of the standard Python installation.
    * **How to check:** Open Command Prompt and type `python --version`.

2.  **Visual Studio Code (VS Code):** A code editor for writing the code.

---

### Step 1: Set Up the Project Folder

Let's create a dedicated folder for this experiment.

1.  **Open Command Prompt** and navigate to your Desktop:
    ```bash
    cd Desktop
    ```

2.  **Create and Enter the Project Folder:**
    ```bash
    mkdir keylogger-demo
    cd keylogger-demo
    ```

---

### Step 2: Open in VS Code and Create the Code File

Now, let's open the project in our code editor.

1.  **Open the folder in VS Code:** In the command prompt, type:
    ```bash
    code .
    ```

2.  **Create a Python File:**
    * In VS Code, create a new file and name it `keylogger_app.py`.

---

### Step 3: Add the Python Code

Copy the complete Python code below and paste it into your `keylogger_app.py` file.

```python
# keylogger_app.py
import tkinter as tk
from tkinter import messagebox
import csv
import datetime
import os

LOG_FILE = "keystrokes_log.csv"

def ensure_log_header():
    """Create the CSV file with a header if it doesn't exist."""
    if not os.path.exists(LOG_FILE):
        with open(LOG_FILE, "w", newline="", encoding="utf-8") as f:
            writer = csv.writer(f)
            writer.writerow(["timestamp_utc", "event_type", "key_name", "character", "window_focused"])

def log_event(event, event_type):
    """Logs the details of a keyboard or focus event to the CSV file."""
    ts = datetime.datetime.utcnow().isoformat() + "Z"
    keysym = getattr(event, "keysym", "")
    char = getattr(event, "char", "").replace('\r', '<Enter>')
    
    # Check if our application window is currently active
    is_focused = (app.focus_get() is not None)

    # Write the event details to the log file
    with open(LOG_FILE, "a", newline="", encoding="utf-8") as f:
        writer = csv.writer(f)
        writer.writerow([ts, event_type, keysym, char, is_focused])

def on_key_press(event):
    """This function is called whenever a key is pressed."""
    # We only log the key press if logging is enabled AND our window is in focus.
    if logging_enabled.get() and (app.focus_get() is not None):
        log_event(event, "KeyDown")

def on_focus_in(event):
    """This function is called when the window gains focus."""
    if logging_enabled.get():
        log_event(event, "FocusIn")

def on_focus_out(event):
    """This function is called when the window loses focus."""
    if logging_enabled.get():
        log_event(event, "FocusOut")

def request_consent_and_start():
    """Show a consent pop-up to the user before starting to log."""
    msg = (
        "This application would like to record keystrokes for this session.\n\n"
        "Data is recorded ONLY while this window is active and focused.\n\n"
        "Do you consent to start logging?"
    )
    if messagebox.askyesno("Consent Required", msg):
        logging_enabled.set(True)
        status_label.config(text="Logging Status: ON (Active in this window)", fg="green")
    else:
        logging_enabled.set(False)
        status_label.config(text="Logging Status: OFF", fg="red")

# --- GUI Setup ---
ensure_log_header()

app = tk.Tk()
app.title("Benign In-App Keylogger Demo")
app.geometry("600x400")

logging_enabled = tk.BooleanVar(value=False)

# Create and arrange widgets in the window
frame = tk.Frame(app, padx=15, pady=15)
frame.pack(fill="both", expand=True)

intro_label = tk.Label(frame, text="This application demonstrates a benign keylogger. It only records keystrokes with your consent and only when this window is active.", wraplength=560)
intro_label.pack(pady=(0, 10))

btn_consent = tk.Button(frame, text="Start/Stop Logging (Requires Consent)", command=request_consent_and_start)
btn_consent.pack(pady=5)

status_label = tk.Label(frame, text="Logging Status: OFF", fg="red", font=("Arial", 10, "bold"))
status_label.pack(pady=5)

text_area = tk.Text(frame, height=10, width=70)
text_area.pack(fill="both", expand=True, pady=(10, 0))
text_area.insert("1.0", "Click here and start typing after giving consent...\n")

# Bind events to the functions
app.bind_all("<Key>", on_key_press) # Binds all key presses
app.bind("<FocusIn>", on_focus_in)   # Binds the event when the window gets focus
app.bind("<FocusOut>", on_focus_out) # Binds the event when the window loses focus

# Start the GUI event loop
app.mainloop()
```

## Step 4: Run the Program

To execute the script:

1. Open the **Integrated Terminal** in VS Code (Menu → Terminal → New Terminal).  

2. Run the Python script with the following command:
```
python keylogger_app.py
```
A window titled "Benign In-App Keylogger Demo" will appear.

Step 5: How to Use and See the Output
Follow these steps to demonstrate the ethical and focus-aware design of the experiment.

Give Consent
Click the "Start/Stop Logging (Requires Consent)" button.

A pop-up will appear asking for your permission. Click "Yes".

The status label in the main window will turn green and display:
Logging Status: ON

Log Keystrokes (While App Is Focused)
Click inside the text box in the application window.

Type a message, for example:

```
Hello Akshat
```

Lose Focus
Click anywhere outside the application window (e.g., desktop or another application).

Type another message, for example:
```
this text should not be logged
```

Check the Logs
In your project folder (keylogger-demo), a file named keystrokes_log.csv will be created.

Open this file (for example, directly in VS Code).

Understand the Output
The keystrokes_log.csv file will contain entries similar to the following:

csv
```
timestamp_utc,event_type,key_name,character,window_focused
...,FocusIn,,,,True
...,KeyDown,H,H,True
...,KeyDown,e,e,True
...,KeyDown,l,l,True
...,KeyDown,l,l,True
...,KeyDown,o,o,True
...,KeyDown,space, ,True
...,KeyDown,A,A,True
...
...,FocusOut,,,,False
```

Observation:
Keystrokes typed while the application window had focus (e.g., "Hello Akshat") are recorded with window_focused=True.
Entries typed after the window lost focus are not recorded, demonstrating that logging stops when the application is not active.
