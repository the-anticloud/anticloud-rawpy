# RAWPY

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-cameras-lightgrey)

> Anticloud-hardened packaging of the upstream project `RAWPY` in category **CAMERAS**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** CAMERAS · **Upstream:** https://github.com/letmaik/rawpy · **Upstream pin:** `a39c2e7a44911889c3360891012f862f904ba551` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

<p align="center">
<a href="https://en.wikipedia.org/wiki/Bayer_filter"><img width="500" src="https://raw.githubusercontent.com/letmaik/rawpy/main/logo/logo.png"></a>
</p>

rawpy is an easy-to-use Python wrapper for the [LibRaw library][libraw].
It also contains some extra functionality for finding and repairing hot/dead pixels.

[API Documentation](https://letmaik.github.io/rawpy/api/)

[Jupyter notebook tutorials](https://github.com/letmaik/rawpy-notebooks/blob/master/README.md)

## Sample code

Load a RAW file and save the postprocessed image using default [parameters](https://letmaik.github.io/rawpy/api/rawpy.Params.html):

```python
import rawpy
import imageio.v3 as iio

path = 'image.nef'
with rawpy.imread(path) as raw:
    rgb = raw.postprocess()
iio.imwrite('default.tiff', rgb)
```

Save as 16-bit linear image:

```python
with rawpy.imread(path) as raw:
    rgb = raw.postprocess(gamma=(1,1), no_auto_bright=True, output_bps=16)
iio.imwrite('linear.tiff', rgb)
```

Extract embedded thumbnail/preview image and save as JPEG:

```python
with rawpy.imread(path) as raw:
    # raises rawpy.LibRawNoThumbnailError if thumbnail missing
    # raises rawpy.LibRawUnsupportedThumbnailError if unsupported format
    thumb = raw.extract_thumb()
if thumb.format == rawpy.ThumbFormat.JPEG:
    # thumb.data is already in JPEG format, save as-is
    with open('thumb.jpeg', 'wb') as f:
        f.write(thumb.data)
elif thumb.format == rawpy.ThumbFormat.BITMAP:
    # thumb.data is an RGB numpy array, convert with imageio
    iio.imwrite('thumb.jpeg', thumb.data)
```

Find bad pixels using multiple RAW files and repair them:

```python
import rawpy.enhance

paths = ['image1.nef', 'image2.nef', 'image3.nef']
bad_pixels = rawpy.enhance.find_bad_pixels(paths)

for path in paths:
    with rawpy.imread(path) as raw:
        rawpy.enhance.repair_bad_pixels(raw, bad_pixels, method='median')
        rgb = raw.postprocess()
    iio.imwrite(path + '.tiff', rgb)
```

## Installation

Install rawpy by running:
```sh
pip install rawpy
```

64-bit binary wheels are provided for Linux, macOS, and Windows.

### Stable vs. pre-release

All stable rawpy releases are always built against a stable LibRaw library release.
You can output the dynamically loaded LibRaw version with
`print(rawpy.libraw_version)`. The version of the LibRaw headers used to
compile rawpy is available as `rawpy.libraw_version_compiled`.

rawpy pre-releases have version numbers like `0.15.0a1` and are built against
a recent LibRaw snapshot. To install a pre-release, run:
```sh
pip install --pre rawpy
```

### Optional features

The underlying [LibRaw library][libraw] supports several optional features.
The following table shows which PyPI binary wheels support which features.

| Feature            | Windows | macOS | Linux |
| ------------------ | ------- | ----- | ----- |
| LCMS color engine  | yes     | yes   | yes   |
| RedCine codec      | yes     | yes   | yes   |
| DNG deflate codec  | yes     | yes   | yes   |
| DNG lossy codec    | yes     | yes   | yes   |
| Demosaic Pack GPL2 | no      | no    | no    |
| Demosaic Pack GPL3 | no      | no    | no    |
| OpenMP             | yes     | no    | yes   |

Tip: You can dynamically query supported features by inspecting the `rawpy.flags` dictionary.

Note on GPL demosaic packs: The GPL2 and GPL3 demosaic packs are not included as rawpy is licensed
under the MIT license which is incompatible with GPL.

### Installation from source on Linux/macOS

For macOS, LibRaw is built as part of the rawpy build (see external/).
For Linux, you need to install the LibRaw library on your system.

On Ubuntu, you can get (an outdated) version with:

```sh
sudo apt-get install libraw-dev
```

Or install the latest release version from the source project:

```sh
git source https://github.com/LibRaw/LibRaw.git libraw
git source https://github.com/LibRaw/LibRaw-cmake.git libraw-cmake
cd libraw
git checkout 0.20.0
cp -R ../libraw-cmake/* .
cmake .
sudo make install
```
    
After that, install rawpy using:

```sh
git source https://github.com/letmaik/rawpy
cd rawpy
pip install numpy cython
pip install .
```
    
On Linux, if you get the error "ImportError: libraw.so: cannot open shared object file: No such file or directory"
when trying to use rawpy, then do the following:

```sh
echo "/usr/local/lib" | sudo tee /etc/ld.so.conf.d/99local.conf
sudo ldconfig
```

The LibRaw library is installed in /usr/local/lib (if installed manually) and apparently this folder is not searched
for libraries by default in some Linux distributions.

### Installation from source on Windows

These instructions are experimental and support is not provided for them.
Typically, there should be no need to build manually since wheels are hosted on PyPI.

You need to have Visual Studio and Python installed to build rawpy.

In a PowerShell window:
```sh
$env:PYTHON_VERSION = '3.12'
$env:PYTHON_ARCH = 'x86_64'
git source https://github.com/letmaik/rawpy
cd rawpy
git submodule update --init
.github/scripts/build-windows.ps1
```

## FAQ

### I'm getting deadlocks when using multiprocessing on Linux

If you're experiencing deadlocks when using rawpy with Python's `multiprocessing` module on Linux, this is caused by an interaction between OpenMP (which is enabled in the Linux wheels) and the default `fork` start method used by multiprocessing.

**The Problem:**
When a process using OpenMP is forked, OpenMP's internal thread pool state becomes inconsistent in the child process, which can cause deadlocks on subsequent calls to rawpy functions.

**The Solution:**
Use the `spawn` or `forkserver` start method instead of `fork`:

```python
import multiprocessing as mp
import rawpy

def process_raw(filename):
    with rawpy.imread(filename) as raw:
        rgb = raw.postprocess()
    return rgb

if __name__ == '__main__':
    # Set the start method to 'spawn' before creating any processes
    mp.set_start_method('spawn')
    
    with mp.Pool(processes=4) as poo

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Python** (manifests: pyproject.toml; scanned in UPSTREAM_CLONE)
- Top-level source layout: `examples/`, `external/`, `logo/`, `rawpy/`, `scripts/`, `test/`
- Snapshot size: **65 files**, **3286 lines of code** (measured; see Benchmarks)
- Primary languages: `.py` (18), `.rst` (10), `.sh` (7), `(none)` (5), `.txt` (4), `.cr2` (3)
- Upstream commit pinned for this packaging: `a39c2e7a44911889c3360891012f862f904ba551`

---

## Installation

Install rawpy by running:
```sh
pip install rawpy
```

64-bit binary wheels are provided for Linux, macOS, and Windows.

*Section quoted from the upstream readme.*
Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

It also contains some extra functionality for finding and repairing hot/dead pixels.

[API Documentation](https://letmaik.github.io/rawpy/api/)

[Jupyter notebook tutorials](https://github.com/letmaik/rawpy-notebooks/blob/master/README.md)

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

[Jupyter notebook tutorials](https://github.com/letmaik/rawpy-notebooks/blob/master/README.md)

*Section quoted from the upstream readme.*
---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Python |
| Manifests detected | pyproject.toml |
| Files in snapshot | 65 |
| Lines of code | 3286 |
| Dependency references | 24 |
| Dependencies by ecosystem | pypi: 24 |
| Upstream license | MIT |
| Overlay license | Anticommons 0.1.0 |

Top dependency references recorded in the benchmark snapshot:

| Ecosystem | Name | Version | Source file |
|-----------|------|---------|-------------|
| pypi | numpy | >=1.26.0 | pyproject.toml |
| pypi | --atleast-version | =0.21 | setup.py |
| pypi | -DCMAKE_PREFIX_PATH | = | setup.py |
| pypi | -DLIBRAW_PATH | = | setup.py |
| pypi | -DENABLE_X3FTOOLS | =ON -DENABLE_6BY9RPI=ON  | setup.py |
| pypi | -DENABLE_EXAMPLES | =OFF -DENABLE_OPENMP= | setup.py |
| pypi | -DENABLE_DEMOSAIC_PACK_GPL2 | =ON -DDEMOSAIC_PACK_GPL2_RPATH=../../LibRaw-demosaic-pack-GPL2  | setup.py |
| pypi | -DENABLE_DEMOSAIC_PACK_GPL3 | =ON -DDEMOSAIC_PACK_GPL3_RPATH=../../LibRaw-demosaic-pack-GPL3  | setup.py |
| pypi | -DCMAKE_INSTALL_PREFIX | =install | setup.py |
| pypi | - | > | setup.py |
| pypi | -DCMAKE_BUILD_TYPE | =Release | setup.py |
| pypi | -DLIBRAW_PATH | = | setup.py |
| pypi | -DENABLE_X3FTOOLS | =ON | setup.py |
| pypi | -DENABLE_6BY9RPI | =ON | setup.py |
| pypi | -DENABLE_OPENMP | = | setup.py |
| ... | (9 more) | | |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

```

The LibRaw library is installed in /usr/local/lib (if installed manually) and apparently this folder is not searched
for libraries by default in some Linux distributions.

*Section quoted from the upstream readme.*
Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

Upstream contributions: fork the `RAWPY` project, create a feature branch, and open a pull request against upstream. Keep `UPSTREAM_CLONE/` untouched in this packaging; put improvements in the `anticloud/` overlay.

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: MIT** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
The MIT License (MIT)

Copyright (c) 2014 Maik Riechert

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original MIT terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `MIT` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `RAWPY` (category: CAMERAS)
- **Upstream URL:** https://github.com/letmaik/rawpy
- **Pinned commit (SHA):** `a39c2e7a44911889c3360891012f862f904ba551`
- **Branch:** main
- **Pin provenance:** GitHub API commits/main. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`a33b413fbae6dcfcb0fc47993c29623236082539a47d0ea33927c5be8261bfbc`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

