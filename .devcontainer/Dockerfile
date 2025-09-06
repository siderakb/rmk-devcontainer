FROM rust:1-slim-bookworm

ARG TARGET_ARCH=thumbv7em-none-eabihf
ARG LLVM_VERSION=14

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    git \
    # build-essential \
    clang-${LLVM_VERSION} \
    libclang-${LLVM_VERSION}-dev \
    && rm -rf /var/lib/apt/lists/*

ENV LIBCLANG_PATH=/usr/lib/llvm-${LLVM_VERSION}/lib

RUN rustup target add ${TARGET_ARCH} \
    && cargo install rmkit flip-link cargo-make \
    && useradd -m -s /bin/bash developer \
    && mkdir -p /usr/local/cargo /usr/local/rustup \
    && chown -R developer:developer /usr/local/cargo /usr/local/rustup

USER developer
RUN echo 'alias rmkuf2="cargo make uf2 --release"' >> ~/.bashrc \
    && echo 'alias rmkbuild="cargo build --release"' >> ~/.bashrc

# ENV CARGO_BUILD_TARGET=${TARGET_ARCH}

WORKDIR /workspace
