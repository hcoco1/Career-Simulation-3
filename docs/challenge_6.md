# **Challenge 6: Passing the Hash**
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

[Home 🏠](index.md){ .md-button .md-button--secondary } [Previus: Challenge 5](challenge_5.md){ .md-button .md-button--primary } [Next: Challenge 7](challenge_7.md){ .md-button .md-button--primary }

