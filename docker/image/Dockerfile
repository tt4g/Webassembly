ARG RUST_VERSION=1.69.0
# https://hub.docker.com/_/rust
FROM rust:${RUST_VERSION}-slim-bullseye

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    # For downalod install shell.
    curl \
    # For install Wasmtime. https://wasmtime.dev/install.sh is using `xz` command.
    xz-utils \
    # Development tools.
    git sudo \
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
RUN curl -sSf --output /tmp/zig-0.11.0.tar.xz https://ziglang.org/builds/zig-linux-x86_64-0.11.0-dev.2892+fd6200eda.tar.xz \
    && [ "c69183bb8988f19add1cbcb0f08b9c651f475825bfe56d6381dfc045d989ba15  /tmp/zig-0.11.0.tar.xz" = "$(sha256sum /tmp/zig-0.11.0.tar.xz)" ] \
    && mkdir /zig \
    && xz -dc /tmp/zig-0.11.0.tar.xz | tar -xf - -C /zig \
    && ln -s /zig/zig-linux-x86_64-0.11.0-dev.2892+fd6200eda/zig /usr/local/bin/zig

# Install Wasmtime.
RUN curl https://wasmtime.dev/install.sh -sSf | bash /dev/stdin --version "v8.0.1"
