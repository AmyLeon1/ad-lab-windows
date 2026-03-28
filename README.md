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
![Group Created](screenshots/ad-user-groups.png)  
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
