## Name
AWS SDK v1 to v2 Java Migration.

## Objective
Convert Java code that uses AWS SDK v1 to use AWS SDK v2.

## AWS SDK v1 to v2 Mappings

### Dependency Pattern
- v1: com.amazonaws:aws-java-sdk-[service]:1.x.x
- v2: software.amazon.awssdk:[service]:2.x.x

### Examples
1. S3:
   - v1: com.amazonaws:aws-java-sdk-s3
   - v2: software.amazon.awssdk:s3

2. SQS:
   - v1: com.amazonaws:aws-java-sdk-sqs
   - v2: software.amazon.awssdk:sqs

3. Lambda:
   - v1: com.amazonaws:aws-java-sdk-lambda
   - v2: software.amazon.awssdk:lambda

## Best Practices for Migration
- Update Dependencies: Replace v1 dependencies with v2 in your build file
- Update Imports: Change package imports from `com.amazonaws.*` to `software.amazon.awssdk.*`
- Client Creation: Use the builder pattern consistently
- Request/Response Objects: Use builder pattern for all requests
- Exception Handling: Update to use unchecked exceptions
- Pagination: Use paginators for better handling of paginated results
- Resource Cleanup: Use try-with-resources as v2 clients implement AutoCloseable
- Async Operations: Update to use CompletableFuture instead of callbacks
