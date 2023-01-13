# THM RootMe Walkthrough By Tirexv2
![RoomLogo](https://miro.medium.com/max/700/0*4vGBRHcDbSK82n4P.png)

This [Room](https://tryhackme.com/room/rrootme) will help you to practice the usage of `Gobuster` , `Nmap` as tools and `Privilege escalation` , `WebShell` 

## Task 1

1- Start Machine

2- Start AttackBox (if you want to use Web GUI) else use OpenVpn

## Task 2

We will scan the machine using Nmap 
`nmap -sC -sV Machine IP`

`-sC` refers to Scan using Defaults scripts

`-sV` refers to pull infos of open ports 

after this scan you will be able to answer first 3 questions 

```
Scan the machine, how many ports are open? : 2 Ports : 80 & 22

What version of Apache is running? : 2.4.29

What service is running on port 22? : SSH
```


