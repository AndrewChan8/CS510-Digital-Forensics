# Project 3: Creating and Analyzing ZFS Pools - Andrew Chan

### 1 - Creating the ZFS Evidence Image
Step 1: Configure VM and Add Virtual Disks

First, download the Ubuntu Desktop ISO for VirtualBox at: 
`https://ubuntu.com/download/desktop`

In the VirtualBox manager, select the VM and click on settings. There go to storage -> Controller: SATA, and add a hard disk. Then, create a new disk with the following settings:

```
Type: VDI
Allocation: Dynamically Allocated
Size: 10 GB
Name: ZFS_Disk_1
```
Repeat this process to create a second disk:
```
Name: ZFS_Disk_2
Size: 10 GB
```

### Step 2 - Install ZFS
Launch the VM using the Ubuntu ISO and install Ubuntu normally.

Install ZFS utilities:
```
sudo apt update
sudo apt install zfsutils-linux
```
Verify the installation:
```
zfs --version
```

### Step 3 - Create the ZFS Pool

Create a mirrored ZFS pool:
```
sudo zpool create project_pool mirror sdb sdc
```
Verify the pool status:
```
zpool status
```
The pool should now be mounted at:
```
/project_pool
```

### Step 4 - Add Data and Create Snapshots

Create an initial file:
```
echo "Version 1: Basic Config" | sudo tee /project_pool/file1.txt
```
Create the first snapshot:
```
sudo zfs snapshot project_pool@snap1
```
Continue modifying or adding files and creating additional snapshots to capture system changes.

### Step 5 - Plant Evidence Artifacts

**Artifact 1 – Hidden Information Inside an Image**

Download an image file:
```
monkey.jpeg
```
Create a secret message:
```
echo "user: help pw: banana" | sudo tee /project_pool/do_not_share.txt
```
Hide the message inside the image:
```
cat ~/Downloads/monkey.jpeg /project_pool/do_not_share.txt | sudo tee /project_pool/image.jpeg
```
Create a snapshot:
```
sudo zfs snapshot project_pool@snap3
```
**Artifact 2 – Deleting Evidence**

Delete the original text file:
```
sudo rm /project_pool/do_not_share.txt
```
Modify image timestamps to obscure activity:
```
sudo touch -t 202510300132 /project_pool/image.jpeg
```
Clear shell history:
```
history -w
rm ~/.bash_history
```
Create another snapshot:
```
sudo zfs snapshot project_pool@snap4
Artifact 3 – Disguised Bash Script
```
Create a simple script:
```
echo '#!/bin/bash' | sudo tee /project_pool/malware.sh
echo 'ping -c 4 8.8.8.8' | sudo tee -a /project_pool/malware.sh
```
Rename it to appear harmless:
```
sudo mv /project_pool/malware.sh /project_pool/holiday_photo.jpg
```
Create a snapshot:
```
sudo zfs snapshot project_pool@snap5
```

### Step 6 - Finalize Evidence and Export ZFS Pool

Set the checksum algorithm:
```
sudo zfs set checksum=sha256 project_pool
```
Export the entire pool with snapshots:
```
sudo zfs send -Rw project_pool@snap6 > ~/project3_finalevidence.zfs
```
Transfer the file using:
- scp
- Google Drive
- OneDrive

To import later:
```
zfs receive
```
### Step 7 - Verify Evidence Integrity

Compute the SHA256 hash:
```
sha256sum project3_finalevidence.zfs
```
Example output:
```
d39ccb4b6b3e253be46df10de73f9236fd86c2e628cccbd03af4b2cc7f3904b8  /home/me/project3_finalevidence.zfs
```
**Part 2 – Forensic Analysis Using Autopsy (Group 8)**

This phase performs forensic analysis on a provided disk image.

Digital forensics emphasizes reconstructing system activity from artifacts to determine what actions occurred on a system. 

**Step 1: Verify Integrity of the RAW Image**

Run a SHA256 hash check:
```
Get-FileHash .\evidence_disk.raw -Algorithm SHA256
```
Example output:
```
SHA256
62DB3E442F82805437D3DE60C575FAF04496DB4EE8E25BE90CD60879B837D1DF
```
Confirm the hash matches the provided value before proceeding.

**Step 2: Load Image into Autopsy**

Open Autopsy

Create a New Case

Select:
```
Add Data Source → Disk Image
```
Load the file:
```
evidence_disk.raw
```
Allow Autopsy to perform automated analysis.

Artifact 1 – Orphan File Discovery
File Metadata
Name:
img_evidence_disk.raw/$OrphanFiles/OrphanFile-13

Type:
File System

MIME Type:
application/octet-stream

Size:
0

File Name Allocation:
Unallocated

Metadata Allocation:
Unallocated

Modified:
2026-02-28 15:45:44 PST

Accessed:
2026-02-28 15:45:44 PST

Created:
2026-02-28 15:45:44 PST

Changed:
2026-02-28 15:45:51 PST
Sleuth Kit istat Output
inode: 13
Not Allocated

Group: 0
Generation ID: 1553334934

uid / gid: 0 / 0
mode: rrw-r--r--

Flags: Extents
size: 0
num of links: 0
Inode Timestamps
Accessed:      2026-02-28 15:45:44 PST
File Modified: 2026-02-28 15:45:44 PST
Inode Modified:2026-02-28 15:45:51 PST
File Created:  2026-02-28 15:45:44 PST
Deleted:       2026-02-28 15:45:51 PST
Direct Blocks