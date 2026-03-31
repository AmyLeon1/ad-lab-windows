# Active Directory Lab - Windows Server & Windows 11

This is a simple lab I set up to practice Active Directory, domain joins, and basic network connectivity between a Windows Server VM and a Windows 11 client VM.

## Steps I followed

1. **Set up Windows Server**
   - Installed Windows Server
   - Promoted to Domain Controller
   - Created a domain: `corp.local`

2. **Created a domain user**
   - User: `amy.admin`
   - Located in Active Directory → Users

3. **Configured Windows 11 VM**
   - Set static IP
   - Preferred DNS = server IP

4. **Tested connectivity**
   - Ran `ping corp.local` to check network/DNS

5. **Joined Windows 11 to the domain**
   - Domain: `corp.local`
   - Logged in as `corp\amy.admin`
     
## Screenshots

![AD Domain Overview](screenshots/ad-domain-overview.png)
![AD Users](screenshots/ad-user-created.png)
![Client DNS](screenshots/client-dns-configuration.png)
![Ping Test](screenshots/network-connectivity-test.png)
![Domain Join Success](screenshots/domain-join-success.png)
![Domain Login Success](screenshots/domain-user-login-success.png)



# Active Directory Lab – OU, Groups, and Shared Folder

This lab demonstrates a basic Active Directory (AD) setup including organizational units (OUs), users, security groups, and shared folder permissions. The purpose is to simulate a simple IT support environment.

---

## 1. Organizational Units (OUs)

Organizational Units were created to structure the domain:

- **Domain:** `corp.local`  
- **OUs created:**
  - `IT-Team`
  - `HR-Team`

Users were added to their respective OUs:

- `IT-Team` OU → user `amy.admin`
- `HR-Team` OU → other users as needed

**Screenshot:**  
![Organizational Units](screenshots/ad-organisational-units.png)  

---

## 2. Users in OUs

Users were created inside their OUs to represent employees of each department. Example:

- User: `amy.admin`  
- OU: `IT-Team`

**Screenshot:**  
![Users in OU](screenshots/ad-users-groups.png)  

---

## 3. Security Groups

Security groups were created to manage access permissions:

- OU: `IT-Team`
- Group created: `IT-Support`  
- Type: **Security**, Scope: **Global**  
- Member added: `amy.admin`  

**Screenshot:**    
![Group Members](screenshots/ad-group-members.png)  

> **Note:** OUs organize users, but permissions are controlled via groups.  

---

## 4. Shared Folder and Permissions

A shared folder was created on the server to demonstrate group-based access:

- Folder path: `C:\IT-Shared`  
- Shared name: `IT-Shared`  
- Permissions: `IT-Support` group → **Full Control / Modify**  

Windows 11 client VM was used to access the shared folder as a domain user (`corp\amy.admin`) and successfully create a test file.

**Screenshots:**  
- Folder path and sharing setup: ![Folder Path](screenshots/shared-folder-created.png)  
- Permissions: ![Folder Permissions](screenshots/group-permissions.png)  
- Access from client: ![Client Access](screenshots/client-access-test.png)  
- Test file created: ![Test File](screenshots/test-file-created.png)  

---

## 5. Summary

- Created a domain `corp.local`  
- Set up OUs to organize users  
- Created security groups to control access  
- Assigned users to groups and verified access to a shared folder from a client VM  
- Demonstrated that **group membership governs access** independent of OU placement  

This lab represents a basic, realistic AD structure suitable for IT support training.

# Active Directory Lab – Group Policy (GPO)

This section of the lab focuses on creating and applying a Group Policy Object (GPO) to control user behaviour in the domain.

---

## 1. Creating and Linking the GPO

I created a new Group Policy Object called `IT-Restrictions` and linked it to the `IT-Team` Organizational Unit (OU). This ensures the policy applies only to users inside that OU.

![GPO Created and Linked](screenshots/gpo-created.png)

---

## 2. Configuring the Policy

Inside the GPO, I enabled a restriction to block access to the Control Panel.

Path used:
- User Configuration → Administrative Templates → Control Panel

Setting enabled:
- Prohibit access to Control Panel

![GPO Setting Enabled](screenshots/gpo-setting-enabled.png)

---

## 3. Applying the Policy on the Client

To apply the policy, I switched to the Windows 11 client VM and ran: 
gpupdate /force


At first, I mistakenly ran this on the server, but realised that Group Policy must be applied on the client machine.

![Group Policy Update](screenshots/gpo-policy-updated.png)

---

## 4. Testing the Policy

I tested the policy by attempting to open the Control Panel on the Windows 11 VM while logged in as the domain user `corp\amy.admin`.

Access was blocked, confirming that the GPO was successfully applied.

![Control Panel Blocked](screenshots/gpo-test-control-panel-blocked.png)

---

### What I learned

- How to create and link a GPO to a specific OU  
- The difference between server-side configuration and client-side application  
- That GPOs apply to users based on their OU location  
- How to use `gpupdate /force` to refresh policies  
- The importance of logging out/in for user policies  
- Basic troubleshooting when a policy does not apply  

