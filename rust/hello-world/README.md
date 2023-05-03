WASI: Hello World project

## Setup

### Create project

```shell
# Create Project.
$ cargo new hello-world --bin
$ cd hello-world
```

### Setup Rust

```shell
# Add target
$ rustup target add wasm32-wasi
```

### Build WASI

```shell
# Build
$ cargo build --target wasm32-wasi
```

#### Wasmer

Run.

```shell
$ wasmer ./target/wasm32-wasi/debug/hello-world.wasm
```

Create executable from `.wasm`.

```shell
# Avoid a Wasmer 3.x bug that could not build if filename contains hyphens.
# https://github.com/wasmerio/wasmer/issues/3834
$ mv ./target/wasm32-wasi/debug/hello-world.wasm ./target/wasm32-wasi/debug/hello_world.wasm

$ wasmer create-exe ./target/wasm32-wasi/debug/hello_world.wasm -o ./hello_world
```

Cross-Compilation: Wasmer 3.x supports cross compile with the
`--target <target-triple>` option. For possible values for `<target-triple>`,
See https://github.com/wasmerio/wasmer/blob/v3.2.1/lib/cli/src/commands/create_exe.rs#L55-L68
or `zig targets | jq ".libc"` to list Zig supports targets.

```shell
# Linux x64 GNU.
$ wasmer create-exe ./target/wasm32-wasi/debug/hello_world.wasm \
    --target x86_64-linux-gnu \
    -o ./hello_world

# MacOS x64.
$ wasmer create-exe ./target/wasm32-wasi/debug/hello_world.wasm \
    --target x86_64-apple-darwin \
    -o ./hello_world

# Windows x64 GNU (MinGW).
$ wasmer create-exe ./target/wasm32-wasi/debug/hello_world.wasm \
    --target x86_64-windows-gnu \
    -o ./hello_world.exe
```

#### Wasmtime

Run.

```shell
$ wasmtime ./target/wasm32-wasi/debug/hello-world.wasm
```
