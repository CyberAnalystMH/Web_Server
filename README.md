# Setting up a Web Server Overview

# Summary

One of the most used cases for Linux Servers is a web server, making the backend code is one thing but deploying the actual web server is a large part of the task for creating a usable and stable web server. 

**This repo will update overtime:**

**Last Update:** 6/8/2026
# Setup

**Theme/Scenario:** Let's say I'm famous and everyone wants to hire me/my company, I made a website where companies puts first name, last name, email and phone number in order to contact me and my small team, that data is stored in a DB. 
 
Now, I don't know any HTML code outside of "Hello World" but Linux Admins are usually supplied with a great HTML or CSS code and we're gonna imagine that I got supplied with one.

Here's the exact simple HTML code I'm using: https://codepen.io/terf/pen/bGRavY

**My made-up tasks are to:** 
- Build a Web Server using HTTP
- Creating a DB for the Web Server
- Securing a Web Server (Only Server side and not code side)
- Applying correct permissions 
- Adding other users for a collaborative projects 

**Hardware and System** 
- Rocky Linux (No GUI)
- 40GB of Storage
- 3 CPUs
- 6GB 
- VM on Proxmox

**Basic Configurations:** 
- For Hostname set-up: hostnamectl hostname-set **webserver** (Since this offline, no domain is specified)
- Static IP: nmcli to see IP, Then: nmtui (Assign static IP)
- Reboot (To ensure it works)

**Tools Used:**
- HTTPD
- MySQL
- MariaDB
- PHP
- Lynis
- Gocryptfs
- Firewalld
- SELinux
- Wazuh
- Suricata
- PAM
- Crontab
- VSFTPD
- Rsync

**Tools Used but not mentioned:**
- Libre Office - For documenting steps and procedures
- Fish - To speed up command workflow
- Btop - To get a nice visual monitoring on system
- GPG for file encryption

# HTTPD


<img width="1081" height="358" alt="image" src="https://github.com/user-attachments/assets/5447c7cf-e6ab-4fc4-942f-b93569b3be3d" /> 


**Annotations:** 
- After installing Rock Linux I did "dnf update -y". 
- Once it finished updating, I did "dnf install httpd -y".
- Then: systemctl enable --now httpd so that system is auto-enabled when the system is booted.
- Then, enabled SSH.

<img width="492" height="43" alt="image" src="https://github.com/user-attachments/assets/a46ad183-2263-4d45-bf0d-ba9176285c7d" /> 

\
<img width="425" height="233" alt="image" src="https://github.com/user-attachments/assets/f8a74a13-613a-40c4-8cae-844c708231bb" />

**Annotations:** 
- Then made a directory for my project: mkdir /server_project with an index.html.
- Instead of /var/www/html, I wanted it to point to /server_project.

<img width="885" height="114" alt="image" src="https://github.com/user-attachments/assets/8bc2d32c-4757-4441-b110-a84ae79a6474" />



**Annotations:** 
- Because SELinux will automatically block a new directories in httpd.conf, semanage must be used.

<img width="648" height="117" alt="image" src="https://github.com/user-attachments/assets/d907e62e-2a09-48db-a31d-ef13d9eb6276" />

\
<img width="196" height="24" alt="image" src="https://github.com/user-attachments/assets/91dbda01-5d05-4020-b56b-8226bcde11af" />


**Annotations:** 
- Default Firewalld doesn't allow http services so it needs to be added.
- Then we restart httpd and do a simple "curl localhost" which works, however when you visit on a different device then the server, it automatically doesn't work.  
- We must change the listen from 80 to "0.0.0.0:80" and restart httpd again.

**Note:** Until the project is finished, SELinux is disable since I'm constantly moving, deleting, adding files and SELinux doesn't like that. 

**Result:** 

<img width="854" height="1206" alt="image" src="https://github.com/user-attachments/assets/0f1f3e05-1f7c-47e7-b97d-d0708f470722" />

**Annotations:** The code is slightly changed using AI just for the sake of demo, the overall HTML is still the same. 

# Database

