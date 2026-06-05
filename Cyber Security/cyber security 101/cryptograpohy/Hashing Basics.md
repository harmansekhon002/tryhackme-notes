hahsA hash value is a fixed size string or characters that is composed by a hash function 

A hash function takes an input of an arbitrary size and returns an output of fixed length i.e. a hash value 

## Hashing Functions

A hash function takes some input data of any size and creates a summary or **digest** of that data

 Good hashing algorithms will be relatively fast to compute and prohibitively slow to reverse, i.e., go from the output and determine the input. Any slight change in the input data, even a single bit, should cause a significant change in the output.

The output of a hash function is typically raw bytes, which are then encoded. Common encodings are base64 or hexadecimal. 

`md5sum`, `sha1sum`, `sha256sum`, and `sha512sum`
produce their outputs in hexadecimal format

### importance of hashing

 When you log into TryHackMe, the server uses hashing to verify your password. 
 In fact, as per good security practices, a server does not record your password; it records the hash value of your password. 
 Whenever you want to log in, it will calculate the hash value of the password you submitted with the recorded hash value.
 Similarly, when you log into your computer, hashing plays a role in verifying your password.
 You interact more indirectly with hashing than you would think, and almost daily in the context of passwords.

## Hash Collision

Two different inputs but same output is known as hash collision

If you have 21 pigeons and 16 pigeonholes, some of the pigeons are going to share the pigeonholes. Consequently, collisions are unavoidable. However, a good hash function ensures that the probability of a collision is negligible.

## Insecure password storage for authentication

We will visit **three insecure practices** when it comes to passwords:

- Storing passwords in plaintext
- Storing passwords using a deprecated encryption
- Storing passwords using an insecure hashing algorithm

**Password salting** refers to adding a **salt**, i.e., a random value, to the password before it is hashed.
## Hashing for password storage

A **Rainbow Table** is a lookup table of hashes to plaintexts, so you can quickly find out what password a user had just from the hash. A rainbow table trades the time to crack a hash for hard disk space, but it takes time to create. Here’s a quick example to get an idea of what a rainbow table looks like.

|Hash|Password|
|---|---|
|02c75fb22c75b23dc963c7eb91a062cc|zxcvbnm|
|b0baee9d279d34fa1dfd71aadb908c3f|11111|
|c44a471bd78cc6c2fea32b9fe028d30a|asdfghjkl|
|d0199f51d2728db6011945145a1b607a|basketball|
|dcddb75469b4b4875094e14561e573d8|000000|
|e10adc3949ba59abbe56e057f20f883e|123456|
|e19d5cd5af0378da05f63f891c7467af|abcd1234|
|e99a18c428cb38d5f260853678922e03|abc123|
|fcea920f7412b5da7be0cf42b8c93759|1234567|
|4c5923b6a6fac7b7355f53bfe2b8f8c1|inS3CyourP4$$|

