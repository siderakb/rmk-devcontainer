
FROM debian:bookworm-slim

# Target architectures:
# "thumbv6m-none-eabi"         Cortex-M0 and Cortex-M0+
# "thumbv7m-none-eabi"         Cortex-M3
# "thumbv7em-none-eabi"        Cortex-M4 and Cortex-M7 (no FPU)
# "thumbv7em-none-eabihf"      Cortex-M4F and Cortex-M7F (with FPU)
# "thumbv8m.base-none-eabi"    Cortex-M23
# "thumbv8m.main-none-eabi"    Cortex-M33 (no FPU)
# "thumbv8m.main-none-eabihf"  Cortex-M33 (with FPU)
ARG TARGET_ARCH=thumbv7em-none-eabihf

# Install basic dependencies and create developer user
RUN apt update && apt install -y --no-install-recommends \
    curl \
    ca-certificates \
    sudo \
    && adduser --disabled-password --gecos '' developer \
    && echo 'developer ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Install Rust
USER developer
WORKDIR /home/developer
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/home/developer/.cargo/bin:${PATH}"

USER root

RUN apt update && apt install -y --no-install-recommends \
    clang \
    libclang-dev \
    build-essential \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

ENV LIBCLANG_PATH=/usr/lib/llvm-14/lib

RUN echo 'export LIBCLANG_PATH=/usr/lib/llvm-14/lib' >> ~/.bashrc

USER developer

RUN rustup target add ${TARGET_ARCH}

RUN cargo install rmkit flip-link cargo-make

RUN echo 'alias rmkuf2="cargo make uf2 --release"' >> ~/.bashrc \
 && echo 'alias rmkbuild="cargo build --release"' >> ~/.bashrc

WORKDIR /workspace