<img width="586" height="26" alt="image" src="https://github.com/user-attachments/assets/1bd4c538-86d8-4bdd-950d-50f5712b3d31" />

<img width="586" height="196" alt="image" src="https://github.com/user-attachments/assets/a02802b8-1fba-40f2-93ac-d669c240577c" />

**Annotations:** I chose "**MariaDB**" to use in pair with MYSQL with PHP format HTTPD to store the contact info, I learned this is called a "**LAMP Stack**" which is Linux, Apache, MySQL, PHP)

<img width="620" height="599" alt="image" src="https://github.com/user-attachments/assets/93969e44-fd17-4da4-adec-978d645fb4e0" />

**Annotations:** This is my php file labeled as "contacts.php" and called out in the HTML code, again I'm sure none of this is secure coding wise since I have limited understanding of code security.

<img width="140" height="468" alt="image" src="https://github.com/user-attachments/assets/6563d461-6b05-4a66-8d8f-3092c43362ae" />

**Annotations:** I used the number 1 as a test trial but whenever I enter more data, it keeps showing the 1 output I put initially and wouldn't populate to more data sets, then I learned that their should be an insert.php and a view.php that are too separate files.

<img width="350" height="21" alt="image" src="https://github.com/user-attachments/assets/9e555b82-4976-46ea-88e0-702467804c5e" />

**Annotations:** contacts.php is my inserting and view.php allows me to view datasets on SQL CLI. 

<img width="669" height="627" alt="image" src="https://github.com/user-attachments/assets/ed941b0a-ed71-4404-9e65-9e44d1cac5d7" />

**Annotations:** This is the view.php, again AI was used to make this, I can make sense of it but I'm not able to create it from scratch but these are only made for sake of demo and focus on the main objective. 


<img width="967" height="314" alt="image" src="https://github.com/user-attachments/assets/d70042e6-aa18-45a7-ae5c-7340cdfd99c0" />

**Annotations:** Names are made up but this is a working DB with working functions, very basic but gets the job done.


# Remote Database

The above DB is on the same server as the Web Server which is okay but not efficient. I followed all the steps I did on the first install then just repeated it on the second server, since the data before were test data, the data is brand new but it works. 

<img width="1260" height="468" alt="image" src="https://github.com/user-attachments/assets/672ce79e-98f6-423a-afc6-52e1229ca3a1" />

**Annotations:** First step is to input the server into hosts then make a ssh keygen then copy the key to the remote server.


<img width="669" height="621" alt="image" src="https://github.com/user-attachments/assets/f96111fd-902d-4bb5-8273-a9a40e8022b9" />

<img width="694" height="404" alt="image" src="https://github.com/user-attachments/assets/4676c424-818f-4af1-92d6-a850583b23d8" />


**Annotations:** After following all the steps, it works on the remote server as well, the only difference is instead of "localhoist" on the php, it's just the IP address of the remote server. Now this wouldn't been much easier if I simply cloned the Web Server and removed the httpd aspects and left everything else but sometimes starting fresh is better.

# Managing Users

**Made-Up Scenario:** Since I'm a Linux Admin and have no skills in code making but my Co-Workers do! But they're somewhat familiar in Linux but they can't do anything other than the basics.

**Rules and Configs:**
- 3 Dev Team collab, named: Luke, Nate, Lisa
- They get a welcome.txt letting them know about updates and what not.
- They're not allowed access to anything expect /collab folder
- Luke is the only one allowed to rwx because he's the Dev Leader, the other two can only Read and Write

<img width="768" height="67" alt="image" src="https://github.com/user-attachments/assets/e66c6b0a-0d6c-4b4e-a613-c95e5e381b64" />

<img width="558" height="17" alt="image" src="https://github.com/user-attachments/assets/ba6ad3ff-ba8e-43bc-aa52-20bca90ea4ad" />

**Annotations:** Chmod is great for overall permissions but it's not permission specified, in this instance we would use Facl or File Access Control List.

<img width="544" height="308" alt="image" src="https://github.com/user-attachments/assets/0de86e15-700c-4afb-af86-624dc1435679" />

