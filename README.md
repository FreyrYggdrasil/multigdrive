# multigdrive
Instructions and scripts to enable running multiple Google Drive instances simultaneously on Windows 10

# steps
1) Create a new Windows 10 user, nomal account
2) Change the role for this account to Administrator
3) Use [SHIFT]-Run on the file GoogleDriveFs.exe
4) Choose "Run as different user
5) Use the in 1) created user account
6) Login to the second Google Drive with the Google Account connected to user 1)
7) Create changes to the registry to be able to run seperate Windows Explorer windows:
8) - Take ownership of reg key HK_CLASSES_ROOT\AppID{CDCBCFCA-3CDC-436f-A4E2-0E02075250C2}, and grant yourself Full Control. This key controls how explorer is allowed to launch
9) - Rename the subkey from "runas" to "_runas_". If you receive an error doing this, then you probably didn't complete step eight correctly.
10) - Do a runas on explorer.exe just as you did in step 3 for the Google Desktop app.
11) When all this is running, add some automation
12) Download Autohotkey from http://www.autohotkey.org/
13) Download PsExec from https://docs.microsoft.com/en-us/sysinternals/downloads/psexec
14) Install both
15) Create a command file with the following lines (16, 17), replace [user2] and [password] for the earlier created user:
16) @echo off
17) psexec -d -u [user2] -p [password] "C:\Program Files\Google\Drive File Stream\45.0.12.0\GoogleDriveFS.exe"
18) This will automatically launch GoogleDriveFS.exe as the second user
19) Create a Autohotkey script with the following lines (20-30):
20) #NoEnv  ; Recommended for performance and compatibility with future AutoHotkey releases.
21) SendMode Input  ; Recommended for new scripts due to its superior speed and reliability.
22) SetWorkingDir %A_ScriptDir%  ; Ensures a consistent starting directory.
23) #NoTrayIcon
24) LWin & o::
25) GetKeyState, state, o
26) If state = D
27)   Msgbox 'Pressed'
28) Else 
29)   run, psexec -d -u [user2] -p [password] "C:\windows\explorer.exe", , Min
30) Return
31) Save and load the Autohotkey script
32) Now the Left Windows Key + "O" will launch Windows Explorer as the other user [User2] , while Left Windows Key + "E" will still open Windows Explorer as [User1].

# caveats

* Using clipboard actions from Windows Explorer screen [User1] to Windows Explorer screen [User2] will not work, not vice versa.
* Drag and drop of files between the Windows Explorer windows will not work.
* Accessing the Google Drive from [User2] as [User1] will not work, nor vice versa. 
* Using (shared) folders on the desktop PC to access files will work.


