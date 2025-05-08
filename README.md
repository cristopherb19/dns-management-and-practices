<p align="center">
<img src="https://i.imgur.com/fTIAX6A.png" alt="dns lab pic"/>
</p>

# Microsoft Azure DNS Lab Tutorial

## Introduction
This lab focuses on DNS configuration and testing using A-Records, Local DNS Cache, and CNAME Records. We will be utilizing the **DC-1** and **Client-1** virtual machines (VMs) that were set up in the previous Active Directory lab. Ensure that these VMs are properly configured and running before proceeding with the exercises.

---

## Prerequisites
- Access to **DC-1** and **Client-1** VMs.
- Administrator credentials for the domain: `mydomain.com\jane_admin` (from the AD lab).
- Working knowledge of DNS records.

---

## A-Record Exercise

1. **Connect to DC-1**:
    - Log into **DC-1** using the domain admin account: `mydomain.com\jane_admin`.

2. **Connect to Client-1**:
    - Log into **Client-1** as an admin: `mydomain\jane_admin`.

3. **Verify that "mainframe" is Unreachable**:
    - On **Client-1**, open the Command Prompt and run:
      ```bash
      ping mainframe
      ```
      Observe that the ping fails.

![image](https://github.com/user-attachments/assets/fbf28958-28b3-4b2d-86d7-ae5fd39b8bd3)

- Run the following command to check for the DNS record: 'nslookup mainframe'
Observe that it fails (no DNS record exists).

![image](https://github.com/user-attachments/assets/23eb5ae0-a5fd-407f-87e9-49ace68a5513)

4. **Create a DNS A-Record**:
    - Switch to **DC-1**.
    - Open the **DNS Manager**.
    - Navigate to the appropriate forward lookup zone.
    - Add an **A-Record** for `mainframe` and point it to **DC-1's Private IP address**.
  
![image](https://github.com/user-attachments/assets/2bcb0188-c6bc-4336-981b-7a7724c247fc)

![image](https://github.com/user-attachments/assets/2782f492-482b-434b-8ef3-583db4574618)

- **Right Click in open space**
![image](https://github.com/user-attachments/assets/69bb0dec-bc01-43b2-823f-4eaa405dcc4f)

![image](https://github.com/user-attachments/assets/47a7481d-2dd1-4dea-a378-bde3a75f8558)

5. **Verify Connectivity to "mainframe"**:
    - Go back to **Client-1**.
    - Run:
      ```bash
      ping mainframe
      ```
      Observe that the ping is now successful.

![image](https://github.com/user-attachments/assets/74a15c6b-b607-4043-8f53-2cd84e375649)

---

## Local DNS Cache Exercise

1. **Modify the A-Record**:
    - On **DC-1**, change the `mainframe` A-Record to point to `8.8.8.8`.
  
![image](https://github.com/user-attachments/assets/7ae441ce-72b2-4515-b52e-29d125778b3a)

2. **Ping "mainframe" from Client-1**:
    - On **Client-1**, run:
      ```bash
      ping mainframe
      ```
      Observe that the ping still resolves to the old address (cached locally).
![image](https://github.com/user-attachments/assets/363c8766-1755-40c5-905f-c67753ce0993)

3. **Check the Local DNS Cache**:
    - On **Client-1**, display the DNS cache using:
      ```bash
      ipconfig /displaydns
      ```
      Observe the cached record for `mainframe`.
![image](https://github.com/user-attachments/assets/05cb1dec-a102-4f93-94b3-33af299f117b)

4. **Flush the DNS Cache**:
    - Clear the local DNS cache using (you may need to run powershell as administrator):
      ```bash
      ipconfig /flushdns
      ```

5. **Verify Cache Clearing**:
    - Confirm the cache is empty by running:
      ```bash
      ipconfig /displaydns
      ```

6. **Verify the Updated A-Record**:
    - Attempt to ping `mainframe` again:
      ```bash
      ping mainframe
      ```
      Observe that it now resolves to the updated address (`8.8.8.8`).

![image](https://github.com/user-attachments/assets/c7192eb1-93a6-4d6d-b9af-2b0b57ee5260)

---

## CNAME Record Exercise

1. **Create a CNAME Record**:
    - On **DC-1**, open the **DNS Manager**.
    - Navigate to the appropriate forward lookup zone.
    - Add a **CNAME Record**:
      - Alias: `bubble`.
      - Points to: `www.google.com`.
     
![image](https://github.com/user-attachments/assets/8f815371-4a40-4733-88b3-2d5dad94370b)

![image](https://github.com/user-attachments/assets/59a16d98-f823-45cf-a53b-223796458b55)

2. **Test the CNAME Record**:
    - On **Client-1**, ping `search`:
      ```bash
      ping search
      ```
      Observe the results of the CNAME record resolution.

3. **Verify Using nslookup**:
    - On **Client-1**, run:
      ```bash
      nslookup search
      ```
      Observe the results, ensuring the alias resolves to `www.google.com`.

![image](https://github.com/user-attachments/assets/307e3e94-055d-4a5e-b9dc-78c0316e9dd5)

---

## Conclusion
Congratulations! You have successfully completed the DNS exercises for A-Records, Local DNS Cache, and CNAME Records.

---