**Annotations:** It's true that Devteam has full access to the collab folder but since Facl kicked in, it's no longer true and only dev_luke has full access.

<img width="552" height="221" alt="image" src="https://github.com/user-attachments/assets/859948d8-959b-4f09-9b02-828da35c900a" />

**Annotations:** Dev_lisa is on a year contract and we want to ensure that dev_lisa loses her access after a their contract ends. 

<img width="987" height="160" alt="image" src="https://github.com/user-attachments/assets/9aec7fe4-81af-4a5c-890b-f2a8988216c8" />

**Annotations:** Everyone gets a welcome.txt, defined in /etc/skel/welcome.txt.

<img width="178" height="69" alt="image" src="https://github.com/user-attachments/assets/d88a215c-3395-45d3-ae78-0bb01ce7efa4" />

<br>

<img width="636" height="774" alt="image" src="https://github.com/user-attachments/assets/7ba6f78c-8717-4c14-824f-d13981ad7229" />

**Annotations:** Hey, new team members! Dev_emma and dev_mark, welcome!....but their aging are pre-defined because they're an intern. They has very limited permissions.

<img width="345" height="62" alt="image" src="https://github.com/user-attachments/assets/59601a8d-769d-46f1-9ceb-66a512921571" />

**Annotations:** Oh wow, 2027 is already here and dev_lisa's contract is over but she has important files that we need, we''ll lock her out for the time being. 

<img width="957" height="54" alt="image" src="https://github.com/user-attachments/assets/e66779a3-ca47-49da-8846-9028e38be185" />

**Annotations:**  /etc/login.defs is very limited on authentication so PAM would need to be used with defined rules. 

<img width="720" height="123" alt="image" src="https://github.com/user-attachments/assets/b86c8055-5e7d-41e6-8837-4145a5292717" />

**Annotations:**  Now the User cannot use a weak password to authenticate and must user a strong password. 

**FTP**

<img width="681" height="138" alt="image" src="https://github.com/user-attachments/assets/a58403e8-641e-4038-b616-868580e72af3" />

**Annotations:** Our talented Interns are great but they have 0 Linux knowledge because they're hard at work to make the Web HTML great, they need a costume folder for me (The Admin) to store their stuff.

<img width="295" height="169" alt="image" src="https://github.com/user-attachments/assets/f21392b9-ae52-4fd5-bd79-69f4681d9314" />

**Annotations:** After installing VSFTPD on Rocky, I made a simple Non-GUI FTP server for interns to access and upload their files needed for the Web Server.

<img width="337" height="295" alt="image" src="https://github.com/user-attachments/assets/27bd64bc-6810-4b7b-9d5e-198936d8821e" />

**Annotations:** Once they login, they get a welcome and a project folder and that's the only thing they're allowed access, server wise. 

<img width="668" height="85" alt="image" src="https://github.com/user-attachments/assets/2e9759c9-881c-4f8a-badd-9055b380c752" />

**Annotations:** The welcome. 


<img width="335" height="49" alt="image" src="https://github.com/user-attachments/assets/3d31524b-d01c-4009-a0b9-49cb9263de87" />

**Annotations:** Since moving more to a remote DB, I think I need help in managing it, oh hey! Look, jr_john who's also a Junior System Admin will be joining the team, he needs some permissions....but it's kinda awkward to have to have two people with root, so I''ll make him also an admin on the DB server. 


# Overall Basic Security 

Linux Hardening is fun and complicated. There are several tools to guide you through it, I like STIGS from the DoD for Linux Hardening best practices, each section is short and sweet on a basic software that lets you keep track but checking wise, I like Lynis.

<img width="640" height="440" alt="image" src="https://github.com/user-attachments/assets/e5c7e0cc-e811-498a-ae14-eca669341d6c" />

**Annotations:** Lets me know where I'm at and where I need to be, getting the harden index to be 100% is pretty hard (No pun-intended...) but anything in the high range 70%-100% is a great head start. Everytime I make a major security change, I run Lynis to see where I'm at again and where I need to be, again. 

