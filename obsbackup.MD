# Automatically Back Up OBS Studio Settings on Fedora (NAS or Local)

> **Disclaimer:** Use at your own risk.

This guide sets up an automatic backup of your OBS Studio settings once a week using **systemd user timers** (a modern replacement for cron).

You can back up to either:
- A NAS mounted at `/mnt/nas_media/OBS-Backup/`
- A local folder in your home directory, like `~/obs-backups/`

This backup will:
- Keep your OBS settings safe
- Automatically delete backups older than 14 days
- Run weekly
- Stay fully controlled by you

---

## What You’ll Need

- Fedora Linux (not tested on other distros)
- OBS Studio installed
- Terminal
- KWrite (or your preferred text editor)
- Optional: A mounted NAS at `/mnt/nas_media/OBS-Backup/`

---

## Step 1: Create a Folder to Store Your Script

Run:

```bash
mkdir -p ~/.local/bin
```

What this does:

- `mkdir` means “make directory”
- `-p` creates the full path if it doesn’t exist
- `~/.local/bin` is a safe location for personal scripts

---

## Step 2: Create the Backup Script

Open a new script file:

```bash
kwrite ~/.local/bin/backup-obs.sh &
```

The `&` runs KWrite in the background so your terminal stays usable.

---

## Step 3: Add the Backup Script

### Option A – NAS Backup

```bash
#!/bin/bash

BACKUP_DIR="/mnt/nas_media/OBS-Backup"
```

### Option B – Local Backup

```bash
#!/bin/bash

BACKUP_DIR="$HOME/obs-backups"
```

Now paste the remainder below **for either option**:

```bash
# Create a timestamp
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")

# Define destination folder
DEST="$BACKUP_DIR/obs-studio-backup-$TIMESTAMP"

# Ensure destination exists
mkdir -p "$DEST"

# Copy OBS config
cp -r "$HOME/.config/obs-studio" "$DEST"

# Delete backups older than 14 days
find "$BACKUP_DIR" -type d -mtime +14 -exec rm -rf {} \;

# Confirmation message
echo "OBS backup created at $DEST"
```

### What Each Line Does

- `#!/bin/bash` — Runs the script using Bash
- `BACKUP_DIR` — Where backups are stored
- `TIMESTAMP` — Creates a unique time label
- `DEST` — Full backup path
- `mkdir -p` — Creates the destination folder
- `cp -r` — Copies the OBS configuration
- `find -mtime +14` — Deletes backups older than 14 days
- `echo` — Prints confirmation

Save the file and close the editor.

---

## Step 4: Make the Script Executable

```bash
chmod +x ~/.local/bin/backup-obs.sh
```

- `chmod` changes permissions
- `+x` makes the script executable

---

## Step 5: Test the Script

If using a NAS, confirm it is mounted:

```bash
ls /mnt/nas_media/OBS-Backup/
```

Run the script:

```bash
~/.local/bin/backup-obs.sh
```

You should see:

```
OBS backup created at /mnt/nas_media/OBS-Backup/obs-studio-backup-YYYY-MM-DD_HH-MM-SS
```

Or for local:

```
OBS backup created at /home/yourname/obs-backups/obs-studio-backup-YYYY-MM-DD_HH-MM-SS
```

---

## Step 6: Set Up Weekly Automatic Backups with systemd

Fedora uses systemd, which includes a built-in scheduler.

### Step 6.1: Create the User systemd Directory

```bash
mkdir -p ~/.config/systemd/user
```

---

### Step 6.2: Create the Service File

```bash
kwrite ~/.config/systemd/user/obs-backup.service &
```

Paste:

```ini
[Unit]
Description=Backup OBS Studio Settings

[Service]
Type=oneshot
ExecStart=%h/.local/bin/backup-obs.sh
```

Explanation:

- `Type=oneshot` runs once and exits
- `%h` means your home directory

Save and close.

---

### Step 6.3: Create the Timer File

```bash
kwrite ~/.config/systemd/user/obs-backup.timer &
```

Paste:

```ini
[Unit]
Description=Run OBS Backup Weekly

[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
```

Explanation:

- `OnCalendar=weekly` runs once per week
- `Persistent=true` catches missed runs
- `WantedBy=timers.target` ensures it starts with your session

Save and close.

---

## Step 7: Enable the Timer

```bash
systemctl --user daemon-reload
systemctl --user enable --now obs-backup.timer
```

- `daemon-reload` refreshes systemd
- `enable` makes it start on login
- `--now` starts it immediately

---

## Step 8: Verify It’s Active

```bash
systemctl --user list-timers
```

You should see something like:

```
NEXT                        LEFT     LAST     UNIT               ACTIVATES
Mon 2025-07-15 10:00:00     5 days   -        obs-backup.timer   obs-backup.service
```

---

## All Done

You now have a fully automated OBS Studio backup system that:

- Backs up weekly
- Deletes backups older than 14 days
- Works with NAS or local storage
- Is fully controlled by you
