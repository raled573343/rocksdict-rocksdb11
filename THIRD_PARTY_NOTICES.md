# Attribution and third-party licensing

This document describes the provenance of the **unofficial** RocksDict + RocksDB 11 distribution. It does **not** replace the actual third-party license texts that must accompany redistributed components or binary packages.

## Attribution

- **RocksDict** — original Python interface, maintained by Congyu Wang and other contributors. Original project: https://github.com/rocksdict/RocksDict . License: **MIT**. Its copyright and permission notice is preserved in this repository's [LICENSE](LICENSE); the complete source fork also retains it at https://github.com/raled573343/RocksDict/blob/b2958391ddea0530e0f299295f7b5fad43590a3a/LICENSE .
- **RocksDB** — Meta and contributors; incorporates previous LevelDB work by Sanjay Ghemawat, Jeff Dean and others. https://github.com/facebook/rocksdb . RocksDB is **dual-licensed GPLv2 or Apache-2.0**, at the licensee's choice. The intended basis for this distribution is **Apache-2.0**; see https://github.com/facebook/rocksdb/blob/v11.8.1/LICENSE.Apache and https://github.com/facebook/rocksdb/blob/v11.8.1/LICENSE.leveldb .
- **Rust RocksDB binding and librocksdb-sys** — see the [submodule pinned by the release source](https://github.com/raled573343/RocksDict/tree/b2958391ddea0530e0f299295f7b5fad43590a3a/rust-rocksdb), its recorded [submodule origin](https://github.com/Congyuwang/rust-rocksdb), and [rust-rocksdb's Apache-2.0 license](https://github.com/zaidoon1/rust-rocksdb/blob/master/LICENSE). Attribution must follow the actual pinned submodule and any vendored components, not only the currently maintained upstream.
- **Other native / transitive dependencies:** the release's Rust manifest requests Snappy, LZ4, Zstandard, zlib and bzip2 compression support. Their licenses and notices, plus any statically linked or vendored dependencies, must be checked against the **actual compiled wheel** and build dependency tree.

## Scope of the MIT license

The root [LICENSE](LICENSE) retains the original MIT notice and covers the original documentation/modifications where applicable. It does **not** assert that the RocksDB engine, the Rust binding, third-party compression libraries, or every binary linked into a wheel are MIT-licensed.

The original names **RocksDict**, **RocksDB**, and **Meta** are used descriptively to identify the upstream software. This distribution is neither official nor endorsed by their respective rights holders.

## Binary redistribution audit — not yet complete

The [already-published release](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) provides prebuilt wheels. Before treating the wheel files as legally audited, or republishing copies in this repository, verify **for each build**:

1. Inspect the wheel's `*.dist-info/licenses/`, `METADATA`, `RECORD`, and any packaged native dependency notices. Confirm the original RocksDict MIT notice is included in distribution materials.
2. Produce a dependency / license inventory for the **pinned versions and actual linked artifacts**, including RocksDB, Rust bindings, compression libraries and any incorporated LevelDB / third-party code.
3. Include an accessible copy of the relevant Apache-2.0 license, other required license texts, and any legally required attribution/NOTICE content **with the distributed binaries** (for example inside wheel metadata and/or in a clearly linked release license bundle, as required by each license).
4. Preserve original copyright, patent, license and attribution notices in redistributed source. Mark modified upstream files where required by the applicable license.
5. Review marketing and package naming for potential trademark confusion. Do not imply upstream endorsement or publish under the existing upstream PyPI project without appropriate authorization.
6. Re-run this check for **each new release and each platform**. Hashes/CI success establish integrity and tested functionality, **not legal compliance**.

This is a technical attribution inventory and publication checklist, not a legal opinion. Consult qualified counsel if a formal distribution/commercial risk assessment is necessary.
