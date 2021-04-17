Archived from https://dougrathbone.com/blog/2013/11/27/deconstructing-the-azure-point-to-site-vpn-for-command-line-usage

This script Adds IP routes to Azure VPN through the P2S VPN

Automating the Routing Additions
Luckily the Windows Task Scheduler supports event listeners that allow us to watch for VPN connections and run commands off the back of them.

Take the below PowerShell Script below and save it for arguments sake in C:\Scripts\UpdateRouteTableForAzureVPN.ps1

Now execute the following from an elevated command prompt window. This tells Windows to add an event listener based task that looks for events to our "Azure VPN" connection and if it sees them, it runs our PowerShell script.

<code>schtasks /create /F /TN "VPN Connection Update" /TR "Powershell.exe -NonInteractive -command C:\Scripts\UpdateRouteTableForAzureVPN.ps1" /SC ONEVENT /EC Application /MO "*[System[(Level=4 or Level=0) and (EventID=20225)]] and *[EventData[Data='Azure VPN']] "</code>

If I then connect to my VPN the above script should execute.

After connecting if I check my routing table by entering route PRINT  into a console application we have our routes to Azure added correctly.