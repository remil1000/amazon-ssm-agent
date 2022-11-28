# Build in container

Targets are in the format: `build-$(os)-(arch)`

Currently:

- `build-linux-amd64`
- `build-linux-arm64`
- `build-darwin-amd64`
- `build-darwin-arm64`

```
make -f Makefile.release build-linux-amd64
```

or with overriden version value

```
VERSION=3.2.286.0 make -f Makefile.release build-linux-amd64
```

Output release (zip file), will be pushed in the `release/` directory

# Sync to S3 bucket

You **must** provide an `AWS_PROFILE` environment variable - check
`Makefile.internal` for valid profiles

```
AWS_PROFILE=sandbox make -f Makefile.release sync
```

```
VERSION=3.2.286.0 AWS_PROFILE=sandbox make -f Makefile.release sync
```


## Override version

`VERSION` is extracted from `git describe --tags` however you may want to
override this value for a test or prerelease.

You can show the inferred version with:

```
make -f Makefile.release show-version
3.2.286.0-4-gc2b1a535
```

and for example override it with:

```
VERSION=3.2.286.0 make -f Makefile.release build-linux-amd64
```

## Clean

```
make -f Makefile.release clean
```

Will call the underlying upstream `makefile` `clean` target, remove the
`release` directory and also revert changes in 3 files used by upstream to
track the SSM agent version
