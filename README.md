# CIAI
CIAI: Comprehensive Interface for Autonomous Integration
A standardized driver integration protocol for synthetic biology automation, enabling seamless, secure, and decoupled interaction between device drivers and upper systems. Similar to SILA2 but optimized for synthetic biology scenarios, CIAI simplifies driver development, ensures cross-system compatibility, and supports independent iteration of upper systems and device ends.
Core Features
Standardized Interface Set: 7 core modular interfaces (Function/Operation/Set/Get/EnterAndExit/Info/HeartBeat) cover all device interaction scenarios.
Complete Decoupling: Separates upper system iteration from device driver development, no mutual impact during upgrades.
Defense-in-Depth Security: TLS 1.3 encryption, two-way authentication, JWT authorization, and AES-256 data encryption meet level-3 cybersecurity requirements.
Multi-Language Support: Official scaffolding for Java and C# (out-of-the-box with interface templates and security configurations).
Easy Integration: Unified request/response formats, dynamic form configuration, and resource management mechanisms reduce development costs.
Full Lifecycle Management: Supports device registration, status monitoring, exception handling, and callback notification.
Interface Architecture
CIAI defines 7 mandatory core interfaces to standardize device-driver interaction:
Interface	Purpose
Function	Core business logic execution (e.g., measurement, cleaning)
Operation	Device debugging and diagnostic operations
Set	Dynamic configuration of device runtime parameters
Get	Real-time query of device status information
EnterAndExit	Management of device access/exit processes in functional islands
Info	Device metadata registration and driver information provision
HeartBeat	Online status monitoring and health check
Request/Response Standard
Request Method: POST for operational interfaces (Function/Operation/Set/EnterAndExit), GET for query interfaces (Get/HeartBeat/Info).
Data Format: JSON (Content-Type: application/json).
Response Structure: Generic Result<T> with status code, message, and business data.
Callback Mechanism: Asynchronous Finish callback for operation completion notification (supports success/error status and error messages).
Security Protocol
CIAI adopts a multi-layered security system to protect interface calls and data privacy:
Transport Layer Security: TLS 1.3 with ECDHE-RSA-AES256-GCM-SHA384 encryption suite; supports one-way/two-way certificate authentication.
Identity Authentication: OAuth 2.0 + JWT token mechanism with RBAC permission control and Redis-based token management.
Data Protection: AES-256-GCM end-to-end encryption + HMAC-SHA256 message authentication; sensitive data desensitization in logs.
Driver Legitimacy: Registration review and digital certificate verification to prevent unauthorized driver access.
Implementation Support
Supported Languages
Java (Spring Boot)：Complete templates for interfaces, security configuration, and thread pool management.
C#：Scaffolding with equivalent interface definitions and security mechanisms (coming soon).
Dependencies
Java: JDK 8+, Spring Boot 2.7+, Redis (for token management).
C#: .NET 6+, Redis (for token management).
Supporting Tools: 星磐驱动客户端 (for certificate management, configuration synchronization, and log management).
Quick Start
Download Scaffolding：Clone the repo and select the Java/C# scaffolding directory.
Configure Security: Import CA certificates via 星磐证书生成器，configure TLS and JWT parameters (see application.yml templates).
Implement Interfaces: Use provided FunctionData, SetData, GetReturn etc. models to implement core business logic.
Register Driver: Complete device metadata registration via the Info interface (supports dynamic form configuration).
Test Integration: Verify interface calls and callbacks with upper systems (compatible with 星磐驱动客户端 for debugging).
Example Code Snippets
Java Function Interface Template
java
@RequestMapping(value = "/Function", method = RequestMethod.POST)
public Result<Finish> executeFunction(@RequestBody FunctionData data) {
    // Asynchronous task execution
    functionService.executeAsync(data);
    return Result.success();
}
Security Configuration (TLS 1.3)
yaml
server:
  ssl:
    enabled: true
    key-store: classpath:client.p12
    key-store-password: your_password
    key-store-type: PKCS12
    protocol: TLSv1.3
    ciphers: TLS_AES_256_GCM_SHA384, TLS_CHACHA20_POLY1305_SHA256
    client-auth: need # Two-way authentication
  port: 12398
Comparison with SILA2
Feature	CIAI	SILA2
Target Field	Synthetic biology automation	General lab automation
Core Focus	Driver-upper system decoupling	Standardized lab device commands
Security Mechanism	Built-in defense-in-depth system	Basic TLS encryption
Multi-Language Support	Java/C# scaffolding (official)	Limited to specific languages
Integration Complexity	Low (out-of-the-box templates)	Medium (custom configuration required)
Contribution
We welcome contributions to expand language support (Python/Go), add device-specific templates, or optimize security mechanisms. Please submit PRs following the code style guidelines in the CONTRIBUTING.md file.
License
This project is licensed under the Apache-2.0 License - see the LICENSE file for details.
