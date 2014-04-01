####Instructions

Microsoft attempted to address the shortcomings of LM with NTLM. NTLM is case sensitive, its character set is 65,535, and it does not limit stored passwords to two 7-character parts. NTLM is considered a much stronger hashing algorithm. In this exercise, we will be using the cracked passwords we obtained from the LM hashes to create a new word list to crack the NTLM Hashes. Note that this is a very common password cracking optimization technique used by penetration testers. It is much faster to crack LM password hashes and then use those discovered passwords to crack the corresponding NTLM hash. Remember we may not be able to authenticate to a host using the LM password, but we if we crack the corresponding NTLM hash and use it.

1. Our first task is to use our .pot files to create a new word list using the LM passwords we discovered in the previous exercise. We will use a little bash-fu scripting to parse the `--show` command within john to create a new wordlist. To do this execute the following command:
```
./john --show --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt | awk -F: '{print $2}' | grep -v "^$" > /root/THA/password_lab/winxp/lmwordlist.txt
```
Note we are piping the show command into awk to print the second field separated by the “:” character. Then this is piped in to grep to print all non-blank lines and redirected to our new file lmwordlist.txt in the /root/THA/password_lab/winxp directory.

2. Since the actual characters of the passwords were already cracked in the LM exercise, all John has to do is decide if a character is upper or lower case. This is far more efficient than just using the original word lists again. Now we can start cracking the NTLM hashes with the following command:
```
./john --rules=NT --format=nt --wordlist=/root/THA/password_lab/winxp/lmwordlist.txt --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt 
```
3. Unlike the first exercise we just went ahead and used the **--rules** parameter. The real secret here is the --rules section we used. The NT rule basically just modifies the case for every letter found in the wordlist incrementally till all combinations have been permutated. Feel free to explore the john.conf file to better understand this.
To show the results for the NTLM passwords we have cracked execute the following command:
```./john --show --format=nt  --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```
Notice that since we have now cracked two different formats with john using the same pot file, we need to specify the format of the hashes we would like john to output results for. John can only operate on a single hash type per running instance, so if a file consists of multiple hash types you need to tell john which one you would like to use.

4. To show the accounts we still have left to crack run the following command:
```
./john --show=LEFT --format=nt  --pot=/root/THA/password_lab/winxp/winxp.pot /root/THA/password_lab/winxp/xp_hashes.txt
```

This concludes the lab exercise.
