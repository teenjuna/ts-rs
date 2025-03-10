# ts-rs

<h1 align="center" style="padding-top: 0; margin-top: 0;">
<img width="150px" src="https://raw.githubusercontent.com/Aleph-Alpha/ts-rs/main/logo.png" alt="logo">
<br/>
ts-rs
</h1>
<p align="center">
generate typescript interface/type declarations from rust types
</p>

<div align="center">
<!-- Github Actions -->
<img src="https://img.shields.io/github/workflow/status/Aleph-Alpha/ts-rs/Test?style=flat-square" alt="actions status" />
<a href="https://crates.io/crates/ts-rs">
<img src="https://img.shields.io/crates/v/ts-rs.svg?style=flat-square"
alt="Crates.io version" />
</a>
<a href="https://docs.rs/ts-rs">
<img src="https://img.shields.io/badge/docs-latest-blue.svg?style=flat-square"
alt="docs.rs docs" />
</a>
<a href="https://crates.io/crates/ts-rs">
<img src="https://img.shields.io/crates/d/ts-rs.svg?style=flat-square"
alt="Download" />
</a>
</div>

### why?
When building a web application in rust, data structures have to be shared between backend and frontend.
Using this library, you can easily generate TypeScript bindings to your rust structs & enums so that you can keep your
types in one place.

ts-rs might also come in handy when working with webassembly.

### how?
ts-rs exposes a single trait, `TS`. Using a derive macro, you can implement this interface for your types.
Then, you can use this trait to obtain the TypeScript bindings.
We recommend doing this in your tests.
[See the example](https://github.com/Aleph-Alpha/ts-rs/blob/main/example/src/lib.rs) and [the docs](https://docs.rs/ts-rs/latest/ts_rs/).

### get started
```toml
[dependencies]
ts-rs = "6.1"
```

```rust
use ts_rs::TS;

#[derive(TS)]
#[ts(export)]
struct User {
    user_id: i32,
    first_name: String,
    last_name: String,
}
```
When running `cargo test`, the TypeScript bindings will be exported to the file `bindings/User.ts`.

### features
- generate interface declarations from rust structs
- generate union declarations from rust enums
- inline types
- flatten structs/interfaces
- generate necessary imports when exporting to multiple files
- serde compatibility
- generic types

### cargo features
- `serde-compat` (default)

  Enable serde compatibility. See below for more info.
- `format` (default)

  When enabled, the generated typescript will be formatted.
  Currently, this sadly adds quite a bit of dependencies.
- `chrono-impl`

  Implement `TS` for types from chrono
- `bigdecimal-impl`

  Implement `TS` for types from bigdecimal
- `uuid-impl`

  Implement `TS` for types from uuid
- `bytes-impl`

  Implement `TS` for types from bytes
- `indexmap-impl`

  Implement `TS` for `IndexMap` and `IndexSet` from indexmap

If there's a type you're dealing with which doesn't implement `TS`, use `#[ts(type = "..")]` or open a PR.

### serde compatability
With the `serde-compat` feature (enabled by default), serde attributes can be parsed for enums and structs.
Supported serde attributes:
- `rename`
- `rename-all`
- `tag`
- `content`
- `untagged`
- `skip`
- `skip_serializing`
- `skip_deserializing`
- `skip_serializing_if = "Option::is_none"`
- `flatten`
- `default`

When ts-rs encounters an unsupported serde attribute, a warning is emitted.

### contributing
Contributions are always welcome!
Feel free to open an issue, discuss using GitHub discussions or open a PR.
[See CONTRIBUTING.md](https://github.com/Aleph-Alpha/ts-rs/blob/main/CONTRIBUTING.md)

### todo
- [x] serde compatibility layer
- [x] documentation
- [x] use typescript types across files
- [x] more enum representations
- [x] generics
- [ ] don't require `'static`

License: MIT
