# Build and Test

**Project:** `RAWPY`
**Upstream:** https://github.com/nicedoc/rawpy
**License:** MIT

## Quick Start

```bash
git clone https://github.com/nicedoc/rawpy
cd rawpy
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B on-device scene analysis — no cloud upload
2. AIOSS tamper-evident photo/video provenance chain (C2PA aligned)
3. AES-256 encryption for all stored media
4. Single-binary firmware with bundled local AI features
5. Zero-cloud: all scene recognition, auto-settings, and editing run on-device
6. GPU/CPU equalizer: uses camera ISP/NPU, falls back to main CPU
7. Zero-telemetry: removes all usage reporting to manufacturer
8. Open RAW processing pipeline replacing proprietary software

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.
