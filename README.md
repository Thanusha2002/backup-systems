#  Bash Scripting Project: Automated Backup System

##  Project Overview

This project is an **Automated Backup System** built using Bash scripting. It automatically backs up important folders, verifies the backups, deletes old backups according to retention rules, and logs every action.

### Why It’s Useful

* Prevents data loss by automatically backing up folders.
* Cleans old backups to save disk space.
* Verifies backup integrity with checksums.
* Configurable through an external file (`backup.config`).
* Supports dry-run, restore, and list features.

---

##  Installation Steps

1. Clone your project repository:

   ```bash
   git clone https://github.com/<yourusername>/backup-system.git
   cd backup-system
   ```
2. Make the script executable:

   ```bash
   chmod +x backup.sh
   ```
3. Open `backup.config` and update settings:

   ```bash
   BACKUP_DESTINATION=/home/user/backups
   EXCLUDE_PATTERNS=".git,node_modules,.cache"
   DAILY_KEEP=7
   WEEKLY_KEEP=4
   MONTHLY_KEEP=3
   ```
4. Create a test folder:

   ```bash
   mkdir -p ~/test_src
   echo "Hello Backup" > ~/test_src/sample.txt
   ```

---

##  How to Use It

###  Dry Run (Test Mode)

Check what will happen without actually creating backups:

```bash
./backup.sh --dry-run ~/test_src
```

**Example output:**

```
[2025-11-03 10:00:00] INFO: Dry run mode enabled.
[2025-11-03 10:00:00] INFO: Would backup folder: /home/user/test_src
[2025-11-03 10:00:00] INFO: Would create archive: backup-2025-11-03-1000.tar.gz
[2025-11-03 10:00:00] INFO: Would skip: .git, node_modules, .cache
```

###  Create a Backup

```bash
./backup.sh ~/test_src
```

**Example output:**


[2025-11-03 10:02:15] INFO: Starting backup of /home/user/test_src
[2025-11-03 10:02:20] SUCCESS: Backup created: backup-2025-11-03-1002.tar.gz
[2025-11-03 10:02:21] INFO: Checksum verified successfully
[2025-11-03 10:02:23] INFO: Deleted old backup: backup-2025-10-10-0800.tar.gz


###  List Available Backups

```bash
./backup.sh --list
```

Shows all available backups with size and date.

###  Restore From a Backup

```bash
./backup.sh --restore backup-2025-11-03-1002.tar.gz --to ~/restored_test
```

Restores your data to a new folder.

---

##  How It Works

### 1. Backup Creation

* Uses `tar` to compress files into `backup-YYYY-MM-DD-HHMM.tar.gz`.
* Generates SHA256 checksum file to verify integrity.

### 2. Verification

* Recalculates checksum and compares it with saved `.sha256` file.
* Extracts a test file from the backup to confirm it’s not corrupted.

### 3. Rotation Algorithm (Auto Cleanup)

Keeps:

* **Last 7 daily** backups
* **Last 4 weekly** backups
* **Last 3 monthly** backups

Deletes any backups older than these.

### 4. Logging

Every action is saved to `backup.log`.

**Example `backup.log` snippet:**

```
[2025-11-03 10:02:15] INFO: Starting backup of /home/user/test_src
[2025-11-03 10:02:20] SUCCESS: Backup created: backup-2025-11-03-1002.tar.gz
[2025-11-03 10:02:21] INFO: Checksum verified successfully
[2025-11-03 10:02:23] INFO: Deleted old backup: backup-2025-10-10-0800.tar.gz
```

---

##  Design Decisions

* **Modular Functions:** Each action (backup, verify, cleanup) is a separate function for clarity.
* **Lock File:** Prevents multiple simultaneous runs.
* **Config File:** Easy to customize without touching code.
* **Dry Run Mode:** Lets users test without risk.

---

##  Testing

| Test           | Command                                                 | Expected Result              |
| -------------- | ------------------------------------------------------- | ---------------------------- |
| Dry Run        | `./backup.sh --dry-run ~/test_src`                      | Shows planned actions only   |
| Real Backup    | `./backup.sh ~/test_src`                                | Creates archive and checksum |
| List           | `./backup.sh --list`                                    | Lists backups                |
| Restore        | `./backup.sh --restore backup-... --to ~/restored_test` | Extracts backup              |
| Error Handling | `./backup.sh /invalid/path`                             | Shows error message          |

---

##  Known Limitations

* Incremental backups not implemented.
* Email notification simulated (writes to `email.txt`).
* Rotation algorithm assumes consistent timestamp format.

---

##  Example Folder Structure

```
backup-system/
├── backup.sh
├── backup.config
├── README.md
└── backups/
    ├── backup-2025-11-03-1002.tar.gz
    ├── backup-2025-11-03-1002.tar.gz.sha256
    └── backup.log
```

Conclusion

This Bash Scripting project successfully automates the process of creating, verifying, and managing backups. It ensures important files are safely stored, old backups are automatically cleaned, and errors are handled efficiently. The script is configurable, reliable, and easy to use—making it a practical tool for regular system backups.




