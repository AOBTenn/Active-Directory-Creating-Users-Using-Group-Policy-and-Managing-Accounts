# Active-Directory-Creating-Users-Using-Group-Policy-and-Managing-Accounts

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

Link to Script https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

<h2>Project Steps</h2

1. Modify Client 1 (second Vm) so Non-admin users can login via Remote Desktop
   Login to Dc-1 as Admin User 
   Remote desktop -> Public ip address -> Enter Username: "Specify Domain name (back slash) Domain admin username" -> Enter Domain admin password

   Rt Click Start -> system -> Remote desktop -> "Select Users That can remotely access this Pc" -> Add -> Under "enter the object names to select" type "domain users" -> check names -> OK x2

2. Create additional user
   Login to Dc-1 as Admin User 
   Remote desktop -> Public ip address -> Enter Username: "Specify Domain name (back slash) Domain admin username" -> Enter Domain admin password

    Rt Click Start -> Run -> Type Powershell -> Rt click, run as Admin -> copy script text into Powershell -> save to desktop -> Run script

3. Open active Directory Users and Computers -> Expand the domain -> "_Employees," Rt click, Refresh

   There should be a list of numerous employees being generated that can now log into Client 1

4. Log out of Client 1 as the Domain Admin

5. Login to client 1 using a random employee
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter employee Username: "Specify domain name (back slash) employee username" -> Enter employee password

6. Log out of client-1

Now  we will move on to dealing with simulated user account problems and Group policy settings

7. Login back into to Dc-1
   Go to Vm in Azure -> click Dc-1 -> Public ip address -> Remote desktop -> Enter Username: "Specify Domain name (back slash) original username" -> Enter password

Attempt to  login to Client-1 as an employee with a bad password until account lockout

8. Login to client 1 random employee
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter employee Username: "Specify domain name (back slash) employee username" -> Enter wrong  employee password 10x

After the 1oth attempt, try to login using the correct password

9. Login to client 1 using a random employee
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter employee Username: "Specify domain name (back slash) employee username" -> Enter employee password

If you are able to login that is because account lockout is not set, so go ahead and log out of Client 1. We are go to set it using group policy.

10. Configure account Lockout (turn on)
    (must be in Dc-1) -> Rt click start -> run -> Type  "gpmc.msc" -> enter -> Rt click  "Default Domain Policy" -> Edit -> computer config ->  Policies -> window Settings -> security settings -> Account policies -> account Lockout policy -> Double click "Account lockout duration" -> check box, set lock

12. Configure (turn on) Account lockout (
    

