# 🛡️ lab virtual VMware

Laboratorio casero de ciberseguridad defensiva orientado a la operación de un SOC (Security Operations Center).

## 🎯 Objetivo

- Despliegue y operación de un SIEM (Wazuh) con IDS/IPS (Suricata) y gestión de casos (TheHive).
- Detección de ataques en endpoints Linux y Windows.
- Análisis de incidentes y mapeo a MITRE ATT&CK.
- Automatización de triaje con Python.
- Documentación técnica de casos reales.

## 🏗️ Arquitectura del laboratorio

El laboratorio se compone de un SIEM central (Wazuh) que recibe logs de varios agentes, más máquinas atacantes y víctimas.

Máquinas del laboratorio:

- Wazuh Manager — SIEM central — Ubuntu Server 22.04 — 192.168.1.10
- Kali Linux — Atacante — Kali Rolling — 192.168.1.20
- Metasploitable — Víctima Linux — Ubuntu 8.04 — 192.168.1.30
- Windows 10 — Víctima Windows — Windows 10 Pro — 192.168.1.40
- Ubuntu Desktop — Cliente — Ubuntu 22.04 — 192.168.1.50

## 🧪 Casos de uso documentados

- 01 — Fuerza bruta SSH — MITRE T1110
- 02 — Detección en Windows con Sysmon — MITRE T1059, T1055
- 03 — Análisis de phishing por SMS — MITRE T1566
- 04 — Enumeración de red con Nmap — MITRE T1046
- 05 — Mitigación con IPTables — MITRE T1562

## 🛠️ Tecnologías utilizadas

- SIEM: Wazuh 4.x
- IDS/IPS: Suricata
- Gestión de casos: TheHive
- Endpoint: Sysmon, Agente Wazuh
- Ataque: Kali Linux, Metasploit, Nmap, Hydra, Burp Suite
- Automatización: Python 3, Bash, PowerShell
- Redes: Cisco IOS (ACLs, Zero Trust)

## 📫 Contacto

- LinkedIn: https://www.linkedin.com/in/franco-aravena-323186368/
- GitHub: https://github.com/FrancoJaq2
