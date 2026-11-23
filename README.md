# llama.cpp for Pascal GPUs (CUDA 12 / sm61)

## Overview

This repository contains a GitHub Actions workflow that automates the build process for the [llama.cpp](https://github.com/ggml-org/llama.cpp) project, specifically for Pascal generation GPUs (e.g., Tesla P4) using CUDA 12.9 and compute capability SM_61. The workflow checks for new releases of `llama.cpp` (both numbered releases such as `1.0.0` and `b####` pre-releases), compiles them with the necessary configurations, and packages the resulting binaries as a tarball for easy distribution.

CUDA 12.9 is used because it is the **last CUDA 12.x release** and the last one that can still target Pascal. CUDA 13.0 dropped Maxwell, Pascal and Volta support entirely.

## Release Policy

- **Polling**: Upstream is checked every 6 hours. The newest numbered release and the newest `b####` release are each built if this repository doesn't already have a release with that tag.
- **Numbered releases** (e.g. `1.0.0`): published as normal releases and **kept forever**.
- **`b####` builds**: published as GitHub *pre-releases*. Only the **newest 10** are kept; older ones (and their tags) are deleted automatically. The count is set by `KEEP_PRERELEASES` in `.github/workflows/build.yml`.
- **Manual builds**: run the workflow from the Actions tab. Enter a `tag` (e.g. `1.0.0` or `b5000`) to build that specific upstream tag, or leave it empty to build whatever is new.
- **Version info**: the build number and commit hash are passed to the build explicitly, so `llama-server --version` reports the correct values (for `b####` builds the build number matches the tag).

## Why Use This?

- **Compatibility**: Ensures compatibility with older Pascal generation GPUs that may not support newer CUDA versions.
- **Automation**: Automates the build process to save time and reduce manual errors.
- **Ease of Use**: Provides pre-built binaries that can be easily downloaded and used without requiring extensive setup.

## How to Use

1. **Download the Tarball**:
   - Go to the [Releases page](https://github.com/your-repo/llama.cpp/releases) of this repository.
   - Download the latest release tarball (e.g., `llama-cuda12-sm61-<tag>.tar.gz`). Pre-releases (`b####`) are the newest builds; numbered releases are the stable ones.

2. **Extract the Tarball**:
   ```sh
   tar -xzvf llama-cuda12-sm61-<tag>.tar.gz
   ```

3. **Run the Binaries**:
   - Navigate to the extracted directory.
   - Run the desired binary (e.g., `./llama-cli`).

### Bundled Libraries

The tarball is self-contained apart from the GPU driver and the base OS:

```
llama-cuda12-sm61-<tag>/
├── llama-cli
├── llama-server
├── llama-bench
└── lib/            # CUDA runtime + other shared libraries
```

The binaries are patched with an RPATH of `$ORIGIN/lib`, so they load the bundled libraries automatically. **You do not need the CUDA toolkit installed** — only a working NVIDIA driver (`libcuda.so.1`), which the driver package provides.

Bundled: CUDA runtime (`libcudart`, `libcublas`, `libcublasLt`), OpenSSL, and any other non-system shared libraries the binaries need.

Not bundled (expected from the host):
- `libcuda.so.1` and `libnvidia-*` — installed with the NVIDIA driver.
- glibc, `libstdc++`, `libgcc_s` — part of the base OS.

Keep the `lib/` folder next to the binaries; moving a binary out of the folder will break it unless you also copy `lib/` or set `LD_LIBRARY_PATH`.

### Two Tarballs

The libraries only change when the CUDA version changes, so they are published **once per CUDA version** instead of being duplicated in every build:

| Release | Contents | Changes when |
|---|---|---|
| `llama-cuda12-sm61-<tag>` | binaries only (~small) | every llama.cpp build |
| `cuda-12.9-libs` | `lib/` with the CUDA runtime + OpenSSL | the CUDA version changes |

Download both and extract the libraries into the binaries folder:

```sh
tar -xzvf llama-cuda12-sm61-<tag>.tar.gz
tar -xzvf cuda-12.9-libs.tar.gz -C llama-cuda12-sm61-<tag>
```

The `cuda-12.9-libs` release also ships a `libs.txt` listing exactly which libraries are inside.

### Example

```sh
# Download and extract the binaries
wget https://github.com/skruje/llama.cpp-cuda12-sm61/releases/download/<tag>/llama-cuda12-sm61-<tag>.tar.gz
tar -xzvf llama-cuda12-sm61-<tag>.tar.gz

# Download and extract the CUDA libraries into the same folder
wget https://github.com/skruje/llama.cpp-cuda12-sm61/releases/download/cuda-12.9-libs/cuda-12.9-libs.tar.gz
tar -xzvf cuda-12.9-libs.tar.gz -C llama-cuda12-sm61-<tag>

# Navigate to the extracted directory
cd llama-cuda12-sm61-<tag>

# Run the llama-cli binary
./llama-cli --version
```

Example output (build number and commit are now filled in; values are illustrative):

```
version: 1.0.0 (build 9000, commit abc1234)
built with GNU 11.4.0 for Linux x86_64
```

## Troubleshooting

- **NVIDIA Drivers**: Check that your NVIDIA drivers are working with `nvidia-smi`. The driver must be **≥ 525.60.13** (any CUDA 12.x driver works, thanks to CUDA minor version compatibility).
- **CUDA Toolkit**: Not required. The CUDA runtime libraries are bundled in `lib/`.
- **`libcuda.so.1: cannot open shared object file`**: The NVIDIA driver is missing or not loaded on the host.
- **`error while loading shared libraries: libcublas.so.12`**: The `cuda-12.9-libs` tarball was not extracted into the binaries folder, or `lib/` was moved away from the binaries.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes.
