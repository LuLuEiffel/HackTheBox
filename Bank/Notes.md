
# Box Name <!-- omit in toc -->

# Table of Contents <!-- omit in toc -->
- [Scan Results](#scan-results)
  - [Nmap](#nmap)
    - [Ports](#ports)
  - [DNS Enumeration](#dns-enumeration)
      - [Nslookup](#nslookup)
      - [Dnsrecon](#dnsrecon)
      - [Zone Transfer](#zone-transfer)
  - [Web](#web)
- [Expolit](#expolit)
- [Post Exploit](#post-exploit)
- [Flags](#flags)
    - [User Flag](#user-flag)
    - [Root Flag](#root-flag)

# Scan Results

## Nmap
```
# Nmap 7.80 scan initiated Tue Feb 18 08:17:08 2020 as: nmap -sC -sV -oA nmap 10.10.10.29
Nmap scan report for 10.10.10.29
Host is up (0.097s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 08:ee:d0:30:d5:45:e4:59:db:4d:54:a8:dc:5c:ef:15 (DSA)
|   2048 b8:e0:15:48:2d:0d:f0:f1:73:33:b7:81:64:08:4a:91 (RSA)
|   256 a0:4c:94:d1:7b:6e:a8:fd:07:fe:11:eb:88:d5:16:65 (ECDSA)
|_  256 2d:79:44:30:c8:bb:5e:8f:07:cf:5b:72:ef:a1:6d:67 (ED25519)
53/tcp open  domain  ISC BIND 9.9.5-3ubuntu0.14 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.9.5-3ubuntu0.14-Ubuntu
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Feb 18 08:17:25 2020 -- 1 IP address (1 host up) scanned in 17.14 seconds

```

### Ports
* 22- SSH  
  
* 53 - tcp dns-nsid.  
  DNS is normally using UDP. TCP tells us that zone transfer might be enabled, and we can ask the DNS server to give us all the information it has on a certain domain. this way we can discover more information about the domain.  

* 80 - Http

## DNS Enumeration
#### Nslookup
```
nslookup 
```
change the default DNS to the bank's DNS by typing:
```
server 10.10.10.29
```
we can verify the change using:
```
lserver
```
now type an addresse to see if we get any information.
* `127.0.0.1` 
* `10.10.10.29`
* `bank.htb`

#### Dnsrecon

```
dnsrecon -r 127.0.0.0/24 -n 10.10.10.29

dnsrecon -r 127.0.1.0/24 -n 10.10.10.29

dnsrecon -r 10.10.10.0/24 -n 10.10.10.29
```

#### Zone Transfer
```
dig axfr @10.10.10.29
```
we get nothing - zone transfers arent enabled for the root zone
now we try:

```
dig axfr bank.htb @10.10.10.29
```
and we get aPfew responses

* chris.bank.htb
* ns.bank.htb
* www.bank.htb
  
add bank as our DNS server by adding to result.conf
```
vi /etc/resolv.conf

nameserver 10.10.10.29
```
now we can `ping bank.htb`

## Web

Opening a browser and going to 10.10.10.29
we get the default apache web page.  
If we go to bak.htb we get a login page.


# Expolit

# Post Exploit

# Flags

### User Flag

### Root Flag

