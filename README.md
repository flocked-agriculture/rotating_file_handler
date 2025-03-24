# Rotating File Handler

This crate contains a simple rotating file handler. It is intended for high frquency robust telemetry logging on physical systems.

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

## Future Work

- support async
- ensure file system interaction is safe
- ensure that another process removing a file being written to does not disrupt logging
- allow for optional use of buffer writing to speed up data transfers at the code of requirement 1. maybe utilize rust features for this?
