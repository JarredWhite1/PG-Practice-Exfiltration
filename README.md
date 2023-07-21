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
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/nmapinital.png" height="80%" width="80%" alt="nmap scan initial"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/nmapAdv2.png" height="80%" width="80%" alt="nmap scan initial"/>
<br />
<br />
Nmap Advanced After finding Open Ports for 22,21,80,111,445,139,3306,33060 :  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/nmapAdv.png" height="80%" width="80%" alt="nmap scan initial"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/nmapAdv3.png" height="80%" width="80%" alt="nmap scan initial"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/nmap%20Adv4.png" height="80%" width="80%" alt="nmap scan initial"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/nmapadv5.png" height="80%" width="80%" alt="nmap scan initial"/>
<br />
<br />
Enumerating Smb, we find no read or write access: <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/smbnoaccess.png" height="80%" width="80%" alt="namp scan adv"/>
<br />
<br />
Enumerating port 80 and running gobuster concurrently We find "pdfkit version 0.8":  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/port80httpenum.png" height="80%" width="80%" alt="port 80 enumeration"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/gobusterNOTHING.png" height="80%" width="80%" alt="nmap scan adv"/>
<br />
<br />
We use searchsploit and find a python module that allows command injection, copy it to our current directory and run the exploit:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/RCEpdfkit.07.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/POCpayload.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/searchsploitversionpdfkit.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/exploitRCEpython.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Here we netcat on one of the ports used on the machine and find we have an initial shell:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/initalshellfromRCE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Zino/blob/main/smbmisclog.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />Going into /var/www/html and looking seems to be the key to finding misconfigs in websites.
Here we find a php file with database creds in it!
Enumerating config files we find stored Mysql credentials:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/enumconfigfiles.png" height="80%" width="80%" alt="mysql pass"/>
<br />
<br />
 Connecting to the Mysql database and further enumeration finds us hashed user credentials:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/mysqlconnectingpass.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/enummysqldatabase.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/enummysqltables.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/enummysqluserspasswords.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 The user passwords are double encoded. I went to www.base64decode.org to decode them. We switch user to Michael:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/decodinguserspasswordsformysql.png" height="80%" width="80%" alt="searchsploit"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/SUtomichaelsacccount.png" height="80%" width="80%" alt="searchsploit"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/michaelaaccountID.png" height="80%" width="80%" alt="searchsploit"/> 
<br />
<br />
  Michael has access to write to etc/passwd. We are going to make a new root user, making the user and password. GitRekt user made:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/writetoetcpasswordfile.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
GitRekt user made. Switching to root shell:  <br/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/gitrektusermade.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/JarredWhite1/PG-Practice-Snookums/blob/main/SUrootandprooftxt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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


