LINUX CHEAT SHEET



Navigation \& Files



pwd                 # Show current directory

ls -l               # List files with details

cd <dir>            # Change directory

cd ..               # Go up one directory

mkdir <dir>         # Make folder

rm -r <dir/file>    # Delete folder/file

touch <file>        # Create empty file

cp <src> <dest>     # Copy file/folder

mv <src> <dest>     # Move/rename



File Viewing \& Editing



cat <file>          # View file content

less <file>         # Scrollable view

head -n 10 <file>   # First 10 lines

tail -n 10 <file>   # Last 10 lines

nano <file>         # Edit file (easy)

vim <file>          # Advanced editor





Permissions \& Ownership



ls -l               # Check permissions

chmod 755 <file>    # Change permissions

chown user:group <file> # Change owner/group



Networking \& Connectivity



ifconfig / ip a     # Show network interfaces

ping <ip/host>      # Test connectivity

netstat -tuln       # Open ports

ss -tuln            # Modern version of netstat

curl <url>          # Get web content

wget <url>          # Download file

nmap <ip>           # Scan ports



Processes \& System Monitoring



ps aux              # Running processes

top                 # Live process view

htop                # Interactive top (if installed)

kill <pid>          # Kill process

killall <name>      # Kill by name

df -h               # Disk usage

du -sh <dir>        # Folder size

free -h             # RAM usage

uptime              # System uptime



Searching



grep "text" <file>      # Search in file

grep -r "text" <dir>    # Search recursively

find /path -name <file> # Find file

locate <file>           # Fast search (database)



Compression \& Archiving

tar -cvf archive.tar <files>    # Create tar

tar -xvf archive.tar            # Extract tar

gzip <file>                     # Compress to .gz

gunzip <file.gz>                # Decompress