<img width="606" height="23" alt="image" src="https://github.com/user-attachments/assets/9fa48354-28ce-4349-b5a0-4b5e15e79704" />


**Annotations:** **We already have SELinux**, so that's good and I'm aware ClamAV is terrible on "Enterprise" level but this is part of my "Defense-In-Depth" action, it' s better then nothing. 

<img width="745" height="839" alt="image" src="https://github.com/user-attachments/assets/4a6d8a27-0672-41dd-927c-a04a2f35081f" />

**Annotations:** After installing **Suricata**, I did basic custom configuration rules to test it out. 

<img width="767" height="44" alt="image" src="https://github.com/user-attachments/assets/bcd8dcc2-ba09-4b6a-94b0-a721239e6344" />


**Annotations:** I like to utilize both pre-made rules and custom rules to fit my needs. It's using the basic suricata.rules and my basic rule configs.

<img width="607" height="56" alt="image" src="https://github.com/user-attachments/assets/407ec5d7-cab6-4796-b869-84c6106bd612" />

<img width="562" height="165" alt="image" src="https://github.com/user-attachments/assets/165c0b4c-2347-41fb-b4eb-5590ec5866b6" />

**Annotations:** Suricata is great but my favorite tool is **Wazuh** and since I already have a Wazuh Server running, it was easy as registering the Web Server to it. I ran did on purpose failed ssh authentication to test it. 

<img width="874" height="115" alt="image" src="https://github.com/user-attachments/assets/fe5fcdb6-b02f-4965-a532-aa7de34a118b" />

**Annotations:** On Firewalld, I made a new zone of ssh_trusted and only allowed 3 IPs to SSH into it and everything else would be explicit deny. 

<img width="608" height="64" alt="image" src="https://github.com/user-attachments/assets/2b1d302b-121f-4ca7-a033-71182b13fb36" />

**Annotations:** On a different machine when we try to remote in, auto-denys. 

<img width="902" height="358" alt="image" src="https://github.com/user-attachments/assets/7715b8e5-1c05-419b-9f7b-c35561715836" />


**Annotations:** Encrypted the Web Server files using **gocryptfs** then ran into issue but then I encrypted /encrypted and put the /server_project into that directory and re-did semanage and restorecon and works. 

**I couldn't do:**
- Full Disk Encryption (FDE) because it's too late but great to setup upon the installer, using LUKS, so **gocryptfs** was the best second thing to do.

# Overall Basic Automation

**Backup:**

<img width="600" height="494" alt="image" src="https://github.com/user-attachments/assets/457e18f1-46cc-4b65-921c-46e1ff3b1587" />

**Annotations:** Thanks to **LearnLinuxTV** I was able to follow in making this great script for file backup.

<img width="545" height="199" alt="image" src="https://github.com/user-attachments/assets/04ac8e69-6943-4c28-9de8-02624ef04784" />

<img width="263" height="27" alt="image" src="https://github.com/user-attachments/assets/145a9026-caa9-4329-9f19-e4d0fdb334a4" />

**Annotations:** It worked nicely.

<img width="689" height="53" alt="image" src="https://github.com/user-attachments/assets/77f4ee67-075e-4f26-86cb-b4f3469f0b0b" />

**Annotations:** Then tuned it into a Systemd so it auto-runs at specified times. 


# Will everything survive a Reboot? 

The answer is: **Yes!** 

Everything works as left when rebooting. 

# Conclusions 

This was a lot of fun, despite simple, It felt rewarding in seeing how real world web servers are made on a small scale, this could still be built on by introducing clusters, backups, larger data sets, etc. For 3 days worth of work, not bad. 

**A.I. Transparency:** AI was only used to write the HTML, PHP files as I have no knowledge in writing in those program languages.  

***No AI was used to write this repository.*** 

# Next Steps 

- Separate the DB from the server and have it on a different server. ✅
- Backup the systems in full or incremental. ✅
- Add more data to the DB. ✅
- Add FIM (File Integrity Monitoring) on both servers.
- Deploy a HA (High Availability) for when the Web Server goes down for any reason. 
- Add more automation.
