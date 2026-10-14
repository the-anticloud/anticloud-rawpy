# Independent Insurance — RAWPY

**Project:** RAWPY  
**Category:** CAMERAS  
**Upstream:** see BENCH.json  
**Pinned commit:** `a39c2e7a44911889c3360891012f862f904ba551`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `6bb7ce3d8cefc14febbcec639e122f54ea872976fe54fe86e14f16c991da2061`  
**Date:** October 2026

## Why AI-specific cover matters

Deploying AI in a regulated sector creates liability surfaces that ordinary
technology cover does not reach: inference liability, audit-trail liability,
data-breach liability and IP-infringement liability.

## How this project's architecture reduces insurable risk

| Risk | Cloud AI | RAWPY with AIOSS |
|---|---|---|
| Audit-trail loss | high — vendor-controlled logs | low — append-only chain, verifiable offline |
| Data breach in transit | high — data transits external servers | low — no external endpoint |
| Compliance violation | high — cannot satisfy air-gap requirements | low — structural |
| IP liability | moderate | low — pinned provenance chain |

## Evidence package for an insurer

- AIOSS chain verification for head `6bb7ce3d8cefc14febbcec639e122f54ea872976fe54fe86e14f16c991da2061`
- The 16-check register with per-check evidence hashes
- Framework control mapping in `BENCH.json`

## Contact

lois@0-1.gg
