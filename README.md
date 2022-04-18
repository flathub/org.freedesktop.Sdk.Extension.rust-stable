# SDK Extension for Rust stable

This extension contains various components of the [Rust](https://www.rust-lang.org) stable toolchain.


## Mold

In order to use the (fast) [mold](https://github.com/rui314/mold) linker:

1. Add `org.freedesktop.Sdk.Extension.llvm13` allong with this extension in order to get clang.
2. Add `/usr/lib/sdk/llvm13/lib` in `append-ld-library-path` and `append-path`. See [llvm13 SDK extension readme](https://github.com/flathub/org.freedesktop.Sdk.Extension.llvm13) for more information.
3. Add `config.toml` with the following content in `CARGO_HOME` directory:

```toml
[target.x86_64-unknown-linux-gnu]
linker = "clang"
rustflags = ["-C", "link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold"]
```

Note: llvm13 is needed until there is a release of gcc12.1.
As soon as gcc12.1 is in the freedesktop sdk, gcc can be used instead of clang. Additionally, step 1 and 2 can be skipped.
