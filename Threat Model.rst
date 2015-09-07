Threat Model for "Pay to Post"
==============================

Pay to Post is a simple application that allows users to create profiles and make posts on other people's profiles (either
anonymously or non-anonymously). Users must keep a credit card and credit card information on file with the application so
the application can automatically deduct the charge whenever they make a post. There is also a monthly maintenance fee and
removing any personal info or canceling the monthly charge is impossible. In addition, spam mail is sent to the user's
address on record to bring in even more profit for the website. This results in a good deal of disgruntled users and it is
likely some of them will hold a grudge against the site and try to attack it with varying degrees of effectiveness.

Assets
------

* PII

  * Credit card data
  * Real name
  * Address

* User data

  * Username
  * Password
  * Public user info

    * About me
    * User roles

* Authentication tokens
* Posts

  * Anonymous (still has user data attached to it)
  * Non-anonymous


Roles
-----

* Admin

  * Can create, read, update, and delete all PII, user data, and posts

* User

  * Can create, read, and update their own PII and user data
  * Can create and read anonymous and non-anonymous posts
  * Can read public user data and all posts.

* Anonymous

  * Can read all posts

Access Control Matrix
---------------------

+-------------+---------+---------------------------------------------------+
|*Role*       |Operation|Assets                                             |
|             |         +---------+---------+---------------------+---------+
|             |         |PII      |User Data|Authentication Tokens|Posts    |
+=============+=========+=========+=========+=====================+=========+
|**Anonymous**|*Create* |Never    |Never    |Never                |Never    |
|             +---------+---------+---------+---------------------+---------+
|             |*Read*   |Never    |Sometimes|Never                |Always   |
|             +---------+---------+---------+---------------------+---------+
|             |*Update* |Never    |Never    |Never                |Never    |
|             +---------+---------+---------+---------------------+---------+
|             |*Delete* |Never    |Never    |Never                |Never    |
+-------------+---------+---------+---------+---------------------+---------+
|**User**     |*Create* |Always   |Always   |Never                |Always   |
|             +---------+---------+---------+---------------------+---------+
|             |*Read*   |Sometimes|Sometimes|Sometimes            |Always   |
|             +---------+---------+---------+---------------------+---------+
|             |*Update* |Sometimes|Sometimes|Never                |Sometimes|
|             +---------+---------+---------+---------------------+---------+
|             |*Delete* |Never    |Never    |Never                |Never    |
+-------------+---------+---------+---------+---------------------+---------+
|**Admin**    |*Create* |Always   |Always   |Never                |Always   |
|             +---------+---------+---------+---------------------+---------+
|             |*Read*   |Always   |Always   |Always               |Always   |
|             +---------+---------+---------+---------------------+---------+
|             |*Update* |Always   |Always   |Never                |Always   |
|             +---------+---------+---------+---------------------+---------+
|             |*Delete* |Always   |Always   |Never                |Always   |
+-------------+---------+---------+---------+---------------------+---------+

Existing Countermeasures
------------------------

Because this application will be written in Python with Flask, buffer overflows
and race conditions are unlikely.

Threat Tree
-----------

Potential Attackers
~~~~~~~~~~~~~~~~~~~

This application is most likely to be attacked by script kiddies or activist hackers,
given the nature of the application. Due to the storage of credit card data,
it is possible that the website may be attacked by organized crime attackers,
but it is unlikely to be the target of governmental forces since it contains no
subversion efforts.

Priority 1
~~~~~~~~~~
#. An attacker can Create, Read, Update, or Delete PII
#. An attacker can Create, Read, Update, or Delete User Data
#. An attacker can Create, Read, Update, or Delete Authentication Tokens
#. An attacker can Create, Read, Update, or Delete Posts
#. An attacker can gain unauthorized access to the filesystem contents
#. An attacker can gain unauthorized code execution on the machine

Priority 2
~~~~~~~~~~
#. An attacker can deny service from the website to its users

Vulnerabilities and Countermeasures
-----------------------------------

* Because the application is using a SQL database, an attacker can use SQL
  injection to Create, Read, Update, or Delete data. Alternatively, an attacker
  can cause unintended behavior by crafted XML or JSON (A1)

  * Use parameterized queries or prepared statements whenever using SQL queries,
    even if it is not obvious that it takes user input
  * Ensure that deserializers can safely handle untrusted data
  * Sanitize data if a deserializer cannot safely handle untrusted data

* The application has broken authentication or session management (A2)

  * Ensure that session tokens are checked and verified at every sensitive
    endpoint of the site
  * Ensure that session tokens are generated with sufficient entropy so they
    aren't predictable and aren't reused
  * Ensure that session tokens expire within a reasonable length of time
  * Ensure proper length and complexity requirements on passwords, and make sure
    the passwords are salted and hashed using a secure algorithm

