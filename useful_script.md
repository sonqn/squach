# Automating System Updates
```bash
#!/bin/bash
# Update and upgrade system packages
echo "Starting system update..."
sudo apt update && sudo apt upgrade -y
echo "System update completed."
```
# Automating System Maintenance
```bash
#!/bin/bash
# Clean up system packages
echo "Starting system cleanup..."
sudo apt autoremove -y
sudo apt autoclean
echo "System cleanup completed."
```
# Automating Disk Usage Monitoring
```bash
#!/bin/bash
# Check disk usage and send alert if usage exceeds 80%
THRESHOLD=80
df -h | awk '{ if($5+0 > THRESHOLD) print $0; }' | while read output;
do
    echo "Disk usage alert: $output"
done
```
# Automating Log Rotation
```bash
#!/bin/bash
# Rotate and compress log files
LOG_DIR="/var/log"
BACKUP_DIR="/var/log/backup"
TIMESTAMP=$(date +"%Y%m%d%H%M%S")
for log_file in $LOG_DIR/*.log;
do
    mv $log_file $BACKUP_DIR/$(basename $log_file)_$TIMESTAMP.log
    echo "Rotated log file: $log_file"
done
```
# Automating Backup
```bash
#!/bin/bash
# Backup a directory and store it in a backup folder with a timestamp
SOURCE="/path/to/important/data"
DEST="/path/to/backup/location"
TIMESTAMP=$(date +"%Y%m%d%H%M%S")
tar -czvf $DEST/backup_$TIMESTAMP.tar.gz $SOURCE
echo "Backup completed: $DEST/backup_$TIMESTAMP.tar.gz"
```
# Automating Security Updates
```bash
#!/bin/bash
# Update security packages
echo "Starting security update..."
sudo apt update && sudo apt upgrade -y --security
echo "Security update completed."
```
# Automating System Reboot
```bash
#!/bin/bash
# Reboot the system
echo "Rebooting the system..."
sudo reboot
```