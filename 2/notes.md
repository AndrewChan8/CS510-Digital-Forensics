# Project 2 Notes
## Finding 1 — Bash History (User: user)

Path: /home/user/.bash_history  
Status: Live file  
Timestamps: Last modified 2026-02-03 16:08:47 PST  

Description:
Bash command history indicates interactive shell usage, directory creation and renaming, and use of sudo/su.

Interpretation:
Commands suggest intentional user activity and system modification rather than automated processes.

Relevance:
Establishes user behavior and supports correlation with filesystem artifacts (e.g., created directories).

## Finding 2 — Cleartext Credentials

Path: /home/user/Videos/GrizzlyVideos/CrazyGrizzlyVideos/grizzly
User Context: user (references squirrel)
Status: Live file
Size: 32 bytes

Description:
ASCII text file containing a username and password in cleartext.

Interpretation:
Indicates insecure credential storage and possible access to another user account.

Relevance:
Supports risk assessment and explains potential cross-user activity observed elsewhere on the system.

## Finding 3 — User-Created Images

Path: /home/user/Pictures/
Files: salmon.jpeg, salmon2.jpeg, salmon3.jpeg, salmon4.jpg, squirrel.jpg
Timestamps: ~2026-02-02 13:40–13:43 PST

Description:
Multiple image files with thematically consistent naming and close creation times.

Interpretation:
Suggests intentional image creation or collection tied to the same activity window as related documents.

Relevance:
Corroborates findings from vim metadata and document files, strengthening behavioral timeline.

## Finding 4 — Themed Multi-Modal Content Creation

Paths:
- /home/user/Documents/salmon*/salmon*
- /home/user/Documents/squirrel/squirrel
- /home/user/Pictures/salmon*.jpg
- /home/user/Pictures/squirrel.jpg

Timestamps:
~2026-02-02 13:40–13:50 PST (clustered)

Description:
Text documents and images organized into themed directories (salmon, squirrel) with consistent naming and close creation times.

Interpretation:
Indicates deliberate creation and organization of related text and image content by the user.

Relevance:
Corroborates shell history, vim metadata, and directory structure; supports a clear behavioral timeline.

## Finding 5 — User Access to Image Files

Path: /home/user/.local/share/recently-used.xbel

Description:
XML usage log records that salmon*.jpg and squirrel.jpg images were opened using Firefox ESR.

Interpretation:
Confirms intentional user access and review of image files, not merely their presence on disk.

Relevance:
Corroborates filesystem artifacts, vim metadata, and shell history, strengthening the activity timeline.

## Finding 6 — Target File as Activity Completion Marker

Path:
/home/squirrel/target

Description:
An ASCII text file containing the message “Good job, this is the end.” Bash history for the squirrel account shows the file was created and edited using vim after a temporary directory was created and removed.

Interpretation:
Indicates deliberate user interaction rather than an automated artifact. The file functions as an explicit endpoint or confirmation marker created by the user.

Relevance:
Corroborates shell history (.bash_history) and editor metadata (.viminfo), reinforcing the reconstructed sequence of user actions and signaling the conclusion of activity for the squirrel