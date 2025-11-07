🗂️ Backup Systems
📘 Overview

Backup Systems is a simple yet efficient shell-based project designed to automate the process of creating, managing, and restoring backups.
It ensures data safety by compressing target directories into .tar.gz archives and storing them in an organized folder structure.

This script is ideal for users who want a lightweight and customizable backup solution for local systems without relying on heavy third-party tools.

⚙️ Features

✅ Automated Backups – Create .tar.gz archives of any specified directory.
✅ Checksum Verification – Generates and verifies SHA-256 checksum files to ensure data integrity.
✅ Restore Option – Restore data from an existing backup archive.
✅ Backup Rotation – Keeps only the most recent backups, automatically removing older ones.
✅ Logging System – Every operation is logged with timestamps for easy troubleshooting.
✅ Dry Run Mode – Preview what would happen before executing real backup actions.
✅ PID Locking – Prevents multiple backup processes from running simultaneously.

📁 Project Structure
Backup-systems/
├── backups/           # Stores all generated backup files (.tar.gz)
├── logs/              # Stores log files for each backup or restore operation
├── test_data/         # Sample data used for testing the backup process
├── backup.config      # Configuration file (backup source, retention policy, etc.)
├── backup.sh          # Main backup script (core automation logic)
└── README.md          # Project documentation

🧰 Requirements

Before using this script, ensure the following are installed:

bash (v4 or later)

tar

sha256sum

gzip

coreutils (for date, mkdir, etc.)

🚀 Usage
1️⃣ Creating a Backup
./backup.sh ./test_data


This will:

Compress the test_data/ directory into a .tar.gz archive

Save it inside the backups/ folder

Generate a corresponding .sha256 checksum file

Log the process in the logs/ directory

2️⃣ Listing Available Backups
./backup.sh --list


Displays all available backup files inside the backups/ directory.

3️⃣ Restoring a Backup
./backup.sh --restore backup-2025-11-06-2254.tar.gz --to /path/to/restore/


Restores the specified backup file into the given destination path.

4️⃣ Verifying Backup Integrity
./backup.sh --verify backup-2025-11-06-2254.tar.gz


Checks the archive’s checksum to ensure it hasn’t been corrupted.

5️⃣ Dry Run (Simulation)
./backup.sh --dry-run ./test_data


Simulates a backup operation without actually creating or modifying any files.

🧾 Logs

All activities are recorded inside the logs/ directory with timestamps, e.g.:

[2025-11-06 22:54:55] INFO: Creating backup backup-2025-11-06-2254.tar.gz
[2025-11-06 22:55:10] SUCCESS: Backup created successfully.

⚖️ Configuration (backup.config)

You can customize the behavior of your backup process using the configuration file:

# Example backup.config
SOURCE_DIR=./test_data
BACKUP_DIR=./backups
LOG_DIR=./logs
RETENTION_COUNT=5

💡 Example Workflow

Place your files in the test_data/ directory.

Run the backup script:

./backup.sh ./test_data


Verify backups using:

./backup.sh --list


Restore a previous version when needed.

🛠️ Troubleshooting
Issue	Possible Cause	Solution
No such file or directory	The directory path or filename is incorrect	Verify the path and try again
Cannot connect to C: (on Windows Git Bash)	Tar command path issue	Use ./backup.sh ./folder_name from the project root
Backup not created	Permission issue	Run with appropriate privileges (chmod +x backup.sh)
👩‍💻 Author

Thanusha2002
A DevOps enthusiast passionate about automation, Linux, and backup solutions.

GitHub: @Thanusha2002
