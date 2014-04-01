####Instructions

Lots of thought has gone into the modern flavors of crypt. The UNIX family of operating systems have been using crypt since 1976 and the algorithms used by crypt have been constantly improved over time to keep up with improvements in password cracking. It provides stretching, salting and defeats parallel cracking by using a unique salt for each password, which is then automatically added to the hash. In this lab, you'll be using a file, which was dumped using the unshadow feature of John The Ripper. Unshadow is a command in john that combines the "passwd" file and the "shadow" file for use with John. This has been done for you, but note that normally this is something you would need to do before beginning the cracking process.

1. Navigate to `/root/THA/password_lab/linux` and open the file `unshadow.txt` in your favorite text editor. Take a moment to look over this file so you can see the difference between this type of hash and the LM and NTLM that we just looked at.
2. In a terminal window, navigate to the directory `/pentest/passwords/john`.
3. Enter the following command:
```
./john  --wordlist=/root/THA/words.txt --pot=/root/THA/password_lab/linux/crypt.pot /root/THA/password_lab/linux/unshadow.txt
```
4. John will begin cracking the crypt password hashes. Note that this will be much slower than the LM hashes were because computing crypt hashes is more complex.
6. Once John finishes, enter the following command to see what you have discovered:
```
./john  --show  --pot=/root/THA/password_lab/linux/crypt.pot /root/THA/password_lab/linux/unshadow.txt
```
You should have cracked somewhere around 100 user accounts. However, just like in the previous lab, many interesting looking user accounts were not cracked.
7. Use the darkc0de.lst word list to attempt to crack some additional passwords. You can do this by executing the following command:
```
./john  --sessi   --format=crypt --wordlist=/pentest/passwords/wordlists/darkc0de.lst --pot=/root/THA/password_lab/linux/crypt.pot /root/THA/password_lab/linux/unshadow.txt
```
8. Nothing here should be new to you by now except the session option. The `--session` option does not require a name; however naming it is best practices as you may have multiple sessions going. Including this session command allows us to stop John and resume it at a later time, even if we have to restart the system.
9. Stop your cracking session by pressing "Ctrl+C".
10. Enter the following command:
```
./john --resume=crypt
```
11. Your session will resume where it left off. This is of immense value as some cracking sessions can take days, or even weeks depending on the complexity of the hashes and the size of the word list used.
12. Also notice that while John is running, you can press any key to get a status of your session. It depends on your setup as to how long it will take to finish this cracking session, but you will probably just want to let this session run in the background for several hours, or overnight.
13. Once John finishes, view your results by entering the following command:
```
./john --show --pot=/root/THA/password_lab/linux/crypt.pot --format=crypt /root/THA/password_lab/linux/unshadow.txt
```

This concludes the lab exercise.
