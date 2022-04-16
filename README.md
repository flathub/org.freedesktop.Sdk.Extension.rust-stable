# SDK Extension for Rust stable

This extension contains various components of the [Rust](https://www.rust-lang.org) stable toolchain.


## Mold

In order to use the (fast) [mold](https://github.com/rui314/mold) linker:

1. Add "org.freedesktop.Sdk.Extension.llvm12" next to this extension in order to get clang
2. Add `.cargo/config.toml` with the following content

```toml
[target.x86_64-unknown-linux-gnu]
linker = "clang"
rustflags = ["-C", "link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold"]
```
