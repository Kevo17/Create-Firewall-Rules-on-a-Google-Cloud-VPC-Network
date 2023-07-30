<h1>Create Firewall Rules on a Google Cloud VPC Network</h1>


<h2>Description</h2>
In this hands-on lab, we will be presented with a custom VPC that has four instances spread across three subnets with zero firewall rules created. We will configure two different firewall rules: one to allow SSH access to all instances on the network, and another one using specific network tags to only allow ICMP (ping) access to one instance, and only from a specific subnet. This will demonstrate using both wide-scope and narrow-scope firewall rules.
<br />


<h2>Environments Used </h2>

- <b>GCP</b>
- <b>Windows 11</b>
- <b>Linux</b> 

<h2>Program walk-through:</h2>

Before starting, delete the default network.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/nfgGv0F.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Create firewall rule.
- Set the following values:
  - Name: ssh-allow
  - Network: custom-vpc
  - Targets: All instances in the network
  - Source filter: IP ranges
  - Source IP ranges: 0.0.0.0/0
  - Protocols and ports: Specified protocols and ports
  - tcp: Check, and type in 22
Click Create. It will take about 10 seconds for the rule to finish creating.


<br/>
 
<p align="center">
<img src="https://i.imgur.com/mQWA1es.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

SSH in the newly made instance to check if it is up and running

<br/>
 
<p align="center">
<img src="https://i.imgur.com/Dv8adbh.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Now we’re going to apply a Network Tag to the instance
Still on the VM instances page, click instance-2.
Click EDIT.
Under Network tags, enter "icmp-allow".
Hit Enter to confirm the tag.
Click Save at the bottom.


<br/>
 
<p align="center">
<img src="https://i.imgur.com/XwF49A9.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Now we’re going to create a Narrorw-Scope Firewall Rule for the instance
- Set the following values:
  - Name: allow-icmp
  - Network: custom-vpc
  - Targets: Specified target tags
  - Target tags: Enter "icmp-allow", and hit Enter
  - Source filter: IP ranges
  - Source IP ranges: Enter the subnet-a IP address you noted a minute ago
  - Protocols and ports: Specified protocols and ports
  - Other protocols: Check, and type in icmp
Click Create.



<br/>
 
<p align="center">
<img src="https://i.imgur.com/Saq0sRW.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Test the ICMP Firewall rule for success<br />
Click SSH next to instance-1a.

Once the connection is established, attempt to ping instance-2:

- ping <INSTANCE-2_INTERNAL_IP><br />
It should be successful.




<br/>
 
<p align="center">
<img src="https://i.imgur.com/ZStDqNs.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Copy the internal IP next to instance-3 (it should be 10.0.3.2).<br />

Back in the instance-1a terminal, attempt to ping instance-3:

- ping <INSTANCE-3_INTERNAL_IP><br />
It will fail because instance-3 doesn't have the icmp-allow applied to it.





<br/>
 
<p align="center">
<img src="https://i.imgur.com/3ttvUBR.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Click SSH next to instance-3.<br />

Once the connection is established, attempt to ping instance-2 again:

- ping <INSTANCE-2_INTERNAL_IP><br />
It should not be successful because the firewall rule allowing ICMP access doesn't apply to the IP range for subnet-c.


<br/>
 
<p align="center">
<img src="https://i.imgur.com/3g4OVRR.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />
