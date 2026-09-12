# Changelog — Sovereign SmartPi Telemetry

All notable changes follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.
Versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Full ecosystem documentation suite (ARCHITECTURE, DEVELOPER_GUIDE, SME_PLAYBOOK, SOP)
- GitHub Actions CI matrix (Python 3.10 / 3.11 / 3.12)
- Multistage Dockerfile with non-root user, health check, OCI labels
- docker-compose.yml with SBB platform network
- n8n custom node integration via `SovereignTools`
- `.env.example` environment template
- CONTRIBUTING, CODE_OF_CONDUCT, SECURITY governance files
- OpenAPI 3.1 compatible REST API spec
- Bandit security scan in CI

## [1.0.0] — 2024-01-01

### Added
- Initial production release of Sovereign SmartPi Telemetry (PKG-025)
- Core microservice on port `8805`
- n8n webhook adapter (`n8n/webhook_adapter.py`)
- REST API (`POST /api/v1/execute`, `GET /health`)
- Components: AcousticCapture, FFTProcessor, BearingFaultClassifier, MQTTPublisher, AnomalyStreamer
- pyproject.toml packaging with `[dev]` extras
- CLI: `sovereign-smartpi-telemetry --help`
- Unit test suite (pure `unittest.TestCase`, no external test framework required)

### Domain: Edge Hardware & Acoustic Diagnostics
Acoustic and vibration telemetry engine for SmartPi and Raspberry Pi. Captures FFT spectra, classifies bearing faults, and streams anomaly alerts via MQTT.

[Unreleased]: https://github.com/BlackFoxgamingstudio/smartpi-telemetry/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BlackFoxgamingstudio/smartpi-telemetry/releases/tag/v1.0.0
