## Name
AWS SDK v1 to v2 Java Migration

## Objective
Convert Java/Kotlin applications that use AWS SDK v1 to use AWS SDK v2, ensuring all modules in multi-module projects are properly migrated to leverage AWS SDK v2's improved performance, non-blocking I/O, and reduced memory usage.

## Summary
This transformation migrates Java/Kotlin code from AWS SDK v1 to AWS SDK v2 by identifying all modules containing AWS SDK dependencies, updating build files across all modules, modifying imports, and converting client initialization patterns, API calls, and exception handling to match AWS SDK v2 patterns. The process includes comprehensive module discovery, systematic dependency updates, codebase modifications following consistent patterns, and thorough validation of each module to ensure complete migration.

## Entry Criteria
1. The project contains Java/Kotlin code that uses AWS SDK v1 libraries (identified by imports from the `com.amazonaws` package).
2. All modules in the project can be identified and analyzed for AWS SDK v1 dependencies.
3. A complete inventory of AWS services used in the project can be created.
4. The project's build system (Maven, Gradle) is identifiable and accessible.
5. The project can be successfully built in its current state with AWS SDK v1.
6. Appropriate test coverage exists to verify functionality after migration.

## Implementation Steps

### 1. Project and Module Analysis
1. **Identify all modules in the project**
   - Run command to discover all build files: `find . -name "build.gradle*" -o -name "pom.xml"`
   - Create an inventory of all modules and their locations
   - Verify the build system type (Maven, Gradle, Gradle Kotlin DSL)

2. **Analyze AWS SDK v1 usage across all modules**
   - For each module, identify presence of AWS SDK v1 dependencies in build files
   - Create a list of all modules that need to be migrated
   - Verify that all modules can be built successfully before migration

3. **Create a module dependency graph**
   - Identify dependencies between modules
   - Determine optimal module migration order based on dependencies
   - Document special considerations for inter-module dependencies

4. **Inventory AWS services used in each module**
   - For each module, identify all AWS services being used
   - Map each v1 service dependency to its v2 equivalent
   - Create a module-specific migration checklist

### 2. Dependency Updates
1. **Update build files for each identified module**
   - Replace v1 dependencies with v2 equivalents following this pattern:
     * v1: `com.amazonaws:aws-java-sdk-[service]:1.x.x`
     * v2: `software.amazon.awssdk:[service]:2.x.x`
   - Add HTTP client dependencies if needed:
     * `software.amazon.awssdk:apache-client` or
     * `software.amazon.awssdk:netty-nio-client` for async operations
   - Verify each build file update before proceeding to the next module
   - Document all dependency changes made for each module

2. **Verify dependency resolution**
   - Run dependency resolution for each updated module
   - Check for version conflicts or missing dependencies
   - Resolve any dependency issues before proceeding

### 3. Code Transformation - By Module
For each module containing AWS SDK v1 code, perform the following transformations:

1. **Identify all files that use AWS SDK v1 in the module**
   - Search for imports from `com.amazonaws` packages
   - Create a list of files that need to be modified
   - Group files by AWS service usage

2. **Update imports**
   - Replace `import com.amazonaws.*` with `import software.amazon.awssdk.*`
   - Update model class imports to their v2 equivalents
   - Remove any unused imports after migration

3. **Transform client initialization**
   - Convert client creation from direct instantiation to builder pattern:
     * v1: `AmazonS3 s3Client = AmazonS3ClientBuilder.standard().build();`
     * v2: `S3Client s3Client = S3Client.builder().build();`
   - Add region configuration to client builders
   - Configure HTTP clients where necessary
   - Implement client reuse patterns (static clients or dependency injection)

4. **Update service operations**
   - Transform request/response patterns to builder pattern:
     * v1: `ListObjectsRequest request = new ListObjectsRequest().withBucketName("bucket");`
     * v2: `ListObjectsRequest request = ListObjectsRequest.builder().bucket("bucket").build();`
   - Update method calls for each service operation
   - Convert response handling to match v2 API patterns

5. **Update exception handling**
   - Replace v1 checked exceptions with v2 unchecked exceptions
   - Update exception types in catch blocks
   - Add appropriate error handling for service-specific exceptions

6. **Transform pagination**
   - Replace manual pagination with v2 paginators
   - Update iteration over paginated results
   - Optimize resource handling for large result sets

7. **Implement resource cleanup**
   - Add try-with-resources for all client objects
   - Ensure proper closing of resources in both synchronous and asynchronous operations

8. **Convert asynchronous operations**
   - Replace v1 callbacks with CompletableFuture
   - Update asynchronous operation patterns
   - Implement proper thread management

9. **Module-specific verification**
   - Compile the modified module
   - Fix any compilation errors before proceeding to the next module
   - Run module-specific unit tests if available

### 4. Integration and Testing
1. **Verify cross-module functionality**
   - Test interactions between modules that use AWS services
   - Verify that module dependencies function correctly with the migrated code
   - Fix any integration issues between modules

2. **Run end-to-end tests**
   - Execute the full test suite
   - Verify that all AWS SDK v2 functionality works as expected
   - Document any behavioral differences between v1 and v2 implementations

### 5. Final Verification and Documentation
1. **Verify complete removal of v1 dependencies**
   - Check all build files to ensure no v1 dependencies remain
   - Verify that no v1 imports are present in any file
   - Ensure all identified modules were successfully migrated

