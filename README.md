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
