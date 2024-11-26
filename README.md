# HELLO

- I had the idea after reading the article [MOCGuard: Automatically Detecting Missing-Owner-Check Vulnerabilities in Java Web Applications](https://www.computer.org/csdl/proceedings-article/sp/2025/223600a010/21B7Q2KLNn2) and I found it very interesting and decided to stop for a while and write this simple code to help anyone who is interested.

# Main Features

- Automatic vulnerability scanning in user tables
- Record integrity analysis
- Detailed record of potential security risks
- Native integration with Spring Framework
- Support for PostgreSQL databases

# Installation

- Make sure to include the following dependencies in your project (for example, in Maven):

            <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-data-jpa</artifactId>
          </dependency>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
          <dependency>
              <groupId>com.h2database</groupId>
              <artifactId>h2</artifactId>
              <scope>runtime</scope>
          </dependency>

# MOCGuardController

- REST controller for exposing verification functionality, endpoint:

          GET /api/mocguard/check: Triggers vulnerability checking

- The system implements an ownership check based on:

- Comparing User ID to Record Owner ID
- Logging potential vulnerabilities
- Exception handling to ensure robustness

# Security Considerations

- The solution assumes that:

- The first field in each table is the user ID
- The second field represents the owner ID

Additional validation in a production environment is recommended

# Error Handling

- Uses Java Logging for detailed recording of:

- Exceptions during verification
- Vulnerabilities detected
- Processing errors

# Known Limitations

- Current support limited to PostgreSQL databases
- Need for manual table mapping configuration
- Performance may be impacted in tables with a large volume of records

# Settings

You can perform configuration by creating the ***application.properties file***

            mocguard.enabled=true
            mocguard.log-level=WARNING
