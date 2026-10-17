# Release and discoverability checklist

This repository is a landing page for the existing unofficial RocksDict 11 source fork and wheels. The commands and GitHub settings below are for a maintainer who controls **both** repositories.

## Make the repository discoverable

- [ ] Change `raled573343/rocksdict-rocksdb11` from **Private** to **Public** in **Settings → General → Danger Zone → Change repository visibility**. Check for secrets/history before publishing.
- [ ] Confirm the README and license pages render in an incognito browser.
- [ ] Set the **About** description: `Unofficial RocksDict with RocksDB 11.8.1 and free-threaded Python 3.14t (cp314t) wheels for Windows/Linux x64.`
- [ ] Set the **Website** field to the actual binaries: `https://github.com/raled573343/RocksDict/releases/tag/cp314t-v0.3.29-rocksdb11.8.1`.
- [ ] Add GitHub Topics: `rocksdb`, `rocksdict`, `python`, `python314`, `free-threading`, `nogil`, `rust`, `python-bindings`, `key-value-store`, `windows`, `linux`. Use accurate topics only.
- [ ] Enable Discussions/Issues if you intend to accept support directly in this landing repo; otherwise keep the source fork issue link prominent.
- [ ] Check search results after GitHub and external crawlers have had a chance to index the now-public repository. Indexing is not immediate or guaranteed.

## Verify the wheel files and legal notices

- [ ] Inspect the **exact** two release wheels, especially wheel `*.dist-info` license metadata.
- [ ] Inventory all incorporated native libraries and review corresponding third-party license text/NOTICE requirements; see [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
- [ ] Provide redistributable license/attribution notices with the binaries and fix packaging if required.
- [ ] Retain upstream copyrights and identify independent modifications clearly.
- [ ] Confirm checksums and CI link correspond to the published artifacts, not a later branch head.
- [ ] Run install/import/version/GIL checks with actual free-threaded Python 3.14 on each supported platform.

## If moving the actual release assets to this repository later

Currently the **canonical release stays in** [raled573343/RocksDict](https://github.com/raled573343/RocksDict/releases). Do not create an empty or misleading release entry here.

1. Complete the binary license/attribution audit above.
2. Download both wheels and `SHA256SUMS` from the verified release, without rebuilding or renaming them.
3. Publish a matching release/tag **with both wheels and hashes** in this repository, using [RELEASE_NOTES.md](RELEASE_NOTES.md) as the release description, **explicitly stating** that the compiled source is in the other repository at commit `b2958391ddea0530e0f299295f7b5fad43590a3a`. The source archive auto-generated for a documentation-only tag here is **not** the wheel source tree.
4. Compare published SHA-256 values byte-for-byte with the original release; do not silently repackage wheels.
5. Update all README, Website and announcement links to the new canonical release only **after** the assets are actually available.
6. For future releases, consider moving source + workflows + legal notices into one authoritative project repository, to avoid making users follow two repositories.

## Communicate responsibly

Suggested announcement title: **Unofficial RocksDict + RocksDB 11.8.1 wheels for free-threaded Python 3.14t (Windows/Linux)**.

Describe precisely: the supported Python ABI, included platforms, linked test run, pinned native version, experimental state, and upstream credit. Avoid `official`, `fully thread-safe`, `production certified`, `drop-in upgrade`, or universal-performance claims without independent evidence. Do not publish a conflicting `rocksdict` package to PyPI.
