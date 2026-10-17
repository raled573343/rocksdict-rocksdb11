# Release notes — RocksDict 0.3.29 for free-threaded Python 3.14t

Two separate, independently published builds are available. **Choose either standard RocksDict or the upgraded native RocksDB 11 version.**

## Release A: standard RocksDict 0.3.29 — CPython 3.14t

**[Download standard RocksDict 0.3.29 wheels and SHA256SUMS](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29)**

- Built from the upstream RocksDict 0.3.29 code for free-threaded CPython 3.14 (`cp314t`), Windows x64 and Linux x86-64 (glibc 2.28+).
- [Upstream 0.3.29 release](https://github.com/rocksdict/RocksDict/releases/tag/v0.3.29) · [Fork source commit `1ae571f`](https://github.com/raled573343/RocksDict/commit/1ae571fc5b107fc7f22b470fbded1e92ed3e4899) · [CI run #39024425551](https://github.com/raled573343/RocksDict/actions/runs/39024425551)
- Assets: `rocksdict-0.3.29-cp314-cp314t-win_amd64.whl`, `rocksdict-0.3.29-cp314-cp314t-manylinux_2_28_x86_64.whl`, `SHA256SUMS`.
- Built and tested for free-threaded CPython; **no RocksDB 11 upgrade is claimed for this release**.

## Release B: RocksDict 0.3.29 + RocksDB 11.8.1

**Unofficial free-threaded Python 3.14t wheels for Windows x64 and Linux x86-64.**

**[Download binaries and SHA256SUMS in the existing release](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1)**.

> This is a release-summary document, **not a GitHub Release in this landing-page repository**. The binaries remain published in the linked source fork.

## Highlights

- RocksDB native engine **11.8.1**.
- CPython **3.14t** free-threaded ABI (`cp314t`) on Windows x64 and Linux x86-64 (glibc >=2.28).
- Familiar `rocksdict` imports and `Rdict` API.
- Wide-column entity operations and selected RocksDB 11 options.
- Windows and Linux build / install / native-version / GIL-state test jobs passed in [CI run #39036624615](https://github.com/raled573343/RocksDict/actions/runs/39036624615).
- Tested source: [b2958391ddea0530e0f299295f7b5fad43590a3a](https://github.com/raled573343/RocksDict/commit/b2958391ddea0530e0f299295f7b5fad43590a3a).

## Artifacts

| Asset | Intended environment |
| --- | --- |
| `rocksdict-0.3.29+rocksdb11.8.1-cp314-cp314t-win_amd64.whl` | Free-threaded CPython 3.14, Windows 64-bit |
| `rocksdict-0.3.29+rocksdb11.8.1-cp314-cp314t-manylinux_2_28_x86_64.whl` | Free-threaded CPython 3.14, Linux x86-64, glibc 2.28+ |
| `SHA256SUMS` | Published SHA-256 hashes |

There are no wheels for conventional Python 3.14, other Python versions, macOS, or ARM64 in this release.

## Upgrade and safety notes

- **Back up the database before testing it with RocksDB 11.** Do not assume you can reopen an upgraded RocksDB 11 database with RocksDB 9.
- Do not assume that a free-threaded wheel makes your complete application thread-safe.
- This variant is **not** published to PyPI. Download only the wheel corresponding to your ABI.
- This is an independent experimental build, not endorsed by the original authors.

See [installation instructions](README.md#download-and-install) and [licensing / redistribution notes](THIRD_PARTY_NOTICES.md).
