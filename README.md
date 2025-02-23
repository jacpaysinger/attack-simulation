# Attack Simulation: Failed Authentication

## Overview
This lab simulates an attack scenario within an Azure environment to observe authentication failures and security logging. It involves creating an attacker VM and attempting unauthorized access against target machines.

## Objectives  
- Simulate common attack vectors on RDP, SQL authentication, and SSH to observe real-world attack patterns.  
- Identify and review authentication failures in Windows Event Viewer and Linux log files to understand how Azure logs suspicious activity.  
- Observe how failed authentication attempts generate alerts and logs, aiding in proactive security monitoring and response.  
- Evaluate the effectiveness of security controls, such as monitoring, logging, and access restrictions, to enhance Azure AD and VM security.
  
## Prerequisites
- An Azure account with a deployed VMs (Windows and Linux) with open Network Security Groups
- Admin permissions to create resources
  
## Environment Setup  

### 1. Admin Mode (Simulating Normal Admin Activity)
I start by creating a new Windows VM within the Azure tenent. This VM is deployed outside of the US and will serve as the attacker VM.
<img width="1333" alt="Screenshot 2025-02-23 at 2 13 41 PM" src="https://github.com/user-attachments/assets/75a4dc44-cfd8-46f1-80d4-d917f0b1a49f" />
</br> I then log into `attack-vm` and then retrieve the public IP address to the previously created `windows-vm` in the Azure Portal for later use. 

### 2. Attacker Mode (Simulating Unauthorized Access Attempts)
Next I generate failed RDP logs:
  - From `attack-vm`, attempt to RDP into `windows-vm` using incorrect credentials.
  - Repeat this step two more times with different incorrect usernames and passwords.
    <img width="1066" alt="Screenshot 2025-02-23 at 1 14 03 AM" src="https://github.com/user-attachments/assets/452ff24b-466d-461a-a9df-3fb300440e34" />

    
I then generate failed MS SQL authentication logs:
  - Attempt to connect to SQL Server on `windows-vm` using an incorrect password<img width="786" alt="Screenshot 2025-02-23 at 2 06 08 AM" src="https://github.com/user-attachments/assets/1c7cd4f1-ce78-4efe-9683-83cc7cc7e290" />

Lastly, I generate failed SSH logs for into the Linux VM:
  - Attempt to SSH into `linux-vm` from `attack-vm` using incorrect credentials.<img width="677" alt="Screenshot 2025-02-23 at 4 08 32 PM" src="https://github.com/user-attachments/assets/942297d2-67ef-4d2e-a7a7-40fed1138592" />


### 3. Admin Mode (Investigating Logs)
I inspect the security logs on the Windows VM:
  - RDP back into `windows-vm`.
  - Examine the Security Log for RDP failures.
  - Check the Application Log for SQL authentication failures.
  - Note relevant EventIDs, messages, and source IP addresses.
    <img width="1210" alt="Screenshot 2025-02-23 at 3 59 09 AM" src="https://github.com/user-attachments/assets/2147077c-9b10-45cd-b4e6-ac76d1005c97" /><img width="1210" alt="Screenshot 2025-02-23 at 3 36 07 AM" src="https://github.com/user-attachments/assets/9bd2dd34-8d16-42ad-b6fc-44503b498a71" />

Next I inspect authentication logs on the Linux VM:
  - SSH into `linux-vm`.
  - Use the following commands to analyze authentication logs:
```powershell
cat auth.log | grep password
```
<img width="831" alt="Screenshot 2025-02-23 at 4 11 37 AM" src="https://github.com/user-attachments/assets/b7c5e4bb-d8b5-44c9-86f6-f5a371483f34" />


### Expected Outcomes
  - `windows-vm` logs multiple failed RDP authentication attempts.

  - `windows-vm` logs failed MS SQL login attempts in the Application Log.

  - `linux-vm` logs failed SSH attempts in /var/log/auth.log.

### Takeaways 
This simulation highlights the importance of monitoring authentication logs and securing remote access within an Azure environment.
