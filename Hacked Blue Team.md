# Hacked Blue Team Lab

Link Lab: https://cyberdefenders.org/blueteam-ctf-challenges/hacked

Tool:
  - FTK Imager: https://www.exterro.com/ftk-product-downloads/ftk-imager-version-4-7-1

Using FTK Imager to view file dump with extension .E01

### Q1: What is the system timezone?
#### ANS: Europe/Brussels

In file "/etc/timezone"

![image](https://github.com/user-attachments/assets/d101bdb8-2915-4adf-b433-65a490025ca5)

### Q2: Who was the last user to log in to the system?
#### ANS: mail

In file "/var/log/auth.log"

![image](https://github.com/user-attachments/assets/a576d9bc-6704-4280-9524-a4fcbc7406f1)

### Q3: What was the source port the user 'mail' connected from?
#### ANS: 57708

In file "/var/log/auth.log"

![image](https://github.com/user-attachments/assets/6fd967cf-9c81-4799-8e01-22ec115c6a65)

### Q4: How long was the last session for user 'mail'? (Minutes only)
#### ANS:  1

In file "/var/log/auth.log"

![image](https://github.com/user-attachments/assets/4ec7978d-3874-4745-a8bb-009060b9a005)

### Q5: Which server service did the last user use to log in to the system?
#### ANS: sshd

In file  "/var/log/auth.log"

![image](https://github.com/user-attachments/assets/25124860-8418-4398-ae08-be8d95d4c336)

### Q6: What type of authentication attack was performed against the target machine?
#### ANS: Brute Force

Many alert "Failed" appear in login -> Brute-force password to login server

![image](https://github.com/user-attachments/assets/74a2281f-7bf6-4656-8e84-dd0b7d286a84)

### Q7: How many IP addresses are listed in the '/var/log/lastlog' file?
#### ANS: 2

Two IP appear in file "lastlog" is 192.168.210.131 and 192.168.56.101

![image](https://github.com/user-attachments/assets/513ac819-0e69-4607-828f-079a66fb6577)

### Q8: How many users have a login shell?
#### ANS: 5

Check in file "/etc/passwd" with user has "/bin/bash"

![image](https://github.com/user-attachments/assets/ec05ef70-3a14-496d-a540-0382f1094ede)

### Q9: What is the password of the mail user?
#### ANS: forensics

In file "/etc/shadow" , you will see password of user in hash

![image](https://github.com/user-attachments/assets/2e65a176-600c-4895-92e8-c352f2584ed0)

Using tool <strong>john</strong> to encrypt password

![image](https://github.com/user-attachments/assets/0309e517-9f39-4d82-aac0-0eab7847f6e7)


### Q10: Which user account was created by the attacker?
#### ANS: php

Check the "auth.log" file to see what users were created. There were 6 users created: mysql, webmin, sshd, postfix, postgres, php. In these 6 users, most of the creations were normal, however, only the "php" user had strange behavior when it was added to the "sudo" group. It is predicted that this could be a user created by the attacker.

![image](https://github.com/user-attachments/assets/daea948a-dc55-4bd2-bf49-f73f2a353585)

### Q11: How many user groups exist on the machine?
#### ANS: 58

Check in file "/etc/group"

![image](https://github.com/user-attachments/assets/0579bf5d-dce0-448d-92ef-1c304d66e5c1)

### Q12: How many users have sudo access?
#### ANS: 2

Check in file  "/etc/group"

![image](https://github.com/user-attachments/assets/79517430-255c-4867-b5aa-15c69d752d8c)

### Q13: What is the home directory of the PHP user?
#### ANS: /usr/php

Check in file "/etc/passwd"

![image](https://github.com/user-attachments/assets/2c1dbf8f-7c7c-4840-b8c6-82b31d32f32e)

### Q14: What command did the attacker use to gain root privilege? (Answer contains two spaces).
#### ANS: sudo su -

First, you know that attacker added user "php" to server. Maybe the attacker will use this user to escalate privileges. However, after checking, it was discovered that this user did not escalate privileges. After checking, there is another user who is also added sudo privileges equivalent to this user "php".

![image](https://github.com/user-attachments/assets/67a05fda-b7c4-4730-9f9a-e3acfe55baeb)

From here we check this user's "bash_history" file to see the commands this user has executed.

![image](https://github.com/user-attachments/assets/07905f01-fc41-4acb-8c0b-c01f2cd3e0c3)

### Q15: Which file did the user 'root' delete?
#### ANS: 37292.c

To know which file the "root" user deleted, we will check its ".bash_history" file.

![image](https://github.com/user-attachments/assets/e5a55c18-d436-417e-953e-4b406e179e2a)

### Q16: Recover the deleted file, open it and extract the exploit author name.
#### ANS: rebel

Restore the file in the "/tmp" folder and we will get the trojan file as follows:

![image](https://github.com/user-attachments/assets/b7b7b6a6-f25e-4a56-ae52-8e5a31369ad0)

Search CVE on browser

![image](https://github.com/user-attachments/assets/cf0d4511-bbcb-464f-a586-b3f9fe034c5a)

### Q17: What is the content management system (CMS) installed on the machine?
#### ANS: Drupal

Check in file "/var/www/html/jabc/index.php"

![image](https://github.com/user-attachments/assets/45f3f5d2-a7de-4e20-9cb1-c6c2a154c0a8)


### Q18: What is the version of the CMS installed on the machine?
#### ANS: 7.26

We have determined that there are 5 files above that are designated as the Root of this CMS. We will check these 5 files first to determine the version.

![image](https://github.com/user-attachments/assets/ed476fd5-86a7-43d4-ac40-27b7143b8c64)

Check file "/includes/bootstrap.inc"

![image](https://github.com/user-attachments/assets/73b94f4a-c3c2-4456-b472-0938db30a5c3)

### Q19: Which port was listening to receive the attacker's reverse shell?
#### ANS: 4444

To know which port is listening for Reverse Shell, you need to check the access log. Normally, uploading this reverse will have to go through the POST method and attack IP is 192.168.210.131.

![image](https://github.com/user-attachments/assets/6bc1f4ed-82c4-4502-b626-0d5d57654ab6)

Using Base64 to decode hash in url, you can see listen port

![image](https://github.com/user-attachments/assets/cf0582cd-2526-46c1-8b95-7e5e9757bf2b)

