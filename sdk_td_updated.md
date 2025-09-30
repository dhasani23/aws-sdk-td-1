## Name
AWS SDK v1 to v2 Migration

## Objective
Convert Java/Kotlin applications that use AWS SDK v1 to use AWS SDK v2, ensuring all modules in multi-module projects are properly migrated to leverage AWS SDK v2's improved performance, non-blocking I/O, and reduced memory usage.

## Summary
This transformation migrates Java/Kotlin code from AWS SDK v1 to AWS SDK v2 by identifying all modules containing AWS SDK dependencies, updating build files across all modules, modifying imports, and converting client initialization patterns, API calls, and exception handling to match AWS SDK v2 patterns. The process includes comprehensive module discovery, systematic dependency updates, codebase modifications following consistent patterns, and thorough validation of each module to ensure complete migration.

## Entry Criteria
1. The project contains Java/Kotlin code that uses AWS SDK v1 libraries (identified by imports from the `com.amazonaws` package).
2. All modules in the project can be identified and analyzed for AWS SDK v1 dependencies.
3. A complete inventory of all AWS services used in the project can be created.
4. The project's build system (Maven, Gradle, etc.) is identifiable and accessible.
5. The project can be successfully built in its current state with AWS SDK v1.

## Best Practices for Migration
- Update Dependencies: Replace v1 dependencies with v2 in all build files
- Update Imports: Change package imports from `com.amazonaws.*` to `software.amazon.awssdk.*`
- Client Creation: Use the builder pattern consistently
- Request/Response Objects: Use builder pattern for all requests
- Exception Handling: Update to use unchecked exceptions
- Pagination: Use paginators for better handling of paginated results
- Resource Cleanup: Use try-with-resources as v2 clients implement AutoCloseable
- Async Operations: Update to use CompletableFuture instead of callbacks

## Important Guardrails
Strictly follow these rules at all times:
- Do NOT disable any tests
- Do NOT disable any plugins
- Do NOT downgrade the JDK version anywhere

## Validation / Exit Criteria
1. All identified modules with AWS SDK v1 dependencies have been migrated to use AWS SDK v2.
2. No references to `com.amazonaws` packages remain in the codebase.
3. All build files in all modules have been updated to use AWS SDK v2 dependencies.
4. The entire project successfully builds with the updated dependencies.
5. Each migrated module compiles and functions correctly individually.
8. No AWS SDK v1 runtime errors occur during application execution.
9. A comprehensive migration report has been created documenting all changes made.
10. All AWS SDK v2-specific best practices have been implemented consistently across all modules.
