# Azure-Honeypot-Project-Experience

<h2> Skills learned</h2>

- Deploying Azure Virtual Machines
- Set up and configured a Windows VM with open RDP access
- Used Microsoft Sentinel for Log Analysis
- Connected Sentinel to a Log Analytics Workspace to monitor and review login attemps.
- Configured Log Analytics Workspace
- Learned how to link resources properly and enable diagnostic settings for real-time logging
- PowerShell Script
- Pulled the script and ran it to then receive the log of failed RDP attempts.
- Troubleshooting Azure logging issues
- Fixed broken log connections, misconfigured diagnostics, and data connectors from Microsoft Defender for cloud
- Understanding SIEM Tool navigation
- Cloud Monitoring and threat detections basics
- Got the opportunity to mess with the tools to see what a SOC analyst might see.

<h2> What I Learned Doing this Project</h2>

This project helped me understand how fast attackers will try to take advantage of an open port, especially RDP (3389). After turning off the firewall, I started seeing login attempts from different countries pretty quickly. It really showed how common these scans are.

I also learned a lot about how different Azure tools work together. Setting up the VM was simple, but connecting it to Log Analytics and Sentinel took some trial and error. I ran into issues where logs weren’t showing up at first because I forgot to connect the VM with it. Once I fixed that, I was able to use Sentinel to view sign-in attempts and analyze them.

Using PowerShell ISE, I ran a script to log the IP addresses of the login attempts and used the IPGeo API to get the location info. At first, the script wasn’t returning anything. I had to verify that the VM was running and receiving traffic. I was able to ping it from my host computer showing me that it was running and I ran a security event KSL scripts on the log query to see that login attempts were working. After adjusting that and setting the API key correctly was I able to get the data I wanted.

If I do this again, I’d try setting up alerts in Sentinel to notify me when login attempts happen. I’d also like to deploy some sort of way to defend against the login attempts or ensuring that no one can access the host from the VM.

Overall, this project gave me a good understanding of what goes into detecting suspicious activity in a cloud environment and the importance to have all the monitoring pieces connected and working right.

<h2> Update 06/02/2025:</h2>
  I was able to update Incidents on the Sentinel by adding a scheduled query rule. The query was KQL<br/>
SecurityEvent<br/>
| where EventID == 4625<br/>
| project TimeGenerated, EventID, WorkstationName, LogonTypeName, Account, Computer, IpAddress<br/>
| extend AccountEntity = Account<br/>
| extend IPEntity = IpAddress<br/>

This also gave me the opportunity to add Alert enhancements into the entity mapping so that Sentinel would better recognize and classify the data for further analysis. I added Account (Fullname, Account), IP (Address, IpAddress), Host (Hostname, Computer). Once I added this I was able to change the query schedule to run every 5 minutes and look back at the data from the last 30 minutes. It grouped related alerts together. I was able to see how quickly RDP attacks came once I started my VM after having it turned off for a few days.

Being able to see the incidents was important because when viewing in full detail you get the option to <ins>Investigate and use any Incident Actions</ins>. Incident Action options are currently "Run Playbook", "Create Automation Rule", and "Create Team". The cool thing is when you investigate you get to see the attack being mapped out!
![Incidents Sentinel](https://github.com/user-attachments/assets/497e4f4a-a401-4499-b79d-e24032154ac1)
![Investigate and incidents](https://github.com/user-attachments/assets/7321cfdf-99e9-4d44-aec8-fd1da62ad5d0)
![Mapped out attack](https://github.com/user-attachments/assets/322b4fdc-b785-4bc0-931d-c540a15c7dd2)
