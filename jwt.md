# JSON Web Tokens

## What is JSON Web Token?

JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. 
This information can be verified and trusted because it is digitally signed. 
JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

Although JWTs can be encrypted to also provide secrecy between parties, we will focus on signed tokens. 
Signed tokens can verify the integrity of the claims contained within it, while encrypted tokens hide those claims from other parties. 
When tokens are signed using public/private key pairs, the signature also certifies that only the party holding the private key is the one that signed it.


## When should you use JSON Web Tokens?

* Authorization:
    * This is the most common scenario for using JWT.
    * Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token.
    * Single Sign On is a feature that widely uses JWT nowadays, because of its small overhead and its ability to be easily used across different domains.

* Information Exchange:
    * JSON Web Tokens are a good way of securely transmitting information between parties.
    * Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are.
    * As the signature is calculated using the header and the payload, you can also verify that the content hasn't been tampered with.


## What is the JSON web Token structure?

*The JSON is Base64Url encoded.*

* Consist of three parts separated by dots (.):
    * Header
    * Payload
    * Signature (only for the server)

* A JWT typically looks like the following : 
    * *xxxxx.yyyyy.zzzzz*

### Header
The header typically consists of two parts: 
* the type of the token, which is JWT
* and the signing algorithm being used, such as *HMAC SHA256* or *RSA*

> Example:
>    ```json 
>    {
>        "alg": "HS256",
>        "typ": "JWT"
>    }
>    ```

### Paylaod
This part contains the claims. 
Claims are statements about an entity (typically, the user) and additional data. 
There are three types of claims: registered, public, and private claims.

* Registered claims: 
    * These are a set of predefined claims which are not mandatory but recommended, to provide a set of useful, interoperable claims.
    * Some of them are:
        * iss (issuer)
        * exp (expiration time)
        * sub (subject)
        * aud (audience)
        * and others

*Notice that the claim names are only three characters long as JWT is meant to be compact.*

* Public claims: 
    * These can be defined at will by those using JWTs.
    * But to avoid collisions they should be defined in the IANA JSON Web Token Registry or be defined as a URI that contains a collision resistant namespace.

* Private claims:
    * These are the custom claims created to share information between parties that agree on using them and are neither registered or public claims.

> An example payload could be:
>    ```json 
>    {
>        "sub": "1234567890",
>        "name": "John Doe",
>        "admin": true
>    }
>    ```

*Do note that for signed tokens this information, though protected against tampering, is readable by anyone. Do **not** put secret information in the payload or header elements of a JWT unless it is encrypted.*

### Signature

To create the signature part you have to take:
* the encoded header,
* the encoded payload,
* a secret, the algorithm specified in the header,
* and sign that.

> For example (use the HMAC SHA256):
> ```json
>   HMACSHA256(
>        base64UrlEncoded(header) + "." + 
>        base64UrlEncoded(paylaod),
>        secret)
> ```

*The signature is used to verify the message wasn't changed along the way, and, in the case of tokens signed with a private key, it can also verify that the sender of the JWT is who it says it is.*

### Putting all together

The output is three Base64-URL strings separated by dots that can be easily passed in HTML and HTTP environments, while being more compact when compared to XML-based standards such as SAML.


## How do JSON Web Tokens work?

In authentication, when the user successfully logs in using their credentials, a JSON Web Token will be returned. 
Since tokens are credentials, great care must be taken to prevent security issues. 
In general, you should not keep tokens longer than required.

*You also should not store sensitive session data in browser storage due to lack of security.*

Whenever the user wants to access a protected route or resource, the user agent should send the JWT, typically in the **Authorization** header using the **Bearer** schema. 
The content of the header should look like the following:
> ```json
>   Authorization: Bearer <token>
> ```

This can be, in certain cases, a stateless authorization mechanism. 
The server's protected routes will check for a valid JWT in the Authorization header, and if it's present, the user will be allowed to access protected resources. 
If the JWT contains the necessary data, the need to query the database for certain operations may be reduced, though this may not always be the case.
If the token is sent in the Authorization header, Cross-Origin Resource Sharing (CORS) won't be an issue as it doesn't use cookies.


## Why should we use JSON Web Tokens?

*JSON Web Tokens (JWT) compared to Simple Web Tokens (SWT) and Security Assertion Markup Language Tokens (SAML)*

As JSON is less verbose than XML, when it is encoded its size is also smaller, making JWT more compact than SAML. 
This makes JWT a good choice to be passed in HTML and HTTP environments.

Security-wise, SWT can only be symmetrically signed by a shared secret using the HMAC algorithm. 
JWT and SAML tokens can use a public/private key pair in the form of a X.509 certificate for signing. 
Signing XML with XML Digital Signature without introducing obscure security holes is very difficult when compared to the simplicity of signing JSON.

JSON parsers are common in most programming languages because they map directly to objects. 
Conversely, XML doesn't have a natural document-to-object mapping. 
This makes it easier to work with JWT than SAML assertions.

Regarding usage, JWT is used at Internet scale. 
This highlights the ease of client-side processing of the JSON Web token on multiple platforms, especially mobile.