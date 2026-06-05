- on the internet everyone can voice their claims through posting it may be a post a blog or anything like that
- It is our job, as readers, to evaluate the information. We will mention a few things to consider when evaluating information:
- **Source**: Identify the author or organisation publishing the information. Consider whether they are reputable and authoritative on the subject matter. Publishing a blog post does not make one an authority on the subject.
- **Evidence and reasoning**: Check whether the claims are backed by credible evidence and logical reasoning. We are seeking hard facts and solid arguments.
- **Objectivity and bias**: Evaluate whether the information is presented impartially and rationally, reflecting multiple perspectives. We are not interested in authors pushing shady agendas, whether to promote a product or attack a rival.
- **Corroboration and consistency**: Validate the presented information by corroboration from multiple independent sources. Check whether multiple reliable and reputable sources agree on the central claims.
## search engines

Almost every Internet search engine allows you to carry out advanced searches. Consider the following examples:

- [Google(opens in new tab)](https://www.google.com/advanced_search)
- [Bing(opens in new tab)](https://support.microsoft.com/en-us/topic/advanced-search-options-b92e25f1-0085-4271-bdf9-14aaea720930)
- [DuckDuckGo(opens in new tab)](https://duckduckgo.com/duckduckgo-help-pages/results/syntax/)

Let’s consider the search operators supported by Google.

- `"exact phrase"`: Double quotes indicate that you are looking for pages with the exact word or phrase. For example, one might search for `"passive reconnaissance"` to get pages with this exact phrase.
- `site:`: This operator lets you specify the domain name to which you want to limit your search. For example, we can search for success stories on TryHackMe using `site:tryhackme.com success stories`.
- `-`: The minus sign allows you to omit search results that contain a particular word or phrase. For example, you might be interested in learning about the pyramids, but you don’t want to view tourism websites; one approach is to search for `pyramids -tourism` or `-tourism pyramids`.
- `filetype:`: This search operator is indispensable for finding files instead of web pages. Some of the file types you can search for using Google are Portable Document Format (PDF), Microsoft Word Document (DOC), Microsoft Excel Spreadsheet (XLS), and Microsoft PowerPoint Presentation (PPT). For example, to find cyber security presentations, try searching for `filetype:ppt cyber security`.

## specialised search engines

- there are some specialised search engines used to find specialised searches some of them are
### shodan
-  It allows you to search for specific types and versions of servers, networking equipment, industrial control systems, and IoTdevices. 
- You may want to see how many servers are still running Apache 2.4.1 and the distribution across countries.
- To find the answer, we can search for `apache 2.4.1`, which will return the list of servers with the string “apache 2.4.1” in their headers.
## censys
- Censys focuses on Internet-connected hosts, websites, certificates, and other Internet assets. 
- Some of its use cases include enumerating domains in use, auditing open ports and services, and discovering rogue assets within a network.
## virusTotal
- [VirusTotal(opens in new tab)](https://www.virustotal.com/) is an online website that provides a virus-scanning service for files using multiple antivirus engines.
- It allows users to upload files or provide URLs to scan them against numerous antivirus engines and website scanners in a single operation. 
- They can even input file hashes to check the results of previously uploaded files.
- one can check the community's comments for more insights. 
- Occasionally, a file might be flagged as a virus or a Trojan; however, this might not be accurate for various reasons, and that's when community members can provide a more in-depth explanation.
## have i been pwned
- [Have I Been Pwned(opens in new tab)](https://haveibeenpwned.com/) (HIBP) does one thing; it tells you if an email address has appeared in a leaked data breach. 
- Finding one’s email within leaked data indicates leaked private information and, more importantly, passwords. 
- Many users use the same password across multiple platforms, if one platform is breached, their password on other platforms is also exposed. 
- Indeed, passwords are usually stored in encrypted format; however, many passwords are not that complex and can be recovered using a variety of attacks.

# Vulnerabilities and exploits
### CVE
- We can think of the Common Vulnerabilities and Exposures (CVE) program as a dictionary of vulnerabilities. 
- It provides a standardised identifier for vulnerabilities and security issues in software and hardware products. 
- Each vulnerability is assigned a CVE ID with a standardised format like `CVE-2024-29988`.
- This unique identifier (CVE ID) ensures that everyone from security researchers to vendors and IT professionals is referring to the same vulnerability, [CVE-2024-29988(opens in new tab)](https://nvd.nist.gov/vuln/detail/CVE-2024-29988) in this case.
- The MITRE Corporation maintains the CVE system.
### Exploit Database
- There are many reasons why you would want to exploit a vulnerable application; one would be assessing a company’s security as part of its red team.
- Now that we have permission to exploit a vulnerable system, we might need to find a working exploit code. 
- One resource is the [Exploit Database(opens in new tab)](https://www.exploit-db.com/)
- The Exploit Database lists exploit codes from various authors; some of these exploit codes are tested and marked as verified.
# Technical documentation

## Linux Manual pages
- Long before the Internet was everywhere, how would you get help using a command in a Linux or Unix-like system?
- The answer would be checking the manual page, man page for short. 
- On Linux and every Unix-like system, each command is expected to have a man page. 
- In fact, man pages also exist for system calls, library functions, and even configuration files.
- Let’s say we want to check the manual page for the command `ip`, We issue the command `man ip`. 
## Microsoft Windows

- Microsoft provides an official [Technical Documentation(opens in new tab)](https://learn.microsoft.com/) page for its products

## Product Documentation

- Every popular product is expected to have well-organised documentation. 
- This documentation provides an official and reliable source of information about the product features and functions.
- Examples include [Snort Official Documentation(opens in new tab)](https://www.snort.org/documents), [Apache HTTP Server Documentation(opens in new tab)](https://httpd.apache.org/docs/), [PHPDocumentation(opens in new tab)](https://www.php.net/manual/en/index.php), and [Node.js Documentation(opens in new tab)](https://nodejs.org/docs/latest/api/).
- It is always rewarding to check the official documentation as it is the most up-to-date and offers the most complete product information.
## social media
- The power of social media is that it allows you to connect with companies and people you are interested in.
- Furthermore, social media offers a wealth of information for cyber security professionals, whether they are searching for people or technical information. 
- When protecting a company, you should ensure that the people you protect are not oversharing on social media. 
- For instance, their social media might give away the answer to their secret questions, such as, “Which school did you go to as a child?”. 
- Such information might allow adversaries to reset their passwords and take over their accounts effortlessly.
