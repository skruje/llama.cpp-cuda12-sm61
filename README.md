# llama.cpp for Pascal GPUs (CUDA 12 / sm61)

## Overview

This repository contains a GitHub Actions workflow that automates the build process for the [llama.cpp](https://github.com/ggml-org/llama.cpp) project, specifically for Pascal generation GPUs (e.g., Tesla P4) using CUDA 12.2 and compute capability SM_61. The workflow checks for new releases of `llama.cpp`, compiles them with the necessary configurations, and packages the resulting binaries as a tarball for easy distribution.

## Why Use This?

- **Compatibility**: Ensures compatibility with older Pascal generation GPUs that may not support newer CUDA versions.
- **Automation**: Automates the build process to save time and reduce manual errors.
- **Ease of Use**: Provides pre-built binaries that can be easily downloaded and used without requiring extensive setup.

## How to Use

1. **Download the Tarball**:
   - Go to the [Releases page](https://github.com/your-repo/llama.cpp/releases) of this repository.
   - Download the latest release tarball (e.g., `llama-cuda12-sm61-<tag>.tar.gz`).

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
./llama-cli version
```

## Troubleshooting

- **NVIDIA Drivers**: Check that your NVIDIA drivers (tested on 535.309.01) are working with `nvidia-smi`.
- **CUDA Version**: Ensure you have CUDA 12.2 or later installed.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes.
