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
    (must be in Dc-1) -> Rt click start -> run -> Type  "gpmc.msc" -> enter -> Rt click  "Default Domain Policy" -> Edit -> computer config ->  Policies -> window Settings -> security settings -> Account policies -> account Lockout policy -> Double click "Account lockout duration" -> check box, set 
    lockout time -> Ok x2 -> Apply

     Double click "Account counter" -> check box, set counter -> Ok x2 -> Apply

11. Login to Client-1 as domain admin
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter Domain Username: "Specify domain admin name (back slash) Domain admin username" -> Enter Domain admin password

12. Force policy to reset
    In Client-1 -> Rt Click start -> command Prompt -> type "gpupdate /force" -> enter

13. Log out of Client 1 as the Domain Admin

Re-attempt to login to Client-1 as an employee with a bad password until account lockout

14. Login to client 1 random employee
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter employee Username: "Specify domain name (back slash) employee username" -> Enter wrong employee password 5x

The account should be locked out, now we will have to unlock the user account

15. Unlock User (employee) Account (In Dc-1)
    Start -> windows Administrative tools -> Active directory Users and computers -> Rt click domain name -> Find -> Type user name -> Find now -> Double click user -> Account tab -> check "Unlock account " -> Apply

16. Reattemp Client-1 login with correct password as random emmployee
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter employee Username: "Specify domain name (back slash) employee username" -> Enter employee password
    
The login should be successful indicating that the account has been unlocked. Now we are going to disable and then reenale the user account to observe what happens.

17. Disable the user account (In Dc-1)
    windows Administrative tools -> Active directory Users and computers -> Rt click domain name -> Find -> Type user name -> Find now -> rt Click user account -> Disable -> Refind

    Observe down arrow nesxt to the account, this signifies that the account has been disabled.

18. Log out of Client 1, re-Login to client 1 as random employee
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter employee Username: "Specify domain name (back slash) employee username" -> Enter employee password

    Observe the Popup Warning stating about the status of the account and who to contact for help to rectify.

19. Re-enable the user account (In Dc-1)
    windows Administrative tools -> Active directory Users and computers -> Rt click domain name -> Find -> Type user name -> Find now -> rt Click user account -> Re-enable -> Refind

    Observe down arrow next to the account has diappeared, this signifies that the account has been unlocked.

20. Login to client 1 as random employee
    Go to Vm in Azure -> click Client-1 -> Public ip address -> Remote desktop -> Enter employee Username: "Specify domain name (back slash) employee username" -> Enter employee password

   Observe that You can now log back into client -1 confirming that this account is unlocked and usable agian.

LKastly we are going to observe the login logs in both D-c-1 and Client-1

21. Check logs in Domain controller (Dc-1)
    Search bar -> type "eventvwr.msc" -> Enter -> Expand windows Logs -> Security -> Rt click Security -> Find -> Type the "User account" -> Find Now

    Observe  the data logs, there is no indication of the failed login attempts only login and log off

22. Check logs in Second Virtual Machine (Client-1)
    Search bar -> type "eventvwr.msc" -> Enter -> Expand windows Logs -> Security 

    Observe we are unable to see anything in the security Folder because we  are not the system administrator

23. Vview logs in client-1 as Admin
     Search bar -> type "eventvwr.msc" -> Run as Administrator -> Enter domain admin username/password -> Enter -> Expand windows Logs -> Security -> Rt click Security -> Find -> Type the "User account" -> Find Now

    Observe  the data logs, there is indication of the failed login attempts with login and log off attempts. The "Audit Failure" and Event id "4625" coincides with the faild attempts.


congratulations on the completion of this lab. You have successfully used active directory to manage simulated employee accounts using group policy is real world sinareos. 
