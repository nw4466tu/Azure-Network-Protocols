<p align="center">
<img src="https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Traffic%20Examination%20(VM)%20Picture.png?raw=true" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines (Cont.)</h1>


<h2>Description</h2>
In this repository, we're going to look at the data traveling back and forth between two virtual machines using Azure. We'll use simple tools like Wireshark and PowerShell to watch and understand how everything works. As well as expiriment with Network Security Groups (NSGs).
<br />


<h2>Environments and Technologies Used</h2>

- <b>PowerShell</b> 
- <b>WireShark</b>
- <b>Remote Desktop</b>
- <b>Various Network Protocols (SSH, DNS, ICMP)
- <b>Firewalls (NSGs)
- <b>Microsoft Azure

<h2>Operating Systems Used </h2>

- <b>Windows 10</b> (21H2)
- <b>Linux Ubuntu</b> x64(Gen2)

<h2>Actions and Observations</h2>

<p>
This is a cotinued lab from my other repository "VM Connection Lab", but in this one we are going to utilize tools like Powershell and Wireshark to read traffic between our two virtual machines using various different network protocols. 

After we connect to the Windows virtual machine, go to WireShark on the web and download it with default settings. This will allow us to observe traffic across various protocols.
<p>
<br />

![WireShark Download](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Download%20WireShark(Windows%2064x%20bit)%20with%20Default%20Settings.PNG?raw=true)

<br />
<p>
Once downloaded, go to quickstart and lool up WireShark. From there we are going to select the ethernet adaptor.
<p>
<br />

![Ethernet Icon](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Ethernet%20Icon%20on%20WireShark.PNG?raw=true)

<br />
<p>
After, hit the small blue WireShark icon at the top left hand of the screen to start capturing packets. This is going to let us see the live traffic that's happening on our virtual machine. As you view the model below, you'll start to see just a bunch of random traffic, even when were not doing anything. You'll see a number under the source IP address that is 10.0.0.4, which is our private IP address that is sending traffic to all these random places on the internet.
<p>
<br />

![WireShark Traffic](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Traffic%20Within%20WireShark(Blue%20icon%20is%20to%20be%20able%20to%20start%20the%20tracking%20of%20traffic).PNG?raw=true)

<br />
<p>
First we are going to filter traffic by puting icmp at the top where you apply filters and hitting enter. This should cause the traffic to disappear. ICMP is the protocol that ping uses, and we use ping to test connectivity to different hosts on the internet or on a network. So in this part of the lab we are going to try and ping our VM2, and in order to do that, we must find out what our IP address is for it essentially. The IP address would be located when you click on virtual machines from the Azure portal (outside of the remote desk top VM). From there we would click VM2 and that would give us all the details inside our virtual machine. When we are pinging VM2, we want to use our private IP address and that would be under networking, so copy that. Once copied, go back into your VM1 and type in PowerShell into the search, and type ping (your own private IP address) 10.0.0.5 and hit enter.
<p>
<br />

![Pinging VM2](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Used%20Windows%20PowerShell%20to%20ping%20VM2%20Private%20IP%20to%20monitor%20traffic%20(using%20icmp%20protocol%20(ping)).PNG?raw=true)

<br />
<p>
This shows us pinging VM2 and it replying 4 seperate times, and then below it shows out of the four, how many were successfully received. It's the same for WireShark as well, where the source 10.0.0.4 is our VM1 address and the destination is our VM2 address 10.0.0.5. It sends an Echo (ping) request, and then on the next line it shows it, but in reverse how VM2 is Echo (ping) replying to the request successfully. If you were wanting to have a perpetual ping, you would (in PowerShell) put the same ping 10.0.0.5 but you would add -t next to it and hit enter.
<p>
<br />

