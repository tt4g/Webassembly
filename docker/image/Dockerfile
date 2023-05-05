ARG RUST_VERSION=1.69.0
# https://hub.docker.com/_/rust
FROM rust:${RUST_VERSION}-slim-bullseye

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    # For downalod install shell.
    curl \
    unzip \
    # For install Wasmtime. https://wasmtime.dev/install.sh is using `xz` command.
    xz-utils \
    # Development tools.
    git sudo jq \
    # Install required packages for `cargo-edit`.
    libssl-dev pkg-config \
    python3=3.9.2-3 \
    ruby=1:2.7+2 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN rustup component add rls rust-analysis rust-src rustfmt clippy \
    && cargo install cargo-edit cargo-watch

# Install Wasmer.
ENV WASMER_DIR="${HOME}/.wasmer"
RUN curl https://get.wasmer.io -sSfL | sh -s "v3.2.1"
ENV PATH="${PATH}:${WASMER_DIR}/bin"

# Install Zig.
# `wasmer crate-exe` command requires Zig for cross-compiling.
RUN curl -sSf --output /tmp/zig-linux-x86_64-0.11.0.tar.xz https://ziglang.org/builds/zig-linux-x86_64-0.11.0-dev.2892+fd6200eda.tar.xz \
    && [ "c69183bb8988f19add1cbcb0f08b9c651f475825bfe56d6381dfc045d989ba15  /tmp/zig-linux-x86_64-0.11.0.tar.xz" = "$(sha256sum /tmp/zig-linux-x86_64-0.11.0.tar.xz)" ] \
    && mkdir /zig \
    && xz -dc /tmp/zig-linux-x86_64-0.11.0.tar.xz | tar -xf - -C /zig \
    && rm /tmp/zig-linux-x86_64-0.11.0.tar.xz \
    && ln -s /zig/zig-linux-x86_64-0.11.0-dev.2892+fd6200eda /zig/zig-linux-x86_64 \
    # Add to `$PATH`.
    && ln -s /zig/zig-linux-x86_64/zig /usr/local/bin/zig

# Install Wasmtime.
RUN curl https://wasmtime.dev/install.sh -sSf | bash /dev/stdin --version "v8.0.1"

# Install wasi-vfs.
RUN curl -LsSf --output /tmp/wasi-vfs-cli-x86_64-unknown-linux-gnu.zip "https://github.com/kateinoigakukun/wasi-vfs/releases/download/v0.2.0/wasi-vfs-cli-x86_64-unknown-linux-gnu.zip" \
    && [ "22f961afb44fdb2abeb049261c4e102dd6537b2db439cb97167a7c52342c2142  /tmp/wasi-vfs-cli-x86_64-unknown-linux-gnu.zip" = "$(sha256sum /tmp/wasi-vfs-cli-x86_64-unknown-linux-gnu.zip)" ] \
    && mkdir /wasi-vfs \
    && unzip /tmp/wasi-vfs-cli-x86_64-unknown-linux-gnu.zip -d /wasi-vfs \
    && rm /tmp/wasi-vfs-cli-x86_64-unknown-linux-gnu.zip \
    # Add to `$PATH`.
    && ln -s /wasi-vfs/wasi-vfs /usr/local/bin/wasi-vfs

# Download ruby.wasm.
RUN curl -LsSf --output /tmp/ruby-3_2-wasm32-unknown-wasi-full.tar.gz https://github.com/ruby/ruby.wasm/releases/latest/download/ruby-3_2-wasm32-unknown-wasi-full.tar.gz \
    && [ "9559f64d408f5c24c7fd8829905df363898ca229a9f838a2b4d465a2476703ff  /tmp/ruby-3_2-wasm32-unknown-wasi-full.tar.gz" = "$(sha256sum /tmp/ruby-3_2-wasm32-unknown-wasi-full.tar.gz)" ] \
    && mkdir /ruby-wasm \
    && tar -C /ruby-wasm -xzf /tmp/ruby-3_2-wasm32-unknown-wasi-full.tar.gz \
    && rm /tmp/ruby-3_2-wasm32-unknown-wasi-full.tar.gz \
    && ln -s /ruby-wasm/3_2-wasm32-unknown-wasi-full /ruby-wasm/3_2
