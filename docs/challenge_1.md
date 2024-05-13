# **Challenge 1: Network Scanning**

!!! note ""

## **Procedure:**

The first step is always reconnaissance. We need to identify all of the relevant targets in our network and find out what they're running.

- Use Nmap to run a basic scan on the subnet your [Kali](https://www.kali.org/ "Kali Linux!")  machine is connected to. You should find four hosts in your results, not including your own Kali machine.
  - **Hint:** Where can you find the subnet range of your Kali VM?
  - **Reminder:** Running Nmap with root privileges might give you a lot more results than intended, showing hosts that are online but with no open ports. Re-run the scan as a normal user (without sudo) or ignore the results with no ports open.
- Next, run service and version detection scans on the specific IP addresses found in your first scan. Scan for services beginning at port 1 and ending at port 5000.
  - **Hint:** How do you perform a service and version scan in Nmap?
  - **Hint:** How do you specify a range of ports to scan in Nmap?
  - **Reminder:** You can scan all four IP addresses in a single command, or scan them each individually. Remember that the more you do in one scan, the longer it will take.

Interpret your results and determine the following:

- Which host is running a web server on a non-standard port? What port is it running on?

- Which host is running an SSH server on a non-standard port? What port is it running on?

- Which machines are running Windows-based operating systems?

## **Solution:**

- Run a basic scan to check the subnet:

```python linenums="1" hl_lines="11"

┌──(kali㉿kali)-[~]
└─$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 02:6c:7f:8f:39:11 brd ff:ff:ff:ff:ff:ff
    inet 172.31.39.126/20 brd 172.31.47.255 scope global dynamic eth0
       valid_lft 3164sec preferred_lft 3164sec
    inet6 fe80::6c:7fff:fe8f:3911/64 scope link 
       valid_lft forever preferred_lft forever
```

  >Based on the output  provided from  `ip addr` command, we can see that the Kali Linux machine is configured with the IP address `172.31.39.126` on the `eth0` interface, and it's within the `/20` subnet. This means the address range you can scan is from `172.31.32.0` to `172.31.47.255`.
  The `eth0` interface is configured with an IP in the `172.31.32.0/20` subnet, indicating it can communicate with any devices whose IP addresses fall within this range. The subnet setup allows your machine to interact with potentially 4094 hosts (from `172.31.32.1` to `172.31.47.254`, excluding the network and broadcast addresses).


- Identify which hosts are up in the `172.31.32.0/20` subnet, you can start with a ping sweep using Nmap. This is a non-intrusive way to discover active hosts.

```python linenums="1"
┌──(kali㉿kali)-[~]
└─$ nmap -sn 172.31.32.0/20
Starting Nmap 7.93 ( https://nmap.org ) at 2024-05-07 23:59 UTC
Nmap scan report for ip-172-31-39-126.us-west-2.compute.internal (172.31.39.126)
Host is up (0.00036s latency).
Nmap scan report for ip-172-31-40-22.us-west-2.compute.internal (172.31.40.22)
Host is up (0.0057s latency).
Nmap scan report for ip-172-31-43-103.us-west-2.compute.internal (172.31.43.103)
Host is up (0.00076s latency).
Nmap scan report for ip-172-31-44-198.us-west-2.compute.internal (172.31.44.198)
Host is up (0.0055s latency).
Nmap scan report for ip-172-31-45-94.us-west-2.compute.internal (172.31.45.94)
Host is up (0.0013s latency).
Nmap done: 4096 IP addresses (5 hosts up) scanned in 63.81 seconds
```

>Now that you have identified the active hosts, you can perform more in-depth scans on each of these IP addresses. The goal here is to discover which services are running on each host and to identify any potential vulnerabilities associated with these services. For each host, you might run the following Nmap command, which scans for service versions on commonly used ports:

- Next, run service and version detection scans on the specific IP addresses found in your first scan. Scan for services beginning at port 1 and ending at port 5000.

```python linenums="1"
┌──(kali㉿kali)-[~]
└─$ 
nmap -sV -p1-5000 172.31.40.22
nmap -sV -p1-5000 172.31.43.103
nmap -sV -p1-5000 172.31.44.198
nmap -sV -p1-5000 172.31.45.94

Starting Nmap 7.93 ( https://nmap.org ) at 2024-05-08 00:05 UTC
Nmap scan report for ip-172-31-40-22.us-west-2.compute.internal (172.31.40.22)
Host is up (0.0016s latency).
Not shown: 4998 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
1013/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.88 seconds
Starting Nmap 7.93 ( https://nmap.org ) at 2024-05-08 00:05 UTC
Nmap scan report for ip-172-31-43-103.us-west-2.compute.internal (172.31.43.103)
Host is up (0.00016s latency).
Not shown: 4996 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.58 seconds
Starting Nmap 7.93 ( https://nmap.org ) at 2024-05-08 00:05 UTC
Nmap scan report for ip-172-31-44-198.us-west-2.compute.internal (172.31.44.198)
Host is up (0.0041s latency).
Not shown: 4999 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
2222/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.87 seconds
Starting Nmap 7.93 ( https://nmap.org ) at 2024-05-08 00:05 UTC
Nmap scan report for ip-172-31-45-94.us-west-2.compute.internal (172.31.45.94)
Host is up (0.00017s latency).
Not shown: 4996 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.78 seconds
```

>Which host is running a web server on a non-standard port? What port is it running on? 

>**Host at 172.31.40.22**
**HTTP (1013/tcp)**: Running Apache httpd 2.4.52 on Ubuntu. While this is a modern version, Apache servers can be vulnerable to misconfigurations, outdated modules, or specific flaws depending on the site setup.

>Which host is running an SSH server on a non-standard port? What port is it running on?

>**Host at 172.31.40.22**
**SSH (22/tcp**): Running OpenSSH 8.9p1 on Ubuntu. This version is relatively up-to-date, so vulnerabilities are likely minimal. However, ensure that only strong, secure authentication methods (e.g., key-based authentication) are enabled.


>Which machines are running Windows-based operating systems?

>**Host at 172.31.43.103 and 172.31.45.94** 
**Microsoft Windows RPC (135/tcp)**
**NetBIOS Session Service (139/tcp)**
**Microsoft DS (445/tcp)**: These services are indicative of a Windows environment and are typical in Windows networking but can be vectors for attacks like SMB Relay, Pass the Hash, etc.
**Microsoft Terminal Services (3389/tcp)**: Remote desktop protocol service is running, which could be vulnerable to brute force attacks or exploits depending on the RDP configuration and patch level.

<div id="disqus_thread"></div>
<script>
    /**
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
    /*
    var disqus_config = function () {
    this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://hcoco1-1.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

!!! note ""

<div class="button-container" markdown="1">
<a href="/Career-Simulation-3/2-instructions/" class="md-button md-button--primary">Previous: Introduction</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/challenge_2/" class="md-button md-button--primary">Next: Challenge 2</a>
</div>




