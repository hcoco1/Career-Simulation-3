# **Challenge 4: Reconnaissance**
!!! note ""
## **Procedure:**

With SSH access to the second Linux machine, our new goal is to find our way into the remaining Windows hosts.

- Inexperienced or negligent developers and administrators frequently keep bad password management practices. Search for text files and scripts that might contain sensitive data, like passwords, keys, or hashes. 

**Hint**: With user-level access, it is a good idea to start by looking into that user's own files before expanding outward. 

- Find the hash that appears to be associated with an Administrator account on a Windows machine. 

## **Solution:**

### Findings in the target Machine: **authorized_keys file**


```bash linenums="1" hl_lines="5 9 12 14 15"
alice-devops@ubuntu22:~$ whoami
alice-devops
alice-devops@ubuntu22:~$ id
uid=1002(alice-devops) gid=1002(alice-devops) groups=1002(alice-devops)
alice-devops@ubuntu22:~$ pwd
/home/alice-devops
alice-devops@ubuntu22:~$ ll
ll: command not found
alice-devops@ubuntu22:~$ ls -a
.  ..  .bash_history  .cache  .config  .local  .ssh  scripts
alice-devops@ubuntu22:~$ cd .ssh
alice-devops@ubuntu22:~/.ssh$ ls -a
.  ..  authorized_keys
alice-devops@ubuntu22:~/.ssh$ cat authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCRJ7M/asVyWPNFMamvQaR56atrCnettKPq29yu9LvNbOnPV88WYpnuQDWf9MYxknmvIoG2GzAYx5LYO/JyK5D8u0wEVnbNKSmiHqYprIbxykmga7IIL5DNp+r+i3mytGGEYmndnhoRMRKQw5PTwYMdanHK/5j4q+dzaSFo/Lxpccb9rFBKg9REf15trLguDHlGyrZAiFf4+fD0ReD7FLdwi+R6sbiHtG6reOZ59NPmkybDitVHSXZJpQ1aNUu/O7CLpvzapIUtDHmGUhrPaZaJ45aKG1xwiJEebglz8RikeHAzEJ7LauOT9f2sCyQiDnnhQk+3kh1wIN2xrDNjZ89gY/OJjxCFD3kRVsetn1aVU1JDSna0ZPXvWxlb/IrdnXHSJSfKMfbF9lUtlgSxbTN08BrNxURZ/GFz7RM6RAW0tBLcgHTWlU2mY1jpCHhcLyvzer2VKc7RncRogOPhCS0ZhevaT0E58XjsAwPqPb/pdg5OubYahF06cGjW4Lfq5vc= root@ubuntu22
alice-devops@ubuntu22:~/.ssh$ 
```
### Findings in the target Machine: **windows-maintenance.sh script**

```bash linenums="1" hl_lines="16-43"
alice-devops@ubuntu22:~$ ls -a
.  ..  .bash_history  .cache  .config  .local  .ssh  scripts
alice-devops@ubuntu22:~$ cd .ssh
alice-devops@ubuntu22:~/.ssh$ ls -a
.  ..  authorized_keys
alice-devops@ubuntu22:~/.ssh$ cat authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCRJ7M/asVyWPNFMamvQaR56atrCnettKPq29yu9LvNbOnPV88WYpnuQDWf9MYxknmvIoG2GzAYx5LYO/JyK5D8u0wEVnbNKSmiHqYprIbxykmga7IIL5DNp+r+i3mytGGEYmndnhoRMRKQw5PTwYMdanHK/5j4q+dzaSFo/Lxpccb9rFBKg9REf15trLguDHlGyrZAiFf4+fD0ReD7FLdwi+R6sbiHtG6reOZ59NPmkybDitVHSXZJpQ1aNUu/O7CLpvzapIUtDHmGUhrPaZaJ45aKG1xwiJEebglz8RikeHAzEJ7LauOT9f2sCyQiDnnhQk+3kh1wIN2xrDNjZ89gY/OJjxCFD3kRVsetn1aVU1JDSna0ZPXvWxlb/IrdnXHSJSfKMfbF9lUtlgSxbTN08BrNxURZ/GFz7RM6RAW0tBLcgHTWlU2mY1jpCHhcLyvzer2VKc7RncRogOPhCS0ZhevaT0E58XjsAwPqPb/pdg5OubYahF06cGjW4Lfq5vc= root@ubuntu22
alice-devops@ubuntu22:~/.ssh$ ls
authorized_keys
alice-devops@ubuntu22:~/.ssh$ cd ..
alice-devops@ubuntu22:~$ ls
scripts
alice-devops@ubuntu22:~$ cd scripts/
alice-devops@ubuntu22:~/scripts$ ls -a
.  ..  windows-maintenance.sh
alice-devops@ubuntu22:~/scripts$ cat windows-maintenance.sh 
#!/usr/bin/bash

# This script will (eventually) log into Windows systems as the Administrator user and run system updates on them

# Note to self: The password field in this .sh script contains
# an MD5 hash of a password used to log into our Windows systems 
# as Administrator. I don't think anyone will crack it. - Alice 

username="Administrator"
password_hash="00bfc8c729f5d4d529a412b12c58ddd2"
# password="00bfc8c729f5d4d529a412b12c58ddd2"

#TODO: Figure out how to make this script log into Windows systems and update them

# Confirm the user knows the right password
echo "Enter the Administrator password"
read input_password
input_hash=`echo -n $input_password | md5sum | cut -d' ' -f1`

if [[ $input_hash == $password_hash ]]; then
        echo "The password for Administrator is correct."
else
        echo "The password for Administrator is incorrect. Please try again."
        exit
fi

#TODO: Figure out how to make this script log into Windows systems and update them
alice-devops@ubuntu22:~/scripts$ 
```

![alt text](images/Pasted%20image%2020240507233552.png)

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
<a href="/Career-Simulation-3/challenge_3/" class="md-button md-button--primary">Previous: Challenge 3</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/challenge_5/" class="md-button md-button--primary">Next: Challenge 5</a>
</div>