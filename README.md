# dockguard
Automated Docker image vulnerability scanning for CI/CD pipelines

# DockGuard

> Automated vulnerability scanning for Docker images, built for CI/CD pipelines.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-pre--release-orange.svg)

**DockGuard** scans Docker images for known vulnerabilities (CVEs), misconfigurations, and exposed secrets, and plugs into your CI/CD pipeline so insecure images are blocked **before** they reach production.

> ⚠️ **Status: pre-release.** The project is under active development. APIs, flags, and report formats may change until `v1.0.0`.

---

## Why DockGuard?

Many small teams, students, and organizations deploy containers without any image security checks, because enterprise tools are expensive or complex to set up. DockGuard aims to be:

- **Simple**: one command locally, one step in your pipeline
- **Free and open source**: no accounts, no licenses
- **Actionable**: reports tell you *what* is wrong, *how severe* it is, and *how to fix it*
- **Pipeline-first**: fails the build when your policy is violated

## Features

| Feature | Status |
|---|---|
| OS package vulnerability scanning (CVE) | 🚧 Planned |
| Application dependency scanning (pip, npm, etc.) | 🚧 Planned |
| Dockerfile misconfiguration checks | 🚧 Planned |
| Secret detection in image layers | 🚧 Planned |
| Severity threshold policy (fail on HIGH/CRITICAL) | 🚧 Planned |
| Reports: terminal, JSON, HTML, SARIF | 🚧 Planned |
| GitHub Actions integration | 🚧 Planned |
| GitLab CI integration | 🚧 Planned |
| Documentation in English and Uzbek | 🚧 Planned |

> Update the status column as features are completed (✅ Done / 🚧 In progress / 🚧 Planned).

## Quick Start

> Commands below are the **target interface**; adjust them to match your implementation.

### Requirements

- Docker 20.10+
- [Python 3.10+ / Go 1.21+ / Node 18+] *(choose your stack)*

### Installation

```bash
git clone https://github.com/<your-username>/dockguard.git
cd dockguard
# install steps here
```

### Scan an image

```bash
dockguard scan nginx:latest
```

### Fail on severity

```bash
dockguard scan myapp:1.0 --fail-on high
```

### Export a report

```bash
dockguard scan myapp:1.0 --format json --output report.json
```

## CI/CD Integration

### GitHub Actions

```yaml
name: Image Security Scan

on: [push, pull_request]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build image
        run: docker build -t myapp:${{ github.sha }} .

      - name: Scan image
        run: dockguard scan myapp:${{ github.sha }} --fail-on high --format sarif --output results.sarif

      - name: Upload results
        uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: results.sarif
```

### GitLab CI

```yaml
image-scan:
  stage: test
  script:
    - docker build -t myapp:$CI_COMMIT_SHORT_SHA .
    - dockguard scan myapp:$CI_COMMIT_SHORT_SHA --fail-on high
```

## How It Works

```
 Docker image
      │
      ▼
 Extract layers & metadata
      │
      ▼
 Detect packages / dependencies
      │
      ▼
 Match against vulnerability database
      │
      ▼
 Apply policy (severity threshold)
      │
      ▼
 Report (terminal / JSON / HTML / SARIF)  →  pass / fail build
```

## Configuration

Example `dockguard.yml`:

```yaml
severity_threshold: high     # fail build on high and above
ignore_unfixed: false        # ignore vulns with no available fix
ignore_cves:
  - CVE-0000-00000           # accepted risk (document why)
formats:
  - terminal
  - json
```

## Roadmap

- [ ] **v0.1**: image scanning for OS packages, terminal and JSON reports
- [ ] **v0.2**: severity policy, exit codes for CI
- [ ] **v0.3**: GitHub Actions and GitLab CI templates
- [ ] **v0.4**: Dockerfile misconfiguration and secret detection
- [ ] **v0.5**: HTML and SARIF reports
- [ ] **v1.0**: stable API, full documentation (EN + UZ)

## Contributing

Contributions are welcome. Whether it's a bug report, a new check, or a documentation fix:

1. Fork the repository
2. Create a branch: `git checkout -b feature/my-feature`
3. Commit your changes and add tests
4. Open a Pull Request

Please read `CONTRIBUTING.md` *(to be added)* before submitting larger changes.

## Security

If you find a security issue in DockGuard itself, please **do not** open a public issue. Contact: `<your-email>`.

## License

Released under the [MIT License](LICENSE).

## Acknowledgments

*(List the vulnerability data sources and open-source tools your project builds on.)*

---

# 🇺🇿 O'zbekcha qisqa tavsif

**DockGuard** Docker obrazlaridagi zaifliklarni (CVE), noto'g'ri sozlamalarni va ochiq qolgan maxfiy ma'lumotlarni avtomatik skanerlaydi va CI/CD jarayoniga integratsiya qilinadi. Xavfli obraz production'ga chiqmasdan oldin build to'xtatiladi.

Loyiha hozircha **pre-release** bosqichida. To'liq o'zbekcha hujjatlar `docs/uz/` papkasida qo'shiladi.