I initially ran `gpupdate /force` on the server instead of the Windows 11 client, which helped me understand that policies are created on the server but must be applied on client machines to take effect.  

This lab helped me understand how Group Policy is used in real IT environments to enforce restrictions and manage users centrally.


## Password and Account Lockout Policy Lab

In this part of the lab, I configured password and account lockout policies using the Default Domain Policy in Active Directory.

I accessed the policy through Group Policy Management and edited the Default Domain Policy.

### Password Policy

I configured the following settings:

- Minimum password length: 8 characters  
- Password complexity: Enabled  

These settings ensure stronger passwords are used across the domain.

![Password Policy](screenshots/passsword-settings-updated.png)

---

### Account Lockout Policy

I configured account lockout settings to improve security and prevent repeated failed login attempts:

- Account lockout threshold: 3 invalid attempts  
- Account lockout duration: 5 minutes (set lower for testing)  
- Reset account lockout counter after: 5 minutes  

This means that if a user enters the wrong password 3 times within 5 minutes, their account will be locked for 5 minutes.

![Account Lockout Policy](screenshots/account-lockout-settings-updated.png)

---

### Testing the Policy

On the Windows 11 client machine, I tested the policy by logging out and attempting to sign in with incorrect credentials multiple times.

After 3 failed login attempts, the account was locked as expected.

I did not need to run `gpupdate /force` in this case, as logging out and logging back in was enough for the policy to apply. This helped demonstrate that some policies are applied automatically during the login process.

![Account Lockout Test](screenshots/account-lockout-test.png)

---

### What I learned

- How to configure password and account lockout policies using Group Policy  
- The difference between password policy and account lockout settings  
- How the lockout threshold, duration, and reset counter work together  
- That some policies apply automatically at login without needing `gpupdate /force`  
- How to test account lockout behaviour in a controlled environment  

This lab helped me understand how organizations enforce password security and protect against repeated login attempts in a real-world environment.

## Shared Folder and Drive Mapping Lab

In this lab, I created a shared folder on the server and mapped it as a network drive on the Windows 11 client.

### Server Setup

- Created a folder called `IT-Shared` on the server  
- Enabled sharing using Properties → Sharing → Advanced Sharing  
- Set permissions for the IT-Team group  

![Shared Folder on Server](screenshots/drive-mapping-gpo.png)

---


### Drive Mapping with GPO

- Created a GPO to map the shared folder to drive letter Z:  
- Linked the GPO to the IT-Team OU  
- Logged out and back in on the Windows 11 client  
- Verified that the Z: drive appears and points to the shared folder  

![Mapped Drive](screenshots/mapped-drive.png)

---

### What I learned

- How to share a folder on the server and assign permissions  
- How to access a shared folder from a client using a network path  
- How to map a network drive using Group Policy  
- That drive mappings are applied when a user logs in  
- That `gpupdate /force` can be used to apply policies immediately  
- If a shared folder is not visible on the client, check that it is still being shared on the server  

This lab helped me understand how shared resources are managed and accessed in a domain environment.

## Password Reset Lab

In this lab, I simulated a common IT support task by resetting a user’s password in Active Directory and forcing them to change it at next login.

### Steps

1. Opened **Active Directory Users and Computers** on the server  
2. Navigated to the `IT-Team` OU  
3. Right-clicked the user `amy.admin`  
4. Selected **Reset Password**  
5. Entered a new password  
6. Selected **User must change password at next logon**  
7. Clicked OK  

![Password Reset](screenshots/password-reset.png)

---

### Testing

On the Windows 11 client:

1. Logged in as `corp\amy.admin`  
2. Was prompted to change the password immediately  
3. Entered a new password and confirmed access  

![Password Change Required](screenshots/password-change-required.png)

---

### What I learned

- How to reset a user password in Active Directory  
- How to force a user to change their password at next login  
- How password policies are enforced during login  
- This is a common IT support task for handling user access issues  

This lab helped me understand how administrators manage user credentials securely in a domain environment.


## Disable User Account Lab

In this lab, I simulated disabling a user account, which is a common task when an employee leaves or no longer requires access.

### Steps

1. Opened **Active Directory Users and Computers** on the server  
2. Navigated to the `IT-Team` OU  
3. Right-clicked the user `amy.admin`  
4. Selected **Disable Account**  

The user icon changed to indicate the account was disabled.

![User Disabled](screenshots/user-disabled.png)

---

### Testing

On the Windows 11 client:

1. Attempted to log in as `corp\amy.admin`  
2. Login failed as expected because the account was disabled  

![Disabled Login Failed](screenshots/disabled-login-failed.png)

---

### Re-enabling the Account

To restore access:

1. Right-clicked the user in Active Directory  
2. Selected **Enable Account**  

---

### What I learned

- How to disable and enable user accounts in Active Directory  
- That disabling an account immediately prevents login access  
- How account status is enforced across the domain  
- This is a key task in user offboarding and access control  

This lab helped me understand how user access is managed securely in real-world IT environments.
