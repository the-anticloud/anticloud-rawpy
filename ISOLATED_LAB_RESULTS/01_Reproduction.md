# Isolated Lab Results — Reproduction Contract

**Project:** `RAWPY`
**Upstream:** https://github.com/nicedoc/rawpy
**Category:** CAMERAS

## Reproduction Contract

A result is admissible here only if all of the following are recorded:

1. The exact commit hash of this fork and the upstream.
2. The environment: OS, Python version, GPU model, CUDA version.
3. The verbatim command run.
4. The pass condition, stated before the run.
5. The observed output, unedited.

## Anticloud-Specific Checks

- PAX L5 Narrow L2 General 27B inference produces output locally (no network call)
- AIOSS ledger file created and contains at least one entry
- AES-256 encrypted output file is non-empty and decryptable with test key
- Single-binary executable runs on clean machine without pip install

## Current State

No lab result recorded yet. The register below is the template:

| Test | Pass Condition | Command | Observed | Status |
| --- | --- | --- | --- | --- |
| Offline PAX inference | Returns text without network | `python test_pax_local.py` | (unrecorded) | NOT RUN |
| AIOSS ledger creation | File exists with genesis hash | `python test_aioss.py` | (unrecorded) | NOT RUN |
| Encryption roundtrip | Decrypt(Encrypt(x)) == x | `python test_crypto.py` | (unrecorded) | NOT RUN |
| Single binary | Runs on clean VM | `./dist/rawpy` | (unrecorded) | NOT RUN |
