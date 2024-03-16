We want an automated way to try the common passwords or the entries from a word list; here comes THC Hydra. Hydra supports many protocols, including FTP, POP3, IMAP, SMTP, SSH, and all methods related to HTTP. The general command-line syntax is: hydra -l username -P wordlist.txt server service where we specify the following options:

```-l username```: -l should precede the username, i.e. the login name of the target. 
```-P wordlist.txt ```: -P precedes the wordlist.txt file, which is a text file containing the list of passwords you want to try with the provided username.
```server``` is the hostname or IP address of the target server.
```service``` indicates the service which you are trying to launch the dictionary attack.
Consider the following concrete examples:

```hydra -l mark -P /usr/share/wordlists/rockyou.txt 10.10.28.143 ftp ```will use mark as the username as it iterates over the provided passwords against the FTP server.
```hydra -l mark -P /usr/share/wordlists/rockyou.txt ftp://10.10.28.143 ``` is identical to the previous example. 10.10.28.143 ftp is the same as ftp://10.10.28.143.
```hydra -l frank -P /usr/share/wordlists/rockyou.txt 10.10.28.143 ssh```  will use frank as the user name as it tries to login via SSH using the different passwords.
