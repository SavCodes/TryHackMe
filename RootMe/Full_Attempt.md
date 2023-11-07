

### Room: RootMe
### Target IP: 10.10.57.6


## Task 1: Deploy the machine
#### Task 1.a: Deploy the machine
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

`gobuster dir -u http://10.10.57.6 -w /usr/share/wordlists/dirb/common.txt | tee web_scan.txt`

This yields the hidden directory: panel

We also find the interesting directory `/uploads`

#### Task 2.a: Scan the machine how many ports are open?
`Task 2.a Answer: 2`

#### Task 2.b: What version of Apache is running?
`Task 2.b Answer: 2.4.29`

#### Task 2.c: What service is running on port 22?
`Task 2.c Answer: SSH`

#### Task 2.d: Find directories on the web server using the GoBuster tool.
`Task 2.d Answer: No answer needed`

#### Task 2.e: What is the hidden directory?
`Task 2.e Answer: /panel/`

## Task 3: Getting a shell
"Find a form to upload and get a reverse shell, and find the flag."

Getting tipped off from `Task 2.e`, I navigate to the hidden directory `/panel/` and see it is an upload form. 
I try to upload a basic php reverse shell from pentest monkey. 

The upload fails, as it seems there is a filter in place preventing `php` file from being uploaded. I check the
page source code to see if it is a simple client-side filter that can easily be disabled, but this is not the case.

To see if there are any php-like file types that bypass the filter, I intercept the upload request using burpesuite.
The request is forwarded to intruder, where the file extension is bruteforced using a list php-like extensions.

Checking the intruder response `Length` for all file extensions tested and comparing them to the php extension response length.
tells us which extensions will bypass the filter. We determine that `.php3`, `.php4`, `.php5`, `phtml`, and `.inc` are all accepted file extensions
and have been successfully uploaded

Navigating to `/uploads` I can see the all files that successfully bypassed the php filter. To catch my shell I
start a `netcat` listener on the port assigned in the php shell 

`nc -nlvp 1234`

Upon opening the malicious php file I receive my reverse shell connection. To find the flag quickly I use the find command
and redirect errors into /dev/null

This could only be done because the file name we are looking for is provided in the question.

`find / -name user.txt 2>/dev/null`

The results show the file path is `/var/www/user.txt` and the contents are displayed using `cat 

#### Task 3.a: user.txt
`Task 3.a Answer: THM{y0u_g0t_a_sh3ll}`



