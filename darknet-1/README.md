# Writeup on Darknet-1

Range: 172.16.8.60 - 90

so first thing first, ping sweep and you will find the host which is

```
    172.16.8.88
```

immediately, I did a nmap scan on it and it returns

```
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.6 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.7
```

so I went to take a look at the webpage which is at port 80. And I was greeted with a directory access.

![alt text](./res/first-greeting.png "First greeting")

And I followed the folder and it was a login page filled with information.

![alt text](./res/loginpage.png "Information-filled login page")

so tried to poke around and gather information on the web application. 

And I discovered this page.

```
http://172.16.8.88/webcalendar/UPGRADING.html
```
![alt text](./res/versionno.png "Version Number")

 which showed me the version number of the web application which ```k5n Webcalendar 1.2.4``` whose exploit can be found on the Internet easily at ```https://www.exploit-db.com/exploits/18775/```
 
 After running the exploit, 

![alt text](./res/how-to-run-webcal-rce.png "Running webcal rce")

you will be given a restricted shell and you are ```www-data```. 

![alt text](./res/webcalendar-shell.png "Restricted shell and settings.php")


So now what you want to do is to escalate your privileges, so I went around poking.
 
 Eventually, I ran ```sudo -l``` and saw this
 
 ![alt text](./res/sudo-l.png "Sudo permissions for www-data")

And so, it is clearly stated that user ```www-data``` can run ```vi``` with ```sudo```. That's the equivalent of being able to read and write as ```root```.

And also, to get root, you can use these commands within ```vi```

```
:set shell=/bin/bash
:shell
```

![alt text](./res/root.png "I got root")


So I just went ahead and did a ```cat /root/proof.txt``` and I got the flag.

```
247beb9a259072431fab695ff2edd723
```

