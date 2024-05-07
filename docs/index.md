# The Penetration Testing Problem

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

```python
with open('problem2.txt', 'r') as file:
    count = 0
    for line in file:
        if line.strip() == '192.168.1.1':
            count += 1
print(f"The IP 192.168.1.1 appears {count} times.")
# The IP 192.168.1.1 appears 5 times.
```