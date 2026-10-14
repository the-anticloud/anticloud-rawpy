# Educators — RAWPY

**Project:** RAWPY  
**Category:** CAMERAS  
**Upstream:** see BENCH.json  
**Pinned commit:** `a39c2e7a44911889c3360891012f862f904ba551`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `6bb7ce3d8cefc14febbcec639e122f54ea872976fe54fe86e14f16c991da2061`  
**Date:** October 2026

## Teaching with RAWPY

The project is usable as a worked example of offline-first packaging with a
cryptographic audit chain. It ships with the assurance suite, the evidence
register and the ledger, so students can verify claims rather than take them on
faith.

## Suggested exercises

1. Run `python tools/run_bench.py --out BENCH.json` and read the 16 results.
2. Recompute the SHA3-256 of a row's evidence file and compare to the register.
3. Walk the AIOSS chain from genesis to head `6bb7ce3d8cefc14febbcec639e122f54ea872976fe54fe86e14f16c991da2061` and confirm every link.
4. Break one artifact and observe the chain fail to verify.

## Licence for teaching

Apache 2.0 terms apply to academic and teaching use. See `24_ANTICOMMONS_LICENSE`.
