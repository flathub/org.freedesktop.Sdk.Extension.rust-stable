# SDK Extension for Rust stable

This extension contains various components of the [Rust](https://www.rust-lang.org) stable toolchain.


## Mold

In order to use the (fast) [`mold`](https://github.com/rui314/mold) linker:

1. Add `org.freedesktop.Sdk.Extension.llvm13` along with this extension in order to get `clang`.
2. Add `/usr/lib/sdk/llvm13/bin` to `append-path`. See [llvm13 SDK extension readme](https://github.com/flathub/org.freedesktop.Sdk.Extension.llvm13) for more information.
3. Set environment variables:
    - `CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_LINKER` to `clang`, and
    - `CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_RUSTFLAGS` to `-C link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold`.

In total, the changed parts of your flatpak manifest should look like this:

```json
{
    "sdk-extensions" : [
        "org.freedesktop.Sdk.Extension.rust-stable",
        "org.freedesktop.Sdk.Extension.llvm13"
    ],
    "build-options": {
        "append-path" : "/usr/lib/sdk/rust-stable/bin:/usr/lib/sdk/llvm13/bin",
        "env": {
            "CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_LINKER": "clang",
            "CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_RUSTFLAGS": "-C link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold",
            "CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_LINKER": "clang",
            "CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_RUSTFLAGS": "-C link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold"
        }
    }
}
```

Note: `llvm13` is needed until there is a release of `gcc12.1`.
As soon as `gcc12.1` is in the freedesktop sdk, `gcc` can be used instead of `clang`.

## Debugging/Development

In order to use this extension in flatpak SDK environment you may add all provided tools in your PATH by executing first:
```
source /usr/lib/sdk/rust-stable/enable.sh
```

You can also combine this extension with `lldb` using the LLVM SDK extension. See the extension's [readme](https://github.com/flathub/org.freedesktop.Sdk.Extension.llvm13) for more information.
