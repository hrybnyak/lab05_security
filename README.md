# lab05_security
For this assignment I have chosen basic ASP.NET Core 4x Identity project as my baseline project. So, what does ASP.NET Identity does good according to OWASP:
-	Limits failed login attempts implemented in Identity Lockout options

-	Implements multi-factor authentication to prevent automated, credential stuffing, brute force, and stolen credential re-use attacks from the box

-	Uses a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Session IDs should not be in the URL, be securely stored and invalidated after logout, idle, and absolute timeouts.

-	By default, it suggests you use email password recovery although you need to implement email service by yourself

What can be and was improved:
-	By default, Identity uses PBKDF2 with HMAC-SHA256, 128-bit salt, 256-bit subkey, 10000 iterations. Although, according to OWASP, we should use PBKDF2 with a work factor of 310,000 or more and set with an internal hash function of HMAC-SHA-256. So, the number of iterations was in fact increased to prevent from modern days attacks.

-	Password requirements were improved, the minimum length now is 8. (According to rule of NIST 800-63 B’s guidelines in section 5.1.1 for Memorized Secrets : Memorized secrets SHALL be at least 8 characters in length if chosen by the subscriber). I also decided to utilize ASP.NET default feature of checking number of unique characters in the password and increased value to 5. Identity by default requires to have at least one digit, one upper case character, one lower case character and one special character, this settings remained unchanged.

-	I also have implemented email service, so now password recovery and email confirmation work as intended.

Overall, I believe ASP.NET Core Identity does pretty good job in their default template complying to OWASP. The biggest issue of the entire ASP.NET Identity I believe is using PBKDF2 with HMAC-SHA256, 128-bit salt, 256-bit subkey, 10000 iterations by default. According to this article (https://www.scottbrady91.com/aspnet-identity/improving-the-aspnet-core-identity-password-hasher), “PBKDF2 has generally been considered “good enough”, assuming you use a high number of iterations and a SHA2 family hash function. It is also FIPS compliant and recommended by NIST (you’ll be able to find FIPS-140 validated implementations). However, it is not so secure against newer attack vectors, such as GPU-based attacks, and as a result, it is often considered weak compared to alternatives such as bcrypt and Argon2. In fact, to defend against modern attacks in 2021, cryptographers suggest that you need to use 310,000 iterations for PBKDF2-HMAC-SHA256. This would make PBKDF2 comparable to bcrypt work factor 8, and it is a little different from ASP.NET Identity’s default 10,000.”
