# Verified result: cp314t license repack preview — 2026-10-17

Run: [#39062998421](https://github.com/raled573343/RocksDict/actions/runs/39062998421) · [Artifact #12111473654](https://github.com/raled573343/RocksDict/actions/runs/39062998421/artifacts/12111473654) · Workflow revision: `f296c962f3b5fe0b8e8423138e392c0b48e8e5de`

**Verdict: PASS for post-build wheel integrity and unchanged native code. NOT YET COMPLETE for third-party licensing or misleading package metadata.**

## Pipeline

The manually triggered workflow completed **successfully** (2026-10-17 19:33:46–19:34:16 UTC). All steps passed, including source SHA checks, adding license documents and updating `RECORD`, Python 3.14t Linux install/import smoke tests for **both** variants with disabled GIL, and preview artifact upload. There was **no native compilation**. Only Linux runtime smoke tests ran; the Windows wheels were structurally inspected but not installed on Windows in this run.

## Independent archive review

Downloaded artifact ZIP (ID `12111473654`) and compared its four inner wheels against the exact four original GitHub Release wheels.

| Variant | Platform | Original SHA-256 | Preview SHA-256 |
| --- | --- | --- | --- |
| [Standard 0.3.29](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29) | Linux x86-64 | `f6617bd24556083f17504abe5b61469f83fda1a514bbba017e7e52c86d7c59dd` | `03110851913fb65a71f6013ba2afaead3c7ad28b670109da69d6d572f8ec15ee` |
| [Standard 0.3.29](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29) | Windows x64 | `84fbc81ea9387eb6106698f51b935aeb6214000c992782e36300a5c64b3fb4ae` | `b616ca12f16834ebaae55fe3d158b8f644d8b324bfb1fe59aa16d46b7f1a4709` |
| [RocksDB 11.8.1](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) | Linux x86-64 | `414c4a36d743b9f3a906a347de70731818f036a6dd094ed1325318e599c70a20` | `4bfd5da03aa54c5134bebd71f684fe0f09cac761d38a500c2eb72a57340a41e1` |
| [RocksDB 11.8.1](https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1) | Windows x64 | `ec0e1246c6a3394948fe1724e89ad133f550b506bacb7b4cd71d83ab9b5d05c9` | `f6286c35c20f4ee7c77b0605990ee414cb045658c84da545057185315c503dbb` |

For **each** repacked wheel:

- ZIP CRC integrity: **PASS**.
- Every listed `RECORD` SHA-256 and length: **PASS**; all archive paths are listed.
- Embedded `.so` / `.pyd`: **byte-for-byte identical** to original (no native code changed).
- Newly packaged under `*.dist-info/licenses/THIRD_PARTY/`: `RocksDB-Apache-2.0.txt`, `LevelDB.txt`, `ATTRIBUTION.txt`. The original `LICENSE` remains.
- `METADATA` includes corresponding `License-File` fields, with paths that exist in the wheel.
- New `SHA256SUMS` lines match all four preview files.
- Only existing archive members changed: `METADATA` and `RECORD`; all other original wheel members are unchanged.

**Important:** preview wheels retain the original wheel **filenames/package versions** but have different cryptographic hashes. Never substitute them silently for the old release files; use clearly labeled new release tags with provenance.

## Open items before a final public Release

1. Complete exact-version license review and ship required notices for all actually bundled native libraries (compression code, Rust bindings, other subcomponents). Adding just RocksDB and LevelDB notices does **not** prove full legal compliance.
2. Correct `METADATA`/embedded project README: upstream `pip install rocksdict` installation claims, irrelevant platform/Python/PyPy classifiers, `Development Status :: 5 - Production/Stable` are misleading for these independent `cp314t` wheel builds.
3. Review package naming/trademark disclosures and include appropriate third-party provenance.
4. For final revised wheels, rerun targeted verification, including **Windows install/import**, because this Linux-only preview run did not execute Windows binaries.
5. Publish immutable/versioned or clearly revisioned release assets and their exact SHA256SUMS, preserving original source commit and workflow links.

**This verification is technical; it is not a legal opinion or a license-compliance certification.**
