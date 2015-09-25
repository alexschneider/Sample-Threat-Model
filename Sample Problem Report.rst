######################################
Problem Report # (Vulnerability Title)
######################################

Executive Summary
=================

DREAD Score
===========

DREAD is a method of classifying the severity of vulnerabilities so that teams
can compare and prioritize problems with applications. It focuses on 5 key
properties for the application:

* **Damage**: How much harm an attacker can cause to an application
* **Reproducability**: Assuming an attacker has the tools to execute an attack,
  how easy is it to duplicate the attack
* **Exploitability**: How advanced an attacker needs to be in order to take
  advantage of a vulnerability
* **Affected Users**: How many users of the application does the vulnerability
  cause harm to
* **Discoverability**: How easy is it for an attacker to find the vulnerability

The score is given on a scale of 1 to 5, and an
aggregate is formed by adding all the numbers together for a total score. The
higher the score, the more severe the vulnerability and the higher priority
suggested to fix it.

================ ===== =======================================
DREAD            Score Explanation
---------------- ----- ---------------------------------------
Damage
Reproducibility
Exploitability
Affected Users
Discoverability
**Total**
================ ===== =======================================

Vulnerability Classifications
=============================

STRIDE, CWE, the OWASP Top 10, and CAPEC are unified ways to identify
vulnerabilities and attacks with unified "buckets" or classifications.
Vulnerabilities will fall into one or more buckets in each category. They are:

* **STRIDE**: Short for:

  * *Spoofing*: An attacker can assume or change attributes of other users
  * *Tampering*: An attacker can cause data delivered to or from the
    application in a way that falls outside intended behavior
  * *Repudiation*: An attacker can perform actions without leaving an
    auditable trail of logs behind (either by manipulating the logs or by the
    logs not existing in the first place)
  * *Information Disclosure*: An attacker view protected assets or sensitive
    details about the application
  * *Denial of Service*: An attacker can prevent users from using the
    application
  * *Elevation of Privileges*: An attacker can log in as other users, either
    having the same level of access to different details, or a higher level
    of access (administrative privileges)

* **CWE_**: Common Weakness Enumeration - standardized list of software
  weaknesses and vulnerabilities that affect applications
* **`OWASP Top 10`_**: List of the most common vulnerabilities in web
  applications
* **CAPEC_**: Common Attack Pattern Enumeration and Classification -
  standardized list of common attacks that can be executed on an application


.. _CWE: https://cwe.mitre.org/
.. _OWASP Top 10: https://www.owasp.org/index.php/Top_10_2013-Top_10
.. _CAPEC: https://capec.mitre.org/

STRIDE
------
* Spoofing
* Tampering
* Information Disclosure
* Denial of Service
* Elevation of Privileges

CWE
---

OWASP Top 10
------------
* A1-Injection
* A2-Broken Authentication and Session Management
* A3-Cross Site Scripting (XSS)
* A4-Insecure Direct Object References
* A5-Security Misconfiguration
* A6-Sensitive Data Exposure
* A7-Missing Function Level Access Control
* A8-Cross Site Request Forgery
* A9-Using Components with Known Vulnerabilities
* A10-Unvalidated Redirects and Forwards

CAPEC
-----


Description of the Vulnerability
================================

Reproduction Steps
==================

Remediation Steps
=================
