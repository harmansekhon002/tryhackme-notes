- **Authentication**: You want to be sure you communicate with the right person, not someone else pretending.
- **Authenticity**: You can verify that the information comes from the claimed source.
- **Integrity**: You must ensure that no one changes the data you exchange.
- **Confidentiality**: You want to prevent an unauthorised party from eavesdropping on your conversations.

	Cryptography can provide solutions to satisfy the above requirements, among many others

## Analogy of asymmetric cryptography

- Lets say you wanna share a key of a lock without anyone intercepting it 
- you would create two keys one for encryption and one for decryption 
- You will keep the decryption or the private key and share the encryption or the public key 
- anyone including you can encrypt the message or put the lock on the message but only you with the private key and open the box or read the message due the decryption key

# RSA Encryption

## Core Idea

Security relies on one asymmetry: **multiplying two primes is easy; factoring the result back is practically impossible.**

---

## Essential Concepts

|Concept|Meaning|
|---|---|
|`a mod b`|Remainder of a ÷ b. e.g. 61777 mod 30888 = **1**|
|GCD(a,b)|Largest number dividing both evenly. GCD(12,8) = **4**|
|Coprime|GCD = 1 — no shared factors. GCD(163, 30888) = **1** ✓|
|ϕ(n)|Count of numbers < n coprime with n. For RSA: ϕ(n) = (p−1)(q−1)|

---

## Steps

**1. Pick two primes** $$p = 157,\quad q = 199,\quad n = p \times q = 31{,}243$$

**2. Compute totient (secret)** $$\phi(n) = 156 \times 198 = 30{,}888$$

**3. Choose public exponent `e`** Pick any e where `GCD(e, ϕ(n)) = 1`. Bob chose **e = 163** (a prime, so automatically coprime with most numbers).

**4. Find private exponent `d`** Solve: $e \times d \equiv 1 \pmod{\phi(n)}$ $$163 \times 379 = 61{,}777 \quad \rightarrow \quad 61{,}777 \mod 30{,}888 = 1 \checkmark$$ So **d = 379**. Found via the Extended Euclidean Algorithm — not guessed.

**5. Keys**

|Key|Value|Shared?|
|---|---|---|
|Public|(n=31243, e=163)|✓ Everyone|
|Private|(n=31243, d=379)|✗ Bob only|

**6. Encrypt** (Alice, using public key) $$y = x^e \mod n = 13^{163} \mod 31243 = 16{,}341$$

**7. Decrypt** (Bob, using private key) $$x = y^d \mod n = 16341^{379} \mod 31243 = 13 \checkmark$$

---

## Why It Works

Because $e \times d \equiv 1 \pmod{\phi(n)}$, Euler's theorem guarantees: $$(x^e)^d \equiv x \pmod{n}$$ The two operations cancel perfectly.

---

## Why It's Secure

To crack RSA an attacker needs `d` → needs ϕ(n) → needs p and q → must **factor n**. Factoring a 2048-bit number takes billions of years. That's it.

> Real RSA uses 2048–4096 bit keys. This example uses small numbers for illustration only.

---

## Analogy

> Bob leaves an open padlock in public. Alice locks her message with it. Only Bob's private key opens it.

---

In cryptography, RSA stands for **Rivest–Shamir–Adleman**, named after its inventors Ron Rivest, Adi Shamir, and Leonard Adleman.

### RSA in Capture the flag

