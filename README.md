# Testing how Go resolves tags outside of main

## Testing Plan

### Part 1

1. Push main branch with just readme
2. Push a stable tag parented to main, with a go module in a sub folder.
3. Push a higher, alpha tag, parented to main, with the same go module in a sub-folder.
4. Check if [pkg.go.dev](https://pkg.go.dev/github.com/danielrbradley/test-go-module-ea5255/testmodule) loads
5. Use `go install` to test resolving the module without a version

Expectation - this won't resolve.

Repeat, but pushing a non-alpha tag.

### Part 2

1. Add a stub go module on the main branch and push
2. Re-test fetching @latest via pkg.go.dev and the CLI

Expectation - the latest stable version will resolve

## Findings

### First Publishing

- When first published, fetching the package using either the package search web interface or using the CLI to download `@latest` failed.
- The first release had to fetched explicitly on the CLI: `go get github.com/danielrbradley/test-go-module-ea5255/testmodule@v1.0.0`
- Once fetched via the CLI, the module then appeared correctly at [pkg.go.dev/github.com/danielrbradley/test-go-module-ea5255/testmodule](https://pkg.go.dev/github.com/danielrbradley/test-go-module-ea5255/testmodule).

### Updates

- After pushing a new tag, the tag does not immediately appear in the web interface. Putting the explicit version in the URL prompt to fetch, which then works.
- After pushing a new tag, running `go get github.com/danielrbradley/test-go-module-ea5255/testmodule@latest` works as expect, and also causes the new tag to appear in the web UI too.

### Recommendations

After we publish any new version, we should proactively call `go get ...` using the exact version we pushed to force the cache to be populated which will also cause it to appear in the Go Pkg UI.
