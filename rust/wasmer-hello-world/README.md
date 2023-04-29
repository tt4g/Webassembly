Waster: Hello World project

### Create project

```shell
# Create Project.
$ cargo new wasmer-hello-world --bin
$ cd wasmer-hello-world
# Add target
$ rustup target add wasm32-wasi
```

### Build WASI

```shell
# Build
$ cargo build --target wasm32-wasi
$ wasmer ./target/wasm32-wasi/debug/wasmer-hello-world.wasm

# Create executable from `.wasm`
$ wasmer create-exe target/wasm32-wasi/debug/wasmer-hello-world.wasm -o wasmer-hello-world
```
