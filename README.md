# RocksDict + RocksDB 11.8.1 for Python 3.14t

**Unofficial RocksDict build with RocksDB 11.8.1, free-threaded CPython 3.14 (`cp314t`), and prebuilt Windows/Linux x64 wheels.**

> **Independent community distribution.** This project is not an official RocksDict, RocksDB, or Meta release and is not affiliated with or endorsed by their maintainers.

[**Download the latest tested wheels (GitHub Release)**](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) · [**Tested source commit**](https://github.com/raled573343/RocksDict/tree/b2958391ddea0530e0f299295f7b5fad43590a3a) · [**Successful CI run**](https://github.com/raled573343/RocksDict/actions/runs/39036624615)

This repository is the **public-facing landing page and release documentation**. The source-code fork, build workflows, and current downloadable binaries are hosted in [raled573343/RocksDict](https://github.com/raled573343/RocksDict). **Downloads currently live in that repository, not in this repository's Releases tab.**

## What is this?

[RocksDict](https://github.com/rocksdict/RocksDict) is a Python interface to the [RocksDB](https://github.com/facebook/rocksdb) embedded key-value store. This experimental fork targets developers who need both a newer RocksDB native engine and **free-threaded Python** support.

It preserves the `import rocksdict` package name and familiar `Rdict` API while adding the pinned RocksDB 11 engine and related bindings. The fork may not be a drop-in replacement for every original RocksDict workload.

### Release compatibility

| Component | Tested release |
| --- | --- |
| Python package | `rocksdict 0.3.29+rocksdb11.8.1` |
| Native RocksDB | **11.8.1** |
| Rust binding | `rust-rocksdb 0.53.0` |
| Python | **CPython 3.14 free-threaded build (`3.14t`)**, 64-bit |
| Windows | `cp314-cp314t-win_amd64` |
| Linux | `cp314-cp314t-manylinux_2_28_x86_64` (glibc 2.28+) |
| Not included | macOS, ARM64, conventional `cp314`, other Python ABIs |
| Package channel | **GitHub Releases only**, not PyPI |

**Do not use `pip install rocksdict` to install this variant.** That command resolves the separately maintained PyPI distribution rather than these specific RocksDB 11 wheels.

## Download and install

1. Install **64-bit free-threaded CPython 3.14** (not regular CPython 3.14).
2. Download the appropriate `.whl` file from the [tested GitHub Release](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1). The release also provides `SHA256SUMS`.
3. Install the local wheel using that same free-threaded interpreter.

**Windows PowerShell:**

```powershell
py -3.14t -m pip install --no-deps ".\rocksdict-0.3.29+rocksdb11.8.1-cp314-cp314t-win_amd64.whl"
```

**Linux x86-64:** activate your Python 3.14t environment, then run:

```bash
python -m pip install --no-deps './rocksdict-0.3.29+rocksdb11.8.1-cp314-cp314t-manylinux_2_28_x86_64.whl'
```

On Linux, verify that `python` resolves to **free-threaded** CPython 3.14 before installation. For download-integrity checking, compare the SHA-256 hash of your wheel to the entry in `SHA256SUMS` from the **same release**. If you downloaded both wheels and the manifest, `sha256sum -c SHA256SUMS` checks both.

### Verify the native engine and GIL state

```python
import sys
import sysconfig
import rocksdict

print("Native RocksDB:", rocksdict.native_rocksdb_version())
print("Free-threaded ABI:", sysconfig.get_config_var("Py_GIL_DISABLED"))
print("GIL enabled:", sys._is_gil_enabled())

assert rocksdict.native_rocksdb_version() == (11, 8, 1)
assert sysconfig.get_config_var("Py_GIL_DISABLED") == 1
assert not sys._is_gil_enabled()
```

Expected: RocksDB `(11, 8, 1)`, free-threaded ABI flag `1`, and GIL enabled `False`. Other extension modules imported into your application can change the process GIL state.

### Minimal usage

```python
from rocksdict import Options, Rdict

db = Rdict("./rocksdb11_example", Options(raw_mode=True))
try:
    db[b"hello"] = b"RocksDB 11"
    assert db[b"hello"] == b"RocksDB 11"
    for key, value in db.items():
        print(key, value)
finally:
    db.close()
```

### Selected RocksDB 11 capabilities

The current fork includes:

- RocksDB 11.8.1 through a pinned native Rust dependency.
- A Python 3.14t wheel ABI that passes a runtime GIL-disabled check.
- Wide-column entity operations (including `put_entity` / `get_entity`) and wide-column write-batch integration.
- Selected RocksDB 11 read-executor, SST ingestion and file-checksum controls.

These are **specific exposed features**, not a claim that every RocksDB 11 API has been wrapped.

## Validation and limitations

The [release CI run](https://github.com/raled573343/RocksDict/actions/runs/39036624615) succeeded on Windows x64 and Linux x86-64. It built both wheels and exercised native-version, no-GIL and integration tests. The tested source revision is [`b2958391ddea0530e0f299295f7b5fad43590a3a`](https://github.com/raled573343/RocksDict/commit/b2958391ddea0530e0f299295f7b5fad43590a3a).

- **Experimental distribution:** successful CI is not a broad production-readiness or thread-safety certification. Stress-test your exact concurrency and data-access patterns.
- **Back up your databases first.** New RocksDB formats/features may create compatibility issues with older RocksDB releases. Do not assume an updated database can be safely downgraded.
- **ABI-specific:** regular CPython 3.14 is not compatible with `cp314t` wheels.
- **No PyPI release of this variant.** Prefer the linked wheel artifacts over similarly named third-party downloads.
- Do not treat a GIL-disabled process as proof that all extension modules and application operations are thread-safe.

## Source, attribution and licenses

This project builds on the work of others and clearly distinguishes their contributions:

| Project | Role | License |
| --- | --- | --- |
| [RocksDict](https://github.com/rocksdict/RocksDict) — Congyu Wang and contributors | Original Python/Rust interface | MIT |
| [RocksDB](https://github.com/facebook/rocksdb) — Meta and contributors | Native database engine | Dual GPLv2 / Apache-2.0; **Apache-2.0 option** used for this distribution |
| [rust-rocksdb](https://github.com/zaidoon1/rust-rocksdb) and its contributors | Rust bindings/native integration | Apache-2.0, subject to individual incorporated dependencies |

The upstream MIT copyright notice remains acknowledged. See [LICENSE](LICENSE) and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for attribution, component-specific licenses, and the **outstanding binary-redistribution notice audit**. A single MIT license **does not** relicense the native engine or bundled third-party components.

All names are used to identify compatible software; this independent distribution is **not endorsed by** the respective rights holders.

## Support and project links

- [Get the currently published wheels](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1)
- [Browse the actual source code](https://github.com/raled573343/RocksDict/tree/b2958391ddea0530e0f299295f7b5fad43590a3a)
- [Report source / wheel bugs](https://github.com/raled573343/RocksDict/issues)
- [Original RocksDict project](https://github.com/rocksdict/RocksDict)
- [Release notes](RELEASE_NOTES.md) and [release / legal publication checklist](RELEASE_CHECKLIST.md)

For problems with **this unofficial build**, open an issue on the maintained fork above rather than assuming the original RocksDict team supports it.
