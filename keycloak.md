# Keycloak 

## Password Policies

Each new ***realm*** created has no password policies associated with it. 
Users can have as short, as long, as complex, as insecure a password, as they want. 
Keycloak has a rich set of password policies you can enable through the **Admin Console**.

* Adding new policies:
  1. Side menu 
  2. Authentication
  3. Password Policy 
  4. Add policy... *(dropdown menu)* 
  5. Save

| Policy type | Policy value | Example |   
| ----------- | ------------ | ------- |
| Hashing Algorithm | Hashing type | pbkdf2-sha256 |
| Hashing Iterations | Number | 27500 |
| Digits  | Number of the digits | 3 |
| Lowercase Characters | Number of the lowercase characters | 5 |
| Uppercase Characters | Number of the uppercase characters | 1 |
| Special Characters | Number of the special characters | 1 |
| Not Username | | The password is not allowed to be the same as the username |
| Not Email | | The password is not allowed to be the same as the email |
| Regular Expression | String | Regular expression patterns that passwords must match
| Password Blacklist | String? | |
| Minimum Length | Number | 8 |
| Expire Password | Number of the days | 365 |
| Not Recently Used | Number | 3 |
| Maximum Length | Number of the maximum length of the password | 64 |

* Password Policy Types:
    * HashAlgorithm:
        * Passwords are not stored as clear text.
        * Instead, they are hashed using standard hashing algorithms before they are stored or validated. 
        * The only built-in and default algorithm available is PBKDF2.
        * *Note that if you do change the algorithm, password hashes will not change in storage until the next time the user logs in.*

    * Hashing Iterations:
        * This value specifies the number of times a password will be hashed before it is stored or verified.
        * The default value is 20,000. 
        * The industry recommended value for this parameter changes every year as CPU power improves.
        * A higher hashing iteration value takes more CPU power for hashing, and can impact performance.   
    
    * Digits - The number of digits required to be in the password string.
    
    * Lowercase Characters - The number of lower case letters required to be in the password string.
    
    * Uppercase Characters - The number of upper case letters required to be in the password string.
    
    * Special Characters - The number of special characters like **'?!#%$'** required to be in the password string.
    
    * Not Username - When set, the password is not allowed to be the same as the username.
    
    * Regular Expression - Define one or more Perl regular expression patterns that passwords must match.
    
    * Expire Password - The number of days for which the password is valid.
    
    * Not Recently Used:
        * This policy saves a history of previous passwords.
        * The number of old passwords stored is configurable.
        * When a user changes their password they cannot use any stored passwords.

---

## Brute Force Detection
> *By default is disabled.*

* To enable:
  1. Click Realm Settings in the menu.
  2. Click the Security Defenses tab.
  3. Click the Brute Force Detection tab.


| Name | Description | Default |   
| ----------- | ------------ | ------- |
| Enabled | Is this type protection enabled | OFF |
| Permanent Lockout | Lock the user permanently when the user exceeds the maximum login failures | OFF |
| Max Login Failures  | How many failures before wait is triggered | 30 |
| Wait Increment | When failure threshold has been met, how much time should the user be locked out? | 1 sec / **Minutes** / hours / days | 
| Quick Login Check Milli Seconds | If a failure happens concurrently too quickly, lock out the user | 1000 |
| Minimum Quick Login Wait | How long to wait after a quick login failure | 1 sec / **Minutes** / hours / days |
| Max Wait | Max time a user will be locked out | 15 sec / **Minutes** / hours / days |
| Failure Reset Time | When will failure count be reset? | 12 sec / min / **Hours** / days |

> ***Bolded** - Second, Minutes, Hours or Days is the default type of time.* 
