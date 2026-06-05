It is writtten in golang 

 Many security professionals use this tool for penetration testing, bug bounty hunting, and cyber security assessments. 
 
 Looking at the phases of ethical hacking, we can place Gobuster between the reconnaissance and scanning phases.

**Enumeration**

Enumeration is the act of listing all the available resources, whether they are accessible or not. For example, Gobuster enumerates web directories.

**Brute force**

Brute force is the act of trying every possibility until a match is found. It is like having ten keys and trying them all on a lock until one fits. Gobuster uses wordlists for this purpose.'                 below:

AttackBox Terminal

```shell-session
root@tryhackme:~# gobuster --help
Usage:
  gobuster [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the request body
  gcs         Uses gcs bucket enumeration mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  tftp        Uses TFTP enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
  -h, --help                  help for gobuster
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

Use "gobuster [command] --help" for more information about a command.
```

The help page contains multiple sections:

- **Usage:** Shows the syntax on how to use the command.
- **Available Commands:** Multiple commands are available to aid us in enumerating directories, files, DNS subdomains, Google Cloud Storage buckets, and Amazon AWS S3 buckets. Throughout this room, we will focus on the `dir`, `dns`, and `vhost` commands. We will cover each of them in the following tasks.
- **Flags:** These are specific options we can configure to customize our commands. ## Example
- - the flags we will often use throughout this room:
    
    |Short Flag|Long Flag|Description|
    |---|---|---|
    |`-t`|`--threads`|This flag configures the number of threads to use for the scan. Each of these threads sends out requests with a slight delay. The default number of threads is 10. This number may be slow when using large wordlists. You can increase or decrease the number of threads depending on the available system resources.|
    |`-w`|`--wordlist`|The flag configures a wordlist to use for iterating. Each wordlist entry is attached to the URL you included in the command.|
    ||`--delay`|This flag defines the amount of time to wait between sending requests. Some web servers include mechanisms to detect enumeration by looking at how many requests are received in a certain period of time. We can increase the delay between subsequent requests to make it look like normal web traffic.|
    ||`--debug`|This flag helps us to troubleshoot when our command gives unexpected errors.|
    |`-o`|`--output`|This flag writes the enumeration results to a file we choose.|
    

## Example

Let us look at an example of how we would use these commands and flags together to enumerate a web directory:

```
gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64
```

