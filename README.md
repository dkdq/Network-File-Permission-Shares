# Network-File-Permission-Shares

## Intro

This project outlines the implementation of how to set network file permissions and sharing it with other users works.

## Tech used

### Operating Systems

- Windows Server 2022
- Windows 10

### Platform and Software

- Remote Desktop Connection
- Windows File Explorer
- Server Manager (File and Storage Services)
- Windows Administritive Tools (optional)

## Configuration Steps
###### top
- [Section 1: Create some sample files with various permissions](#section-1-create-some-sample-files-with-various-permissions)
- [Section 2: Attempt to access file shares as a normal user](#section-2-attempt-to-access-file-shares-as-a-normal-user)
- [Section 3: Create an "ACCOUNTANTS" Security Group, assign permissions and user then testing](#section-3-create-an-accountants-security-group-assign-permissions-and-user-then-testing)
- [Section 4: Map a network drive in Client](#section-4-map-a-network-drive-in-client)
- [Section 5: Map a network personal drive in Active Directory](#section-5-map-a-network-personal-drive-in-active-directory)
- [Bonus Section: Install Windows Administritive Tools in Client](#bonus-section-install-windows-administritive-tools-in-client)

### Section 1: Create some sample files with various permissions

Create "read-access" file and assign permission

- Log into DC-1 -> start menu -> file explorer -> This PC -> Windows (C:) -> right click blank space -> New -> Folder -> type read-access -> right click read-access folder -> properties -> Sharing -> Share -> type domain users -> Add -> Share -> Done -> Close -> create a file inside
<img src="images/readaccess.png">

Create "read-access" file and assign permission

- Right click blank space -> New -> Folder -> type write-access -> right click read-access folder -> properties -> Sharing -> Share -> type domain users -> Add -> Permission Level -> Read/Write -> Share -> Done -> Close
<img src="images/writeaccess.png">

Create "no-access" file and assign permission

- Right click blank space -> New -> Folder -> type no-access -> right click read-access folder -> properties -> Sharing -> Share -> type domain admins -> Add -> Permission Level -> Read/Write -> Share -> Done -> Close
<img src="images/writeaccess.png">

### Section 2: Attempt to access file shares as a normal user
[back to top](#top)

- Log into Client-1 -> start menu -> file explorer -> Network (Error message shown, so we try (win+r -> type \\dc)) -> test the 3 files
<img src="images/network.png">
<img src="images/dc.png">
<img src="images/files.png">
<img src="images/readtest.png">
<img src="images/writetest.png">
<img src="images/noaccesstest.png">

### Section 3: Create an "ACCOUNTANTS" Security Group, assign permissions and user then testing
[back to top](#top)

Create "ACCOUNTANTS" file in DC and Client, then assign permission and user

- Go back DC-1 -> Server Manager -> ***Active Directory Users and Computers*** -> right click mydomain.com -> New -> ***Organisational Unit*** -> type _ACCOUNTING -> OK -> click _ACCOUNTING -> right click blank space -> New -> Group -> type ACCOUNTANTS -> go to Client-1 -> Windows (C:) -> right click accounting -> properties -> Sharing -> Share -> type accountants -> Add -> ***Permission Level*** -> Read/Write -> Share -> Close -> Back to DC-1 -> double click ACCOUNTANTS -> Members -> Add -> type baf.wes -> Check Names -> OK -> Apply -> OK
<img src="images/accfolder.png">
<img src="images/accgroup.png">
<img src="images/accpermission.png">
<img src="images/accuser.png">

Testing

- Go to Client-1 -> Log off and log back in -> win+r -> type cmd -> type net user baf.wes /domain   
win+r -> type \\dc -> accounting -> create file -> 
<img src="images/acctesting.png">
<img src="images/accedittesting.png">

### Section 4: Map a network drive in Client
[back to top](#top)

- Go to Client-1 -> start menu -> file explorer -> right click "This PC" -> Map network drive -> type "the Network Path of the folder" (right click the folder in DC-1 -> Properties -> Sharing -> Network Path) in Folder -> Finish
<img src="images/map1.png">
<img src="images/map2.png">
<img src="images/map3.png">

### Section 5: Map a network personal drive in Active Directory
[back to top](#top)

Create a Personal folder in SHARES

- Go to DC-1 -> Server Manager -> File and Storage Services -> Shares
<img src="images/admap1.png">
<img src="images/admap2.png">

Assign permission to users

- File explorer -> This PC -> Windows (C:) -> Shares -> right click Personal -> properties -> Security -> Advanced -> ***Disable inheritance*** -> Convert inherited permission into explicit permissions on this object -> Remove Users twice -> Add -> john_admin -> tick Modify and Write -> OK
<img src="images/admap3.png">
<img src="images/admap4.png">

Create Personal Security Group and add member into group

- Server Manager -> tools -> ***Active Directory Users and Computers*** -> mydomain.com -> Users -> right click blank space -> New -> Group -> Group name -> type Personal -> OK -> right click Personal -> properties -> Members -> Add -> type baf.wes -> Check Names -> OK
<img src="images/admap5.png">
<img src="images/admap6.png">

Add permission entry of Personal group in Personal folder

- File explorer -> This PC -> Windows (C:) -> Shares -> right click Personal -> properties -> Security -> Advanced -> ***Disable inheritance*** -> Convert inherited permission into explicit permissions on this object -> Add -> type personal -> Check Names -> tick Modify and Write -> OK -> right click Personal -> properties -> Sharing -> Share -> type personal -> Add -> ***Permission Level*** -> Read/Write -> Share
<img src="images/admap7.png">
<img src="images/admap8.png">

Map a network drive in Active Directory

- Server Manager -> tools -> ***Active Directory Users and Computers*** -> mydomain.com -> _EMPLOYEES -> right click baf.wes -> properties -> Profile -> Connect -> select a drive (Y:) -> To: -> type \\dc\\Personal\%username% -> OK
<img src="images/admap9.png">
<img src="images/admap10.png">

### Bonus Section: Install Windows Administritive Tools in Client
[back to top](#top)

Log in as john_admin in Client to perform activities (Active Directory Users and Computers etc.) instead of DC 
<img src="images/wat.png">

:warning:Last but not least, please don't forget to clean up your Azure resource groups as ***you can incur charges*** that use up your $200 free credits!

## Credits
[back to top](#top)

- Josh Madakor [link](https://github.com/joshmadakor1)