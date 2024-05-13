# **Challenge 5: Password Cracking**
!!! note ""
## **Procedure:**

With a password hash in our hands, we need to crack it to discover the actual password. 

- With any means you prefer, crack the MD5 hash you found to reveal the original password. 
    - You can crack the hash using existing tools installed in Kali. (Remember the available wordlists in /usr/share/wordlists!)
    - You could write a Python script to crack it using the same wordlists as other tools.
    - You can check third-party hash databases to see if it is a known hash.

## **Solution:**

### Using **[John the Ripper](https://en.wikipedia.org/wiki/John_the_Ripper)** to the crack password.

#### hash.txt file created containing the password (`Administrator:00bfc8c729f5d4d529a412b12c58ddd2`)

```bash linenums="1" hl_lines="2 12"
┌──(kali㉿kali)-[~]
└─$ sudo john --format=raw-md5 hash.txt

Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 512/512 AVX512BW 16x3])
Warning: no OpenMP support for this hash type, consider --fork=2
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 44 candidates buffered for the current salt, minimum 48 needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst
pokemon          (Administrator)     
1g 0:00:00:00 DONE 2/3 (2024-05-08 04:06) 16.66g/s 53966p/s 53966c/s 53966C/s keller..karla
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 
```
### Using **[CrackStation](https://crackstation.net/)** to crack the password

![alt text](images/Pasted%20image%2020240508001032.png)

### Creating a **python script** to crack the password

```python linenums="1" hl_lines="4 27"
import hashlib

# The target hash from the password we want to crack
target_hash = "00bfc8c729f5d4d529a412b12c58ddd2"

# Function to read passwords from a file
def read_passwords_from_file(filename):
    with open(filename, 'r') as file:
        return [line.strip() for line in file]

# Function to attempt to crack the hash using passwords from a file
def crack_password(target_hash, password_file):
    # Read the passwords from the file
    password_list = read_passwords_from_file(password_file)
    
    for password in password_list:
        # Generate the MD5 hash of the current password
        hash_object = hashlib.md5(password.encode())
        hashed_password = hash_object.hexdigest()

        # Check if this hash matches the target hash
        if hashed_password == target_hash:
            return password
    return None

# File with the list of potential passwords
password_file = '10-million-password-list-top-1000000.txt'


cracked_password = crack_password(target_hash, password_file)

if cracked_password:
    print(f"Password cracked: {cracked_password}")
else:
    print("Password not found in the provided list.")
```
### Running the script in kali ([Password list](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10-million-password-list-top-1000000.txt))

```zsh linenums="1" hl_lines="18"
┌──(kali㉿kali)-[~]
└─$ cd DCV-Storage
                                                                                                                              
┌──(kali㉿kali)-[~/DCV-Storage]
└─$ ll
total 8332
-rw-r--r-- 1 kali kali 8529108 May  8 20:24 10-million-password-list-top-1000000.txt
                                                                                                                              
┌──(kali㉿kali)-[~/DCV-Storage]
└─$ nano crack_password.py
                                                                                                                              
                                                                                                                              
┌──(kali㉿kali)-[~/DCV-Storage]
└─$ chmod +x crack_password.py
                                                                                                                              
┌──(kali㉿kali)-[~/DCV-Storage]
└─$ python3 crack_password.py
Password cracked: pokemon
```


## The password is **pokemon**

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
<a href="/Career-Simulation-3/challenge_4/" class="md-button md-button--primary">Previous: Challenge 4</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/challenge_6/" class="md-button md-button--primary">Next: Challenge 6</a>
</div>