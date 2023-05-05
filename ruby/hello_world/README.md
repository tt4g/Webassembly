Run Ruby code with [ruby.wasm](https://github.com/ruby/ruby.wasm).

## Wasmer

Run.

```shell
$ wasmer /ruby-wasm/3_2/usr/local/bin/ruby --mapdir /src:./src -- /src/main.rb 
```

## Wasmtime

Run.

```shell
$ wasmtime /ruby-wasm/3_2/usr/local/bin/ruby --dir=./src -- ./src/main.rb
```

## Packing

Packing ruby.wasm with [wasi-vfs](https://github.com/kateinoigakukun/wasi-vfs).

Generate a new `hello_world.wasm` with `/src` added to the virtual file system.
The entry point is `ruby.wasm`. `/usr` directory needed by `ruby.wasm` is also added to the virtual file system.

```shell
$ wasi-vfs pack /ruby-wasm/3_2/usr/local/bin/ruby --mapdir /src::./src --mapdir /usr::/ruby-wasm/3_2/usr -o hello_world.wasm
```

### Wasmtime

Run.

```shell
$ wasmer ./hello_world.wasm -- /src/main.rb
```

Create executable.

```shell
$ wasmer create-exe ./hello_world.wasm -o hello_world

# Note: This `/src/main.rb` is the path in the virtual file system.
#  It is not the host machine path.
$ ./hello_world /src/main.rb
```

### Wasmtime

Run.

```shell
$ wasmtime ./hello_world.wasm  -- /src/main.rb
```
