# multigdrive
Instructions and scripts to enable running multiple Google Drive instances simultaneously on Windows 10

# steps
1) Create a new Windows 10 user, normal account
2) Change the role for this account to Administrator
3) Use [SHIFT]-Run on the file GoogleDriveFs.exe
4) Choose "Run as different user
5) Use the in 1) created user account, Google Drive launches
6) Login to the second Google Drive with the Google Account connected to user 1)
7) Create changes to the registry to be able to run seperate Windows Explorer windows:
    1) Take ownership of reg key HK_CLASSES_ROOT\AppID{CDCBCFCA-3CDC-436f-A4E2-0E02075250C2}, and grant yourself Full Control. 
This key controls how explorer is allowed to launch
    2) Rename the subkey from "runas" to "_runas_". If you receive an error doing this, then you probably didn't complete step seven.1 correctly.
    3) Do a runas on explorer.exe just as you did in step 3 for the Google Desktop app.

8) When all this is running, add some automation
9) Download Autohotkey from http://www.autohotkey.org/
10) Download PsExec from https://docs.microsoft.com/en-us/sysinternals/downloads/psexec
11) Install both
12) Create a command file with the following lines (16, 17), replace [user2] and [password] for the earlier created user and put in the correct path o your Google Drive installation:
    1) @echo off
    2) psexec -d -u [user2] -p [password] "C:\Program Files\Google\Drive File Stream\45.0.12.0\GoogleDriveFS.exe"
13) This command file will after double clicking automatically launch GoogleDriveFS.exe as the second user
14) Create a Autohotkey script with the following lines (20-30):
    1) #NoEnv
    2) SendMode Input
    3) SetWorkingDir %A_ScriptDir%
    4) #NoTrayIcon
    5) LWin & o::
    6) GetKeyState, state, o
    7) If state = D
    8) Msgbox 'Pressed'
    9) Else 
    10) run, psexec -d -u [user2] -p [password] "C:\windows\explorer.exe", , Min
    11) Return
15) Save and load the Autohotkey script
16) Now the Left Windows Key + "O" will launch Windows Explorer as the other user [User2] , while Left Windows Key + "E" will still open Windows Explorer as [User1].

# caveats

* Using clipboard actions from Windows Explorer screen [User1] to Windows Explorer screen [User2] will not work, nor vice versa.
* Drag and drop of files between the Windows Explorer windows will not work.
* Accessing the Google Drive from [User2] as [User1] will not work, nor vice versa. 
* Using (shared) folders on the desktop PC to access files will work. But you may have to give [User2] and [User1] correct permissions to do so.


