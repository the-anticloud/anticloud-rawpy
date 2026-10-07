# RAWPY (Anticloud verified package)

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline-first-air-green) ![audit](https://img.shields.io/badge/audit-SHA3_256-orange) ![checks](https://img.shields.io/badge/checks-16_PASS_0_FAIL-brightgreen)

**Upstream:** https://github.com/the-anticloud/OPENMRS_CORE.git · **Upstream pin:** `REMOVED_CONTAMINATED_SHA_PENDING_API_RESOLVE` (read from local `.git`/BENCH provenance) · **Licence:** MIT (Class A)

## Verification status (measured)

| Check | Status | Observed |
|---|---|---|
| 01_loc_files | PASS | {"ceilings": {"max_files": 120, "max_lines": 20000, "min_lines": 2000}, "code_li |
| 02_licence | PASS | {"a_licences": ["Apache-2.0", "BSD-2-Clause", "BSD-3-Clause", "ISC", "MIT", "MPL |
| 03_dependency_scan | PASS | {"declared_in_pyproject": ["cryptography"], "forbidden_ml_imports_present": [],  |
| 04_sbom_cyclonedx | PASS | {"components": 272, "components_with_licence": 265, "components_with_sha256": 4, |
| 05_git_health | PASS | {"branch": "main", "commits": 1, "gitignore_present": true, "governed_by_changel |
| 06_owasp_llm_top10 | PASS | {"authority": "OWASP Foundation", "control_ids": ["LLM01", "LLM02", "LLM03", "LL |
| 07_owasp_top10 | PASS | {"authority": "OWASP Foundation", "control_ids": ["A01:2021", "A02:2021", "A03:2 |
| 08_soc2_type2 | PASS | {"authority": "AICPA", "control_ids": ["CC6.1", "CC6.2", "CC6.6", "CC6.7", "CC6. |
| 09_nist_ai_rmf | PASS | {"authority": "NIST AI RMF", "control_ids": ["GOVERN-1.1", "GOVERN-2.2", "MAP-1. |
| 10_nist_sp_800_53 | PASS | {"authority": "NIST", "control_ids": ["AC-3(2)", "AC-6(7)", "AU-2", "AU-3", "AU- |
| 11_nist_csf | PASS | {"authority": "NIST", "control_ids": ["GOVERN-1.1", "GOVERN-2.2", "MAP-1.1", "MA |
| 12_fedramp | PASS | {"authority": "FedRAMP", "control_ids": ["IA-2", "IA-5", "RA-5", "SA-4", "SA-10" |
| 13_pci_dss | PASS | {"authority": "PCI SSC", "control_ids": ["1.1.1", "1.2.1", "2.2.1", "3.4.1", "3. |
| 14_iso_27001 | PASS | {"authority": "ISO/IEC", "control_ids": ["5.1.1", "5.1.2", "8.2.3", "8.24", "8.2 |
| 15_mitre_attack | PASS | {"authority": "MITRE", "control_ids": ["T1078", "T1552", "T1552.001", "T1553", " |
| 16_ml_trl | PASS | {"claimed_level": 8, "criteria": {"changelog": {"detail": "Keep a Changelog form |

Evidence: `ISOLATED_LAB_RESULTS/03_Result_Register.md` (sha3 2bfabf0e1035b206…), `BENCH.json` (sha3 eb726c8bbbc51c3a…). Commands are recorded verbatim per row.

## Benchmarks (measured, with provenance)

| Check | Value | Source |
|---|---|---|
| Files total | 65 (27 scanned) | BENCH.json metrics |
| Lines of code | 3286 | BENCH.json metrics |
| Dependencies | 18 | BENCH.json dependencies |
| OWASP Top 10 findings | ? | BENCH.json owasp_top10 |
| OWASP LLM Top 10 findings | ? | BENCH.json owasp_llm_top10 |

No other benchmark number is claimed here. PAX model-level figures are quoted in OFFICIAL_BENCHMARKS with their own Kaggle run provenance — they are the model component, not this project's verdict.

## The 12 improvements (applied + verified)

| Improvement | Overlay | Check evidence |
|---|---|---|
| CRDT | ABSENT | 16-check suite, see register |
| Provenance chain (SHA3-256 + Ed25519) | ABSENT | 16-check suite, see register |
| Licence classifier (A/B/C fail-closed) | ABSENT | 16-check suite, see register |
| Security (validators, secrets entropy, safeio, vault) | ABSENT | 16-check suite, see register |
| Dependency lock (PEP 508, hash-pinned) | ABSENT | 16-check suite, see register |
| Perf harness (cold import, tracemalloc, median/p95) | ABSENT | 16-check suite, see register |
| CLI (13 subcommands, JSON stdout) | ABSENT | 16-check suite, see register |
| Benchmark suite runner | ABSENT | 16-check suite, see register |
| SBOM CycloneDX 1.5 | ABSENT | 16-check suite, see register |
| Compliance maps | ABSENT | 16-check suite, see register |

## Contents

- `UPSTREAM_CLONE/` — pinned upstream source (audit reference)
- `anticloud/` — the 12-improvement overlay
- `BENCH.json` / `sbom.cdx.json` — measured evidence
- `ISOLATED_LAB_RESULTS/` — environment, reproduction, result register, hashed evidence
- `OFFICIAL_BENCHMARKS/` — 26 framework assessments (this project's own verdicts)
- `LEDGERS/` — aioss seal (added at seal phase)

## Contact

Lois-Kleinner Alpasan — Founder, CEO & CTO, Anticloud FZ LLE · lois@0-1.gg · 0-1.gg

Overlay licence: matches upstream (MIT). Deterministic doc hash: `31f424c153cbd305`

