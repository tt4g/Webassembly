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

# Install Wasmer.
ENV WASMER_DIR="${HOME}/.wasmer"
RUN curl https://get.wasmer.io -sSfL | sh -s "v3.2.1"
ENV PATH="${PATH}:${WASMER_DIR}/bin"

# Install Wasmtime.
RUN curl https://wasmtime.dev/install.sh -sSf | bash /dev/stdin --version "v8.0.1"

RUN rustup component add rls rust-analysis rust-src rustfmt clippy \
    && cargo install cargo-edit cargo-watch
