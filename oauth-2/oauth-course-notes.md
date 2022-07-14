# OAuth 2.0 in Spring Boot Applications

## Introduction to OAuth 2

> **OAuth** = **O**pen + **Auth**orization

* Different authorizations for different devices / scenarios.
* OAuth 2.0 - is an Authorization Framework.
* Good for web and mobile devices.
* OAuth - is a delegated authorization framework allows:
  * Grand access user data which is stored on another device;
  * Data access restriction;
  * Grand permission to a third party software, website or device access.
  * More secure than old ways (third party software saves username and password)


## OAuth 2 Roles
*Different actors in OAuth 2.0*

* Roles:
  1. Resource Owner:
     * a user who owns the information;
     * he is capable to grand the access to this information;
  2. Client:
     * an application that access the information (*Web application, mobile application or Postman*);
  3. Resource Server
     * Server user to store the information (*own server, Cloud server*);
     * In case with microservices each of them will be considered on separate resource server;
  4. Authorization server
     * Issues the access tokens to client application after successfully authentication of the user;
     * Microsoft, Google, Twitter, Facebook have own authorization servers;
     * We can start own authorization server:
       * Spring Authorization server
       * Keycloak Authorization server (*on Docker*)
     * Standalone authorization servers:
       * AWS Cognito
       * Microsoft Identity Platform
       * Okta


## State of OAuth 2.0 in Spring Security 5

* Spring Security OAuth Project (*Deprecated*):
  * Client Support
  * Resource Server
  * Authorization Server

* The New OAuth Stack in Spring Security 5:
  * Client Support
  * Resource Service 
  * Authorization Server (*same specifications as Keycloak*)
    * *Spring Authorization Server is a framework that provides implementations of the OAuth 2.1 and OpenID Connect 1.0 specifications and other related specifications. It is built on top of Spring Security to provide a secure, light-weight, and customizable foundation for building OpenID Connect 1.0 Identity Providers and OAuth2 Authorization Server products.*


## OAuth 2.0 Client Types

* Secure app running on Server (*Confidential Client*)
* Native Apps on User Device (*Public Client*)
* Single Page Browser-based App (*Public Client*)

**Confidential Client** - able to securely authenticate with the authentication server using their *client id* and *secret key*.

**Public client** - can not guarantee the safety and confidentiality of the *client id* and *security key*.

All are capable to send HTTP requests to the Authorization Server.
For client application to be able to communicate with the Authorization Server, need to:
* be registered with it 
* get **client id** 
* get **client secret key** 

> **Client id** and **secret key** are used by Authentication server and client application to communicate with each other.   


## OAuth Access Token

When the client app has the **Access Token**, it can access the resource server without asking for username and password every time.

The **Access Token** is secured as sensitive information and if is necessary to store it, the client application must store it a secure location. 

* **Access Token Types**:
  * Identifier type:
    * short text determined by auth server;
    * doesn't contain any authorization information;
    * used to look at the authentication database on Authentication server:
      * Example:
        * *access_token*
        * *user_id*
        * *scope*
        * *expires*
  * Self-contain the authorization information:
    * contains much more information;
    * JSON object;
    * base64 encoded;
    * must not contain sensitive information;
    * separated in 3 parts (*by ' . '*):
      1. Header section: 
         1. how to validate the token
         2. type of the token
         3. how it was sign 
      2. Payload section:
         1. information or claims (custom claims)
      3. Signature:
         1. used to validate the access token


## OpenID Connect

Additional layer to provide the client application with information about the current authenticated user.

Additional layer on top of OAuth 2.0.

It is an identity layer that can provide the client application with the identity information about the user.

The Authorization server that supports **OpenID Connect** and provide client application with the identity information is also called **Identity Provider**.   

With the OpenID Connect, the Authorization server additionally with the *Access Token* provides and **ID Token**.   

### ID Token

The **ID Token** contains user identity information and the client application can use this *ID Token* to extract basic information for authenticated user.

**ID Token** used to declare who has to be and *Access Token* is used to validate is the user is allowed to access the requested resource from the *Resource server*.

---

## OAuth 2.0 Grant Types

> **Grand Type** is a way an application gets an access token.
 
* Selected by the case at the moment:
  * Server Side Web Application (**Authorization Code**)
  * Server Side Script with no UI (**Client credentials**)
  * Javascript Single Page Application (**PKCE Enhanced Authorization code**)
  * Mobile Native Application (**Authorization Code** or **PKCE Enhanced Authorization code**)
  * Devices (**Device code**)

**PKCE Enhanced Authorization code** - case when the application can not save the credentials confidential. 

**Device code** - case when the device can not open the browser or execute the code on the server side.

**Implicit Flow** - deprecated - used by JavaScript applications / Mobile applications.

**Password grant** - deprecated and **not recommended**.

**Refresh Token** - used to exchange a *refresh token* for an *access token*.


## PKCE-enhanced Authorization Code

***P**roof **K**ey for **C**ode **E**xchange*

* Most used by Native and JS applications.
* Uses *code_challenge*, *code_challenge_method* (S256 or plain) and *code_verifier*.
 

### Generating Code Verifier

Example in Java: 
*Generate random bytes, convert them to String and then Base64 encoder. 
The value is used in the request to exchange the authorization code for an access token.*

```java
  import java.io.UnsupportedEncodingException;
  import java.security.MessageDigest;
  import java.security.NoSuchAlgorithmException;
  import java.security.SecureRandom;
  import java.util.Base64;
  
  public class PkceUtil {
      
      String generateCodeVerifier() throws UnsupportedEncodingException {
          
          SecureRandom secureRandom = new SecureRandom();
          
          byte[] codeVerifier = new byte[32];
          
          secureRandom.nextBytes(codeVerifier);
          
          return Base64.getUrlEncoder().withoutPadding().encodeToString(codeVerifier);
      }
  }
```

### Generating Code Challenge


