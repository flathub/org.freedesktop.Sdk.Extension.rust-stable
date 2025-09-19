# SDK Extension for Rust stable

This extension contains various components of the [Rust](https://www.rust-lang.org) stable toolchain.

To be able to use the Rust binaries at build time, add the following to your Flatpak manifest:

```json
{
    "sdk-extensions": [
        "org.freedesktop.Sdk.Extension.rust-stable"
    ],
    "build-options": {
        "append-path": "/usr/lib/sdk/rust-stable/bin",
    }
}
```

## Mold

This extension bundles also the (fast) [`mold`](https://github.com/rui314/mold) linker, which can be
used to improve linking time.

### With gcc

1. Add this extension.
2. Set the `RUSTFLAGS` environment variable to `-C link-arg=-fuse-ld=mold`.

In total, the changed parts of your Flatpak manifest should look like this:

```json
{
    "sdk-extensions": [
        "org.freedesktop.Sdk.Extension.rust-stable"
    ],
    "build-options": {
        "append-path": "/usr/lib/sdk/rust-stable/bin",
        "env": {
            "RUSTFLAGS": "-C link-arg=-fuse-ld=mold"
        }
    }
}
```

### With clang

1. Add this extension.
2. Add a version of the [`org.freedesktop.Sdk.Extension.llvm{version}`](https://github.com/flathub?q=org.freedesktop.Sdk.Extension.llvm)
   extension compatible with the `runtime` in your Flatpak manifest.
   
   For example, if your runtime is based on the `org.freedesktop.Platform//25.08` runtime, you can
   use the [llvm20](https://github.com/flathub/org.freedesktop.Sdk.Extension.llvm20)
   or [llvm21](https://github.com/flathub/org.freedesktop.Sdk.Extension.llvm21) extensions.
   
   See the README of the chosen extension for more information.
4. Set the following environment variables for each target triple that you want to build:
    - `CARGO_TARGET_{TARGET_TRIPLE}_LINKER` to `clang`, and
    - `CARGO_TARGET_{TARGET_TRIPLE}_RUSTFLAGS` to `-C link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold`.
    
    The `TARGET_TRIPLE`s supported by Flathub are `X86_64_UNKNOWN_LINUX_GNU` and
    `AARCH64_UNKNOWN_LINUX_GNU`.

In total, the changed parts of your Flatpak manifest should look like this:

```json
{
    "sdk-extensions": [
        "org.freedesktop.Sdk.Extension.rust-stable",
        "org.freedesktop.Sdk.Extension.llvm21"
    ],
    "build-options": {
        "append-path": "/usr/lib/sdk/rust-stable/bin:/usr/lib/sdk/llvm21/bin",
        "env": {
            "CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_LINKER": "clang",
            "CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_RUSTFLAGS": "-C link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold",
            "CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_LINKER": "clang",
            "CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_RUSTFLAGS": "-C link-arg=-fuse-ld=/usr/lib/sdk/rust-stable/bin/mold"
        }
    }
}
```

## Debugging/Development

In order to use this extension in flatpak SDK environment you may add all provided tools in your PATH by executing first:
```
source /usr/lib/sdk/rust-stable/enable.sh
```

You can also combine this extension with `lldb` using the LLVM SDK extension. See the extension's [readme](https://github.com/flathub/org.freedesktop.Sdk.Extension.llvm20) for more information.