- `gobuster dir` indicates that we will use the directory and file enumeration mode.
- `-u "http://www.example.thm/"` tells Gobuster that the target URL is [http://example.thm/(opens in new tab)](http://example.thm/).
- `-w /usr/share/wordlists/dirb/small.txt` directs Gobuster to use the _small.txt_ wordlist to brute force the web directories. Gobuster will use each entry in the wordlist to form a new URL and send a GET request to that URL. If the first entry of the wordlist were images, Gobuster would send a GET request to [http://example.thm/images/.(opens in new tab)](http://example.thm/images/)
- `-t 64` sets the number of threads Gobuster will use to 64. This improves the performance drastically.

## Use Cases 

DIR often follow a convention which is why gobuster is able to brute force it easily 

For example, the  directory structure on the web server hosting WordPress looks something  like this:

AttackBox Terminal

```shell-session
root@tryhackme:~# tree -L 3 -d
.
└── html
    └── wordpress
        ├── wp-admin
        ├── wp-content
        └── wp-includes
```

Gobuster is powerful because it allows you to scan the website and return the status codes. These status codes immediately tell you if you, as an outside user, can request that directory or not.

## Help 

Many flags are used to fine-tune the `gobuster dir` command. It is out of scope to go over each one of them, but in the table below, we have listed the flags that cover most of the scenarios:

| Flag | Long Flag                  | Description                                                                                                                                                                                                                   |
| ---- | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c` | `--cookies`                | This flag configures a cookie to pass along each request, such as a session ID.                                                                                                                                               |
| `-x` | `--extensions`             | This flag specifies which file extensions you want to scan for. E.g., .php, .js                                                                                                                                               |
| `-H` | `--headers`                | This flag configures an entire header to pass along with each request.                                                                                                                                                        |
| `-k` | `--no-tls-validation`      | This flag  skips the process that checks the certificate when https is used. It often happens for CTF events or test rooms like the ones on THM a self-signed certificate is used. This causes an error during the TLS check. |
| `-n` | `--no-status`              | You can set this flag when you don’t want to see status codes of each response received. This helps keep the output on the screen clear.                                                                                      |
| `-P` | `password`                 | You can set this flag together with the --username flag to execute authenticated requests. This is handy when you have obtained credentials from a user.                                                                      |
| `-s` | `--status-codes`           | With this flag, you can configure which status codes of the received responses you want to display, such as 200, or a range like 300-400.                                                                                     |
| `-b` | `--status-codes-blacklist` | This flag allows you to configure which status codes of the received responses you don’t want to display. Configuring this flag overrides the -s flag.                                                                        |
| `-U` | `--username`               | You can set this flag together with the `--password` flag to execute authenticated requests. This is handy when you have obtained credentials from a user.                                                                    |
| `-r` | `--followredirect`         | This flags configures Gobuster to follow the redirect that it received as a response to the sent request. A HTTP redirect status code (e.g., 301 or 302) is used to redirect the client to a different URL.                   |
## How To Use dir Mode
```
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js
```

## Subdomain enumeration

## Overview
Gobuster's `dns` mode is used to brute-force subdomains of a target domain. This is a critical reconnaissance step in penetration testing because organizations often patch their main domain but leave vulnerabilities exposed on forgotten or development subdomains (e.g., `dev.target.com` or `mobile.target.com`).

Unlike `dir` mode (which guesses directories *after* the URL), `dns` mode guesses hostnames *before* the domain name.

## Core Syntax
The `dns` mode requires two mandatory flags to work: the target domain (`-d`) and the wordlist (`-w`).

`gobuster dns -d <target-domain> -w <path-to-wordlist>`

**Example:**
`gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`

## Key Flags

| Flag | Long Flag | Description |
| :--- | :--- | :--- |
| `-d` | `--domain` | **(Required)** Specifies the target parent domain you want to enumerate (e.g., `example.com`). |
| `-i` | `--show-ips` | Displays the resolved IP addresses next to the found subdomains. |
| `-c` | `--show-cname` | Shows CNAME (Canonical Name) records, which indicate if a subdomain is an alias for another domain. *Note: Cannot be used with `-i`.* |
| `-r` | `--resolver` | Forces Gobuster to use a specific custom DNS server to look up the addresses, rather than your system's default DNS. |

## Important Notes
* Standard directory wordlists (like `directory-list-2.3-medium.txt`) are generally poor choices for DNS brute-forcing. 
* Always use a dedicated DNS/subdomain wordlist (like those found in SecLists) because subdomains use different naming conventions than directories.

## Vhost enumeration

## Overview
Gobuster's `vhost` mode brute-forces Virtual Hosts. Virtual hosts are multiple different websites or subdomains hosted on the **same physical server**, sharing a **single IP address**. 

Unlike `dns` mode (which queries public DNS servers), `vhost` mode connects directly to the target IP and manipulates the HTTP `Host:` header to discover hidden sites.

## Core Syntax
In lab environments without proper DNS routing, you must manually construct the Host header using specific flags.

`gobuster vhost -u "http://<TARGET_IP>" --domain <BASE_DOMAIN> -w <WORDLIST> --append-domain`

**Example Command:**
`gobuster vhost -u "http://10.10.94.214" --domain example.thm -w subdomains.txt --append-domain --exclude-length 250-320`

## Key Flags

| Flag | Long Flag | Description |
| :--- | :--- | :--- |
| `-u` | `--url` | **(Required)** The IP address or base URL of the target server. |
| | `--domain` | Specifies the base domain to use in the Host header (e.g., `example.thm`). |
| | `--append-domain` | Glues the wordlist entry to the `--domain` (e.g., `shop` + `example.thm` = `shop.example.thm`). **Crucial for accurate requests.** |
| | `--exclude-length` | Hides responses of a specific byte size. Essential for filtering out wildcard "catch-all" error pages to find true positives. |

## How it works under the hood
Gobuster modifies this specific line in the HTTP GET request for every word in the wordlist:
> GET / HTTP/1.1
> **Host: [WORDLIST-ENTRY].[DOMAIN]**
> User-Agent: gobuster/3.6