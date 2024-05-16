- Differenza tra ID Token e Access Token
- Definizione Scope

# Spring Boot Security with OAuth 2.0 Example

This is a simple Spring Boot application demonstrating how to implement security using Spring Security with OAuth 2.0. The application exposes a protected endpoint that requires authentication using OAuth 2.0 tokens provided by Azure AD.

## Overview

The project is organized into the following components:

### Security Configuration

- **`SecurityConfig`**: This class configures Spring Security to enforce authentication for all HTTP requests..

### Controller and Protected Endpoint

- **`DataController`**: This class defines a REST controller with an endpoint `/api/data` that returns a simple response. This endpoint is protected and requires authentication to be accessed.

### Properties Configuration

- **`application.properties`**: This file contains configuration properties for Spring Security OAuth 2.0, including the issuer URI and the URI to fetch the JWT key set.

### Security Filter

#### How the Filter Works

The `SecurityConfig` class defines a security filter chain that acts as a bearer token authentication mechanism for incoming HTTP requests. Each request is intercepted and its `Authorization` header is inspected to extract the JWT (JSON Web Token) bearer token. This token is then validated using cryptographic operations against the public keys exposed by the identity provider through a JWKS (JSON Web Key Set) endpoint.

During the validation process, the JWT is checked for its signature's validity, expiration, and issuer authenticity. If the token fails any of these checks, or if it's missing or malformed, the request will be rejected with a 401 Unauthorized response.

This approach ensures that only requests bearing valid JWT tokens issued by the trusted identity provider (Azure AD EnterID) are allowed to access the protected endpoints of the application. Unauthorized requests are promptly rejected, maintaining the security and integrity of the system.


Here's the code snippet from `SecurityConfig` that configures the security filter chain:

```java
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
                .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
                .httpBasic(Customizer.withDefaults());

        return http.build();
    }

}
```

The `filterChain` method configures the security filter chain to require authentication for all HTTP requests (`anyRequest().authenticated()`) and uses HTTP Basic authentication with default settings (`httpBasic(Customizer.withDefaults())`).

## Getting Started

To run the project, follow these steps:

1. Clone this repository to your local machine.
2. Set the `ISSUER_URI` in the `application.properties` file to your Azure AD issuer URI.
3. Open a terminal and navigate to the project directory.
4. Run the project using Maven:

```bash
mvn spring-boot:run -f pom.xml -DISSUER_URI=YOUR_ISSUER_URI
```

Replace `YOUR_ISSUER_URI` with your actual Azure AD issuer URI.

Once the application is running, you can access the protected endpoint at `http://localhost:8080/api/data`. However, attempting to access this endpoint without proper authentication will result in a 401 Unauthorized response.

## Contributing

Contributions are welcome! If you find any issues or have suggestions for improvements, please feel free to open an issue or create a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Feel free to customize the overview further to include additional information or instructions specific to your project or environment.