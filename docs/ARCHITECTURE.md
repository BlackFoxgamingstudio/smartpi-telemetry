# Architecture: Sovereign SmartPi Telemetry

## Overview

**Package ID:** `PKG-025`  
**Domain:** Edge Hardware & Acoustic Diagnostics  
**Microservice Port:** `8805`  
**n8n Webhook Path:** `smartpi-telemetry-trigger`  
**GitHub:** [BlackFoxgamingstudio/smartpi-telemetry](https://github.com/BlackFoxgamingstudio/smartpi-telemetry)

Acoustic and vibration telemetry engine for SmartPi and Raspberry Pi. Captures FFT spectra, classifies bearing faults, and streams anomaly alerts via MQTT.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign SmartPi Telemetry   │
                     │       Port: 8805            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  AcousticCapture | FFTProcessor    | BearingFault  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `AcousticCapture`
Handles all acousticcapture operations. Exposes async methods callable from the core dispatcher.

### `FFTProcessor`
Handles all fftprocessor operations. Exposes async methods callable from the core dispatcher.

### `BearingFaultClassifier`
Handles all bearingfaultclassifier operations. Exposes async methods callable from the core dispatcher.

### `MQTTPublisher`
Handles all mqttpublisher operations. Exposes async methods callable from the core dispatcher.

### `AnomalyStreamer`
Handles all anomalystreamer operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-smartpi-telemetry", "port": 8805}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-smartpi-telemetry:
  image: sovereign-smartpi-telemetry:latest
  ports: ["8805:8805"]
  healthcheck:
    test: curl -f http://localhost:8805/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`iot`, `acoustic`, `fft`, `mqtt`