The math behind RSA comes up relatively often in CTFs, requiring you to calculate variables or break some encryption based on them. Many good articles online explain RSA, and they will give you almost all of the information you need to complete the challenges. One good example of an RSA CTF challenge is the [Breaking RSA](https://tryhackme.com/r/room/breakrsa) room.

There are some excellent tools for defeating RSA challenges in CTFs. My favourite is [RsaCtfTool(opens in new tab)](https://github.com/Ganapati/RsaCtfTool), which has worked well for me. I’ve also had some success with [rsatool(opens in new tab)](https://github.com/ius/rsatool).

You need to know the main variables for RSA in CTFs: p, q, m, n, e, d, and c. As per our numerical example:

- p and q are large prime numbers
- n is the product of p and q
- The public key is n and e
- The private key is n and d
- m is used to represent the original message, i.e., plaintext
- c represents the encrypted text, i.e., ciphertext

Crypto CTF challenges often present you with a set of these values, and you need to break the encryption and decrypt a message to retrieve the flag.

## Diffie-Hellman key exchange

- Lets say Bob and Alice have a secret to share 
- Bob has a and alice has b and they have a common secret c
- so they will first combine each of their secrets with the common secret and then they will send it to each other 
- then they will combine the other part which they recieved to their own and each will have the same secret key for communication
 
 >A becomes AC 
 >B becomes BC
 >then after exchange 
 >ABC and ABC 
 
 1. Alice and Bob agree on the **public variables**: a large prime number _p_ and a generator _g_, where 0 < _g_ < _p_. These values will be disclosed publicly over the communication channel. Although insecurely small, we will choose _p_ = 29 and _g_ = 3 to simplify our calculations.
2. Each party chooses a private integer. As a numerical example, Alice chooses _a_ = 13, and Bob chooses _b_ = 15. Each of these values represents a **private key** and must not be disclosed.
3. It is time for each party to calculate their **public key** using their private key from step 2 and the agreed-upon public variables from step 1. Alice calculates _A_ = _g__a_ mod _p_ = 313 mod 29 = 19 and Bob calculates _B_ = _g__b_ mod _p_ = 315 mod 29 = 26. These are the public keys.
4. Alice and Bob send the keys to each other. Bob receives _A_ = _g__a_ mod _p_ = 19, i.e., Alice’s public key. And Alice receives _B_ = _g__b_ mod _p_ = 26, i.e., Bob’s public key. This step is called the **key exchange**.
5. Alice and Bob can finally calculate the **shared secret** using the received public key and their own private key. Alice calculates _B__a_ mod _p_ = 2613 mod 29 = 10 and Bob calculates _A__b_ mod _p_ = 1915 mod 29 = 10. Both calculations yield the same result, _g__a__b_ mod _p_ = 10, the shared secret key.
## SSH in cryptography

If a public key is not recognised by the SSH it will issue a warning and ask us if we still want to continue 
this happens probably because a man in the middle is trying to intercept the connection

### Authentication of the client

we may use passwords or someday we might encounter a machine with key authentication

By default, SSH keys are RSA keys. You can choose which algorithm to generate and add a passphrase to encrypt the SSH key.

`ssh-keygen` is the program usually used to generate key pairs. It supports various algorithms, as shown on its manual page below.

Terminal

```shell-session
root@TryHackMe# man ssh-keygen
[...]
-t dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk | rsa
Specifies the type of key to create. The possible values are “dsa”, “ecdsa”, “ecdsa-sk”, “ed25519”, “ed25519-sk”, or “rsa”.
[...]
```

The following is just for your information. At this stage, we recommend that you recognise their names only.

- **DSA (Digital Signature Algorithm)** is a public-key cryptography algorithm specifically designed for digital signatures.
- **ECDSA (Elliptic Curve Digital Signature Algorithm)** is a variant of DSA that uses elliptic curve cryptography to provide smaller key sizes for equivalent security.
- **ECDSA-SK (ECDSA with Security Key)** is an extension of ECDSA. It incorporates hardware-based security keys for enhanced private key protection.
- **Ed25519** is a public-key signature system using EdDSA (Edwards-curve Digital Signature Algorithm) with Curve25519.
- **Ed25519-SK (Ed25519 with Security Key)** is a variant of Ed25519. Similar to ECDSA-SK, it uses a hardware-based security key for improved private key protection.

Let’s generate a key pair with the default options.

Terminal

```shell-session
root@TryHackMe# ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/strategos/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/strategos/.ssh/id_ed25519
Your public key has been saved in /home/strategos/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:4S4DQvRfp52UuNwg++nTcWlnITEJTbMcCU0N8UYC1do strategos@g5000
The key's random art image is:
+--[ED25519 256]--+
|  .       +XXB.  |
| . .     . oBBo  |
|  . . . = + o=o  |
| .   . * X .o.E  |
|  . . o S +  o . |
|   . . o .. + o  |
|      o +. + o   |
|       +. .      |
|        ..       |
+----[SHA256]-----+
```

In the above example, we didn’t use a passphrase to show you the content of the private key. Let’s look at the generated public key, `id_ed25519.pub`, and the generated private key, `id_ed25519`.

Terminal

```shell-session
strategos@g5000:~/.ssh$ cat id_ed25519.pub 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINqNMqNhpXZGt6T8Q8bOplyTeldfWq3T3RyNJTmTMJq9 strategos@g5000
strategos@g5000:~/.ssh$ cat id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDajTKjYaV2Rrek/EPGzqZck3pXX1qt090cjSU5kzCavQAAAJA+E+ajPhPm
owAAAAtzc2gtZWQyNTUxOQAAACDajTKjYaV2Rrek/EPGzqZck3pXX1qt090cjSU5kzCavQ
AAAEB981T2ngdoNm8gEzRU35bGHofqRMjfo5egxl0/9fap/NqNMqNhpXZGt6T8Q8bOplyT
eldfWq3T3RyNJTmTMJq9AAAACm9xYWJAZzUwMDABAgM=
-----END OPENSSH PRIVATE KEY-----
```

Note that the private key is shared above for demonstration purposes and was purged afterwards. Sharing a private key would be the most insecure act anyone can commit against their security. On another note, had we used `-t rsa`, the resulting keys would have been much longer.

### SSH private keys

Using tools like John the Ripper, you can attack an encrypted SSH key to attempt to find the passphrase, highlighting the importance of using a complex passphrase and keeping your private key private.

the permissions must be set up correctly to use a private SSH key; otherwise, your SSH client will ignore the file with a warning. Only the owner should be able to read or write to the private key (`600` or stricter). `ssh -i privateKeyFileName user@host` is how you specify a key for the standard Linux OpenSSH client.

**Keys Trusted by the Remote Host**

The `~/.ssh` folder is the default place to store these keys for OpenSSH. The `authorized_keys` (note the US English spelling) file in this directory holds public keys that are allowed access to the server if key authentication is enabled. By default on many Linux distributions, key authentication is enabled as it is more secure than using a password to authenticate. Only key authentication should be accepted if you want to allow SSH access for the root user.

### Using SSH Keys to Get a “Better Shell”

During CTFs, penetration testing, and red teaming exercises, SSH keys are an excellent way to “upgrade” a reverse shell, assuming the user has login enabled. Note that www-data usually does not allow this, but regular users and root will work. Leaving an SSH key in the `authorized_keys` file on a machine can be a useful backdoor, and you don’t need to deal with any of the issues of unstabilised reverse shells like Control-C or lack of tab completion.

## Digital Signatures and Certificates

Using asymmetric cryptography, you produce a signature with your private key, which can be verified using your public key. Only you should have access to your private key, which proves you signed the file

The simplest form of digital signature is encrypting the document with your private key. If someone wants to verify this signature, they would decrypt it with your public key and check if the files match.

The answer lies in certificates. The web server has a certificate that says it is the real tryhackme.com. The certificates have a chain of trust, starting with a root CA (Certificate Authority). From install time, your device, operating system, and web browser automatically trust various root CAs. Certificates are trusted only when the Root CAs say they trust the organisation that signed them. In a way, it is a chain; for example, the certificate is signed by an organisation, the organisation is trusted by a CA, and the CA is trusted by your browser. Therefore, your browser trusts the certificate. In general, there are long chains of trust. You can take a look at the certificate authorities trusted by Mozilla Firefox [here(opens in new tab)](https://wiki.mozilla.org/CA/Included_Certificates) and by Google Chrome [here(opens in new tab)](https://chromium.googlesource.com/chromium/src/+/main/net/data/ssl/chrome_root_store/root_store.md).

# PGP & GPG — Public Key Cryptography Basics

## PGP (Pretty Good Privacy)
- Encryption + digital signature system
- Uses public-key cryptography
- Ensures confidentiality, integrity, authentication

## GPG (GNU Privacy Guard)
- Free & open-source implementation of PGP
- Uses OpenPGP standard
- Commonly used in Linux & cybersecurity labs

---

## Key Pair
- **Public Key** → shared with others
- **Private Key** → kept secret

---

## Encryption
**Goal:** Confidentiality  
- Sender encrypts using receiver’s **public key**
- Receiver decrypts using **private key**

```
Encrypt → Public Key
Decrypt → Private Key
```

---

## Digital Signatures
**Goal:** Authentication + Integrity  
- Sender signs using **private key**
- Receiver verifies using **public key**

```
Sign → Private Key
Verify → Public Key
```

---

## Hybrid Encryption (How PGP Works)
1. Generate random symmetric session key
2. Encrypt message using symmetric key
3. Encrypt session key using recipient’s public key
4. Send encrypted message + encrypted session key

---

## GPG Commands

### Generate Key Pair
```
gpg --gen-key
```

### Export Public Key
```
gpg --export -a "name" > public.key
```

### Import Public Key
```
gpg --import public.key
```

### Encrypt File
```
gpg -e -r "recipient" file.txt
```

### Decrypt File
```
gpg -d file.txt.gpg
```

### Sign File
```
gpg --sign file.txt
```

### Verify Signature
```
gpg --verify file.txt.gpg
```

## Uses
- Secure email
- File encryption
- Software signing
- Verify downloads
- CTF challenges
