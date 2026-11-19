# RustXec: A Vulnerability Reproduction Dataset for Assessing Security Risks in Open-Source Rust Applications

This work presents RustXec, a comprehensive reproducible dataset for Rust vulnerabilities. RustXec includes 102 vulnerabilities from RustSec spanning 2021-2025, which covers eight vulnerability categories. For each vulnerability, we provide verified PoVs to demonstrate the vulnerability as described in the vulnerability report and provide a containerized execution environment, enabling developers to easily reproduce vulnerabilities without complex environment setup. 

RustXec is available at Zenodo: https://zenodo.org/records/17508733

## Demo Video

https://github.com/user-attachments/assets/60b75120-c0a3-408f-9da0-6f929793d91e

## Dataset Overview

The repository ships with `metadata.csv` (vulnerability metadata), `dockers-clean-tar/` (102 compressed test cases), and `docker-images-tar/` (pre-built Docker images).

After extraction (see Environment Setup), test cases are available in `dockers-clean/`. Each test case contains a Dockerfile, Makefile, vulnerable source code (`env/source/`), and example outputs (`env/example-log/`).

`metadata.csv` contains vulnerability details from RustSec advisories:

- `id`: RustSec ID (e.g., `RUSTSEC-2021-0003`)
- `package`: Affected crate name
- `date`: Report date
- `categories`: Vulnerability category
- `CWE`: CWE identifiers
- `url`: Primary reference URL
- `references`: Additional URLs
- `severity`: Critical / High / Medium / Low
- `aliases`: CVE and GHSA identifiers
- `keywords`: Descriptive tags
- `versions`: Patched and unaffected version ranges
- `affected` / `affected.functions`: Vulnerable functions
- `fix commit links`: Fix commit URLs
- `pov candidate links`: PoV source URLs

## System Requirements

The following system configuration is required:

1. OS: Ubuntu 22.04 / 24.04
2. Disk: 300 GB minimum

## Environment Setup (~90 min)

Complete the following steps to set up the environment:

1. Install packages: Run `sudo apt install -y curl git make build-essential`
2. Install Docker: See [Install Docker Engine](https://docs.docker.com/engine/install/ubuntu/)
3. Extract test cases (~30 min): Run `make docker-extract-all`
4. Setup Docker environment (~60 min): Run `make docker-build-all` or `make docker-import-all`

## Running a Single Test Case

To analyze a specific vulnerability (e.g., RUSTSEC-2021-0003):

1. Navigate to test case: `cd dockers-clean/RUSTSEC-2021-0003`
2. Build Docker environment: `make docker-build`
3. Compile vulnerable project: `make cargo-build`
4. Execute PoV test: `make cargo-test`

Test results are saved in `env/log/`. Compare with `env/example-log/` for validation.

To cleanup: `make cargo-clean && make docker-clean`

## Running All Test Cases (~45 min)

To build and test all 102 vulnerabilities: `make cargo-build-all && make cargo-test-all`

To cleanup all artifacts: `make cargo-clean-all && make docker-clean-all`
