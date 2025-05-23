# GPO Secure Tool - README

## 🔐 What is This Tool?

The **GPO Secure Tool** is a Python-based utility to **export** and **reapply** Group Policy Objects (GPOs) securely on a Windows machine. It uses Microsoft's `lgpo.exe` to perform GPO operations and stores backups in an organized and integrity-checked format.

---

## 📦 Features

- Export GPOs into timestamped, versioned `.zip` backups.
- Generate SHA-256 hash files for tamper detection.
- Use a rotating log system for all events and errors.
- GUI interface with two main buttons: **Export GPOs** and **Reapply GPOs**.
- Logs stored in `/GPO_Backups/logs/gpo_tool.log`.

---

## 🛠️ Setup Instructions

### 1. Prerequisites

- Install Python (3.8+ recommended)
- Install required libraries:
  ```bash
  pip install pyinstaller
  ```

- Download `LGPO.exe` from Microsoft:
  - https://www.microsoft.com/en-us/download/details.aspx?id=55319
  - Place it in the same directory as your script or ensure it’s in PATH.

---

### 2. Running the Script (Developer Mode)

To create the **baseline export and hash**, do the following:

#### 🔧 Step-by-step:
1. Open the script.
2. Find the line:
   ```python
   GENERATE_MODE = False
   ```
3. Change it to:
   ```python
   GENERATE_MODE = True
   ```
4. Run the script:
   ```bash
   python gposecuretool.py
   ```

5. It will create a baseline GPO `.zip` and `.hash` file in:
   ```
   ./GPO_Backups/
   ```

6. You will see a message:
   ```
   Baseline generated. Now set GENERATE_MODE = False.
   ```

7. Now go back to the script and set:
   ```python
   GENERATE_MODE = False
   ```

---

## 📤 Exporting GPOs

1. Launch the script normally.
2. Click **Export GPOs**.
3. It will:
   - Export to a folder
   - Zip it
   - Create a hash
   - Save to `./GPO_Backups/GPO_Backup_<number>_<timestamp>.zip`

If integrity check is disabled (`SKIP_INTEGRITY = True`), it won't verify. For added security, set:
```python
SKIP_INTEGRITY = False
```

---

## 📥 Reapplying GPOs

1. Click **Reapply GPOs**.
2. A file browser will pop up.
3. Select a `.zip` backup exported by this tool.
4. If `SKIP_INTEGRITY = False`, the script will check the `.hash` file next to the zip.
5. If verified, the GPO will be restored to your system using:
   ```bash
   lgpo.exe /g <unzipped_folder>
   ```

---

## 📁 Folder Structure

```
GPO_Backups/
├── GPO_Backup_001_20240101_101010.zip
├── GPO_Backup_001_20240101_101010.hash
├── logs/
│   └── gpo_tool.log
```

---

## 🧪 Logs and Debugging

- Logs are stored in:
  ```
  GPO_Backups/logs/gpo_tool.log
  ```

- Uses **RotatingFileHandler**:
  - 500KB per log file
  - Keeps last 5 logs

---

## 🧯 Safety Notes

- Always run as **Administrator**.
- Do not edit `.zip` or `.hash` manually.
- Export and test on isolated environments first.

---

## 🧑‍💻 Author Notes

- This script is intended for internal enterprise use.
- Tamper detection is critical: always maintain hash integrity if `SKIP_INTEGRITY = False`.
