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

<img src="https://imgur.com/Eni7Nur.png" alt="dns lab pic"/>

- Run the following command to check for the DNS record: 'nslookup mainframe'
Observe that it fails (no DNS record exists).

<img src="https://imgur.com/Gu8CYOX.png" alt="dns lab pic"/>

4. **Create a DNS A-Record**:
    - Switch to **DC-1**.
    - Open the **DNS Manager**.
    - Navigate to the appropriate forward lookup zone.
    - Add an **A-Record** for `mainframe` and point it to **DC-1's Private IP address**.
  
<img src="https://imgur.com/qXPhvAD.png" alt="dns lab pic"/>

<img src="https://imgur.com/qwPPMoB.png" alt="dns lab pic"/>

- **Right Click in open space**
<img src="https://imgur.com/rcIg1dU.png" alt="dns lab pic"/>

<img src="https://imgur.com/rwxt6Gc.png" alt="dns lab pic"/>

5. **Verify Connectivity to "mainframe"**:
    - Go back to **Client-1**.
    - Run:
      ```bash
      ping mainframe
      ```
      Observe that the ping is now successful.

<img src="https://imgur.com/ytF3Gnt.png" alt="dns lab pic"/>

---

## Local DNS Cache Exercise

1. **Modify the A-Record**:
    - On **DC-1**, change the `mainframe` A-Record to point to `8.8.8.8`.
  
<img src="https://imgur.com/RFDW63r.png" alt="dns lab pic"/>

2. **Ping "mainframe" from Client-1**:
    - On **Client-1**, run:
      ```bash
      ping mainframe
      ```
      Observe that the ping still resolves to the old address (cached locally).
<img src="https://imgur.com/UPV2EqX.png" alt="dns lab pic"/>

3. **Check the Local DNS Cache**:
    - On **Client-1**, display the DNS cache using:
      ```bash
      ipconfig /displaydns
      ```
      Observe the cached record for `mainframe`.
<img src="https://imgur.com/9o7BdQt.png" alt="dns lab pic"/>

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

<img src="https://imgur.com/WBb9Fmv.png" alt="dns lab pic"/>

---

## CNAME Record Exercise

1. **Create a CNAME Record**:
    - On **DC-1**, open the **DNS Manager**.
    - Navigate to the appropriate forward lookup zone.
    - Add a **CNAME Record**:
      - Alias: `bubble`.
      - Points to: `www.google.com`.
     
<img src="https://imgur.com/TRVyrpq.png" alt="dns lab pic"/>

<img src="https://imgur.com/rTZ5NqR.png" alt="dns lab pic"/>

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

<img src="https://imgur.com/9D0cZON.png" alt="dns lab pic"/>

---

## Conclusion
Congratulations! You have successfully completed the DNS exercises for A-Records, Local DNS Cache, and CNAME Records.

---
