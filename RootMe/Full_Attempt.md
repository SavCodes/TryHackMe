

### Room: RootMe
### Target IP: 10.10.57.6


## Task 1: Deploy the machine
### Task 1.a: Deploy the machine
`Task 1.a Answer: No answer needed`

## Task 2: Reconnaissance 
Looking at first three questions in `Task 2`, we must find how many ports are open and what versions
of their respective service they are running. To answer, I will run some basic nmap enumeration and tee
the results to a new file `version_scan.txt`.

`nmap 10.10.57.6 -sV -v | tee version_scan.txt`

From the nmap results we can see two services running: 
  PORT: 22 => SSH OpenSSH 7.6p1 Ubuntu 4ubuntu0.3
  PORT: 80 => HTTP Apache httpd 2.4.29

Questions three and four require directory enumeration with gobuster. I'll be enumerating with a kali
default directory wordlist from dirb `common.txt`

`gobuster dir -u http://10.10.57.6 -w /usr/share/wordlists/dirb/common.txt`

This yields the hidden directory: panel

### Task 2.a: Scan the machine how many ports are open?
`Task 2.a Answer: 2`

### Task 2.b: What version of Apache is running?
`Task 2.b Answer: 2.4.29`

### Task 2.c: What service is running on port 22?
`Task 2.c Answer: SSH`

### Task 2.d: Find directories on the web server using the GoBuster tool.
`Task 2.d Answer: No answer needed`

### Task 2.e: What is the hidden directory?
`Task 2.e Answer: /panel/`
