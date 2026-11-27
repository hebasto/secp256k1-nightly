# Nightly libsecp256k1 Tests

This repository performs nightly tests of [libsecp256k1](https://github.com/bitcoin-core/secp256k1) across various operating systems and compilers.

The results are reported to [CDash](https://cdash.fish.foo/index.php?project=secp256k1).

## Modern BSD Derivatives

| Operating System | Releases | Status | Notes |
|------------------|:--------:|--------|-------|
| [OpenBSD](https://www.openbsd.org/) | 7.9, 8.0 | [![OpenBSD](https://github.com/hebasto/secp256k1-nightly/actions/workflows/openbsd.yml/badge.svg?event=workflow_run)](https://github.com/hebasto/secp256k1-nightly/actions/workflows/openbsd.yml?query=event%3Aworkflow_run) | Static and shared libraries <br> Constant-time tests - N/A |
