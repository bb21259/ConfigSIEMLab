<h1>Configuring a SIEM Lab</h1>

 ### [YouTube Demonstration](https://youtu.be/7eJexJVCqJo)

<h2>Description</h2>
In this lab, I setup a SIEM using Azure's sentinel and connect it to a live windows virtual machine acting as a honey pot. Using some tools on Azure I am able to view live RDP brute force attacks from around the world. I will be using a custom PowerShell script to look up the attackers Geolocation information and plot it on the Azure Sentinel Map.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Microsoft Azure</b>
- <b>IP Geolocation API</b>
- <b>VirtualBox</b>


<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

<h2>Program walk-through:</h2>

<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<b>As the first step, go ahead and set up an Azure account (it's free) you have $200 of resources given to you to start with. Next you're going to configurea Windows VM in Azure to have its all its ports open to make it an inviting honeypot for attackers to try getting into. Make sure to add the next resources we use into te same resource group. <b/>
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<b>Next, you will create a log analytics workspace to supply our sentinel SIEM with the logs it will use. After it is created make sure to change its defender plan to only be on for servers and have the raw data collection set to collect all events.<b/>
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<b>After connecting to your VM using RDP open event viewer on the VM. Open up the security logs and look for event ID 4625, this event # logs whenever a failed login through RDP has occured, as you can see it also reveals network information about the device which attempted this connection. The IP address in these logs will be used in a powershell script that will find the geolocation of the IP address through an websites API I will show later.<b/>
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<b>To make the honeypot more visible to attackers it must accept ICMP traffic. Open up the windows firewall edit the firewall properties to have the domain, public and private firewalls turned off.<b/>
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<b>Now to that powershell script, opening Powershell ISE I create a new file and insert the script I have provided, as mentioned earlier this will interact with the geolocation.io API to provide us with valuable information to put in our log analytics workspace. Save this script.<b/>
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<b>Next you must create an accout for ipgeolocation.io (if youdon't already have one) and after that is done you are given an API key which you will insert in the powershell script where it says "YOUR_API_KEY"<b/>
<b>For the next step I create a custom log back in the LAWs, creating an MMA based log I copy and paste the failed.rdp.log from the VM as the sample log, make sure the collection path is exactly as it is listed on the VM. Opening logs under new query I check to make sure the log I created is pulling the logs from the VM correctly, if you are following along and you run the query and nothing appears give it a 5-10 minutes for the logs to show, the LAW and VM need some time to properly connect<b/>
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
