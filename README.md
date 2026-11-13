# go-mod-version

A CircleCI orb that reads the Go version from `go.mod`.

The orb passes the version to your CircleCI pipeline. You can use it to
choose a Go Docker image without writing the version in your CircleCI
configuration.

## Features

- Reads the Go version from `go.mod`.
- Uses the `toolchain` version when it is available.
- Uses the `go` version when `toolchain` is missing or set to `default`.
- Passes the version to the continuation configuration as a pipeline parameter.

## Requirements

- CircleCI Dynamic Configuration must be enabled for your project.
- Your repository must have a `go.mod` file.

## Example

Create `.circleci/config.yml`:

```yaml
version: 2.1
setup: true

orbs:
  go-version: kyash/go-mod-version@0.0.4

workflows:
  setup:
    jobs:
      - go-version/prepare
```

Create `.circleci/continue_config.yml`:

```yaml
version: 2.1

parameters:
  go-version:
    type: string
    default: ""

jobs:
  build:
    docker:
      - image: cimg/go:<< pipeline.parameters.go-version >>
    steps:
      - checkout
      - run: go version
      - run: go build ./...

workflows:
  build:
    jobs:
      - build
```

For example, if your `go.mod` contains:

```go
go 1.26.0

toolchain go1.26.10
```

The orb passes `1.26.10` to the continuation configuration.
The `build` job then uses `cimg/go:1.26.10`.

## Contributing

Contributions are welcome!

If you find a bug or have an idea, please open an issue or a pull request.

## License

[MIT License](LICENSE)

Copyright (c) 2026 Kyash
