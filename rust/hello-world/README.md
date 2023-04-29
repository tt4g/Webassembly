WASI: Hello World project

### Create project

```shell
# Create Project.
$ cargo new hello-world --bin
$ cd hello-world
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
$ wasmer create-exe target/wasm32-wasi/debug/hello-world.wasm -o ./hello-world
```

#### Wasmtime

Run.

```shell
$ wasmtime ./target/wasm32-wasi/debug/hello-world.wasm
```
