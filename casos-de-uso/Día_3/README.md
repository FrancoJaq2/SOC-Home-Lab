# 🛡️ INC-20261002-001 — Caída y Recuperación del Wazuh Manager

![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-blue)
![Status](https://img.shields.io/badge/Status-Cerrado-green)
![Severidad](https://img.shields.io/badge/Severidad-Alta-red)
![Clasificación](https://img.shields.io/badge/Clasificaci%C3%B3n-Verdadero%20Positivo%20Benigno-yellow)

## 📋 Resumen del Incidente

| Campo | Valor |
|-------|-------|
| **ID** | INC-20261002-001 |
| **Fecha de Detección** | 2026-10-01 17:19 UTC |
| **Fecha de Cierre** | 2026-10-02 18:14 UTC |
| **Analista** | Jaq |
| **Clasificación** | Verdadero Positivo Benigno |
| **Severidad** | Alta (SIEM caído ~25 horas) |
| **Activo Afectado** | `siem-server` (192.168.1.10) — Ubuntu Server |

---

## 🎯 Objetivo del Ejercicio

Diagnosticar y resolver la caída del **Wazuh Manager** que impedía el funcionamiento del dashboard. El objetivo era restaurar el servicio sin reinstalar, entendiendo la causa raíz.

**Entorno del laboratorio:**
- **SIEM:** Wazuh (Ubuntu Server) — `siem-server` (192.168.1.10)
- **Endpoint:** EndeavourOS (Arch Linux) — `endpoint-lab` (192.168.1.20)
- **Conexión:** SSH desde el endpoint hacia el servidor SIEM.

---

## 🚨 Detección

El dashboard de Wazuh mostraba el error **"API is down"**. Los checks de conexión fallaban.

**Evidencia del Dashboard (Health Check):**

![Dashboard API is down](./capturas/dashboard-api-down.png)

*Nota: La captura ha sido sanitizada.*

**Detalles del fallo:**
- **Servicio:** `wazuh-manager`
- **Estado:** `failed (Result: timeout)`
- **Error en logs:** `List 'etc/lists/malicious-ioc/malicious-ip' could not be loaded`

---

## 🔍 Investigación y Evidencia

Una vez detectado el problema, se procedió a investigar en el **servidor SIEM**.

### 1. Verificación del estado de los servicios

```bash
$ sudo systemctl status wazuh-indexer wazuh-manager wazuh-dashboard --no-pager
```

**Interpretación:** El indexer estaba arrancando, el dashboard funcionaba, pero el manager estaba en `failed (timeout)`.

### 2. Revisión de logs del manager

```bash
$ sudo journalctl -u wazuh-manager -n 50 --no-pager
```

**Interpretación:** Se detectó el warning de la lista `malicious-ip` que no se podía cargar.

### 3. Verificación de la lista problemática

```bash
$ sudo cat /var/ossec/etc/lists/malicious-ioc/malicious-ip
196.251.85.62:
196.251.67.42:
196.251.69.43:
...
```

**Interpretación:** Las IPs tenían dos puntos (`:`) al final, lo cual es un formato incorrecto. Wazuh espera una IP por línea sin caracteres extra.

---

## 🧠 Análisis

**Hipótesis evaluadas:**

1. **Fallo de red (Descartada):** El indexer y el dashboard funcionaban.
2. **Corrupción de base de datos (Descartada):** El indexer estaba `active (running)`.
3. **Timeout de systemd (Confirmada):** El manager tardaba más de 45 segundos en arrancar.
4. **Lista de IOCs con formato incorrecto (Confirmada):** La lista `malicious-ip` tenía IPs con dos puntos al final.

**Razonamiento:**
El manager carga reglas, decodificadores y listas al arrancar. Una lista con formato incorrecto ralentiza el arranque. Sumado a un timeout de systemd demasiado corto, el manager nunca terminaba de arrancar. La solución fue doble: aumentar el timeout y corregir el formato de la lista.

---

## ✅ Acciones Tomadas

- [x] Diagnóstico del estado de los servicios (`systemctl status`).
- [x] Revisión de logs (`journalctl`).
- [x] Aumento del timeout de systemd a 300 segundos.
- [x] Corrección del formato de la lista `malicious-ip` (eliminación de dos puntos).
- [x] Reinicio del manager y verificación.
- [x] Verificación del dashboard (todos los checks en verde).

---

## 📚 Lecciones Aprendidas

1. **Timeout de systemd:** Los servicios pesados pueden necesitar más tiempo del que systemd les da por defecto.
2. **Formato de listas de IOCs:** Wazuh espera un formato específico (una IP por línea, sin puertos ni dos puntos).
3. **Diagnóstico por capas:** Verificar cada componente por separado ayuda a aislar el problema.
4. **Documentación:** Tener un runbook de recuperación ahorra tiempo en futuros incidentes.

---

## 📎 Anexos

- [Captura del dashboard con "API is down" (sanitizada)](./capturas/dashboard-api-down.png)
- [Captura del dashboard funcionando correctamente (sanitizada)](./capturas/dashboard-ok.png)
- [Informe completo del incidente (INC-20261002-001.md)](./INC-20261002-001.md)

---

## 🔗 Referencias

- [Wazuh Documentation — Troubleshooting](https://documentation.wazuh.com/current/user-manual/index.html)
- [systemd — TimeoutStartSec](https://www.freedesktop.org/software/systemd/man/systemd.service.html)
- [Home SOC Lab — Repositorio Principal](https://github.com/FrancoJaq2/SOC-Home-Lab)

---

*Informe elaborado como parte del Home SOC Lab. Datos personales sanitizados según la política del repositorio.*
