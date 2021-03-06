####Instructions

LM hashes are generated by a weak hashing function that is primarily used with Microsoft LAN Manager and in Microsoft Windows versions prior to Windows 2000. Support for the legacy LAN Manager protocol continued in later versions of Windows for backward compatibility, but it was recommended by Microsoft to be turned off by administrators; as of Windows Vista, the protocol is disabled by default, but continues to be used by some non-Microsoft CIFS implementations. The three main faults with LM hashes are that passwords are limited to a maximum of 14 characters and passwords longer than 7 characters are divided into two pieces and each piece is hashed separately. This means that if a password is 10 characters long, it is only really as strong as a 7-character password, since cracking the 3 extra characters would be fairly trivial. The final fault is that the LM hashing algorithm converts all passwords to uppercase letters.

1. First, pull up your Kali VM (or Backtrack), open a terminal window and navigate to `/root/THA/password_lab/winxp`.

2. Open the file `xp_hashes.txt` with your favorite text editor and look over the contents of this file. This file was generated using a program called **fgdump**, which is a popular program used for dumping password hashes. This was done for you, but do note that this is a step you would normally have to perform yourself before starting the cracking process.

3. Notice that the format for LM hashes is: `username:hash1:hash2:::`. The first hash is our LM hash. The second hash is the NTLM hash. For this exercise, we are focusing on the LM hashes. Note that if a password exceeds 14 characters, there is no LM hash for it which explains why some of the hashes display `NO PASSWORD*********************`.

4. Open a new terminal window and type the following command to navigate to the John The Ripper directory within Kali (or Backtrack):
```bash
cd /pentest/passwords/john
```

5. In the same terminal, enter the following command to start the cracking process:
```bash
./john --wordlist=/root/THA/words.txt --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
This command starts john using a supplied word list, words.txt. It then writes the results to a .pot file called `winxp.pot`. The last part of the command specifies which hash file to target. John should then begin trying to crack the LM hashes. Since this is a small word list, and most of these passwords were in that word list, this should complete quite fast.

7. John will display the username and password combinations it was able to find. To view our winxpLM.pot file, type the following command:
```bash
./john --show --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
This command tells john to display the results of our crack. The last part of the command lets John know the hash file that was used to generate that .pot file. John was able to crack a good portion of the user accounts using this word list. However, this word list is quite small and many of the user accounts were not cracked because of this.
Lets use another password list within Kali/Backtrack to try cracking a few more passwords. To do this enter the following command:
```bash
./john --wordlist=/pentest/passwords/wordlists/darkc0de.lst --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
8. This is similar to our previous command, but we are using the darkc0de word list, which is much larger than words.txt. To display what we have now cracked, type the following command:
```bash
./john --show --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
9. Notice we have cracked several more passwords using this additional wordlist. To show the passwords we have left to crack execute the following command:
```bash
./john --show --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
10. John has several rule definitions built into the john.conf file that allows us to basically managle wordlist files and do things like character substitutions. To use the default wordlist rule set execute the following command:
```bash
./john --rules --wordlist=/root/THA/words.txt --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
Notice we only added the --rules argument to the previous words.txt word list and were able to crack a few additional passwords.
11. Lets run this same command, but this time use the darkcode word list by executing the following command:
```bash
./john --show=left --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
12. To view the passwords we have successfully cracked so far execute the following command:
```bash
./john --show --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt 
```
13. To view the passwords still remaining to be cracked execute the following command:
```bash
./john --show=LEFT --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
