- in the basic terms scrambling of sensitive data so only authorised individuals can access it
## A Real-World Scenario

- Imagine you're running a small medical clinic. You need to send patient records, including names, medical conditions, and treatment history, to specialists and insurance companies over the internet. The problem? Data doesn't travel directly from you to the recipient. It bounces through dozens of computers and routers along the way. Without protection, anyone with access to those systems could read, change, or block your data.
- here the scrambling is used to protect integrity
## terminology
 - **Plaintext** - A message you can read normally. Like `HELLO` or `Patient name: Alice Smith`.
    
- **Ciphertext** - A scrambled version that's not supposed to make sense. Like `KHOOR` or `Sdwlhqw qdph: Dolfh Vplwk`.
    
- **Key** - The secret ingredient that controls how scrambling and unscrambling work. Think of it as a password that the algorithm uses.
    
- **Algorithm** - The public recipe—the set of steps that explain how to use the key on the message. Everyone can know the algorithm. Security comes from keeping the key secret.

**Encryption process: plaintext + encryption algorithm + key  → ciphertext**

![Block diagram of encryption using a secret key](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1771331232484.png)

and then

**Decryption process: ciphertext + decryptiong algorithm + key   → plaintext**
![[Pasted image 20260319175300.png]]   
## The Lockbox Analogy

Think about a physical lockbox:

- The **algorithm** is how the lock works. Anyone can see you insert a key and turn it, hence it's not secret.
- The **key** is your specific metal key. Only people with that exact key can open your box.
- The **plaintext** is the letter inside the box.
- The **ciphertext** is that locked box travelling through the postal system.
>That's **symmetric encryption** in a nutshell: one key locks the box, the same key unlocks it.

## the casesar cypher - algorithm plus key
- The Caesar cipher is named after Julius Caesar, who reportedly used this technique over 2000 years ago to send military messages.

**How It Works**

- The Caesar cipher shifts each letter in your message by a fixed number of positions in the alphabet. That fixed number is your **key**.


>Real algorithms like AES (Advanced Encryption Standard) are vastly more complex and secure. But they follow the same basic idea: algorithm + key + plaintext → ciphertext.

## Symmetric Encryption Explained

The Caesar cipher is an example of **symmetric encryption**. This means that:

- The same key **encrypts** (locks) and **decrypts** (unlocks) the message.
- Both sender and receiver need a copy of that key.
- The key has to stay secret from everyone else.

Some of the benefits of using symmetric encryption are:

- **It's fast.** Symmetric algorithms can churn through huge amounts of data really quickly.
- **It's efficient.** Perfect for encrypting files, hard drives, and network traffic where speed matters.

> **How do Alice and Bob share that key safely in the first place?**

If they send the key over the internet in plain view, an eavesdropper can grab it. Then that eavesdropper can decrypt every future message.

You might think, "_Just encrypt the key!_",  but then you'd need another key to encrypt _that_ key, and then another key for _that_ key, and you see the problem. Infinite regress.

This is called the **key distribution problem**, and it's the Achilles' heel of symmetric encryption when used alone.

## Asymmetric encryption

Asymmetric encryption uses **two mathematically linked keys**:

- A **public key** that anyone can know and use.
- A **private key** that only one person keeps secret.

Here's the clever part:

- If you encrypt something with someone's **public key**, only their **private key** can decrypt it.
- If you encrypt something with your **private key**, anyone with your **public key** can decrypt it (this is primarily used for digital signatures, which we won't delve into here).

The two keys are connected by some serious maths, but it would take an ordinary computer hundreds or even thousands of years to recover the private key from the public key. This computational difficulty is what makes asymmetric encryption secure.

## The Mailbox Analogy

![Illustration of asymmetric encryption. Alice places an envelope into a public mailbox labeled “Public Key (Anyone can use this).” Bob kneels beside it holding a key labeled “Private Key (Only Bob can open this),” representing the use of different keys for encryption and decryption](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1768818761779.png)Let us use a physical mailbox on a street corner as an example:

- The **mail slot** at the top is the **public key**. Anyone walking by can drop off a letter. It's completely open and accessible.
- The **locked door** at the front is the **private key**. Only the mailbox owner has the key to open it and grab the letters.
## Solving the Key Distribution Problem

With asymmetric encryption, Alice and Bob don't need to share a secret key beforehand. A simple flow of events can be as follows:

1. Bob creates a **public key** and a **private key** on his computer. He keeps the private key to himself and shares the public key with the world.
2. Alice grabs Bob's public key (maybe from his website or a key server).
3. Alice encrypts her message using Bob's public key and sends it off.
4. Bob receives it and decrypts it using his private key.
## Real-world Use: HTTPS

The most common everyday use of asymmetric encryption is in **HTTPS**—the secure protocol you use whenever you see that padlock in your browser.

Here's what happens when you visit `https://google.com`:
1. Your browser requests the website's public key.
2. The website sends back its public key wrapped in a **certificate** (more on this shortly).
3. Your browser and the website use asymmetric encryption to agree on a shared secret (a symmetric key) without anyone else being able to see it.
4. From there on, they switch to fast **symmetric encryption** using that shared secret for the rest of the session.

This combo is sometimes called a **hybrid approach**:

- **Asymmetric encryption** solves the problem of key distribution.
- **Symmetric encryption** handles the heavy lifting because it's way faster.

A certificate is a digital document that:

- Contains someone's public key.
- States who that key belongs to (like `example.com`).
- A trusted authority digitally signs it, called a **Certificate Authority** (CA).
