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




