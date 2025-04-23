 Description:
"GameZone" is a solid CTF room where I walked through a full attack chain — from initial access via SQLi to gaining full root control over the target machine.

Started with a basic Nmap scan — found two open ports: SSH and HTTP.

 Accessed the website and discovered a login page. After poking around, I found a hidden directory: /portal.php, which included a search form — likely vulnerable to SQL injection.

Captured the request with BurpSuite and saved it to a .txt file for SQLMap.
Used SQLMap to enumerate the database:

First, retrieved the database name

Then dumped table names

Finally, extracted the contents of the tables

Found credentials: agent47 with a hashed password.

 Identified the hash as SHA-256, decoded it using dcode.fr, and cracked it again with John the Ripper to confirm. Got a valid password.
 Logged into the target via SSH as agent47. Initial foothold acquired.

 Next step: there was a web service running locally on port 10000, not accessible from outside. I set up reverse SSH port forwarding with:

ssh -L 10000:localhost:10000 agent47@<IP>Accessed localhost:10000 in my browser and found an admin panel of a CMS.

 Used searchsploit to find a matching exploit for the CMS version. Loaded up Metasploit:

Selected the correct module

Set the required OPTIONS, and disabled SSL to avoid errors

Launched the exploit

 Got a Meterpreter session, upgraded to an interactive shell, and finally accessed /root.

 Grabbed the root flag — mission complete.

Takeaway:
GameZone is an awesome practical lab for real-world attack simulation. I practiced:

Manual and automated SQLi (with SQLMap)

Hash cracking (SHA-256 via dcode and John)

Reverse SSH tunneling

Privilege escalation via Metasploit
