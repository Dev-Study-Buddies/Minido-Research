# Security

**Anonymity**
* The ability to exist and communicate in a manner that does not reveal any personally identifiable information about the source

**Privacy**
* Unhindered agency to express oneself selectively, with direct control over one's own information and explicit boundaries

**Authenticity**
* Provide, with a high level of confidence, an assurance of the identity of an individual or message through reliable and verifiable means

**Phishing**
* Messages designed to trick a person into revealing sensitive information
* Red flags:
  * Email or domain not matching the real thing
  * Spelling/ grammer mistakes
  * government type messages
  * includes attachments
  * asks for credentials

**Malware** - Software that is intended to damage or disable computers

**Rootkits** - malware installed in the system level to provide unauthorized access and control

**Keylogger** - a type of malware that records keystrokes to potentially capture sensitive information

**Man in the Middle** - where attacker secretly relays and possibly alters the communication between 2 parties who believe they are directly communicating with each other
  * HTTPS allows traffic to be encrpyted and autheneticated which protects privacy and prevents MITM attacks

**VPN** - virtual private network which is used to redirect your internet traffic through an encrpyted service for privacy and security

**Hashing passwords**
* Irreversible one-way transformation of a string into a scrambled cipher that can't be deciphered and returned back to readable text
* Websites will store hashed passwords so that if their databases are leaked, hackers can't use the hashed passwords to login (or use the passwords to try to login to other sites)
* When user enters password into a website, web app will hash the password, and compare this with the stored hashed password
* The way hashing works is that a function is applied to a string; that same function will always produce the same hashed string given that the input is the same
  * To protect against the possibility of users having the same hashed passwords (hash collisions), you salt them
  * Salting is the act of adding a series of random, unique string to a password before its hashed
  * This unique component is what makes passwords unique
  * Salts are stored in the database with the user password

**Encryption**
* Its like hashing in that it transforms text to cipher text
* However, this transformation is *reversible*
* Symmetric (Private Key)
  * This is where messages are encrypted/ decrypted with the same key
  * requires all parties to have this one key
  * if an attacker gets this key, then encryption is compromised
  * good for personal use
* Asymmetric (Public key)
  * encryption (done with public key) / decryption (done with private key) is done with 2 separate keys
  * So you give people the public key, they encode messages with it, and then you decrypt it with your private key
    * this means that only you can decrypt messages meant for you because only you would have private key

**HTTPS/ TLS/ SSL**
* SSL and TLS are the protocols used for securing HTTP, which makes it HTTPS (its that green lock icon you see in the URL field)
* SSL is deprecated and replaced by TLS
* TLS protects data in transit from your browser to the web server
  * Without TLS/ HTTPS, any information submitted back and forth from browser and server can be clearly read or altered (Man in Middle Attack)
* Process:
  * When you visit a website, before the page loads, the server and browser communicate and share a SSL certificate
  * The browser verifies the certificate is valid
  * Once verified, a secured channel is created between browser and server to send data back and forth securely
* To ensure your website uses HTTPS, you need to purchase SSL certificate (ex: cloudflare & letsencrypt ) or create your own using something like OpenSSL
  * self-signed certificates (like the ones you create with openssl) will cause browsers to show a warning that your website is not fully secure
* Mixed Content error
  * this is where your resources (images, CSS, JS, etc) is served over an insecure connection

**Authentication**
* key based auth
  * username (unique identifier) and password (client-known secret)
* OAuth
* Single Sign On (SSO)
  * Authenticates same user on different portals/ organization sites over the same browser session

**MISC**
* Keeping application updated (patching vulnerabilities)
* Trusting/ vetting 3rd party libraries (especially npm packages)
* Static code analysis and automated tools for securing web app 
  * snyk
  * sonarqube
  * securityheaders.io
  * etc
* Compliance (HIPAA, PCI, GDPR etc)