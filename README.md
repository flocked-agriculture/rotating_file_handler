# Rotating File Handler

[![Crates.io Version](https://img.shields.io/crates/v/rotating_file_handler.svg)](https://crates.io/crates/rotating_file_handler)
[![Documentation](https://docs.rs/rotating_file_handler/badge.svg)](https://docs.rs/rotating_file_handler)
![Build + Test](https://github.com/flocked-agriculture/rotating_file_handler/actions/workflows/main_ci.yml/badge.svg?branch=main)
![Build + Test + Deploy](https://github.com/flocked-agriculture/rotating_file_handler/actions/workflows/release.yml/badge.svg)

This crate contains a simple rotating file handler. It is intended for high frequency robust telemetry logging on physical systems.

## Technical Requirements

1. Must support flushing a provided record immediately so that in the event of powerloss timely log data is still present (RIP Ingenuity or Ginny).
2. Must support custom file headers.
3. Must be safe against corruption from async, threading, and parallel processing.
4. Must support the write trait.

## Installation

Install with cargo:

`cargo add rotating_file_handler`

## Example

Simple example for creating a file handler and logging binary data.

```rust
use std::io::Write;

use rotating_file_handler::RotatingFileHandler;

fn main() -> std::io::Result<()> {
    let mut handler = RotatingFileHandler::new("docs_log.txt", 1024, 3, None)?;
    handler.emit(b"Hello, world!")?;
    handler.emit(b"Logging some more data...")?;
    Ok(())
}
```

## License

Licensed under either of the following:

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

## Contribution

Public contribution is welcome and encouraged. Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.

### Documentation

This is a relatively simple developer facing crate so documentation should primarily exist in line per the rust documentation. Anything that does not associated with a specific block of code such as crate requirements, installation instructions, etc should be documented in this README.md.

### Code Standards

- all code should be formatted per cargo's default fmt command
- code should target 80% automated code coverage

### Release Process

Releases are managed through both git tags and branches. Branches are used for convenience and tags actually trigger the relevant release actions. Whenever there is a new major or minor release a branch must be created at the relevant hash in the format v\<major\>.\<minor\> (ie v1.33). Branches with such a format are protected by a ruleset and can only be modified by admins. All release tags must point to hashes on said branch. There is also a ruleset protecting all git tags matching the semantic versioning format v*.*.\* so that only admins can add such tags.

#### Major or Minor Release

In summary, you must be an admin and complete the following steps:

- pick a hash
- confirm all automated tests have passed
- create a branch at the relevant hash in the format v\<major\>.\<minor\> (ie v1.33).
- if necessary perform any last minuted changes
- create a git tag pointing to the tip of that branch in the format v\<major\>.\<minor\>.0 (ie v1.33.0).

The git tag will kick off an automated process that deploys the crate to crates.io after validating crate version matches the tag version and all automated tests pass.

## Future Work

- support async
- ensure file system interaction is safe
- ensure that another process removing a file being written to does not disrupt logging
- allow for optional use of buffer writing to speed up data transfers at the code of requirement 1. maybe utilize rust features for this?
