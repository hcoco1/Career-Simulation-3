# **Challenge 2: Initial Compromise**
!!! note ""
## **Procedure:**

Next, we need to find our initial compromise vector. Servers hosting openly accessible services, like websites and unsecured databases, are great places to start.

- Access the site hosted on the webserver you found in the previous step.
  - **Hint:** How do you access a website on a custom port number?
- Explore the web pages available to you. What would be a good place to attempt some attacks?
  - **Hint:** Your first goal should be to test anything that handles user input.
- Demonstrate you can run commands on the target system by running the `whoami` command.

## **Solution:**

- Access the site hosted on the webserver you found in the previous step.

```python linenums="1"
┌──(kali㉿kali)-[~]
└─$ firefox 172.31.40.22:1013
```

![alt text](images/Pasted%20image%2020240507225701.png)

- Accesing the Apache server.


![alt text](images/Pasted%20image%2020240507225721.png)

- The input allow to perform a SQL injection Attack


![alt text](images/Pasted%20image%2020240507225931.png)