Websites like [CrackStation(opens in new tab)](https://crackstation.net/) and [Hashes.com(opens in new tab)](https://hashes.com/en/decrypt/hash) internally use massive rainbow tables to provide fast password cracking for **hashes without salts**.

## Salt

The salt is a randomly generated value stored at the starting or the end of a password before the hashing to change the hash value even if two passes are the same

each user's password must be given a unique salt

Hash functions like Bcrypt and Scrypt handle this automatically. Salts don’t need to be kept private.

### examples of secure password storage

Consider this example following good security practices when storing user passwords:

1. We select a secure hashing function, such as Argon2, Scrypt, Bcrypt, or PBKDF2.
2. We add a unique salt to the password, such as `Y4UV*^(=go_!`
3. Concatenate the password with the unique salt. For example, if the password is `AL4RMc10k`, the result string would be `AL4RMc10kY4UV*^(=go_!`
4. Calculate the hash value of the combined password and salt. In this example, using the chosen algorithm, you need to calculate the hash value of `AL4RMc10kY4UV*^(=go_!`.
5. Store the hash value and the unique salt used (`Y4UV*^(=go_!`)

## Recognising password hashes

Mostly hashes are stored in MD5 instead of NTLM
Tools like hashID exist but are unreliable 

### Linux passwords

On Linux, password hashes are stored in `/etc/shadow`, which is normally only readable by root. They used to be stored in `/etc/passwd`, which was readable by everyone.
The file contains nine fields some of them for hashing are
prefix options salt hash
prefix can be used to determine the type of the system linux or unix
It specifies the hashing algorithm

Common unix prefixes

|Prefix|Algorithm|
|---|---|
|`$y$`|yescrypt is a scalable hashing scheme and is the default and recommended choice in new systems|
|`$gy$`|gost-yescrypt uses the GOST R 34.11-2012 hash function and the yescrypt hashing method|
|`$7$`|scrypt is a password-based key derivation function|
|`$2b$`, `$2y$`, `$2a$`, `$2x$`|bcrypt is a hash based on the Blowfish block cipher originally developed for OpenBSD but supported on a recent version of FreeBSD, NetBSD, Solaris 10 and newer, and several Linux distributions|
|`$6$`|sha512crypt is a hash based on SHA-2 with 512-bit output originally developed for GNU libc and commonly used on (older) Linux systems|
|`$md5`|SunMD5 is a hash based on the MD5 algorithm originally developed for Solaris|
|`$1$`|md5crypt is a hash based on the MD5 algorithm originally developed for FreeBSD|
#### Modern Linux Example

Consider the following line from a modern Linux system’s `shadow` password file.

Terminal

```shell-session
root@TryHackMe# sudo cat /etc/shadow | grep strategos
strategos:$y$j9T$76UzfgEM5PnymhQ7TlJey1$/OOSg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4:19965:0:99999:7:::
```

The fields are separated by colons. The important ones are the username and the hash algorithm, salt, and hash value. The second field has the format `$prefix$options$salt$hash`.

In the example above, we have four parts separated by `$`:

- `y` indicates the hash algorithm used, **yescrypt**
- `j9T` is a parameter passed to the algorithm
- `76UzfgEM5PnymhQ7TlJey1` is the salt used
- `/OOSg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4` is the hash value
### MS Windows Passwords

MS Windows passwords are hashed using NTLM, a variant of MD4. They’re visually identical to MD4 and MD5 hashes, so it’s very important to use context to determine the hash type.

On MS Windows, password hashes are stored in the SAM (Security Accounts Manager). MS Windows tries to prevent normal users from dumping them, but tools like mimikatz exist to circumvent MS Windows security. Notably, the hashes found there are split into NT hashes and LM hashes.

A great place to find more hash formats and password prefixes is the [Hashcat Example Hashes(opens in new tab)](https://hashcat.net/wiki/doku.php?id=example_hashes) page. For other hash types, you’ll typically need to check the length or encoding or even conduct some research into the application that generated them. Never underestimate the power of research.

## Password Cracking

You can use a graphics card to crack many hash types quickly. Some hashing algorithms, such as Bcrypt, are designed so that hashing on a GPU does not provide any speed improvement over using a CPU; this helps them resist cracking.

## Cracking on VM's

When you need to crack a hash every CPU cycle is important

- [Hashcat](chatgpt://generic-entity?number=0) runs **much faster on your host machine** because it can use your **GPU** 
- Running it inside a **VM (Virtual Machine)** usually **reduces speed** since GPU access is limited
- If you’re using [Microsoft Windows](chatgpt://generic-entity?number=1), you can download the Windows build and run it via [PowerShell](chatgpt://generic-entity?number=2)
- You _can_ use [OpenCL](chatgpt://generic-entity?number=3) in a VM, but cracking will be **slower than on host**

[John the Ripper(opens in new tab)](https://www.openwall.com/john/) uses CPU by default and works in a VM out of the box, although you may get better speeds running it on the host OS to avoid any virtualisation overhead and make the most of your CPU cores and threads.

## Hashcat

Hashcat uses the following basic syntax: `hashcat -m <hash_type> -a <attack_mode> hashfile wordlist`, where:

- `-m <hash_type>` specifies the hash-type in numeric format. For example, `-m 1000` is for NTLM. Check the official documentation (`man hashcat`) and [example page(opens in new tab)](https://hashcat.net/wiki/doku.php?id=example_hashes) to find the hash type code to use.
- `-a <attack_mode>` specifies the attack-mode. For example, `-a 0` is for straight, i.e., trying one password from the wordlist after the other.
- `hashfile` is the file containing the hash you want to crack.
- `wordlist` is the security word list you want to use in your attack.

For example, `hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt` will treat the hash as Bcrypt and try the passwords in the `rockyou.txt` file.has
## Hashing for integrity checking 

### Integrity checking 

if two files have the same hash on the web server and your local computer then it is the same file that you downloaded from the internet 

if two files have the same hash then those files are duplicates and can be used to clear the storage
### HMAC

**HMAC (Keyed-Hash Message Authentication Code)** is a type of message authentication code (MAC) that uses a cryptographic hash function in combination with a secret key to verify the authenticity and integrity of data.

The following steps give you a fair idea of how HMAC works.

1. The secret key is padded to the block size of the hash function.
2. The padded key is XORed with a constant (usually a block of zeros or ones).
3. The message is hashed using the hash function with the XORed key.
4. The result from Step 3 is then hashed again with the same hash function but using the padded key XORed with another constant.
5. The final output is the HMAC value, typically a fixed-size string.

![A visual representation of the HMAC function.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1725294564965.svg)  

Technically speaking, the HMAC function is calculated using the following expression:

_H__M__A__C_(_K_,_M_) = _H_((_K_⊕_o__p__a__d_)||_H_((_K_⊕_i__p__a__d_)||_M_))

Note that M and K represent the message and the key, respectively.

## conclusion

