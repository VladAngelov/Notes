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


### Authorization Code Grant

Example:
*Website visitor decide to connect with his Google / Facebook account and fetch some data. 
So he clicks on the 'Connect with account' button and the website redirect the user to the Authorization server url.* 

| KEY | VALUE | COMMENT |
| --- | ----- | ------- |
| response_type | code | Required - "code" |
| state | some_text_for_state | Recommended |
| redirect_uri | user_password | Optional |
| scope | {Scopes} | Optional. Type of information to access. |
| client_id | {client_id} | Required | 


### PKCE-enhanced Authorization Code

***P**roof **K**ey for **C**ode **E**xchange*

* Most used by Native and JS applications.
* Uses *code_challenge*, *code_challenge_method* (S256 or plain) and *code_verifier*.
 
Request parameters:

| KEY | VALUE | COMMENT |
| --- | ----- | ------- |
| response_type | code | Required - "code" |
| state | some_text_for_state | Recommended |
| redirect_uri | user_password | Optional |
| scope | {Scopes} | Optional. Type of information to access. |
| client_id | {client_id} | Required | 
| code_challenge | {code_challenge} | Base64 value - *code_verifier* |
| code_challenge_method | {code_challenge_method} | ex.: *S256* or *plain* |
| code_verifier | {code_verifier} | For Exchange OAuth Code - without *code_challenge* and *code_challenge_method* |


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


### Generating Code Challenge *value*
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
      // ...
      
      // codeVerifier from generateCodeVerifier()
      String generateCodeChallenge(String codeVerifier) throws UnsupportedEncodingException {          
          byte[] bytes = codeVerifier.getBytes("US-ASCII");
          
          MessageDigest messageDigest = MessageDigest.getInstance("SHA-256");
          messageDigest.update(bytes, 0, bytes.length);
          
          byte[] digest = messageDigest.digest();
          
          return Base64.getUrlEncoder().withoutPadding().encodeToString(digest);
      }
  }
```

### Client Credentials
*Machine to Machine Request*

Mostly the *Client Application* running on the server side, example:
*Spring Boot Microservices Application that needs to request data from another Spring Boot Microservice.
If there is no access token, it will send request for to the Authorization Server to request the access token. 
This time it will request directly to the token endpoint with **grant_type=client_credentials** and the client credentials (client_id & client_secret).
After the response with the access token, the application can request data from the Resource Server.* 

| KEY | VALUE | 
| --- | ----- | 
| grant_type | client_credentials |
| scope | {Scopes} |
| client_id | client_id_for_the_provider |
| client_secret | client_secret_from_the_provider |


### Password Grant
***Only if the application does not support redirect!***

*Resource Owner* have to provide Username and Password to the *Client* to acquire *Access Token* form *Authorization Server*.

In the post:

| KEY | VALUE |
| --- | ----- | 
| grant_type | password |
| username | user_name |
| password | user_password |
| client_id | client_id_for_the_provider |
| client_secret | client_secret_from_the_provider | 
| scope | profile |

 
## Refresh Token

Example: 

* First authentication:

|   |     |     |     |
|---| --- | --- | --- |
| Client Application | Request an Access Token | ---> | Authorization Server |
| Client Application | <--- | Access Token & Refresh Token | Authorization Server |
| Client Application | Request protected resource | ---> | Resource Server |
| Client Application | <--- | Protected Resource | Resource Server |

* When the token is expired:

|   |   |     |     |
|---| --- | --- | --- |
| Client Application | Request protected resource | ---> | Resource Server |
| Client Application | <--- | Invalid Token | Resource Server |

* User Refresh Token: 

|   |   |     |     |
| --- | --- | --- | --- |
| Client Application | Refresh Access Token | ---> | Authorization Server |
| Client Application | <--- | New Access Token & Refresh Token (optionally) | Authorization Server |

### Refresh Token that never expires 
*Request example with Password grant type*

Params:

| KEY | VALUE |
| --- | ----- | 
| grant_type | password |
| username | user_name |
| password | user_password |
| client_id | client_id_for_the_provider |
| client_secret | client_secret_from_the_provider | 
| scope | profile offline_access |

Response:
* access_token: ...
* expires_in: ...
* **refresh_expires_in: 0** (*seconds*)
* refresh_token: ...
* token type: ...

### Refresh the Refresh Token

| KEY | VALUE |
| --- | ----- | 
| grant_type | refresh_token |
| client_id | client_id_for_the_provider |
| client_secret | client_secret_from_the_provider | 
| refresh_token | refresh_token_from_previous_request |


---

## Keycloak

* Keycloak is an open source Identity and Access Management solution.
* Supports Single-Sign On (SSO).
* Social Login.
* User Federation


### Realms

* Master - the top realm, can add, edit or delete other realms.
* Realm - configure user's rights policy, limits, roles and groups.
* Create new Clients

### Create a new OAuth client application

* Client ID - name of the client
* Client Protocol - openid-connect / saml
* Root URl - optional
* Settings:
  * only 1 required field - *Valid Redirect URIs* (after success log in)

### Generate secret key

1. Set the *Access Type* to **confidential** (default - public) in the *Settings* tab in the "*Clients*".
2. Get already generated secret or "*Regenerate Secret*"


### Enable / Disable OAuth 2.0 Authorization Flow

1. Administration console --> Clients --> Settings --> Standard Flow Enabled --> OFF
2. Administration console --> Clients --> Settings --> Advanced Settings --> Proof Key for Code Exchange Code Challenge Method --> S265 / plain / empty
3. Administration console --> Clients --> Settings --> Save

### OAuth 2.0 Client Scopes

Administration console --> Client Scopes --> Edit (for already existing scope) --> Client Scopes (tab) --> select necessary scopes 

---

## Resource Server

* Create project with dependencies:
  * Spring Web
  * Spring Boot DevTools
  * OAuth2 Resource Server
* Start as a normal Maven project.
* Settings(yaml) file:
  * Port
  * Access Token uri for correct authentication 


### Use the token

Example for using JWT with controller:

```java
import java.util.Collections;
import org.springframework.security.core.annotation.AuthenticationPrincipal;
import org.springframework.security.oauth2.jwt.Jwt;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/token")
public class TokenController {
    
