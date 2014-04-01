####Instructions

The MD5 Message-Digest Algorithm is a widely used cryptographic hash function that produces a 128-bit (16-byte) hash value. MD5 has been utilized in a wide variety of security applications, and is also commonly used to check data integrity. Once again, the MD5 hash dump has been provided for you, but this is normally something you would need to do on your own before beginning the cracking process.

1. Navigate to the linux directory located at `/root/THA/password_lab/linux` and open the file `md5.txt` in your favorite text editor.
2. Take a moment to look at the structure of the md5 hashes.
3. In a terminal window, navigate to `/pentest/passwords/john` directory.
4. We are going to use John once again to crack the hashes using a word list, but this time we are cracking md5 hashes.
6. Enter the following command:
```
./john  --format=raw-md5  --pot=/root/THA/password_lab/linux/md5.pot --wordlist=/root/THA/words.txt  /root/THA/password_lab/linux/md5.txt
```
7. John should finish fairly fast. To see what you were able to crack, enter the following command:
```
./john  --show --format=raw-md5  --pot=/root/THA/password_lab/linux/md5.pot /root/THA/password_lab/linux/md5.txt
```
8. John should have cracked around 100 users. By now you should be very familiar with the syntax required to run john, so this time run john using the darkcode wordlist and check your results.

This concludes the lab exercise.
