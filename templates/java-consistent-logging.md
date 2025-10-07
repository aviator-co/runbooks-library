# Implement Consistent Logging Patterns for Java Services

Establish standardized logging practices across all Java microservices to improve observability, debugging, and monitoring capabilities.

## Summary of changes

- Current Java services use inconsistent logging frameworks (mix of java.util.logging, Log4j, Logback) with varying log formats
- Implement centralized logging configuration with structured JSON output and consistent log levels
- Add correlation ID tracking, performance metrics logging, and standardized error handling patterns
- Ensure all services use SLF4J facade with Logback as the implementation for consistency

## Execution Steps

### Step 1: Logging Infrastructure Setup

Establish the foundation for consistent logging by creating shared configuration and utilities.

### 1.1: Create Shared Logging Configuration Module

Create a new Maven module that will contain shared logging configurations and utilities for all Java services.

- Create new Maven module **logging-commons** in the root of the project with appropriate **pom.xml**
- Add dependencies for **SLF4J API**, **Logback Classic**, and **Jackson** for JSON formatting
- Create **LoggingConstants.java** class with standardized log level constants and message formats
- Create **CorrelationIdFilter.java** for HTTP request correlation ID generation and MDC population

### 1.2: Design Standard Logback Configuration

Implement a base Logback configuration that all services will inherit from.

- Create **logback-spring.xml** template with JSON console appender using **JsonEncoder**
- Configure log patterns to include timestamp, level, service name, correlation ID, thread, and message
- Add conditional file appender configuration for production environments
- Create **application-logging.properties** with externalized logging level configurations

### 1.3: Implement Logging Utility Classes

Build helper classes to standardize common logging patterns across services.

- Create **StructuredLogger.java** wrapper class for consistent structured logging methods
- Implement **PerformanceLogger.java** for standardized method execution time logging
- Create **SecurityAuditLogger.java** for security-related events with consistent format
- Add **LoggingTestUtils.java** for testing logging behavior in unit tests

### Step 2: Service Migration Strategy

Migrate existing services to use the new logging infrastructure in a phased approach.

### 2.1: Update Core Service Dependencies

Modify the most critical service first to establish the migration pattern.

- Update **user-service/pom.xml** to include **logging-commons** dependency and remove old logging dependencies
- Add **@Import(LoggingConfiguration.class)** to main Spring Boot application class
- Replace existing logger declarations with **StructuredLogger** instances throughout the codebase
- Update all log statements to use new structured logging methods with consistent parameters

### 2.2: Migrate Remaining Services

Apply the same migration pattern to all other Java services in the system.

- Update **order-service**, **payment-service**, and **notification-service** pom.xml files with new dependencies
- Replace logger instances and update log statements in each service following the established pattern
- Add correlation ID filter registration to each service's **WebMvcConfigurer** implementation
- Ensure all exception handling uses standardized error logging patterns from **StructuredLogger**

### 2.3: Update Integration Tests

Modify integration tests to work with the new logging infrastructure.

- Update **application-test.properties** files in all services to use appropriate test logging levels
- Modify integration test classes to use **LoggingTestUtils** for verifying log output
- Add tests to verify correlation ID propagation across service calls
- Update Docker compose files for local testing to capture structured JSON logs

### Step 3: Advanced Logging Features

Implement additional logging capabilities for better observability and monitoring.

### 3.1: Add Performance Monitoring

Integrate performance logging into critical service methods.

- Create **@LogExecutionTime** annotation for automatic method performance logging
- Implement aspect-oriented programming (AOP) aspect **PerformanceLoggingAspect** to handle the annotation
- Add performance logging to all **@RestController** methods and critical **@Service** methods
- Configure performance thresholds in **application.properties** for slow operation alerts

### 3.2: Implement Security Audit Logging

Add comprehensive security event logging across all services.

- Integrate **SecurityAuditLogger** into Spring Security authentication and authorization flows
- Add security event logging to all authentication attempts, authorization failures, and sensitive operations
- Implement audit logging for data access patterns and administrative actions
- Create security log aggregation configuration for centralized security monitoring

### 3.3: [NO-CODE-CHANGE] Create Logging Documentation

Document the new logging standards and practices for the development team.

- Create **LOGGING_STANDARDS.md** documentation with coding guidelines and examples
- Document correlation ID flow patterns and best practices for distributed tracing
- Create troubleshooting guide for common logging configuration issues
- Prepare training materials for development team on new logging practices

### Step 4: Monitoring and Alerting Integration

Configure monitoring systems to work with the new structured logging format.

### 4.1: Update Log Aggregation Configuration

Modify existing log aggregation systems to parse the new JSON log format.

- Update **Fluentd** or **Logstash** configuration files to parse JSON structured logs
- Configure log routing rules based on service names and log levels
- Add correlation ID indexing for distributed request tracing
- Update log retention policies for different log levels and service types

### 4.2: Create Logging Dashboards

Build monitoring dashboards to visualize the structured log data.

- Create **Grafana** dashboards for service health monitoring based on error log frequency
- Implement performance monitoring dashboards using execution time logs
- Create correlation ID trace visualization for distributed request tracking
- Add alerting rules for critical error patterns and performance threshold breaches

## Manual testing plan

- Deploy updated services to staging environment and verify JSON log output format in container logs
- Generate test requests across multiple services and verify correlation ID propagation through log aggregation system
- Trigger various error conditions and verify consistent error log format and security audit entries
- Test performance logging by executing slow operations and verifying execution time logs appear with correct thresholds
- Verify log level configuration changes take effect without service restart using Spring Boot Actuator
- Test log aggregation pipeline by searching for correlation IDs across multiple services in log management system
- Validate monitoring dashboards display accurate service health and performance metrics from structured logs