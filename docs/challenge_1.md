# **Challenge 1: Network Scanning**
!!! note ""
## **Procedure:**

The first step is always reconnaissance. We need to identify all of the relevant targets in our network and find out what they're running.

- Use Nmap to run a basic scan on the subnet your Kali machine is connected to. You should find four hosts in your results, not including your own Kali machine.
  - **Hint:** Where can you find the subnet range of your Kali VM?
  - **Reminder:** Running Nmap with root privileges might give you a lot more results than intended, showing hosts that are online but with no open ports. Re-run the scan as a normal user (without sudo) or ignore the results with no ports open.
- Next, run service and version detection scans on the specific IP addresses found in your first scan. Scan for services beginning at port 1 and ending at port 5000.
  - **Hint:** How do you perform a service and version scan in Nmap?
  - **Hint:** How do you specify a range of ports to scan in Nmap?
  - **Reminder:** You can scan all four IP addresses in a single command, or scan them each individually. Remember that the more you do in one scan, the longer it will take.

Interpret your results and determine the following:
- Which host is running a web server on a non-standard port? What port is it running on?
- Which host is running an SSH server on a non-standard port? What port is it running on?
- Which machines are running Windows-based operating systems?

[Next](challenge_2.md){ .md-button .md-button--primary}