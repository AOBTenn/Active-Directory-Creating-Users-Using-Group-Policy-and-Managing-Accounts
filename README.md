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

2. Create Additional User

Login to Dc-1 as Admin User 

Remote desktop -> Public Ip Address -> Enter Username: "Specify Domain name (back slash) Domain Admin Username" -> Enter Domain Admin Password

![image](https://github.com/user-attachments/assets/28ca1201-9e52-4b3a-8b9b-2e75c58b26dc)

Rt Click Start -> Run -> Type Powershell -> Rt Click, Run as Administrator -> Copy script text into Powershell -> Save to desktop -> Run script

![image](https://github.com/user-attachments/assets/7d301e22-606d-43d9-b973-5fdb5c212300)
![image](https://github.com/user-attachments/assets/bab4505c-25d6-4d70-a151-6d0ffde84b5f)
![image](https://github.com/user-attachments/assets/e8d72974-40d1-4388-9dad-573210e45875)
![image](https://github.com/user-attachments/assets/1323a908-37b8-4907-a4e4-9b4bdf8dbf18)
![image](https://github.com/user-attachments/assets/8a61ffe7-d702-4d45-a0d8-c7a35f081482)
![image](https://github.com/user-attachments/assets/22ae019c-7eca-4cb7-86d2-6fe9f561afe0)

3. Check for Users in Active Directory "_Employees" folder

Start -> Windows Administrative Tools -> Open Active Directory Users and Computers -> Expand the Domain -> "_Employees," Rt Click, Refresh

![image](https://github.com/user-attachments/assets/0f3efa4d-dc18-42fd-a5b4-da0e7ff66e0e)

There should be a list of numerous employees being generated that can now log into Client 1

![image](https://github.com/user-attachments/assets/1d1ed3f7-4b13-4093-bde3-22bc7fed15ce)

4. Log out of Client 1 as the Domain Admin

5. Login to client 1 using a Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

![image](https://github.com/user-attachments/assets/be3b8dc0-eb0c-48bb-97de-4d64db22f818)
![image](https://github.com/user-attachments/assets/414a21c3-fd16-4c6c-b3aa-bac671591e44)

6. Log out of Client-1

Now  we will move on to dealing with simulated user account problems and Group Policy settings
   
Attempt to login to Client-1 as an employee with a bad password until account lockout

2. Login to Client 1 Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter wrong Employee Password 10x

![image](https://github.com/user-attachments/assets/03bb7020-9222-47fb-acec-8318ca865e80)

After the 1oth attempt, try to login using the correct password

3. Login to Client 1 using a Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

![image](https://github.com/user-attachments/assets/ff1af80f-b656-434b-8369-c0172c1e5495)

If you are able to login that is because account lockout is not set, so we are go to set it using Group Policy. Log out of Client 1.

4. Configure Account Lockout (turn on)

(must be in Dc-1) -> Rt Click Start -> Run -> Type "gpmc.msc" -> Enter -> Rt Click  "Default Domain Policy" -> Edit -> Computer Config ->  Policies -> Window Settings -> Security Settings -> Account Policies -> Account Lockout Policy -> Double Click "Account Lockout Duration" -> Check box, Set Lockout Time -> Ok x2 -> Apply

![image](https://github.com/user-attachments/assets/143fb8ac-fd58-409b-84ac-f42aa81f280c)
![image](https://github.com/user-attachments/assets/34853f52-3cf7-4e81-a1a7-982943332972)
![image](https://github.com/user-attachments/assets/724a735a-f352-4005-84c4-7152be49b620)
![image](https://github.com/user-attachments/assets/53ae625f-7418-4598-97cd-9ad99648cbba)
![image](https://github.com/user-attachments/assets/919fe15b-aa00-4098-97f1-dff84f0355d1)
![image](https://github.com/user-attachments/assets/370a48fa-2842-49ba-ac51-e417f5035733)
![image](https://github.com/user-attachments/assets/05492c2d-ca08-4af5-8758-af19b5677b73)
![image](https://github.com/user-attachments/assets/e334c20c-d8ec-4ba0-ab4c-5b1c94a50d2c)

Force Policy to Reset

5. Login to Client-1 as Domain Admin

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Domain Username: "Specify Domain Admin Name (back slash) Domain Admin Username" -> Enter Domain Admin Password

![image](https://github.com/user-attachments/assets/296593e7-4cb5-44ed-b18b-981f8fd85161)

Rt Click Start -> Command Prompt -> Type "gpupdate /force" -> Enter

![image](https://github.com/user-attachments/assets/9fdbc689-a15e-43a5-ad2d-7bc5126aff3d)
![image](https://github.com/user-attachments/assets/55c5feef-9626-42e1-ab36-c7e1092c1ac6)

7. Log out of Client-1 as the Domain Admin

Re-attempt to login to Client-1 as Random Employee with a bad password until account lockout

8. Login to Client-1 Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter wrong EWmployee Password 5x

![image](https://github.com/user-attachments/assets/f4d0a94b-4f50-419c-8fd0-3f94ccc072c6)

The account should be locked out, now we will have to unlock the user account

![image](https://github.com/user-attachments/assets/6430ec38-0993-4de2-8dd3-6ef8408673c1)

9. Unlock User (employee) Account (In Dc-1)

Start -> Windows Administrative Tools -> Active Directory Users and Computers -> Rt Click Domain Name -> Find -> Type User Name -> Find now -> Double click User -> Account tab -> Check "Unlock Account " -> Apply

![image](https://github.com/user-attachments/assets/273a2fe9-db8f-4903-b288-2b49019ffa58)
![image](https://github.com/user-attachments/assets/5c44defc-aa71-447e-a24b-bbf275cfc531)
![image](https://github.com/user-attachments/assets/e8ebb917-0a60-4270-bebe-e8df91ac4a99)
![image](https://github.com/user-attachments/assets/d8c882ba-78b3-43f6-9e77-eff2ea4584be)

10. Re-attempt Client-1 login with correct password as Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Random Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

![image](https://github.com/user-attachments/assets/90dc65ab-5c9d-45e9-8e94-3807149e13d3)

The login should be successful indicating that the account has been unlocked. 

Now we are going to disable and then reenale the user account to observe what happens.

1. Disable the User account (In Dc-1)

Windows Administrative Tools -> Active Directory Users and Computers -> Rt Click Domain Name -> Find -> Type User Name -> Find now -> Rt Click User account -> Disable -> Refind

![image](https://github.com/user-attachments/assets/273a2fe9-db8f-4903-b288-2b49019ffa58)
![image](https://github.com/user-attachments/assets/f2917449-699d-4bd8-b4f6-9ef7b65e6b07)
![image](https://github.com/user-attachments/assets/861b605d-9e6b-4d13-b732-3a6f31ba4320)

Observe down arrow nesxt to the account, this signifies that the account has been disabled.

![image](https://github.com/user-attachments/assets/5dc7385b-e48d-477e-8a48-d9a997d6fd33)

3. Log out of Client-1, re-Login to Client-1 as Random Employee

Go to Vm in Azure -> Click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Random Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

![image](https://github.com/user-attachments/assets/93143a3b-96c2-4624-8e53-31e48d001582)

Observe the Popup Warning stating about the status of the account and who to contact for help to rectify.

![image](https://github.com/user-attachments/assets/9412ce47-0fad-40b0-8e4f-a77ad140c55f)

5. Re-enable the User account (In Dc-1)

W#indows Administrative Tools -> Active Directory Users and Computers -> Rt Click Domain Name -> Find -> Type User Name -> Find now -> Rt Click User Account -> Re-enable -> Refind

![image](https://github.com/user-attachments/assets/273a2fe9-db8f-4903-b288-2b49019ffa58)
![image](https://github.com/user-attachments/assets/18437b0e-5f45-40d3-8b2c-b354ec8bce43)
![image](https://github.com/user-attachments/assets/7c4c4b91-d40e-4bce-a09b-045d7f7c1307)

Observe down arrow next to the account has diappeared, this signifies that the account has been unlocked.

![image](https://github.com/user-attachments/assets/42f4b300-a298-4546-ac3c-b43d16a588bf)

7. Login to client-1 as Random Employee

Go to Vm in Azure -> click Client-1 -> Public Ip Address -> Remote Desktop -> Enter Random Employee Username: "Specify Domain Name (back slash) Employee Username" -> Enter Employee Password

![image](https://github.com/user-attachments/assets/fdf95b82-5447-4254-94cd-5e9ffefd6364)

Observe that You can now log back into Client-1 confirming that this account is unlocked and usable agian.

Lastly we are going to observe the login logs in both D-c-1 and Client-1

1. Check logs in Domain Controller (Dc-1)

Search bar -> Type "eventvwr.msc" -> Enter -> Expand Windows Logs -> Security -> Rt Click Security -> Find -> Type the "User account" -> Find Now

![image](https://github.com/user-attachments/assets/34e6483f-afd3-4d66-99f5-6a9ef765420e)
![image](https://github.com/user-attachments/assets/4d2e8084-54c0-4d14-b27c-bee3147a3905)
![image](https://github.com/user-attachments/assets/19e19923-4f78-47e6-8191-6f49eaca9a2f)
![image](https://github.com/user-attachments/assets/125a2838-2a2e-438d-8044-07e8c6632971)

Observe the data logs, there is no indication of the failed login attempts only login and log off

![image](https://github.com/user-attachments/assets/91eaa556-46ac-449e-be44-d475c9780a90)

2. Check logs in Second Virtual Machine (Client-1)

Search bar -> Type "eventvwr.msc" -> Enter -> Expand Windows Logs -> Security 

![image](https://github.com/user-attachments/assets/1cc4b723-d954-46b3-ba05-b20ef090cec4)

Observe we are unable to see anything in the security Folder because we are not the system administrator

![image](https://github.com/user-attachments/assets/41a91a5e-bf1b-49c8-9c4c-1ca3afb37075)

3. View logs in Client-1 as Admin

Search bar -> Type "eventvwr.msc" -> Run as Administrator -> Enter Domain Admin Username/Password -> Enter -> Expand Windows Logs -> Security -> Rt Click Security -> Find -> Type the "User account" -> Find Now

![image](https://github.com/user-attachments/assets/e481eadc-4728-41c7-9857-8ed4afad2641)
![image](https://github.com/user-attachments/assets/f566feb3-7813-434b-9409-6d99eafc890c)

Observe the data logs, there is indication of the failed login attempts with login and log off attempts. The "Audit Failure" and Event Id "4625" coincides with the faild attempts.

![image](https://github.com/user-attachments/assets/ec534d88-0b8b-436e-b609-fc783fd14795)

Congratulations on the completion of this lab. You have successfully used Active Directory to manage simulated employee accounts using Group Policy is real world scenarios.
