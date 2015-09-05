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
* Authentication tokens
* Posts
  * Anonymous (still has user data attached to it)
  * Non-anonymous

## Roles
* Admin
  * Can create, read, update, and delete all PII, user data, and posts
* Users - 
  * Can create, read, and update their own PII and user data
  * Can create and read anonymous and non-anonymous posts 
  * Can read public user data and all posts.
* Anonymous
  * Can read all posts

### Access Control Matrix