2. **Generate migration report**
   - Document all modules that were migrated
   - List all services that were converted from v1 to v2
   - Note any special considerations or workarounds implemented
   - Record any features that required significant changes

3. **Update application documentation**
   - Document any API changes that affect application behavior
   - Update any internal documentation that references AWS SDK usage
   - Provide guidance on AWS SDK v2 best practices

## Validation / Exit Criteria
1. All identified modules with AWS SDK v1 dependencies have been migrated to use AWS SDK v2.
2. No references to `com.amazonaws` packages remain in the codebase.
3. All build files have been updated to use AWS SDK v2 dependencies.
4. The entire project successfully builds with the updated dependencies.
5. All tests pass, including unit tests, integration tests, and any AWS service-specific tests.
6. Each migrated module compiles and functions correctly individually.
7. Module interactions that involve AWS services continue to function as expected.
8. The application behavior remains consistent with pre-migration functionality.
9. No AWS SDK v1 runtime errors occur during application execution.
10. Performance metrics (response times, resource usage) are comparable or improved.
11. A comprehensive migration report has been created documenting all changes made.
12. All AWS SDK v2-specific best practices have been implemented consistently across all modules.

## AWS SDK v1 to v2 Mappings

### Dependency Pattern
- v1: com.amazonaws:aws-java-sdk-[service]:1.x.x
- v2: software.amazon.awssdk:[service]:2.x.x

### Common Service Mappings
1. S3:
   - v1: com.amazonaws:aws-java-sdk-s3
   - v2: software.amazon.awssdk:s3

2. DynamoDB:
   - v1: com.amazonaws:aws-java-sdk-dynamodb
   - v2: software.amazon.awssdk:dynamodb
   - v2 Enhanced: software.amazon.awssdk:dynamodb-enhanced

3. SQS:
   - v1: com.amazonaws:aws-java-sdk-sqs
   - v2: software.amazon.awssdk:sqs

4. SNS:
   - v1: com.amazonaws:aws-java-sdk-sns
   - v2: software.amazon.awssdk:sns

5. Lambda:
   - v1: com.amazonaws:aws-java-sdk-lambda
   - v2: software.amazon.awssdk:lambda

6. EC2:
   - v1: com.amazonaws:aws-java-sdk-ec2
   - v2: software.amazon.awssdk:ec2

7. CloudWatch:
   - v1: com.amazonaws:aws-java-sdk-cloudwatch
   - v2: software.amazon.awssdk:cloudwatch

8. ECS:
   - v1: com.amazonaws:aws-java-sdk-ecs
   - v2: software.amazon.awssdk:ecs

### HTTP Client Dependencies
- Apache HTTP Client: software.amazon.awssdk:apache-client
- Netty NIO Client: software.amazon.awssdk:netty-nio-client

## Common Migration Patterns

### Client Initialization
- v1: 
```
AmazonS3 s3Client = AmazonS3ClientBuilder.standard()
    .withRegion(Regions.US_WEST_2)
    .build();
```

- v2:
```
S3Client s3Client = S3Client.builder()
    .region(Region.US_WEST_2)
    .build();
```

### Request Creation
- v1:
```
ListObjectsRequest request = new ListObjectsRequest()
    .withBucketName("my-bucket")
    .withPrefix("my-prefix/");
```

- v2:
```
ListObjectsRequest request = ListObjectsRequest.builder()
    .bucket("my-bucket")
    .prefix("my-prefix/")
    .build();
```

### Exception Handling
- v1:
```
try {
    s3Client.getObject(new GetObjectRequest("bucket", "key"));
} catch (AmazonServiceException e) {
    // Handle service exception
} catch (AmazonClientException e) {
    // Handle client exception
}
```

- v2:
```
try {
    s3Client.getObject(GetObjectRequest.builder().bucket("bucket").key("key").build());
} catch (S3Exception e) {
    // Handle all S3 exceptions
}
```

### Pagination
- v1:
```
ListObjectsRequest request = new ListObjectsRequest().withBucketName("bucket");
ObjectListing result;
do {
    result = s3Client.listObjects(request);
    for (S3ObjectSummary summary : result.getObjectSummaries()) {
        // Process object
    }
    request.setMarker(result.getNextMarker());
} while (result.isTruncated());
```

- v2:
```
ListObjectsRequest request = ListObjectsRequest.builder().bucket("bucket").build();
s3Client.listObjectsPaginator(request).objectSummaries().forEach(summary -> {
    // Process object
});
```

### Asynchronous Operations
- v1:
```
s3Client.getObjectAsync(new GetObjectRequest("bucket", "key"), new AsyncHandler<GetObjectRequest, GetObjectResult>() {
    @Override
    public void onSuccess(GetObjectRequest request, GetObjectResult result) {
        // Handle success
    }
    
    @Override
    public void onError(Exception e) {
        // Handle error
    }
});
```

- v2:
```
S3AsyncClient s3AsyncClient = S3AsyncClient.builder().build();
s3AsyncClient.getObject(
        GetObjectRequest.builder().bucket("bucket").key("key").build(),
        AsyncResponseTransformer.toBytes())
    .thenAccept(result -> {
        // Handle success
    })
    .exceptionally(e -> {
        // Handle error
        return null;
    });
```
