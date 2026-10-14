# Students — RAWPY

**Project:** RAWPY  
**Category:** CAMERAS  
**Upstream:** see BENCH.json  
**Pinned commit:** `a39c2e7a44911889c3360891012f862f904ba551`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `6bb7ce3d8cefc14febbcec639e122f54ea872976fe54fe86e14f16c991da2061`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `a39c2e7a44911889c3360891012f862f904ba551`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `6bb7ce3d8cefc14febbcec639e122f54ea872976fe54fe86e14f16c991da2061`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
