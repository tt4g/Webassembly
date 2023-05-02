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

#### Wasmtime

Run.

```shell
$ wasmtime ./target/wasm32-wasi/debug/hello-world.wasm
```
