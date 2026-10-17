# Release and discoverability checklist

This repository is a landing page for **both** unofficial RocksDict 0.3.29 free-threaded Python 3.14t wheel releases: an upstream-aligned build and the experimental RocksDB 11.8.1 variant. The commands and GitHub settings below are for a maintainer who controls **both** repositories.

## Make the repository discoverable

- [ ] Change `raled573343/rocksdict-rocksdb11` from **Private** to **Public** in **Settings → General → Danger Zone → Change repository visibility**. Check for secrets/history before publishing.
- [ ] Confirm the README and license pages render in an incognito browser.
- [ ] Set the **About** description: `Unofficial RocksDict 0.3.29 wheels for free-threaded Python 3.14t: standard and RocksDB 11.8.1 builds, Windows/Linux x64.`
- [ ] Set the **Website** field to the actual releases index: `https://github.com/raled573343/RocksDict/releases`. The README prominently links **both** version-specific pages.
- [ ] Add GitHub Topics: `rocksdb`, `rocksdict`, `python`, `python314`, `free-threading`, `nogil`, `rust`, `python-bindings`, `key-value-store`, `windows`, `linux`. Use accurate topics only.
- [ ] Enable Discussions/Issues if you intend to accept support directly in this landing repo; otherwise keep the source fork issue link prominent.
- [ ] Check search results after GitHub and external crawlers have had a chance to index the now-public repository. Indexing is not immediate or guaranteed.

## Verify the wheel files and legal notices

- [x] Inspect **all four wheels across both releases**, including `*.dist-info` license metadata — see [2026-10-17 wheel audit](WHEEL_AUDIT_2026-10-17.md). Only MIT is embedded; the notice gap needs remediation.
- [ ] Inventory all incorporated native libraries and review corresponding third-party license text/NOTICE requirements; see [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
- [ ] **Packaging fix required:** include applicable native third-party license/NOTICE texts within each wheel (not just in this documentation repo), update license metadata, and check with PEP 639-compatible tooling.
- [ ] Retain upstream copyrights and identify independent modifications clearly.
- [x] Compare SHA-256 of all four uploaded wheel files to the GitHub Release asset `digest` values — exact matches; see [wheel audit](WHEEL_AUDIT_2026-10-17.md). Linked CI runs are recorded separately by release.
- [ ] For **new/repacked** wheels, re-run install/import/version/GIL checks with actual free-threaded Python 3.14 on each supported platform. Original release CI passed, but no new wheel has been built yet.

## License-patching preview Action (no Rust/C++ build)

- [x] Created [manual GitHub Actions workflow](https://github.com/raled573343/RocksDict/actions/workflows/append-wheel-licenses.yml) and [wheel repacker](https://github.com/raled573343/RocksDict/blob/main/tools/repack_cp314t_licenses.py) in the **source** repository. It caches originals, verifies SHA-256 for all four wheels, adds the exact RocksDB Apache-2.0/LevelDB notices, rebuilds RECORD, verifies native extensions are byte-identical, and uploads new wheels with SHA256SUMS as a **preview**.
- [x] Ran initial manual action on **2026-10-17**: [successful run #39062998421](https://github.com/raled573343/RocksDict/actions/runs/39062998421), [artifact #12111473654](https://github.com/raled573343/RocksDict/actions/runs/39062998421/artifacts/12111473654), and [independent verification](WHEEL_PREVIEW_VERIFICATION_2026-10-17.md): four wheel hashes/RECORD valid, native code identical; Linux no-GIL import passed.
- [ ] **Prepared after the successful run:** the updated manual action includes Snappy, LZ4, Zstd, zlib, bzip2 notices and revised package METADATA, but **has NOT yet been run**. Execute [the updated workflow](https://github.com/raled573343/RocksDict/actions/workflows/append-wheel-licenses.yml) once, then independently verify the resulting preview before promoting to releases.
- [ ] Inventory and package **remaining third-party native notices** before claiming the new wheels are fully license-compliant or promoting the preview to release assets.
- [ ] Verify correction of outdated PyPI/OS/Python claims in newly repacked wheel METADATA; this correction is implemented in the script but **not verified by a new run**.

## If moving the actual release assets to this repository later

Currently the **canonical release stays in** [raled573343/RocksDict](https://github.com/raled573343/RocksDict/releases). Do not create an empty or misleading release entry here.

1. Complete the **remaining native dependency/license inventory and packaging remediation** described in [the verified wheel audit](WHEEL_AUDIT_2026-10-17.md). Do not treat the original MIT-only wheel license payload as final.
2. Download the two wheels and matching `SHA256SUMS` from **each** verified release, without rebuilding or renaming them. Do not mix checksum manifests.
3. Publish **separate** matching release tags with the corresponding two wheels and SHA256SUMS, using [RELEASE_NOTES.md](RELEASE_NOTES.md) as the description and explicitly stating that the compiled source is in the other repository: standard release commit `1ae571fc5b107fc7f22b470fbded1e92ed3e4899` or RocksDB 11 commit `b2958391ddea0530e0f299295f7b5fad43590a3a`, respectively. Any auto-generated source archive for a documentation-only tag here is **not** the wheel source tree.
4. Compare published SHA-256 values byte-for-byte with the original release; do not silently repackage wheels.
5. Update all README, Website and announcement links to the new canonical release only **after** the assets are actually available.
6. For future releases, consider moving source + workflows + legal notices into one authoritative project repository, to avoid making users follow two repositories.

## Communicate responsibly

Suggested announcement title: **Unofficial RocksDict 0.3.29 wheels for free-threaded Python 3.14t — original build and RocksDB 11 variant (Windows/Linux)**.

Describe precisely: the **two different available builds**, supported Python ABI, included platforms, corresponding test runs/source commits, pinned native version **only for the RocksDB 11 build**, experimental state, and upstream credit. Avoid `official`, `fully thread-safe`, `production certified`, `drop-in upgrade`, or universal-performance claims without independent evidence. Do not publish a conflicting `rocksdict` package to PyPI.
