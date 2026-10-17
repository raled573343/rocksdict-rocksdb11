# Audit of four published Python 3.14t wheel files — 2026-10-17

**Scope:** the four wheel files uploaded for inspection (two releases × Windows/Linux x64), checked against the GitHub Release API asset SHA-256 digests. This is a package-content and dependency-signals audit, **not** an external legal opinion and **not** a runtime certification.

## 1. Identity, integrity and packaging

| Release | Platform | Size (bytes) | SHA-256 | ZIP & RECORD |
| --- | --- | ---: | --- | --- |
| [Standard 0.3.29](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29) | Windows x64 | 3,932,516 | `84fbc81ea9387eb6106698f51b935aeb6214000c992782e36300a5c64b3fb4ae` | PASS |
| [Standard 0.3.29](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29) | Linux x86-64 | 4,882,440 | `f6617bd24556083f17504abe5b61469f83fda1a514bbba017e7e52c86d7c59dd` | PASS |
| [RocksDB 11.8.1](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) | Windows x64 | 4,360,890 | `ec0e1246c6a3394948fe1724e89ad133f550b506bacb7b4cd71d83ab9b5d05c9` | PASS |
| [RocksDB 11.8.1](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) | Linux x86-64 | 5,802,308 | `414c4a36d743b9f3a906a347de70731818f036a6dd094ed1325318e599c70a20` | PASS |

**Verification methods:** compute SHA-256 of each complete uploaded file; compare it with the `digest` of the same named asset from each GitHub Release API response; use `ZipFile.testzip()`; for **every** archive entry, check the SHA hash and uncompressed size recorded in `*.dist-info/RECORD` wherever present. The four files match **byte-for-byte by SHA-256** with GitHub's published asset digests. This is stronger than file-size verification.

All four wheels contain a native extension, `METADATA`, `WHEEL`, `RECORD` and a single legal text at:

```text
rocksdict-<version>.dist-info/licenses/LICENSE
```

The included license is the **original RocksDict MIT license**, preserving `Copyright (c) 2021 Congyu`. **No separate RocksDB Apache-2.0, inherited LevelDB, rust-rocksdb, compression-library, or other third-party license/NOTICE file is present in any of the four wheel archives.**

## 2. ABI, native dependencies and provenance

- `WHEEL` tags consistently declare `cp314-cp314t-win_amd64` or `cp314-cp314t-manylinux_2_28_x86_64`, as advertised. Tags alone cannot establish runtime thread-safety; see the linked original CI runs.
- Both Linux native extensions require `GLIBC_2.28` at most among the observed GLIBC symbol versions (as reported by `readelf --version-info`) and list only standard system runtime libraries as ELF `DT_NEEDED` entries. Native compression components may be **statically linked** and therefore are not fully identified by `DT_NEEDED` alone.
- Both Windows native extensions import `python314t.dll`, Windows system DLLs and Microsoft Visual C++ runtime DLLs (`MSVCP140.dll`, `VCRUNTIME140.dll`, `VCRUNTIME140_1.dll`) in the PE import table.
- `Cargo.lock` for the **standard** source revision pins `librocksdb-sys 0.17.1+9.8.4` / `rocksdb 0.24.0`; the **RocksDB 11** revision pins `rust-librocksdb-sys 0.48.1+11.8.1` / `rust-rocksdb 0.53.0`. Other native-related dependencies include `bzip2-sys`, `lz4-sys` and `zstd-sys`; the build selects additional compression features. Lockfile presence does **not** alone establish that every listed crate is linked into the wheel.
- Each built wheel's `METADATA` contains the **original upstream README**, which still tells users to run `pip install rocksdict` and claims other platforms and Python versions. Its classifiers also label the distribution `Development Status :: 5 - Production/Stable`, list Python 3.7–3.14 and PyPy/macOS, and show only `License :: OSI Approved :: MIT License`. These statements do not accurately describe **these ABI-specific unofficial wheels**. The accurate guidance is in this landing repository's README.

## 3. License / distribution risks

**Observation (confirmed):** the wheels contain only the original MIT license and no other distinct license/notice files. Their native dependency graph requires **additional, version-specific legal verification**, especially RocksDB, the pinned Rust binding and statically linked compression components. This gap is **not automatic proof of infringement**; nevertheless, the current artifacts should **not be described as fully license-audited**.

RocksDB is distributed under a GPLv2 / Apache-2.0 choice. For the Apache-2.0 option, downstream distributors must satisfy the license and any applicable copyright/NOTICE conditions. The associated [Apache-2.0](LICENSES/Apache-2.0.txt) and [LevelDB](LICENSES/LevelDB.txt) texts have been preserved **in this documentation repository**, but being linked from a separate GitHub repository **is not equivalent to verifying inclusion and compliant delivery with the published wheel files**.

Additional applicable third-party license terms must be determined from the **actual pinned source and incorporated binaries**. Do not assume that a single top-level MIT license covers the complete bundled native dependency stack.

### Recommended remediation (future builds)

1. Create a per-variant, pinned dependency/license inventory, recording libraries that are actually linked into each wheel. Include all bundled copyright, license and NOTICE content from the relevant exact source revisions.
2. Preserve the original RocksDict MIT license verbatim. Include necessary Apache-2.0, LevelDB and other third-party notices in a defined `LICENSES/` or equivalent subtree within the distribution source. Verify the pinned Rust binding's precise attribution rather than assuming a different upstream GitHub repository is identical.
3. Use Python packaging's [PEP 639 `[project].license-files`](https://peps.python.org/pep-0639/) facility (if supported by the chosen build backend), or an equivalent deterministic wheel packaging method, so required legal texts are emitted into `*.dist-info/licenses/` and declared with matching `License-File` fields in `METADATA`.
4. Correct project metadata/README embedded in **both** wheel variants: platform/Python claims, upstream credit, official-vs-unofficial distinction, link to the correct distribution page and version-specific installation instructions. Do not overclaim a single SPDX expression until all bundled components have been inventoried.
5. Add CI tests that **unzip each generated wheel** and assert all required legal files are present, that metadata entries point to real files, and that every `RECORD` entry/hash is valid. Continue independent install / native-version / GIL-disabled checks.
6. Publish corrected packages via **new, clearly identified releases** after CI. If wheel bytes change, the original SHA-256 signatures/digests necessarily change: **never imply a repacked file is byte-identical to the old tested binary**. Do not silently replace old binaries without clear provenance.
7. Independently decide whether legal remediation of the **already-published** assets is required (e.g. attach supplemental notices, withdraw/replace downloads), with competent legal review if necessary.

## 4. Audit limitations

- The inspected runtime does **not** provide free-threaded Python 3.14 on both target operating systems, so wheels were **not imported/executed** here. This audit uses archive structure and static library inspection; runtime checks are represented separately by the linked successful Actions.
- A complete dependency SBOM, verification of all dependency licenses and actual static linkage per build are **not finished**.
- The GitHub Release assets' stored SHA-256 digests **match all four uploaded files**. This establishes artifact identity, not entitlement to redistribute every included component.
- This report does **not** assert that third-party copyright or trademark restrictions have been fully cleared.

**Source and CI:** [standard wheel build](https://github.com/raled573343/RocksDict/actions/runs/39024425551), [RocksDB 11 wheel build](https://github.com/raled573343/RocksDict/actions/runs/39036624615), [standard source commit](https://github.com/raled573343/RocksDict/commit/1ae571fc5b107fc7f22b470fbded1e92ed3e4899), [RocksDB 11 source commit](https://github.com/raled573343/RocksDict/commit/b2958391ddea0530e0f299295f7b5fad43590a3a).
