# RocksDict wheels for free-threaded Python 3.14t — standard & RocksDB 11

**Unofficial Windows/Linux x64 wheels of RocksDict 0.3.29 for free-threaded CPython 3.14 (`cp314t`), offered in two variants: upstream-aligned and RocksDB 11.8.1.**

> **Independent community distribution.** This project is not an official RocksDict, RocksDB, or Meta release and is not affiliated with or endorsed by their maintainers.

## Choose your build

| Download | Contents | Best for |
| --- | --- | --- |
| **[RocksDict 0.3.29 — Python 3.14t](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29)** | An upstream-aligned RocksDict 0.3.29 build targeting the free-threaded CPython 3.14 ABI | Using the existing RocksDict implementation on Python without the GIL |
| **[RocksDict 0.3.29 + RocksDB 11.8.1](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1)** | Experimental RocksDict 0.3.29 variant with native RocksDB **11.8.1** and updated Rust bindings, targeting the same free-threaded ABI | Evaluating the RocksDB 11 engine and its exposed features |

**[Read the benchmarks: free-threaded Python 3.14t, RocksDict, and RocksDB 11.8.1 — in-process vs. multiprocess (English + Русский)](https://github.com/raled573343/rocksdict-rocksdb11/issues/2).** The article reports up to **5.04×** in a warm-cache, eight-thread *RocksDict GET microbenchmark* versus the same Python build with the GIL enabled; it **does not** claim a fivefold HTTP-service speedup.

**Both release pages contain updated Windows x64 and Linux x86-64 wheels plus matching `SHA256SUMS`.** The release assets were repackaged on **2026-10-17** to include third-party license notices and corrected wheel metadata, with compiled native extensions unchanged ([publication run #39065343524](https://github.com/raled573343/RocksDict/actions/runs/39065343524)). Neither release supports conventional (GIL-enabled) CPython 3.14, macOS, or ARM64. These packages share the `rocksdict` import name: **install one variant per Python environment**, not both at once.

- **Standard build:** [source commit `1ae571f`](https://github.com/raled573343/RocksDict/commit/1ae571fc5b107fc7f22b470fbded1e92ed3e4899) · [CI run #39024425551](https://github.com/raled573343/RocksDict/actions/runs/39024425551) · [upstream 0.3.29](https://github.com/rocksdict/RocksDict/releases/tag/v0.3.29)
- **RocksDB 11 build:** [source commit `b295839`](https://github.com/raled573343/RocksDict/commit/b2958391ddea0530e0f299295f7b5fad43590a3a) · [CI run #39036624615](https://github.com/raled573343/RocksDict/actions/runs/39036624615)

This repository hosts the **landing page, documentation, audit notes and benchmark article**. The source-code fork, build workflows and downloadable wheels live in [raled573343/RocksDict](https://github.com/raled573343/RocksDict). **Use the two linked Releases in that source repository for downloads; this repository does not host the wheel assets.**

## What is this?

[RocksDict](https://github.com/rocksdict/RocksDict) is a Python interface to the [RocksDB](https://github.com/facebook/rocksdb) embedded key-value store. The **standard variant** supplies the familiar RocksDict 0.3.29 API for free-threaded Python. The **RocksDB 11 variant** additionally upgrades the native engine and Rust bindings while retaining `import rocksdict` and the familiar `Rdict` API. The upgraded variant may not be a drop-in replacement for every original RocksDict workload.

### Release compatibility

| Component | Standard Python 3.14t build | RocksDB 11 build |
| --- | --- | --- |
| Python distribution | `rocksdict 0.3.29` | `rocksdict 0.3.29+rocksdb11.8.1` |
| Native engine | **RocksDB 9.8.4** (pinned in the standard release source) | **RocksDB 11.8.1** |
| Rust binding | `rocksdb 0.24.0` / `librocksdb-sys 0.17.1+9.8.4` | `rust-rocksdb 0.53.0` / `rust-librocksdb-sys 0.48.1+11.8.1` |
| Python | **Free-threaded CPython 3.14t**, 64-bit | **Free-threaded CPython 3.14t**, 64-bit |
| Windows | `cp314-cp314t-win_amd64` | `cp314-cp314t-win_amd64` |
| Linux | `cp314-cp314t-manylinux_2_28_x86_64` (glibc 2.28+) | Same |
| Not included | macOS, ARM64, regular `cp314`, other Python ABIs | Same |
| Distribution | GitHub Releases | GitHub Releases |

**Do not use `pip install rocksdict` to choose either of these exact releases.** The PyPI `rocksdict` distribution is maintained separately. Download the wheel from the appropriate GitHub release page.

## Download and install

1. Install **64-bit free-threaded CPython 3.14** (not regular CPython 3.14).
2. Choose either the [standard build](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29) or the [RocksDB 11 build](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1), and download the wheel matching your platform. Each release has its **own** `SHA256SUMS`.
3. Install the local wheel using that same free-threaded interpreter.

**Windows PowerShell — standard build:**

```powershell
py -3.14t -m pip install --no-deps ".\rocksdict-0.3.29-cp314-cp314t-win_amd64.whl"
```

**Windows PowerShell — RocksDB 11 build:**

```powershell
py -3.14t -m pip install --no-deps ".\rocksdict-0.3.29+rocksdb11.8.1-cp314-cp314t-win_amd64.whl"
```

**Linux x86-64 (inside Python 3.14t) — standard build:**

```bash
python -m pip install --no-deps './rocksdict-0.3.29-cp314-cp314t-manylinux_2_28_x86_64.whl'
```

**Linux x86-64 — RocksDB 11 build:**

```bash
python -m pip install --no-deps './rocksdict-0.3.29+rocksdb11.8.1-cp314-cp314t-manylinux_2_28_x86_64.whl'
```

On Linux, verify that `python` resolves to **free-threaded** CPython 3.14 before installation. For download-integrity checking, compare the SHA-256 hash of your wheel to the entry in `SHA256SUMS` from the **same release**. If you downloaded both wheels and the manifest, `sha256sum -c SHA256SUMS` checks both.

### Verify your Python environment (both builds)

```python
import sys
import sysconfig
import rocksdict

print("Free-threaded ABI:", sysconfig.get_config_var("Py_GIL_DISABLED"))
print("GIL enabled:", sys._is_gil_enabled())

assert sysconfig.get_config_var("Py_GIL_DISABLED") == 1
assert not sys._is_gil_enabled()
```

**Additional verification for the RocksDB 11 build only** (this version-reporting API is specific to the upgraded variant):

```python
print("Native RocksDB:", rocksdict.native_rocksdb_version())
assert rocksdict.native_rocksdb_version() == (11, 8, 1)
```

Other extension modules imported into your application can change the process GIL state.

### Minimal usage

```python
from rocksdict import Options, Rdict

db = Rdict("./rocksdict_example", Options(raw_mode=True))
try:
    db[b"hello"] = b"RocksDict"
    assert db[b"hello"] == b"RocksDict"
    for key, value in db.items():
        print(key, value)
finally:
    db.close()
```

### Selected capabilities of the RocksDB 11 variant

The **RocksDB 11 release** upgrades the native engine and exposes selected newer features:

- RocksDB 11.8.1 through pinned native Rust dependencies.
- Like the standard wheel, it targets Python 3.14t and passes a runtime GIL-disabled check.
- Wide-column entity operations (including `put_entity` / `get_entity`) and wide-column write-batch integration.
- Selected RocksDB 11 read-executor, SST ingestion and file-checksum controls.

These are **specific exposed features**, not a claim that every RocksDB 11 API has been wrapped.

## Validation and limitations

The **standard Python 3.14t release** reports successful wheel install, no-GIL smoke and Python unit tests in [CI run #39024425551](https://github.com/raled573343/RocksDict/actions/runs/39024425551), built from [`1ae571fc5b107fc7f22b470fbded1e92ed3e4899`](https://github.com/raled573343/RocksDict/commit/1ae571fc5b107fc7f22b470fbded1e92ed3e4899).

The **RocksDB 11 release** passed Windows x64 and Linux x86-64 builds and native-version, no-GIL and integration checks in [CI run #39036624615](https://github.com/raled573343/RocksDict/actions/runs/39036624615), from [`b2958391ddea0530e0f299295f7b5fad43590a3a`](https://github.com/raled573343/RocksDict/commit/b2958391ddea0530e0f299295f7b5fad43590a3a).

**Current GitHub Release assets:** On **2026-10-17**, [Action #39065343524](https://github.com/raled573343/RocksDict/actions/runs/39065343524) updated **both existing release tags** with repackaged wheels and new `SHA256SUMS`. ZIP/`RECORD` integrity, metadata and notice inclusion were verified; the native `.so`/`.pyd` bytes were kept unchanged. The repack action passed Linux CPython 3.14t no-GIL import smoke for both variants. Windows-native runtime tests were performed in the original build CI, **not repeated in the repack action**. [Original release backups](https://github.com/raled573343/RocksDict/actions/runs/39065343524/artifacts/12113435505) were saved as an Actions artifact (retention-limited).

- **Experimental distribution:** successful CI is not a broad production-readiness or thread-safety certification. Stress-test your exact concurrency and data-access patterns.
- **Back up your databases first.** New RocksDB formats/features may create compatibility issues with older RocksDB releases. Do not assume an updated database can be safely downgraded.
- **ABI-specific:** regular CPython 3.14 is not compatible with `cp314t` wheels.
- **These exact builds are distributed via GitHub Releases.** Prefer the linked wheel artifacts over similarly named third-party downloads.
- Do not treat a GIL-disabled process as proof that all extension modules and application operations are thread-safe.

## Source, attribution and licenses

This project builds on the work of others and clearly distinguishes their contributions:

| Project | Role | License |
| --- | --- | --- |
| [RocksDict](https://github.com/rocksdict/RocksDict) — Congyu Wang and contributors | Original Python/Rust interface | MIT |
| [RocksDB](https://github.com/facebook/rocksdb) — Meta and contributors | Native database engine | Dual GPLv2 / Apache-2.0; **Apache-2.0 option** used for this distribution |
| [rust-rocksdb](https://github.com/zaidoon1/rust-rocksdb) and its contributors | Rust bindings/native integration | Apache-2.0, subject to individual incorporated dependencies |

The original RocksDict MIT copyright notice remains intact. **Current released wheels** additionally include the RocksDB Apache-2.0 text, inherited LevelDB notices, Snappy, LZ4, Zstandard, zlib and bzip2 license texts, attribution, and updated `METADATA`/`RECORD`. The native binaries were not rebuilt for this packaging revision; [Action #39065343524](https://github.com/raled573343/RocksDict/actions/runs/39065343524) published the updated wheels into the existing Releases. The [earlier audit](WHEEL_AUDIT_2026-10-17.md) and [preview verification](WHEEL_REPACK_V2_VERIFICATION_2026-10-17.md) describe historical **pre-publication** files, not the current download state. See [LICENSE](LICENSE), [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) and [open rights-compliance issue #1](https://github.com/raled573343/rocksdict-rocksdb11/issues/1): **a complete review of the actual bundled Rust/native dependencies is still outstanding**, so these releases must not be represented as legally certified. A root MIT license does not relicense incorporated third-party code.

All names are used to identify compatible software; this independent distribution is **not endorsed by** the respective rights holders.

## Support and project links

- **[Download standard RocksDict 0.3.29 for Python 3.14t](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29)**
- **[Download RocksDict 0.3.29 + RocksDB 11.8.1 for Python 3.14t](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1)**
- **[Read the free-threaded Python / RocksDB performance case study (English + Русский)](https://github.com/raled573343/rocksdict-rocksdb11/issues/2)** — warm-cache GET microbenchmark versus in-process and multiprocess HTTP
- [Browse standard build source](https://github.com/raled573343/RocksDict/tree/1ae571fc5b107fc7f22b470fbded1e92ed3e4899)
- [Browse RocksDB 11 build source](https://github.com/raled573343/RocksDict/tree/b2958391ddea0530e0f299295f7b5fad43590a3a)
- [Report packaging / license gaps and follow remediation](https://github.com/raled573343/rocksdict-rocksdb11/issues/1)
- [Source-code repository and CI](https://github.com/raled573343/RocksDict) (Issues are currently disabled there)
- [Original RocksDict project](https://github.com/rocksdict/RocksDict)
- [Release notes](RELEASE_NOTES.md), [2026-10-17 published-release action](https://github.com/raled573343/RocksDict/actions/runs/39065343524), [historical wheel audit](WHEEL_AUDIT_2026-10-17.md), [repack preview verification](WHEEL_REPACK_V2_VERIFICATION_2026-10-17.md) and [release checklist](RELEASE_CHECKLIST.md)

For packaging/license concerns about **these unofficial wheels**, use [this project's remediation issue](https://github.com/raled573343/rocksdict-rocksdb11/issues/1), not the original RocksDict project. The source-code fork currently has GitHub Issues disabled.
