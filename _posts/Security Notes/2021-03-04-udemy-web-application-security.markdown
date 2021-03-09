---
layout: post
title: Udemy - Web Application Security for Absolute Beginners
categories: security
permalink: pretty
---

Course URL: [https://ibm-learning.udemy.com/course/web-application-security-for-absolute-beginners-no-coding/learn/lecture/8084852#overview](https://ibm-learning.udemy.com/course/web-application-security-for-absolute-beginners-no-coding/learn/lecture/8084852#overview)

---

### SUMMARY

OWASP top 10 common cyber security attacks! Stop hackers, manage web application security and apply security principles!

---

### OWASP Top 10

Open Web Application Security Project

#### OWASP Top 10 2021 Update

1. Injection
2. Broken uthentication
   a. Previously "and Session Management"
3. Sensitive Data Exposure
4. XML External Entities
5. BrokenAccess Control
6. Security Misconfiguration
7. Cross-Site Scripting (XSS)
   a. Previously was "Insufficient Attack Protection"
8. Insecure Deserialization
   a. Previously was "Cross Site Request Forgery (CSRF)"
9. Using Components with Known Vulnerabilities
10. Insufficient Logging & Monitoring
    a. Previously was "Underprotected APIs"

---

### Injection

- Untrusted user input is interpreted by server and executed
- Data can be stolen, modified, or deleted.
- To prevent:
  - Reject untrusted/invalid input data
  - Use latest frameworks
  - Typically found by penetration testers / secure code review
  - Never trust user input
- Most common type: SQL injection

### Broken Authentication and Session Management

- Incorrectly build auth. and session management scheme that allows an attacker to impersonate another user.
- Attacker can take identity of victim.
- To prevent: Don't develop your own authentication.
  - Instead use open source frameworks that are actively maintained by the community
  - Use strong passwords (upper, lower, number, special characters)
  - Require current credential when sensitive information is requested or changed
  - Multi-factor authentication (e.g. sms, password, fingerprint, iris scan, etc)
  - Log out or expire session after X amount of time
  - Be careful with "remember me" functionality

### Sensitive Data Exposure

- Sensitive data is exposed, e.g. social security numbers, passwords, health records
- Data that are lost, exposed or currpted can have severe impact on business community
- To prevent:
  - Always obscure data (credit card numbers are almost always obscured)
  - Update cryptographic algorithm (MD5, DES, SHA-0, and SHA-1 are insecure)
  - Use salted encryption on storage of passwords
    - Salting is important because without same passwords will have the same hash, for easy reverse translating

#### Encryption can be used to protect data in three states:

(from: [https://cloud.google.com/security/encryption-in-transit](https://cloud.google.com/security/encryption-in-transit))

- _Encryption at rest_ protects data while stored.
  - Advance Encryption Standard (AES) is often used to encrypt data at rest.
- _Encryption in transit_ protects data while it moves between site and the cloud provider, or between two services.
  - encrypt the data before transmission; authenticate the endpoints; and decrypt and verifying the data on arrival
  - Transport Layer Security (TLS) is often used to encrypt data in transit for transport security
  - Secure/Multipurpose Internet Mail Extensions (S/MIME) is used often for email message security

### XML External Entity (XXE) injection

- Many older or poorly configured XML processors evaluate external entity references within XML documents
- Extraction of data, remote code execution and denial of service attack can result
- To prevent:
  - Use JSON, avoid serialization of sensitive data
  - Patch or upgrade all XML processors and libraries
  - Disable XXE and implement whitelisting. Reject all external entities except those in whitelist.
  - Detect, resolve and verify XXE with static application testing tools (are often open source). These tools not only cover XXEs but other vulnerabilities.

### Broken Access Control

- Restrictions on what authenticated users are allowed to do are not properly enforced.
  - e.g. a patient once authenticated changes url to doctor url of the site and has access to it
- Attackers can assess data, view sensitive files, and modify data.
- To prevent:
  - Application should not solely rely on user input; check access rights on UI level and server level for request for resources (e.g. data)
  - Deny access by default

### Security Misconfiguration

- Human mistake of misconfiguring the system (e.g. providing a user with a default password)
- Impact depends on the misconfiguration. Worst misconfiguration could result in the loss of the system.
- To prevent:

  - Force change of default credentials.
  - Least privilege: Turn everything off by default (debugging, admin interface, etc)
  - Static tools that sacn code for default settings
  - Keep patching; updating and testing the system
    - Though security misconfiguration is not always fixable with a patch. For instance, you might want to allow unnamed accounts for some users for a particular reason, even though this is considered bad practice. Typically this is not patchable.
  - Regularly audit system deployment in production

#### Example: Security Misconfiguration

- How elegantly does the system fail?
  - We should not see errors for incorrect input type. Inputs should be sanitized.
  - Do not disclose the design of your system in your error message.
    - e.g.
  ```javascript
  int id = int.Parse(Request.QueryString["Id"]);
  // The admin id has special rights over the system
  var hasAdminRights = id == 837235272;
  ```
  - Never show the physical location of the file on the developer's machine
    - e.g. Source: c:\Projects\VulnerableApplicationWebDefault.aspx.cs
  - Entire stack trace of the error should not disclose internal events and methods
  - Do not show the .NET framework that the app is executing on, which may disclose how the app may handle certain conditions

### Cross-Site Scripting (XSS)

- Similar to injection except untrusted input is interpreted by the browser and executed (rather than the server)
  - Persistent XSS attack: Malicious script on a social media page page is persisted in the database. Viewing the social media page will infect your own social media page, and is further propagated this way.
  - Non-persistent XSS attacks: Not stored in the database. Attacker creates URL with malicious code. Anyone who clicks on the URL is effected.
- This can be used to hijack user sessions, deface web sites, change content
- To prevent:
  - Escape untrusted input data
  - Use the latest UI framework

### Insecure Deserialization

- Error in translations between objects.
  - Before an object can be stored in a database, serialization -- translated -- and before it can be used again, deserialization -- translated back.
  - The attack happens for example if the hacker can find a way to modify the translation of the object. E.g. change the price of a shoe object before it is used in the checkout.
- Can result in remote code execution, denial of service. Impact depends on type of data on that server.
- To prevent:
  - Again: VALIDATE USER INPUT
  - Implement digital signatures on serialized objects to enforce integrity
  - Restrict usage and monitor deserialization and log exceptions and failures

### Using components with known vulnerabilities

- Third-party components that the focal system uses (e.g. authentication frameworks)
  - e.g. Heartbeat OpenSSL/TLS. Faulty solution kept the socket open. Keeping connection with the user through sockets take resources. To combat this, the web browser sends a heartbeat message to the server: please keep the connection open. This hearthbeat message can contain anything under 65 KB. The attacker sends a 1 KB message to the server, but tells the server the message is 64 KB big. The server will return the 1 KB message and Add 63 KB out of memory. Data from memory can be anything: e.g. usernames and passwords.
- Depending on the vulnerability, the impact varies
- To prevent:
  - Always stay current with third-party components
  - If possible, follow best practice of virtual patching

### Insufficient logging and monitoring

- Not able to witness or discover an attack when it happens or happened
  - Average time between a data breach and the actual discovery of that data breach is 190 days (2016 report)
- This allows attacker to persist and tamper, extract, or destroy your data without you noticing it
- To prevent:
  - Log login, access control, and server-side input validation failures
  - Ensure logs can be consumed easily, but cannot be tampered with
  - Continuously improve monitoring and alerting process
  - Mitigate impact of breach: Rotate, repave, and repair. Assume you have already been breached.
    - Rotate: Change keys (login credentials) fast (e.g. once or twice a day, automatically)
    - Repave: Revert to a last-known good state, so if a maintenance or staff makes a misconfiguration, it is automatically reverted regularly.
    - Repair: Whenever patches are available, repair as quickly as possible.

---

### Bonus

#### Defense in depth

- There are many layers of security that are not discussed in this course
- See the Oracle pyramid of security (from the top down)
  - Data
    - Database security (online storage and backups)
    - Content security, information rights management
    - Message level security
  - Application
    - Federation (SSO, Identity Propagation, Trust)
    - Authentication, Authorization, Auditing (AAA)
    - Security Assurance (coding practices)
  - Host
    - Platform O/S, Vulnerability Management (patches), Desktop (malware protection)
  - Internal Network & Perimeter
    - Transport Layer Security (encryption, identity)
    - Firewallls, network address translation, denial of service prevention, message parsing and validtion
  - Physical
    - Fences, walls, guards, locks, keys, badges
  - Policies, Procedures, Awareness
    - Data classification, password strengths, code reviews, usage policies
    - This is the foundation of the pyramid

#### STRIDE (basics)

- Used to identify threats.
- Why?
  - To examine what can go wrong
  - What are you going to do about it
  - Determine whether you are doing a good job
- STRIDE
  - Spoofing
    - Pretend to be someone you are not
  - Tampering
    - Removing the traces you have entered the system
  - Repudiation
    - When you establish a contract and then back down from it; e.g. if you cannot prove a transaction has happened. You want a system to be non-repudiatable.
  - Information disclosure
    - You don't want info to be disclosed to unauthorized users or other systems.
  - Denial of service
    - DDoS attack
  - Elevation of privilege
    - Changing existing rights to higher rights

#### Secure development processes

- Microsoft Secure Development Lifecycle (MS SDL)

  - Training
    - Core training
  - Requirements
    - Define quality gates/ bug bar
    - Analyze security and privacy risk
  - Design
    - Attack surface analysis (examine points of attack)
    - Threat modeling
      - STRIDE can help analyze threats
  - Implementation
    - Specify tools
    - Enforce banned functions
    - Static analysis
  - Verification
    - Dynamic/Fuzz testing
    - Verify threat models/ attack surface
    - e.g. penetration testing
  - Release
    - Response plan
    - Final security overview
    - Release archive
  - Response
    - Response execution

- Other secure development processes are:
  - Software Assurance Maturity Model (previously called CLASP)
  - Touchpoints for software security

---

Old videos

### Insufficient attack protection

- Applications that are attacked but do not recognize it as an attack, letting the attacker attack again and again (similar to lack of logging)
- Results in leak of data, decreased application availability
- To prevent:
  - Detect and log normal and abnormal use of application
  - Respond by automatically blocking abnormal users or range of IP addresses
  - Patch abnormal use quickly

### Cross-Site Request Forgery (CSRF)

- An attack that forces a victim to execute unwanted actions on a web application in which they are currently authenticated
  - E.g. You have an authenticated url. The attacker modifies this URL and embeds it into another website that is open in your browser on a different tab. Can hide this URL behind an image, and as soon as the victim clicks on the image, funds will be transfered (based on the request in this modified URL).
- Victim unknowingly executes transactions
- To prevent:
  - Reautheticate for all critical actions (e.g. transfer money)
  - Include hidden token in request
  - Most web frameworks have built-in CSRF protection, but it isn't enabled by default

### Underprotected APIs

- Applications expose rich connectivity options through APIs, in the browser to a user. These APIs are often unprotected and contain numerous vulnerabilities.
  - e.g. API gets hacked and sends the wrong response
- May result in data theft, corruption, unauthorized access, etc.
- To prevent:
  - Ensure secure communication between client browser and server API
  - Reject untrusted/invalid input data
  - Use latest framework
  - Vulnerabilities are typically found by penetration testers and secure code reviewers

---

### FAQ

#### Q: How can you test whether your website uses the latest security protocols?

A: Navigate to ssllabs.com to test the security protocols of your website for free!

#### Q: Where can I legally test my hacking skills for free?

A: http://google-gruyere.appspot.com

#### Q: What are Insecure Direct Object References?

A: A reference to a file, database, or directory exposed to user via the browser. Any user can navigate to almost any part of the system (that they are not authorized to access) and attack the system by modifying the URL through the browser. To prevent: Check access rights (e.g. proper authorization), and sanitize inputs.

For example, if you book a ticket and want to see a report of your ticket:
http://airlinewebsite.com/file.jsp?file=report.txt

The attacker can modify the url and navigate to the root folder to see all files.
http://airlinewebsite.com/file.jsp?file=**../../../root