![Perpetual Ping](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/If%20you%20want%20a%20perpetual%20ping%20(you'd%20ping%20the%20private%20IP%20and%20then%20-t).PNG?raw=true)

<br />
<p>
While this is pinging if you wanted to change the firewall on VM2, let's say to not allow ICMP traffic to come through. Were going to minimize VM1 and go back to our Azure portal and search Network Security Groups (NSGs). Click on VM2-nsg, and then click inbound security rules because we are going to block that traffic coming from ICMP.
<p>
<br />

![Inbound Security Rules](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Click%20on%20Inbound%20Security%20Rules%20(inside%20network%20security%20group%20NSG2)%20to%20deny%20traffic%20to%20VM2%20vis%20versa.PNG?raw=true)

<br />
<p>
It'll show the existing rules inside our VM2. Were going to create another rule by clicking the add icon at the top. And then source is going to be from anywhere, but we can say specifically from VM1 if we wanted too. Source port ranges is fine, destination is fine with any as long as it's coming through this NSG. Leave service as custom, and change protocol to ICMP. Since we want to block traffic, we would chnage action to deny. One of the last options is priority which means in which order does the rule get evaluated in. As you can see on inbound security rules, it has SSH as 300 for the priority, but let's say this rule was for instance allow ICMP traffic, if we put it before the traffic would be denied. As well if we put it after like 310 or something, traffic would always be allowed. In this case we put 200 for the priority, and always write a descriptive name for your rule, so you know how it works.
<p>
<br />

![NSG on VM2](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Creating%20settings%20for%20VM2%20NSG%20to%20block%20all%20ICMP%20traffic%20(Priority%20number%20is%20200%20so%20that%20it%20comes%20before%20300%20incase%20300%20was%20too%20allow%20all%20ICMP%20traffic).PNG?raw=true) ![New NSG Settings](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/New%20Settings%20Implimented.PNG?raw=true)

<br />
<p>
To be able to see the results, we'll go back into our VM1 from remote desktop and look at PowerShell. It'll show how the ping immediately timed out.
<p>
<br />

![Denied ping](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Once%20you%20apply%20the%20settings%20to%20the%20NSG%20your%20pings%20will%20be%20denied.PNG?raw=true)

<br />
<p>
And the same for WireShark, where it'll have our VM1 continuously requesting, but with no reply. If you want to allow it again, you would go back to the Azure portal, go to VM2-nsg and then click on the name of the rule we created. Instead of deny, hit allow and click save. Go back to VM1 connection and we'll start to see traffic replying back again (or you can just delete it).
<p>
<br />

![Reconnected Traffic](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/Once%20you%20allow%20the%20traffic%20again%20inside%20the%20NSG%20settings,%20it%20should%20begin%20replying%20to%20the%20requests%20again.PNG?raw=true)

<br />
<p>
Next we are going to look at SSH traffic (which is a protocol that allows users to access a remote computer), so type SSH is the command line inside WireShark and hit enter. And then in PowerShell instead of pinging VM2, we're going to SSH (or secure shell) into it from VM1. So in PowerShell type ssh then space, and then the username we used to create our VM's with (labuser), @ VM2's IP. So in this case it would look like this:   ssh labuser@10.0.0.5.
<p>
<br />

![SSH Example](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/SSH%20Connecting%20to%20VM2%20via%20SSH%20code.PNG?raw=true)

<br />
<p>
Hit enter, and you'll start to see immediately when we tried to initiate the SSH connection, you saw some SSH traffic spam in WireShark. Then in PowerShell it'll ask you if you want to continue connecting, type yes and hit enter. After this it'll ask for the password that was created for the username:labuser. When you type the password in, you won't be able to see it due to security reasons by the way. So when you hit enter for that, it should connect to VM2, and you'll see that by at the bottom it says labuser@VM2 in green. Since we're connecting to Linux, we will have to use those type of commands. For instance if we type: id (if you wanted to find out the user and group names as well as numeric ID's of users on the server), you'd see more traffic get spammed on WireShark. We could also type: uname -a (prints system information), pwd (prints the full name/path of the current/working directory), ls -lasth (lists the folders and files in the current directory) and in this one we could make a file, like if we type: touch hi.txt hit enter then type: ls -lasth again we'll be able to see our file being created. And then to close our SSH connection just type: exit and hit enter, then you'll be back in VM1's command line.
<p>
<br />

![Linux Commands](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/SSH%20Different%20types%20of%20commands%20using%20SSH%20protocol%20(and%20the%20traffic%20in%20it).PNG?raw=true)

<br />
<p>
Another more direct way (beyond the scope of this lab) to filter by SSH, would be to filter WireShark by: tcp.port == 22 because SSH uses port 22. So when we hit enter, it'll start filtering by this port which again is SSH.
<p>
<br />

![tcp.port == 22](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/SSH%20Or%20a%20more%20direct%20way%20to%20connect%20to%20VM2%20via%20SSH.PNG?raw=true)

<br />
<p>
Another way to filter traffic would be through DHCP. DHCP is used to automatically assign your IP address(usually something that happens on the background), and were going to force the renewal of an IP address. So in PowerShell were going to type: ipconfig /renew and when he hit enter, this VM1 is going to broadcast on our virtual network basically telling it to give us a new IP. Since were using Azure, the way it works is that there's actually a DHCP server inside the virtual network that's somewhat invisable, but that should re-issue our IP address to us. Once it's sent through WireShark, we should start to see traffic on PowerShell.
<p>
<br />

![DHCP Traffic](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/DHCP%20Showing%20(DHCP)%20traffic%20over%20the%20network%20and%20we%20got%20our%20IP%20address%20re-issued%20to%20us%20again.PNG?raw=true)

<br />
<p>
One last (for this lab) way to filter traffic would be through DNS (or udp.port == 53). DNS is basically like the phonebook of the internet. It translates domain names like "www.google.com" into numeric IP addresses that computers use to identify each other on the internet. Just like every port, type it into PowerShell and then in WireShark, in order to look it up we would type: nslookup www.google.com and hit enter. We should get an answer from DNS about some of googles IP addresses.
<p>
<br />

![DNS Traffic](https://github.com/nw4466tu/Azure-Network-Protocols/blob/main/DNS%20Traffic.PNG?raw=true)

<br />
<p>

<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
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
