# Verification of revised cp314t license-packaging preview — 2026-10-17

**Status:** TECHNICAL PASS (four repacked wheels); LICENSE REVIEW STILL OPEN. **Do not confuse this CI artifact with a published GitHub Release.**

- [Successful GitHub Actions run #39063972719](https://github.com/raled573343/RocksDict/actions/runs/39063972719)
- [Actual downloadable Action artifact #12112730954](https://github.com/raled573343/RocksDict/actions/runs/39063972719/artifacts/12112730954)
- Workflow commit: [`71b66ff7c9818a44e5464e5f08f8a8b7389305bb`](https://github.com/raled573343/RocksDict/commit/71b66ff7c9818a44e5464e5f08f8a8b7389305bb)
- Run status `completed`, conclusion `success`, 2026-10-17 19:47:34–19:48:00 UTC.

## Independently verified final SHA-256

Downloaded the complete Action artifact and independently checked each wheel against its **original** published version from the previous two GitHub releases.

| Variant | Platform | Verified SHA-256 of revised wheel |
| --- | --- | --- |
| [RocksDict 0.3.29 standard](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29) | Windows x64 | `21a8d6daf207eb88fdfbbae6c1ac9b218a9780eee5189e4a86b958c88df24648` |
| [RocksDict 0.3.29 standard](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29) | Linux x86-64 | `2d79b73615571de8bf0440aeb64ef40693130a5e1abf11cc604c9b93fe0cab0c` |
| [RocksDict + RocksDB 11.8.1](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) | Windows x64 | `fd436b2dec0132607f8f382f14539538a3584480ee54f4c0ea1eff9741247af8` |
| [RocksDict + RocksDB 11.8.1](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) | Linux x86-64 | `d47dfed260b7953f50e2da3337c1701448e6faf1aaa65b2c07ec43591cb93f5d` |

The SHA-256 of each repacked wheel agrees with its matching `SHA256SUMS` in the Action artifact. **Hashes differ from the original published GitHub Releases**, because the wheel metadata and new notices were changed.

## Four-wheel structural acceptance

Independent programmatic validation passed for all four wheels:

- [x] Outer Action artifact and every inner wheel have valid ZIP CRCs.
- [x] Each revised wheel's `RECORD` lists **all 17 members**, without missing or duplicate paths; each recorded SHA-256 and uncompressed size verifies.
- [x] Native binary (`.so` or `.pyd`) bytes are **identical** to the corresponding original release. In fact every original wheel member except `METADATA` and `RECORD` is byte-identical.
- [x] Original upstream MIT license preserved unchanged.
- [x] Eight added files: Apache-2.0, inherited LevelDB, Snappy, LZ4, Zstd, zlib, bzip2, attribution.
- [x] All nine `License-File` paths (including original MIT) in `METADATA` resolve to existing files.
- [x] Correct 3.14t-only independent-distribution README embedded in the new `METADATA`; original misleading `Production/Stable`, PyPy, macOS, Python 3.7 classifiers removed.
- [x] No-GIL install/import smoke passed in CI on Linux x86-64 for both releases, including the RocksDB 11.8.1 native-version assertion.

## Outstanding checks and caveats

- **No Windows runtime test of the revised wheels** was executed by this workflow. Native Windows extension bytes are unchanged from the original Windows wheels, which passed their original build/validation CI.
- Original CycloneDX SBOMs are preserved byte-for-byte and list **53/56 Cargo components** (standard Linux/Windows), **64/66** (RocksDB 11 Linux/Windows). Their license expressions include MIT, Apache-2.0, BSD-3-Clause, ISC, Unicode-3.0, Apache-2.0 WITH LLVM-exception, etc. The SBOM does **not** establish which dependencies are statically included at runtime; license/NOTICE compliance for all incorporated code has not yet been individually verified.
- The original wheels contain `rocksdict 0.3.29` or `0.3.29+rocksdb11.8.1` distribution identifiers. The revised wheels **retain those package versions and filenames**. Do not silently overwrite original assets; give the new release(s) distinct GitHub tags and provenance, or choose an explicit packaging version revision that does not break the provenance.
- The Action artifact is temporary; permanent availability requires publication in GitHub Releases.
- A successful CI run does not constitute a legal opinion or a comprehensive production-safety certification.

## Manager acceptance

**ACCEPTED for zero-recompile license/metadata repackaging.** Keep [packaging and rights issue #1](https://github.com/raled573343/rocksdict-rocksdb11/issues/1) **open** for remaining native dependency license inventory and final release publication. No unnecessary Rust/C++ rebuild is required solely to add/adjust wheel metadata or license files.
