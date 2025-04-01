# Active-Directory-Creating-Users-Using-Group-Policy-and-Managing-Accounts

![image](https://github.com/user-attachments/assets/af3b9cd7-edfc-4505-85bf-2e31649020c5)

<h2>Description </h2>

In this final project we are going us Active Directory with Group Policy to manage employee accounts.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory
  
<h2>Operating Systems Used </h2>

Windows 10</b> (21H2)
Windows Server 2022 Azure Edition

<h2>List of Prerequisites</h2>

Link to Script https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

<h2>Project Steps</h2>              

1. Modify Client 1 (second Vm) so Non-Admin Users can login via Remote Desktop

Login to Dc-1 as Admin User

Remote Desktop -> Public Ip Address -> Enter Username: "Specify Domain name (back slash) Domain Admin Username" -> Enter Domain Admin Password

![image](https://github.com/user-attachments/assets/ae6a4e61-9f41-4923-8a48-b44fc655ff8d)

Rt Click Start -> System -> Remote Desktop -> "Select Users That can Remotely Access this Pc" -> Add -> Under "Enter the Object Names to Select" type "domain users" -> Check Names -> OK x2

![image](https://github.com/user-attachments/assets/a1667041-76d0-4bb4-b8c5-b0ab9d352849)
![image](https://github.com/user-attachments/assets/876fc695-93c7-41d7-9ffb-5d2c7c6ef5e5)
![image](https://github.com/user-attachments/assets/06826e10-7fab-4345-be37-319e4e567edb)
![image](https://github.com/user-attachments/assets/b626a4ab-1058-4a42-b19a-3bdc433bd1a1)

3. Create Additional User

Login to Dc-1 as Admin User 

Remote desktop -> Public Ip Address -> Enter Username: "Specify Domain name (back slash) Domain Admin Username" -> Enter Domain Admin Password

![image](https://github.com/user-attachments/assets/28ca1201-9e52-4b3a-8b9b-2e75c58b26dc)

Rt Click Start -> Run -> Type Powershell -> Rt Click, run as Admin -> Copy script text into Powershell -> Save to desktop -> Run script

![image](https://github.com/user-attachments/assets/7d301e22-606d-43d9-b973-5fdb5c212300)
![image](https://github.com/user-attachments/assets/bab4505c-25d6-4d70-a151-6d0ffde84b5f)
![image](https://github.com/user-attachments/assets/e8d72974-40d1-4388-9dad-573210e45875)
![image](https://github.com/user-attachments/assets/1323a908-37b8-4907-a4e4-9b4bdf8dbf18)
![image](https://github.com/user-attachments/assets/8a61ffe7-d702-4d45-a0d8-c7a35f081482)
![image](https://github.com/user-attachments/assets/22ae019c-7eca-4cb7-86d2-6fe9f561afe0)

5. Open Active Directory Users and Computers -> Expand the Domain -> "_Employees," Rt click, Refresh

There should be a list of numerous employees being generated that can now log into Client 1

7. Log out of Client 1 as the Domain Admin

8. Login to client 1 using a Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

10. Log out of Client-1

Now  we will move on to dealing with simulated user account problems and Group policy settings

1. Login back into to Dc-1

Go to Vm in Azure -> click Dc-1 -> Public Ip Address -> Remote Desktop -> Enter Username: "Specify Domain Name (back slash) original Username" -> Enter Password
   
Attempt to login to Client-1 as an employee with a bad password until account lockout

2. Login to Client 1 Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter wrong Employee Password 10x

After the 1oth attempt, try to login using the correct password

3. Login to Client 1 using a Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

If you are able to login that is because account lockout is not set, so go ahead and log out of Client 1. We are go to set it using group policy.

4. Configure Account Lockout (turn on)

(must be in Dc-1) -> Rt Click Start -> Run -> Type "gpmc.msc" -> Enter -> Rt Click  "Default Domain Policy" -> Edit -> Computer Config ->  Policies -> Window Settings -> Security Settings -> Account Policies -> Account Lockout Policy -> Double Click "Account Lockout Duration" -> Check box, Set 
Lockout Time -> Ok x2 -> Apply

Double click "Account Counter" -> Check box, Set Counter -> Ok x2 -> Apply

5. Login to Client-1 as Domain Admin

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Domain Username: "Specify Domain Admin Name (back slash) Domain Admin Username" -> Enter Domain Admin Password

6. Force Policy to Reset

In Client-1 -> Rt Click Start -> Command Prompt -> Type "gpupdate /force" -> Enter

7. Log out of Client-1 as the Domain Admin

Re-attempt to login to Client-1 as Random Employee with a bad password until account lockout

8. Login to Client-1 Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter wrong EWmployee Password 5x

The account should be locked out, now we will have to unlock the user account

9. Unlock User (employee) Account (In Dc-1)

Start -> Windows Administrative Tools -> Active Directory Users and Computers -> Rt Click Domain Name -> Find -> Type User Name -> Find now -> Double click User -> Account tab -> Check "Unlock Account " -> Apply

10. Re-attempt Client-1 login with correct password as Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Random Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password
    
The login should be successful indicating that the account has been unlocked. 

Now we are going to disable and then reenale the user account to observe what happens.

1. Disable the User account (In Dc-1)

Windows Administrative Tools -> Active Directory Users and Computers -> Rt Click Domain Name -> Find -> Type User Name -> Find now -> Rt Click User account -> Disable -> Refind

Observe down arrow nesxt to the account, this signifies that the account has been disabled.

3. Log out of Client-1, re-Login to Client-1 as Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Random Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

Observe the Popup Warning stating about the status of the account and who to contact for help to rectify.

5. Re-enable the User account (In Dc-1)

W#indows Administrative Tools -> Active Directory Users and Computers -> Rt Click Domain Name -> Find -> Type User Name -> Find now -> Rt Click User account -> Re-enable -> Refind

Observe down arrow next to the account has diappeared, this signifies that the account has been unlocked.

7. Login to client-1 as Random Employee

Go to Vm in Azure -> click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Random Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

Observe that You can now log back into Client-1 confirming that this account is unlocked and usable agian.

Lastly we are going to observe the login logs in both D-c-1 and Client-1

1. Check logs in Domain Controller (Dc-1)

Search bar -> Type "eventvwr.msc" -> Enter -> Expand Windows Logs -> Security -> Rt Click Security -> Find -> Type the "User account" -> Find Now

Observe the data logs, there is no indication of the failed login attempts only login and log off

2. Check logs in Second Virtual Machine (Client-1)

Search bar -> Type "eventvwr.msc" -> Enter -> Expand Windows Logs -> Security 

Observe we are unable to see anything in the security Folder because we are not the system administrator

3. View logs in Client-1 as Admin

Search bar -> Type "eventvwr.msc" -> Run as Administrator -> Enter Domain Admin Username/Password -> Enter -> Expand Windows Logs -> Security -> Rt Click Security -> Find -> Type the "User account" -> Find Now

Observe the data logs, there is indication of the failed login attempts with login and log off attempts. The "Audit Failure" and Event Id "4625" coincides with the faild attempts.


Congratulations on the completion of this lab. You have successfully used Active Directory to manage simulated employee accounts using Group Policy is real world scenarios.
