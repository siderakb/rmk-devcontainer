
FROM mcr.microsoft.com/devcontainers/rust:1-1-bookworm

# Target architectures:
# "thumbv6m-none-eabi"         Cortex-M0 and Cortex-M0+
# "thumbv7m-none-eabi"         Cortex-M3
# "thumbv7em-none-eabi"        Cortex-M4 and Cortex-M7 (no FPU)
# "thumbv7em-none-eabihf"      Cortex-M4F and Cortex-M7F (with FPU)
# "thumbv8m.base-none-eabi"    Cortex-M23
# "thumbv8m.main-none-eabi"    Cortex-M33 (no FPU)
# "thumbv8m.main-none-eabihf"  Cortex-M33 (with FPU)
ARG TARGET_ARCH=thumbv7em-none-eabihf

USER root

RUN apt update && apt install -y --no-install-recommends \
    clang \
    libclang-dev \
    build-essential \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

ENV LIBCLANG_PATH=/usr/lib/llvm-14/lib

RUN echo 'export LIBCLANG_PATH=/usr/lib/llvm-14/lib' >> ~/.bashrc

USER vscode

RUN rustup target add ${TARGET_ARCH}

RUN cargo install rmkit flip-link cargo-make

WORKDIR /workspace
