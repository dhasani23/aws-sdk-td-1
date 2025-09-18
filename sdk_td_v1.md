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

## Important Verification Criteria
- Do not add new dependencies to build files if they are already included as transitive dependencies of other necessary dependencies. For example, `software.amazon.awssdk:auth:2.21.42` is a transitive dependency of `software.amazon.awssdk:s3:2.21.42`. If `s3` needs to be upgraded, it is redundant to also add the `auth` dependency to the build file. If needed, you can use `./gradle dependencies --configuration runtimeClasspath` to show all project dependencies, with the transitive ones being indented within their parent dependencies.
- Review the plan.json generated at the beginning (including any feedback provided to the plan by the user) to ensure each step was performed as stated. Pay special attention to multi-module apps: ensure that each module containing AWS SDK dependencies is upgraded as specified in the plan.
- Do not create brand new files to upgrade to AWS SDK V2: replace existing V1 functionality instead. However, new test files are acceptable, if needed.
- Do not terminate the transformation before going through each of these verification criteria.
