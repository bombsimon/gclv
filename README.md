# `gclv` - `golangci-lint` version manager

This is a version manager for [`golangci-lint`][golangci-lint]. It works similar
to `pyenv` or `rvm` where you manage multiple binaries through `gclv`.

## Usage

There are two ways to select `golangci-lint` version with `gclv`. For other
available commands and usage, use `gclv --help`.

### Manually

Just specify the version you want to use. If it does not exist locally it will
be downloaded. If it exists it will be re-linked. If it's already selected,
nothing will happen.

```sh
$ gclv --select v1.42.0                                                                                                                                                      130 ↵
v1.42.0 was not found - downloading
golangci/golangci-lint info checking GitHub for tag 'v1.42.0'
golangci/golangci-lint info found version: 1.42.0 for v1.42.0/darwin/arm64
golangci/golangci-lint info installed /Users/some.user/.golangci-lint/golangci-lint
Switching to v1.42.0
```

### With `golangci-lint` configuration

Since `golangci-lint` doesn't care about unknown configuration you can specify
what version you want to use in your [`.golangci.yml`][golangci-lint-config]
and `gclv` will be able to pick that up.

All you need to do is to add the key `golangci-lint-version` to your existing
configuration file. There are two files in the `examples` directory that can be
used to test this.

```sh
$ cd examples
$ config=golangci-1.40.yml

$ gclv --from-config $config
Switching to v1.40.0

$ golangci-lint --version
golangci-lint has version 1.40.0 built from 5c6adb63 on 2021-05-10T10:45:21Z

$ golangci-lint run --config $config
main.go:5: unnecessary leading newline (whitespace)
```

#### Wrapping `golangci-lint`

`gclv` also has a `--run` flag which can be used to determine version from a
`.golangci.yml` configuration file and after switching to that version run
`golangci-lint` with any additional flags.

If no configuration is passed, `golangci-lint config path` will be used to
determine which configuration to parse version from. If the configuration found
does not contain a version declaration (`golangci-lint-version`), whatever
version currently set will be used.

```sh
$ gclv --run ./examples --config examples/golangci-1.40.yml
Switching to v1.40.0
examples/main.go:5: unnecessary leading newline (whitespace)

$ gclv --run ./examples --config examples/golangci-1.43.yml
Switching to v1.43.0
examples/main.go:5: unnecessary leading newline (whitespace)

$ gclv --remove v1.41.1
Removed v1.41.1

$ gclv --run ./examples
v1.41.1 was not found - downloading
golangci/golangci-lint info checking GitHub for tag 'v1.41.1'
golangci/golangci-lint info found version: 1.41.1 for v1.41.1/darwin/arm64
golangci/golangci-lint info installed /Users/simon.sawert/.golangci-lint/golangci-lint
Switching to v1.41.1
examples/main.go:5: unnecessary leading newline (whitespace)
```

  [golangci-lint]: https://golangci-lint.run/
  [golangci-lint-config]: https://golangci-lint.run/usage/configuration/#config-file
