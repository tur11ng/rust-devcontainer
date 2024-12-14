FROM ubuntu:latest

RUN apt update

# Generic prerequisites
RUN apt install --yes build-essential git
RUN apt install --yes llvm clang \
    libclang-dev lld gdb clangd \
    curl

# Rust prerequisites
RUN curl https://sh.rustup.rs -sSf | \
    sh -s -- --default-toolchain stable -y
ENV PATH=/root/.cargo/bin:$PATH

# Rust for linux prerequisites
RUN rustup override set stable
RUN rustup component add rust-src
RUN rustup component add rustfmt
RUN rustup component add clippy
RUN cargo install --locked bindgen-cli

WORKDIR /workspaces