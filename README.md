<h1>Configuring a SIEM Lab</h1>

 Video up in the future...
 <br/>
 <img src="https://i.imgur.com/XDGyFk3.png" height="80%" width="80%" alt="TheOverview"/>

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

<h2>Lab walk-through:</h2>

<p align="center">
Launch Azure: <br/>
<img src="https://i.imgur.com/qxfzI5V.png" height="80%" width="80%" alt="TheFirstStep"/>
<br />
<b>As the first step, go ahead and set up an Azure account (it's free) you have $200 of resources given to you to start with. Next you're going to configure a Windows VM in Azure to have its all its ports open to make it an inviting honeypot for attackers to try getting into. Make sure to add the next resources we use into te same resource group. <b/>
<br/>
<br />
Using Log Analytics Workspaces:  <br/>
<img src="https://i.imgur.com/IsGnkHj.png" height="80%" width="80%" alt="TheLawStep"/>
<br />
<b>Next, you will create a log analytics workspace to supply our sentinel SIEM with the logs it will use. After it is created make sure to change its defender plan to only be on for servers and have the raw data collection set to collect all events.<b/>
<br />
 <br/>
An explation to the raw data the SIEM will use: <br/>
<img src="https://i.imgur.com/JsBwZwe.png" height="80%" width="80%" alt="TheExplanation"/>
<br />
<b>After connecting to your VM using RDP open event viewer on the VM. Open up the security logs and look for event ID 4625, this event # logs whenever a failed login through RDP has occured, as you can see it also reveals network information about the device which attempted this connection. The IP address in these logs will be used in a powershell script that will find the geolocation of the IP address through a websites API I will show later.<b/>
<br />
<br />
Make the honeypot appealing:  <br/>
<img src="https://i.imgur.com/Lh88KFN.png" height="80%" width="80%" alt="TheFWStep"/>
<br />
<b>To make the honeypot more visible to attackers it must accept ICMP traffic. Open up the windows firewall edit the firewall properties to have the domain, public and private firewalls turned off. Additionally I open up a new RDP window and purposely fail the connection to the VM a few times, this will later serve to verify the Powershell script and custom log are working right.<b/>
<br />
<br/>
Using Powershell to interact with an API:  <br/>
<img src="https://i.imgur.com/rCGW34q.png" height="80%" width="80%" alt="PowershellStep"/>
<br />
<b>Now to that powershell script, opening Powershell ISE I create a new file and insert the script I have provided, as mentioned earlier this will interact with the geolocation.io API to provide us with valuable information to put in our log analytics workspace. Save this script.<b/>
<br />
<br/>
Using ipgeolocation.io API:  <br/>
<img src="https://i.imgur.com/f2EAdoJ.png" height="80%" width="80%" alt="APIStep"/>
<br />
<b>Next you must create an accout for ipgeolocation.io (if youdon't already have one) and after that is done you are given an API key which you will insert in the powershell script where it says "YOUR_API_KEY"<b/>
 <br/>
 <br/>
 Using LAW to collect failed RDP connections from the VM:<br/>
<img src="https://i.imgur.com/cjGn3B9.png" height="80%" width="80%" alt="LAWqueryStep"/>
 <br/>
<b>For the next step I create a custom log back in the LAWs, creating an MMA based log I copy and paste the failed.rdp.log from the VM as the sample log, make sure the collection path is exactly as it is listed on the VM. Opening logs under new query I check to make sure the log I created is pulling the logs from the VM correctly, if you are following along and you run the query and nothing appears give it a 5-10 minutes for the logs to show, the LAW and VM need some time to properly connect.<b/>
<br/>
<br/>
 Log succesfully querying data:<br/>
 <img src="https://i.imgur.com/Tnz5O6b.png" height="80%" width="80%" alt="SIEMFirstStep"/>
 <br/>
<b>After the logs have loaded in I opened the sentinel SIEM I created earlier for this resource group and began making a new workbook. Here I add a query entering the other set of code I have provided and have all the extracted data from the LAW log displayed on a map.<b/>
 <br/>
<br/>
Creating a workbook in Sentinel:<br/>
<img src="https://i.imgur.com/cKtwq3R.png" height="80%" width="80%" alt="SIEMSecondStep"/>
<br/>
 As you can see the map display the country, coordinates, and amount of attempts of the RDP connections. This is why I purposely failed a few connections to the VM prior to make sure all of the tools are working as intended.<b/>
<br />
<br/>
Watching the SIEM function properly:  <br/>
<img src="https://i.imgur.com/CxUwati.png" height="80%" width="80%" alt="SAveWorkbookstep"/> <br/>
<img src="https://i.imgur.com/WTPgZjz.png" height="80%" width="80%" alt="Final Prod"/>
 <br/>
 The SIEM will go from looking like the first image to something like the next over the next few hours as people from aroud the world attempt to connect to the VM. The SIEM gives a count, country, and even color concentration of the areas where there are the most attempted connections. That concludes this lab for now perhpas in the future there will be additional improvements I will update this one with.<br/>
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
