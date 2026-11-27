# Nightly libsecp256k1 Tests

This repository performs nightly tests of [libsecp256k1](https://github.com/bitcoin-core/secp256k1) across various operating systems and compilers.

The results are reported to [CDash](https://cdash.fish.foo/index.php?project=secp256k1).

## Modern BSD Derivatives

| Operating System | Releases | Status | Notes |
|------------------|:--------:|--------|-------|
| [OpenBSD](https://www.openbsd.org/) | 7.9, 8.0 | [![OpenBSD](https://github.com/hebasto/secp256k1-nightly/actions/workflows/openbsd.yml/badge.svg?event=workflow_run)](https://github.com/hebasto/secp256k1-nightly/actions/workflows/openbsd.yml?query=event%3Aworkflow_run) | Static and shared libraries <br> Constant-time tests - N/A |

## 32-bit ARM, cross-build

Built with a [CMake nightly build](https://cmake.org/files/dev/) and the [`arm-linux-gnueabihf.toolchain.cmake`](https://github.com/bitcoin-core/secp256k1/blob/master/cmake/arm-linux-gnueabihf.toolchain.cmake) toolchain file from the source tree.

| Host OS | Toolchain | Status | Notes |
|---------|-----------|--------|-------|
| Ubuntu 26.04, x86_64 | GCC, [`gcc-arm-linux-gnueabihf`](https://packages.ubuntu.com/gcc-arm-linux-gnueabihf) | [![ARM32](https://github.com/hebasto/secp256k1-nightly/actions/workflows/arm32.yml/badge.svg?event=workflow_run)](https://github.com/hebasto/secp256k1-nightly/actions/workflows/arm32.yml?query=event%3Aworkflow_run) | With and without ARM assembly <br> Tests run under [QEMU](https://www.qemu.org/) user-mode emulation |
