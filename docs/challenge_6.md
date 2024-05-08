# **Challenge 6: Metasploit**
!!! note ""
## **Procedure:**

```python linenums="1"
with open('problem2.txt', 'r') as file:
    count = 0
    for line in file:
        if line.strip() == '192.168.1.1':
            count += 1
print(f"The IP 192.168.1.1 appears {count} times.")
# The IP 192.168.1.1 appears 5 times.
```


```python linenums="1" hl_lines="2 3"
with open('problem2.txt', 'r') as file:
    count = 0
    for line in file:
        if line.strip() == '192.168.1.1':
            count += 1
print(f"The IP 192.168.1.1 appears {count} times.")
# The IP 192.168.1.1 appears 5 times.
```

!!! note ""

<div class="button-container" markdown="1">
<a href="/Career-Simulation-3/challenge_5/" class="md-button md-button--primary">Previous: Challenge 5</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/challenge_7/" class="md-button md-button--primary">Next: Challenge 7</a>
</div>

