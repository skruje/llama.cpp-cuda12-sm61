# llama.cpp for Pascal GPUs (CUDA 12 / sm61)

## Overview

This repository contains a GitHub Actions workflow that automates the build process for the [llama.cpp](https://github.com/ggml-org/llama.cpp) project, specifically for Pascal generation GPUs (e.g., Tesla P4) using CUDA 12.2 and compute capability SM_61. The workflow checks for new releases of `llama.cpp` (both numbered releases such as `1.0.0` and `b####` pre-releases), compiles them with the necessary configurations, and packages the resulting binaries as a tarball for easy distribution.

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

### Example

```sh
# Download and extract the tarball
wget https://github.com/skruje/llama.cpp-cuda12-sm61/releases/download/<tag>/llama-cuda12-sm61-<tag>.tar.gz
tar -xzvf llama-cuda12-sm61-<tag>.tar.gz

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

- **NVIDIA Drivers**: Check that your NVIDIA drivers (tested on 535.309.01) are working with `nvidia-smi`.
- **CUDA Version**: Ensure you have CUDA 12.2 or later installed.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes.
