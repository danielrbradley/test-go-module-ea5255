# Testing how Go resolves tags outside of main

## Part 1

1. Push main branch with just readme
2. Push a stable tag parented to main, with a go module in a sub folder.
3. Push a higher, alpha tag, parented to main, with the same go module in a sub-folder.
4. Check if [pkg.go.dev](https://pkg.go.dev/github.com/danielrbradley/test-go-module-ea5255/testmodule) loads
5. Use `go install` to test resolving the module without a version

Expectation - this won't resolve.

Repeat, but pushing a non-alpha tag.

## Part 2

1. Add a stub go module on the main branch and push
2. Re-test fetching @latest via pkg.go.dev and the CLI

Expectation - the latest stable version will resolve