    public String getToken(@AuthenticationPrincipal Jwt jwt) {
        
        // Through - jwt. we can access all jwt properties / data
        
        return jwt.getTokenValue();
        
        // or
        
        return Collections.singleton("principal", jwt); // Map<String, Object>
    }
}
```

## Scope-Base Access Control
* Definition:
  * Scope is a mechanism in OAuth 2.0 to limit an application access ti a user's account. An application can request one or more scopes, 
  this information is then presented to the user in the consent screen, 
  and the token issued to the application will be limited to the scopes granted.

### WebSecurityConfigurerAdapter and enable Web Security

> WebSecurityConfigurerAdapter **is deprecated**!
> 
> Replaced by **SecurityFilterChain**

Example:

```java
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

@EnableWebSecurity
public class WebSecurity extends SecurityFilterChain {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(
                    authz -> authz.anyRequest().authenticated())
            .httpBasic(Customizer.withDefaults());
     
        return http.build();
    }
}
```

---

## Spring Security

### Roles

| User | Admin | Super Admin |
| ---- | ----- | ----------- |
| View profile | View profile | View profile |
| View other users | View other users | View other users |    
| Edit own profile | Edit own profile | Edit own profile |    
|                  | Delete other users | Delete other users |
|                  |                    | Edit/Delete other Admins |    


### Authority

Authority name = Role Name = ROLE_ADMIN

|     |     |     |  
| --- | --- | --- |
| hasRole("ADMIN") | \  |
|  |  | Collection< GrantedAuthority> |
| hasAuthority("ROLE_ADMIN") | / | |


### Role converting

If we want to convert some role, we have to use *org.springframework.core.convert.converter.Converter*


For example in this case is used Keycloak:

1. Add converter class, which implements *Converter*:

```java
import org.springframework.core.convert.converter.Converter;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.oauth2.jwt.Jwt;

import java.util.ArrayList;
import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class KeycloakRoleConverter implements Converter<Jwt, Collection<GrantedAuthority>> {

    @Override
    public Collection<GrantedAuthority> convert(Jwt jwt) {
        Map<String, Object> realmAccess = (Map<String, Object>) jwt.getClaims().get("realm_access");

        if (realmAccess == null || realmAccess.isEmpty()) {
            return new ArrayList<>();
        }
        Collection<GrantedAuthority> returnValue = ((List<String>) realmAccess.get("roles"))
                .stream().map(roleName -> "ROLE_" + roleName)
                .map(SimpleGrantedAuthority::new)
                .collect(Collectors.toList());

        return returnValue;
    }
}
```

2. Add fields in config file:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.oauth2.server.resource.authentication.JwtAuthenticationConverter;
import org.springframework.security.web.SecurityFilterChain;

import javax.servlet.Filter;
import javax.servlet.http.HttpServletRequest;
import java.util.List;

@EnableWebSecurity
public class WebSecurity implements SecurityFilterChain {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        JwtAuthenticationConverter jwtAuthenticationConverter = new JwtAuthenticationConverter();
        jwtAuthenticationConverter.setJwtGrantedAuthoritiesConverter(new KeycloakRoleConverter());

        http
            .authorizeHttpRequests(
                    auth -> auth.anyRequest().authenticated())
            .httpBasic(Customizer.withDefaults())
            .oauth2ResourceServer()
            .jwt()
            .jwtAuthenticationConverter(jwtAuthenticationConverter);

        return http.build();
    }
}
```

--- 

## Method-Level Security

We can use annotations to denied or allow execution of method based on conditions.

* Some examples:
  * user's role 
  * user's authority
  * @PreAuthorize:
   ```java
    public class UserController { 
        @PreAuthorize("hasAuthority('DELETE_AUTHORITY') or #id == principal.userId")
        @DeleteMapping(path = '/{id}')
        public ResponseEntity deleteUser(@PathVariable String id) {
            // some code here
        }   
    }
    ```
  * @PostAuthorize - first execute the method, then check if the return object is allowed for this user:
  ```java
  public class UserController { 
     @PostAuthorize("hasRole('ADMIN') or returnObject.userId == principal.userId")
     @GetMapping(path = '/{id}')
     public UserRest getUser(@PathVariable String id) {
        UserRest returnValue = new UserRest();
  
         // some code here
  
        return returnValue;
     } 
  }
    ```

The annotations can be used on class level.

The annotations of method **have higher priority** then the annotations of the class.  

Example:
```java
@RestController
@Secured("ROLE_ADMIN")
public class UserController { 
     @PreAuthorize("permitAll")
     @GetMapping(path = "/user/status/check")
     public String userStatusCheck() {
       
        return "Working for users";
     } 
     
     @GetMapping(path = "/managers/status/check")
     public String managersStatusCheck() {
       
        return "Working for managers";
     } 
 }
```

To enable we have to add **@EnableGlobalMethodSecurity** / (*@EnableGlobalMethodSecurity(securedEnabled=true)*)
from *org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity*.


```java
public class UserController {
    @PostAuthorize("returnObject.userId == #jwt.subject")
    @GetMapping(path = "/{id}")
    public UserRest getUser(@PathVariable String id, @AuthenticationPrincipal Jwt jwt) {
        return new UserRest("user_firs_name", "user_last_name", "user_id");
    }
}
```

---

## Resource Server Behind the API Gateway
