---
layout: post
title: Udemy - Web Application Security fro Absolute Beginners
categories: security
permalink: date
---

Course URL: [https://ibm-learning.udemy.com/course/web-application-security-for-absolute-beginners-no-coding/learn/lecture/8084852#overview](https://ibm-learning.udemy.com/course/web-application-security-for-absolute-beginners-no-coding/learn/lecture/8084852#overview)

---

## SUMMARY

OWASP top 10 common cyber security attacks! Stop hackers, manage web application security and apply security principles!

---

## OWASP Top 10

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

## Injection

- Untrusted user input is interpreted by server and executed
- Data can be stolen, modified, or deleted.
- To prevent:
  - Reject untrusted/invalid input data
  - Use latest frameworks
  - Typically found by penetration testers / secure code review
  - Never trust user input
- Most common type: SQL injection

## Broken Authentication and Session Management

- Incorrectly build auth. and session management scheme that allows an attacker to impersonate another user.
- Attacker can take identity of victim.
- Don't develop your own authentication.
