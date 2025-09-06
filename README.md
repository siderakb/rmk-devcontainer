# RMK Docker & DevContainers

[RMK](https://github.com/HaoboGu/rmk) Rust-based keyboard firmware Docker image and development container setup.

## Usage

Open **VS Code** -> `Ctrl`+`Shift`+`P` → **Dev Containers: Reopen in Container**

```bash
# Inside the container...
cd example-nrf5840

# Build .hex firmware
cargo build --release
# or
rmkbuild

# Build .uf2 firmware
cargo make uf2 --release
# or
rmkuf2
```

### Create a New RMK Project

Update `TARGET_ARCH` and rebuild if your project uses a different MCU architecture, select your MCU target architecture and update the `TARGET_ARCH` value in `devcontainer.json`.

| TARGET\_ARCH                | MCU                               |
| --------------------------- | --------------------------------- |
| `thumbv6m-none-eabi`        | Cortex-M0, Cortex-M0+             |
| `thumbv7m-none-eabi`        | Cortex-M3                         |
| `thumbv7em-none-eabi`       | Cortex-M4, Cortex-M7 (no FPU)     |
| `thumbv7em-none-eabihf`     | Cortex-M4F, Cortex-M7F (with FPU) |
| `thumbv8m.base-none-eabi`   | Cortex-M23                        |
| `thumbv8m.main-none-eabi`   | Cortex-M33 (no FPU)               |
| `thumbv8m.main-none-eabihf` | Cortex-M33 (with FPU)             |

Inside the *container*:

```bash
rmkit init
```

Refer to [Local compilation: Create firmware project](https://rmk.rs/docs/user_guide/2-2_local_compilation.html#create-firmware-project).

### On the Host

Build the Docker image:

```bash
docker build --build-arg TARGET_ARCH=thumbv7em-none-eabihf -t rmk-dev ./.devcontainer
# or
podman build --build-arg TARGET_ARCH=thumbv7em-none-eabihf -t rmk-dev ./.devcontainer
```

Build firmware directly:

```bash
podman run --rm \
       -v "${PWD}:/workspace" \
       -w /workspace \
       rmk-dev \
       bash -c "cd example-nrf52840 && cargo build --release"
```
