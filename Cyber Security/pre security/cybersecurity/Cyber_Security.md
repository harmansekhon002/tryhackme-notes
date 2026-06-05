# 🔐 Cyber Security Notes

---

## 🛡️ Defensive Security

Defensive security is the practice of **protecting and securing services and systems** from threats.

### Core Focus Areas
- Detecting and investigating attacks in real time
- Responding to threats **before damage occurs**
- Using monitoring dashboards to predict and prevent oncoming attacks

> [!tip] Containment
> We can stop the attacker from continuing **immediately** while we investigate and fix any security vulnerabilities. This is known as ***containment***.

---

## ⚔️ Offensive Security

Offensive security involves **finding vulnerabilities and exploiting them** — this is what ethical hackers (and attackers) do.

### Directory Brute Forcing

A common technique is directory brute forcing using a tool like `dirb`:

```bash
dirb http://fakebank.thm
```

**How it works:**
- Takes a wordlist of paths like `/login`, `/admin`, `/images`, etc.
- Fires each as an HTTP request to the target URL
- Any URL that returns **`200`, `301`, or `302`** gets flagged as **found**

> [!warning] Note
> This is a web penetration testing command and a pretty common **directory brute force attack**. Only use on systems you have permission to test.

---

## 📌 Tags

#cybersecurity #defensive-security #offensive-security #ethical-hacking #penetration-testing
