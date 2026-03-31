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
