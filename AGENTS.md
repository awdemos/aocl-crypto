# AOCL-Cryptography — Agent Guide

## Overview

AMD AOCL-Cryptography fork. CMake-based C/C++ crypto library optimized for AMD Zen™.
Contains the core library (`lib/`), benchmarks (`bench/`), unit tests (`tests/`),
examples (`examples/`), and docs (`docs/`).

## Project Layout

| Path | Purpose |
|------|---------|
| `lib/` | Core library (arch dispatch, C/C++ APIs) |
| `bench/` | Benchmarks for cipher/digest/ECDH/RSA/HMAC/CMAC/Poly1305 |
| `tests/` | Unit tests, KAT vectors, fuzz harnesses, example tests |
| `examples/` | Standalone sample programs |
| `docs/` | Compat matrices, integration guides, quick-start |
| `cmake/` | CMake modules and presets |
| `BUILD.md` | Full build/configuration reference |
| `BUILD_Windows.md` | Windows-specific build instructions |

## Build Commands

```bash
# Pull KAT test data (requires git-lfs)
git lfs pull

mkdir build && cd build
cmake -DOPENSSL_INSTALL_DIR=/path/to/openssl \
      -DAOCL_UTILS_INSTALL_DIR=/path/to/aocl-utils \
      -DCMAKE_BUILD_TYPE=Release ..
make -j$(nproc)
```

With Ninja:

```bash
cmake -G Ninja -DOPENSSL_INSTALL_DIR=/path/to/openssl \
      -DAOCL_UTILS_INSTALL_DIR=/path/to/aocl-utils \
      -DCMAKE_BUILD_TYPE=Release ..
ninja
```

List CMake presets:

```bash
cmake --list-presets=configure
```

## Test Commands

```bash
cd build
ctest --output-on-failure -j$(nproc)
```

## Code Style & Lint

- Follow the existing brace/indent style in `lib/`.
- `.clang-tidy` configures static analysis.
- Do not modify AMD copyright headers.

## Key Conventions

- **CPU dispatch**: optimized routines live under `lib/arch/`, selected at runtime via CPUID.
- **API split**: `lib/capi/` exposes a C API; internal C++ implementation is under `lib/`.
- **OpenSSL**: required for reference impl in tests/benchmarks (3.1.3+).
- **AOCL-Utils**: recommended for feature dispatch.

## Common Gotchas

- `git-lfs` is required; tests fail without pulled KAT vectors.
- Clang/AOCC builds need a compatible GNU `libstdc++` 11.3+ toolchain.
- On RHEL 8, activate a newer `gcc-toolset` before configuring.
- Windows builds use `BUILD_Windows.md`.

## Deployment

No CI workflow is present. Build/test locally; publish static/shared library artifacts
from `build/lib`.
