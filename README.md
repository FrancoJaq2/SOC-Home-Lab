# SOC Home Lab — Wazuh (Hardware)

Laboratorio casero de ciberseguridad defensiva orientado a la operación de un SOC (Security Operations Center), construido sobre hardware real.

## Objetivo

- Despliegue y operación de un SIEM (Wazuh) sobre infraestructura física.
- Detección y análisis de eventos en endpoints Linux.
- Triage manual de alertas y clasificación de incidentes.
- Documentación técnica de casos reales como portafolio profesional.

## Arquitectura

| Rol | Hostname | IP | Sistema operativo | Función |
|-----|----------|----|-------------------|---------|
| Servidor SIEM | `siem-server` | 192.168.1.15 | Ubuntu Server (headless) | Wazuh Manager, Indexer y Dashboard |
| Agente | `workstation-01` | 192.168.1.11 | EndeavourOS (Arch Linux) | Endpoint monitoreado |

Administración del servidor por SSH. Red local privada (RFC 1918), sin exposición a internet.

## Stack

- **Wazuh 4.14.x** — SIEM/XDR (Manager, Indexer, Dashboard)
- **OpenSearch** — motor de indexación y búsqueda
- **GitHub** — documentación y portafolio

## Hoja de ruta

- [x] **Etapa 1** — Triage manual básico
- [ ] **Etapa 2** — Estructuración de la investigación e informes de incidente
- [ ] **Etapa 3** — Métricas de cierre de jornada (shift report)
- [ ] **Etapa 4** — Enriquecimiento automático (VirusTotal / AbuseIPDB)
- [ ] **Etapa 5** — Respuesta activa (bloqueo automático de IP)
- [ ] **Etapa 6** — Escalamiento de incidentes (handoff a N2)
- [ ] **Etapa 7** — Integración de ticketing (osTicket)

## Informes de incidente

| ID | Fecha | Caso | Regla | Clasificación | Informe |
|----|-------|------|-------|---------------|---------|
| INC-20260925-001 | 2026-09-25 | Rootcheck sobre `/usr/bin/diff` | 510 | Falso positivo | [Ver](./casos-de-uso/INC-20260925-001.md) |

## Estructura del repositorio


## 📫 Contacto

- LinkedIn: https://www.linkedin.com/in/franco-aravena-323186368/
- GitHub: https://github.com/FrancoJaq2
