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
- <b>Various Network Protocols (SSH, DNS, HTTP/S, ICMP)
- <b>Firewalls
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
While this is pinging if you wanted to change the firewall on VM2, let's say to not allow ICMP traffic to come through. 
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