* The application allows for Cross Site Scripting (A3)

  * Ensure that any user input that may be reflected (either from a form or in
    a query parameter) is escaped or checked against a whitelist of acceptable
    characters
  * Utilize `Content Security Policy (CSP)`_

* The application allows insecure direct object references (i.e. look up
  information by account number) (A4)

  * Use indirect object references and perform a 1-1 mapping on the application
    side
  * If direct references must be used, verify the user making the request has
    authorization to access the resource

* The security is misconfigured on the server or in the application (A5)

  * Ensure the operating system and all components of the application (libraries,
    databases) are updated fully
  * Ensure unnecessary features (admin consoles, accounts, directory listing,
    etc.) are disabled.
  * Change default passwords
  * Return generic error messages when there's an issue (Don't allow a user to
    see a full stack trace)
  * Ensure all security features of frameworks used are enabled

* The application exposes sensitive data to attackers (A6)

  * Ensure that the application uses TLS and strong cipher suites only
  * Make sure cookies are set to Secure and if possible HttpOnly
  * Use HSTS_ and `Certificate Pinning`_ if possible
  * Use proper `HTTP Security Headers`_
  * Don't store passwords using a reversible encryption algorithm - only use
    strong hashes (like bcrypt and scrypt)
  * For things that must be stored using reversible encryption, like credit card
    information, use strong encryption algorithms like AES and use secure key
    storage
  * When serializing data to send to a client, ensure that the serialized data
    doesn't contain any data not directly needed at the time of the response

* An attacker can access application functionality they don't have access to (A7)

  * Make sure that all authenticated endpoints verify authentication for all
    functionality. For example, use middleware to ensure requests made to
    handlers for administrative pages or user pages have the requisit level
    required

* An attacker can execute a `Cross Site Request Forgery (CSRF)`_ attack (A8)

  * Use CSRF `synchronizer token`_ (preferably at the double submit cookies and
    the encrypted token patterns)
  * Check the referrer header to make sure it makes sense (this is difficult to
    spoof)
  * Ask the user to perform a challenge when making requests that are secure or
    sensitive in nature (i.e. re-entering passwords, entering a captcha, etc)

* The application uses components that have known vulnerabilities (A9)

  * Prior to using any third party components, verify that the current versions
    don't show up on the `CVE list`_ or the `National Vulnerability Database`_
  * Make sure that all libraries and applications used on the server are updated
    and appropriate access controls are used to prevent more access than
    absolutely required

* The application does not validate user input used for redirects (A10)

  * Don't allow user input to be used directly in a redirect - redirect based on
    a mapping of options to acceptable targets
  * If user input must be used for a redirect, validate using a URL parsing
    library against a whitelist of acceptable URLs

* An attacker can exhaust resources of an application

  * Ensure that rate limiting is implemented to a reasonable level
  * Use performant cryptography and hashing methods that are still
    cryptographically secure
  * Write code that fails prior to doing expensive computations on data (though
    if the data is sensitive, take care not to introduce vulnerabilities to
    `timing attacks`_)
  * Require authentication before doing expensive computations, and limit users
    to a quota.
  * Ensure sufficient entropy is generated, and if entropy is perpentually low,
    consider using the built in hardware RNG or haveged_

* An attacker can learn information from poorly designed or written cryptography

  * **DON'T WRITE YOUR OWN ENCRYPTION OR SECURE HASHING ALGORITHM**
  * There is almost never a reason to design encryption or hashing algorithms
  * If secure encryption or hashing libraries don't exist for the language,
    seriously consider using another language
  * Use secure algorithms like SHA-256 for hashing, bcrypt/scrypt for hashing
    with a salt, AES for encryption, HMAC for authenticating messages
  * Ensure items that need to be cryptographically secure use secure random
    number generators with sufficient entropy

* An attacker can gain access to filesystem contents or run unauthorized code
  (RCE)

  * Ensure that if the application accesses filesystem contents, sufficient
    controls and restrictions are in place to prevent arbitrary file reads
  * Don't construct language statements based on unverified user input




.. _Content Security Policy (CSP): https://www.owasp.org/index.php/Content_Security_Policy
.. _HSTS: https://www.owasp.org/index.php/HTTP_Strict_Transport_Security
.. _Certificate Pinning: https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning
.. _HTTP Security Headers: https://www.owasp.org/index.php/List_of_useful_HTTP_headers
.. _Cross Site Request Forgery (CSRF): https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet
.. _synchronizer token: https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern
.. _CVE list: https://cve.mitre.org/
.. _National Vulnerability Database: https://nvd.nist.gov/home.cfm
.. _timing attacks: https://www.owasp.org/index.php/Covert_timing_channel
.. _haveged: http://www.issihosts.com/haveged/

Summary
-------

Because the potential for attack is high due to the nature of the application,
extra care must be taken in the development of it to ensure security issues are
kept to a minimum. Throughout the coding process, the threat model will be
kept updated as new assets are added, vulnerabilities anticipated, or
countermeasures implemented.
