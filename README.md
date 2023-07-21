# PG-Practice-Snookums
"Snookums" machine writeup


<h1>Snookums Machine Writeup</h1>


<h2>Description</h2>
Project consists of performing penetration test on a Linux machine called "Snookums".
<br />


<h2>Languages and Utilities Used</h2>

- <b>Nmap</b> 
- <b>Searchsploit</b>
- <b>Smbclient</b>
- <b>Mysql</b>

<h2>Environments Used </h2>

- <b>Kali Linux</b>

<h2>Program walk-through:</h2>

<p align="center">
Running Inital Nmap Fast Scan against target: <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/nmapinital.png" height="80%" width="80%" alt="nmap scan initial"/>
<br />
<br />
Nmap Inital-Finding Open Ports for :  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/nmap2.png" height="80%" width="80%" alt="nmap scan initial"/>
<br />
<br />
Nmap Advanced Scan to drill down and further enumerate Services: <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/nmapAdv1.png" height="80%" width="80%" alt="namp scan adv"/>
<br />
<br />
Nmap Advanced Scan Open Ports and Services:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/nmapAdv2.png" height="80%" width="80%" alt="nmap scan adv"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/nmapAdv3.png" height="80%" width="80%" alt="nmap scan adv"/>
<br />
<br />
Seeing that 445 is open, we smb into shares to see what we can read or write. We find that we can read logs:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/smb1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/smbclient1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Here we can get and read the local.txt flag we also further enumerate by getting the misc.log and reading it:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/smblocaltxt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/smbmisclog.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 We find stored credentials to an application in the misc.log.  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/smbmisclogadminpass.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 Traveling to port 8003 in a web browser, we find the web application we can use the creds on. Further enumeration allows us to find the version of the application, "Booked 2.7.5"  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/URLbooked.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 We use searchsploit and find an RCE exploit to use. We copy it with the "-m" command to the current working directory for use.  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/searchsploitBooked.png" height="80%" width="80%" alt="searchsploit"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/bookedRCE.png" height="80%" width="80%" alt="searchsploit"/>
<br />
<br />
 We use python, entering in the creds and IP address of the target and get a low level shell:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/runningBookedRCE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To get privilege escalation we find a writable script(cleanup.py). We base 64 encode a reverse shell and write it to the cleanup.py:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/Crontabuploadrevshell.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 We use netcat on our host machine to catch our root shell and proof.txt:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/CatchRootshell.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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


