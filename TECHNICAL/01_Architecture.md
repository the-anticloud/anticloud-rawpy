# Technical Architecture — RAWPY

**Upstream:** [https://github.com/nicedoc/rawpy](https://github.com/nicedoc/rawpy)
**License:** MIT
**Category:** CAMERAS
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

Python RAW image processing

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B on-device scene analysis — no cloud upload
2. AIOSS tamper-evident photo/video provenance chain (C2PA aligned)
3. AES-256 encryption for all stored media
4. Single-binary firmware with bundled local AI features
5. Zero-cloud: all scene recognition, auto-settings, and editing run on-device
6. GPU/CPU equalizer: uses camera ISP/NPU, falls back to main CPU
7. Zero-telemetry: removes all usage reporting to manufacturer
8. Open RAW processing pipeline replacing proprietary software

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_rawpy.spec` or `go build -o rawpy`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |