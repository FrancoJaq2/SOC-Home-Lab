**Análisis de Vulnerabilidades, Plan de Mitigación,**

**Reorganización de Red y Comunicación de Hallazgos**

**Alumno:** Franco Aravena Quinteros

**Profesora:** Caroline Camus Rivas

**Curso:** Auditorías y Hardening

**Fecha de entrega:** 12 de mayo de 2026

INSTITUTO PROFESIONAL DE CHILE -- IPCHILE

**INTRODUCCIÓN**

------------------------------------------------------------------------

El presente informe corresponde a una auditoría de ciberseguridad sobre
TecnoData SpA, empresa chilena proveedora de servicios de almacenamiento
digital, respaldo de información y análisis de datos, con sede en
Santiago. El contexto surge a raíz de un aumento inusual de alertas en
el SIEM de la organización, en un escenario nacional donde los
incidentes de ciberseguridad afectan crecientemente a instituciones
públicas y privadas.

Durante una auditoría interna, el equipo detectó servidores sin parches
críticos del CSIRT de Gobierno, servicios obsoletos habilitados y
políticas de contraseñas que no cumplían con la Norma Técnica de
Seguridad de la Información del Estado de Chile (NTSI). Ante esto, la
gerencia solicitó una evaluación urgente del nivel de riesgo.

Para abordar dicha evaluación se utilizaron Nessus Professional y
OpenVAS para el escaneo de vulnerabilidades, las metodologías OWASP
sobre los portales web de clientes nacionales, y Metasploit para pruebas
de penetración controladas que confirmaron la posibilidad real de
comprometer datos sensibles. Además, se analizaron registros del
firewall perimetral, detectando segmentación de red insuficiente,
cifrado irregular en bases de datos y falencias en el monitoreo
continuo.

Este informe se estructura en cuatro secciones: (1) identificación de
vulnerabilidades críticas; (2) plan de mitigación priorizado; (3)
reorganización de red y controles de acceso; y (4) comunicación ética de
hallazgos a la gerencia. Todas las respuestas se fundamentan en
normativa chilena vigente y estándares internacionales de
ciberseguridad, con el objetivo de entregar a TecnoData SpA un plan
integral que fortalezca su postura de seguridad y proteja los activos
digitales de las entidades que confían en sus servicios.

**PREGUNTA 1: Identificación de vulnerabilidades críticas y su impacto
en la continuidad operativa de TecnoData SpA.**

**[1.1 Vulnerabilidades detectadas con Nessus y OpenVAS]{.underline}**

Los escaneos ejecutados con Nessus Professional y OpenVAS identificaron
las siguientes fallas de infraestructura, clasificadas por criticidad:

- **Servidores sin parches del CSIRT de Gobierno:** la ausencia de
  actualizaciones expone vulnerabilidades documentadas en el CVE
  explotables de forma automatizada, permitiendo ejecución remota de
  código (RCE) y acceso no autorizado a sistemas críticos. Un servidor
  comprometido puede detener la prestación de servicios a las
  municipalidades asociadas.

- **Servicios obsoletos en fin de vida útil (EOL):** protocolos como
  Telnet, FTP y SMBv1 carecen de actualizaciones de seguridad y operan
  frecuentemente con credenciales débiles o por defecto, ampliando
  innecesariamente la superficie de ataque.

- **Puertos expuestos innecesariamente:** puertos como el 23 (Telnet),
  21 (FTP), 445 (SMB) y 3389 (RDP) visibles desde internet son
  activamente sondeados por actores externos, hecho confirmado por los
  registros del firewall perimetral.

- **Sistemas operativos y aplicaciones desactualizados:** versiones sin
  soporte del fabricante no reciben parches ante nuevas
  vulnerabilidades, facilitando propagación lateral de código malicioso
  como ransomware en la red interna.

**[1.2 Vulnerabilidades detectadas mediante OWASP]{.underline}**

La aplicación de las metodologías OWASP sobre los portales consumidos
por clientes nacionales reveló:

- **Validación insuficiente de entradas (OWASP A03 -- Injection):**
  permite ataques de inyección SQL (SQLi) y Cross-Site Scripting (XSS).
  Un ataque SQLi exitoso podría extraer o eliminar datos personales
  protegidos por la Ley 19.628, generando responsabilidades legales para
  la empresa.

- **Configuraciones inseguras (OWASP A05 -- Security
  Misconfiguration):** cabeceras HTTP de seguridad ausentes, directorios
  de administración expuestos y mensajes de error detallados que revelan
  información del sistema facilitan que un atacante identifique la
  arquitectura interna.

**[1.3 Impacto en la continuidad operativa]{.underline}**

Las vulnerabilidades identificadas afectan directamente los tres pilares
de la seguridad: confidencialidad, integridad y disponibilidad. La
explotación confirmada mediante Metasploit demuestra que el riesgo es
real e inmediato.

  ----------------------------------------------------------------------------
    **Vulnerabilidad**    **Criticidad**   **Impacto**
  ----------------------- ---------------- -----------------------------------
  Servidores sin parches  Crítica          Interrupción de servicios,
                                           compromiso total

       Servicios EOL      Alta             Vector de entrada y movimiento
        habilitados                        lateral

     Puertos expuestos    Alta             Acceso no autorizado desde internet

    Inyección SQL / XSS   Crítica          Fuga masiva de datos personales

      Configuraciones     Alta             Exposición de arquitectura interna
         inseguras                         
  ----------------------------------------------------------------------------

La materialización de cualquiera de estas vulnerabilidades puede
resultar en interrupción de servicios municipales, pérdida de datos
personales, sanciones bajo la Ley 19.628 y obligación de notificación al
CSIRT Nacional.

**PREGUNTA 2: Plan de acciones inmediatas para mitigar los riesgos
detectados en servidores, políticas de contraseñas y alineación con
ISO/IEC 27001.**

**[2.1 Marco de priorización]{.underline}**

------------------------------------------------------------------------

El plan se estructura bajo el principio de gestión de riesgos de ISO/IEC
27001:2022, priorizando las acciones según su criticidad e impacto en la
continuidad operativa. Se definen tres fases: inmediata (0-7 días),
corto plazo (8-30 días) y mediano plazo (31-90 días).

**[2.2 Fase inmediata (0--7 días) --- Prioridad Crítica]{.underline}**

- **Aplicación de parches críticos:** ejecutar los parches recomendados
  por el CSIRT de Gobierno en todos los servidores afectados,
  priorizando aquellos expuestos a internet. Se debe establecer una
  ventana de mantenimiento de emergencia y documentar cada intervención
  conforme al Anexo A.12 de ISO 27001 (Gestión de operaciones).
  Justificación: los exploits para vulnerabilidades conocidas son
  automatizables y pueden ejecutarse en minutos.

- **Deshabilitar servicios EOL y cerrar puertos innecesarios:**
  inventariar todos los servicios activos, deshabilitar Telnet, FTP no
  cifrado y SMBv1, reemplazarlos por SSH, SFTP y SMBv3, e implementar
  reglas de firewall bajo política de denegación por defecto (deny-all).
  Alineado con la NTSI en su sección de gestión de configuraciones
  seguras.

- **Política de contraseñas y MFA:** configurar longitud mínima de 12
  caracteres, complejidad obligatoria, historial de últimas 10
  contraseñas, bloqueo tras 5 intentos fallidos y expiración cada 90
  días. Implementar autenticación multifactor (MFA) para accesos
  privilegiados y remotos. Justificación: cumplimiento con NTSI y
  Dominio A.9 de ISO 27001.

**[2.3 Fase corto plazo (8--30 días) --- Prioridad Alta]{.underline}**

- **Corrección de vulnerabilidades OWASP:** implementar validación y
  sanitización de entradas en el servidor, configurar cabeceras HTTP de
  seguridad (CSP, X-Frame-Options, HSTS) y deshabilitar mensajes de
  error detallados en producción. Justificación: los portales procesan
  datos personales regulados por la Ley 19.628.

- **Cifrado en bases de datos con datos personales:** implementar
  AES-256 para datos en reposo y TLS 1.2 o superior para comunicaciones
  en tránsito, gestionando claves mediante un KMS dedicado. Conforme al
  Dominio A.10 de ISO 27001 y la Ley 19.628.

**[2.4 Fase mediano plazo (31--90 días) --- Prioridad
Media]{.underline}**

- **Implementación formal de SGSI:** designar un oficial de seguridad
  (CISO), formalizar el alcance del SGSI, realizar evaluación de riesgos
  bajo ISO 27005 e implementar auditorías internas periódicas.
  Proporciona el marco formal para la gestión continua y sostenida de la
  seguridad.

  ------------------------------------------------------------------------
   **Fase**   **Acción**              **Plazo**   **Norma**
  ----------- ----------------------- ----------- ------------------------
     1 --     Parches en servidores   0-7 días    NTSI / ISO A.12
    Crítica                                       

     1 --     Servicios EOL / Puertos 0-7 días    NTSI / ISO A.9
    Crítica                                       

     1 --     Contraseñas y MFA       0-7 días    NTSI / ISO A.9
    Crítica                                       

   2 -- Alta  OWASP y cifrado de BD   8-30 días   OWASP / Ley 19.628

  3 -- Media  SGSI ISO 27001          31-90 días  ISO/IEC 27001:2022
  ------------------------------------------------------------------------

**PREGUNTA 3: Reorganización de la red, controles técnicos de acceso y
monitoreo continuo en TecnoData SpA.**

**[3.1 Diagnóstico]{.underline}** La auditoría confirmó dos fallas
críticas de arquitectura: segmentación de red insuficiente y privilegios
mal asignados. La combinación de ambas permite que, ante cualquier punto
de compromiso, un atacante pueda realizar movimiento lateral sin
encontrar barreras técnicas, accediendo a sistemas críticos y datos
sensibles de forma directa. Esto viola el Dominio A.13 (Seguridad de las
comunicaciones) y el Dominio A.9 (Control de accesos) de ISO/IEC
27001:2022.

------------------------------------------------------------------------

**[3.2 Reorganización de la red mediante VLANs y DMZ]{.underline}**

La red debe segmentarse en zonas lógicas con acceso restringido entre
ellas:

- **VLAN de Producción:** servidores de datos y almacenamiento. Acceso
  exclusivo para administradores autenticados con MFA desde equipos de
  gestión dedicados.

- **VLAN de Bases de Datos:** aislada de internet y de usuarios finales.
  Solo los servidores de aplicación autorizados pueden establecer
  conexiones, mediante reglas de firewall explícitas.

- **DMZ (Zona Desmilitarizada):** los portales web consumidos por
  municipalidades se ubican en esta zona intermedia, gestionada por
  firewalls de doble etapa. Si un portal es comprometido, el atacante no
  obtiene acceso directo a la red interna.

- **VLAN de Usuarios Internos:** estaciones de trabajo del personal, sin
  acceso directo a servidores de producción ni bases de datos.

- **VLAN de Gestión:** exclusiva para herramientas de administración,
  SIEM y accesos SSH/RDP a servidores.

Frente a los portales en la DMZ se debe implementar un Web Application
Firewall (WAF) configurado con el OWASP ModSecurity Core Rule Set, que
filtre en tiempo real ataques de inyección, XSS y otras amenazas a nivel
de aplicación.

**[3.3 Controles técnicos de acceso]{.underline}**

- **Mínimo privilegio:** revisar y reasignar todos los permisos de
  usuarios, procesos y servicios al mínimo necesario para su función.
  Eliminar cuentas inactivas y separar cuentas administrativas de
  operativas (ISO A.9.2).

- **Autenticación Multifactor (MFA):** obligatoria para accesos a VLANs
  de gestión, servidores de producción y paneles administrativos.
  Reducción directa del riesgo de compromiso de credenciales.

- **VPN corporativa:** todo acceso remoto debe canalizarse
  exclusivamente a través de VPN con cifrado fuerte (IKEv2/IPSec) y
  autenticación basada en certificados de cliente.

**[3.4 Monitoreo continuo]{.underline}**

- **Optimización del SIEM:** configurar reglas de correlación que
  generen alertas ante múltiples fallos de autenticación, accesos fuera
  de horario habitual, transferencias de datos anómalas o conexiones
  desde IPs de reputación negativa.

- **IDS/IPS de red:** desplegar Suricata o Snort entre segmentos de red
  para inspeccionar tráfico en tiempo real. Integrado con el SIEM para
  visión unificada del estado de seguridad.

- **Gestión de registros:** todos los sistemas envían logs al SIEM
  centralizado con sincronización NTP. Retención mínima de 12 meses
  conforme a Ley 19.628 y recomendaciones del CSIRT Nacional. Acceso de
  solo lectura para todos los usuarios excepto el equipo de seguridad.

Estas medidas transforman la arquitectura de TecnoData desde un modelo
plano y vulnerable hacia una defensa en profundidad, donde múltiples
capas de controles dificultan y detectan cualquier intrusión o
movimiento lateral no autorizado.

**PREGUNTA 4: Comunicación de hallazgos críticos a la gerencia:
comprensión, toma de decisiones informada y cumplimiento ético.**

**[4.1 Principios que guían la comunicación del auditor]{.underline}**

------------------------------------------------------------------------

La comunicación de los hallazgos no es un trámite administrativo: es un
acto con consecuencias organizacionales, legales y éticas. Los
principios que la guían son objetividad, confidencialidad, integridad,
competencia técnica e impacto organizacional. En TecnoData SpA estos
principios son especialmente relevantes, dado que la información
recopilada incluye brechas no reportadas y datos personales protegidos
por la Ley 19.628, cuya divulgación inadecuada podría generar
responsabilidades legales adicionales.

**[4.2 Estructura del informe final]{.underline}**

- **Informe ejecutivo:** redactado en lenguaje claro y sin tecnicismos
  sin definición previa. Máximo dos páginas que sinteticen el nivel de
  riesgo global, los hallazgos más críticos en términos de impacto de
  negocio (económico, legal, reputacional), el estado de cumplimiento
  respecto a NTSI y Ley 19.628, y las recomendaciones priorizadas con
  sus plazos. Se acompaña de un mapa de calor de riesgos que permita
  comprensión inmediata por parte de directivos sin formación técnica.

- **Informe técnico detallado:** destinado al equipo de TI. Incluye
  resultados completos de Nessus, OpenVAS y OWASP, hallazgos con sus
  CVE, análisis de registros del firewall y especificaciones de cada
  control recomendado. Las evidencias de las pruebas de penetración se
  incluyen sin código de explotación funcional, por ética profesional.

**[4.3 Sesión de presentación a la gerencia]{.underline}**

Los hallazgos deben presentarse en una sesión formal con CEO, CFO,
responsable de TI y asesor legal. La presentación comienza con el
alcance y metodología de la auditoría, continúa con los hallazgos
ordenados de mayor a menor criticidad, describiendo cada uno en términos
de impacto (¿qué puede perder la empresa?), probabilidad (¿qué tan
probable es que ocurra?) y urgencia (¿en cuánto tiempo puede ser
explotado?). El auditor debe evitar tanto el lenguaje alarmista como la
minimización de la severidad: cada problema se presenta acompañado de
una recomendación concreta. Al finalizar se habilita un espacio de
preguntas para clarificar dudas.

**[4.4 Consideraciones éticas y legales]{.underline}**

- **Confidencialidad:** toda la información recopilada debe tratarse con
  máxima reserva. El auditor firma NDA antes del proceso y entrega los
  informes únicamente a destinatarios autorizados, en canales cifrados.

- **Notificación al CSIRT Nacional:** el auditor tiene la
  responsabilidad profesional de informar a la gerencia sobre la
  obligación legal de notificar al CSIRT ante incidentes que afecten
  datos personales o servicios críticos. Ocultar o minimizar estos
  hallazgos constituye una falta grave de ética profesional y puede
  generar responsabilidades legales adicionales.

- **Impacto organizacional:** el auditor debe anticipar y comunicar de
  forma transparente las fricciones que generarán las medidas
  recomendadas: resistencia de usuarios, períodos de indisponibilidad
  planificados y redefinición de roles. La gerencia debe comprender que
  estas fricciones son el costo necesario frente al costo
  significativamente mayor de no actuar.

**[4.5 Cronograma y métricas de avance]{.underline}**

  ----------------------------------------------------------------------------
      **Fase**      **Acciones clave**    **Plazo**   **Métrica de éxito**
  ----------------- --------------------- ----------- ------------------------
    1: Contención   Parches, EOL, MFA     0-7 días    0 puertos críticos
                                                      expuestos

   2: Remediación   OWASP, cifrado, VLANs 8-30 días   0 vulns. críticas OWASP

         3:         IDS/IPS, SIEM, ZT     31-90 días  Detección \< 1 hora
   Fortalecimiento                                    

     4: Madurez     SGSI, auditorías,     90-180 días 0 incidentes críticos
                    capacit.                          
  ----------------------------------------------------------------------------

El seguimiento de métricas se presenta mensualmente a la gerencia,
alineado con el ciclo de mejora continua PDCA que fundamenta ISO/IEC
27001.

**CONCLUSIÓN**

------------------------------------------------------------------------

La auditoría de ciberseguridad realizada sobre TecnoData SpA revela un
escenario de riesgo crítico que exige una respuesta organizacional
inmediata y estructurada. Los hallazgos identificados con Nessus,
OpenVAS, OWASP y Metasploit no son hipótesis teóricas, sino
vulnerabilidades confirmadas que, de ser explotadas, podrían comprometer
datos personales de entidades municipales, interrumpir servicios
críticos y generar responsabilidades legales bajo la Ley 19.628.

La falta de gestión del ciclo de vida de los sistemas, evidenciada en
servidores sin parches, servicios obsoletos y aplicaciones sin
mantenimiento, es la causa raíz de la mayor parte de las
vulnerabilidades críticas. Abordarla mediante procesos formales de
gestión de parches es la medida con mayor impacto positivo e inmediato
sobre la postura de seguridad de la organización.

La ausencia de segmentación de red transforma cualquier vulnerabilidad
individual en un riesgo sistémico. Sin barreras técnicas internas, un
único punto de compromiso puede derivar en una brecha total de la
infraestructura. La implementación de VLANs, firewalls internos y DMZ
es, por tanto, una inversión prioritaria e indispensable.

El cumplimiento normativo con la NTSI, la Ley 19.628, los lineamientos
del CSIRT Nacional y la norma ISO/IEC 27001 no es una carga burocrática,
sino el marco que guía las decisiones de seguridad hacia estándares
probados, protege a la empresa de responsabilidades legales y fortalece
la confianza de sus clientes institucionales.

Finalmente, la dimensión ética de la auditoría es inseparable de la
técnica. TecnoData SpA custodia datos de entidades públicas y ciudadanos
chilenos: esa responsabilidad exige los más altos estándares de gestión
de la seguridad de la información. La implementación del plan propuesto
permitirá a la empresa evolucionar desde una postura reactiva y
vulnerable hacia una arquitectura de defensa en profundidad, resiliente
y alineada con las mejores prácticas nacionales e internacionales.

**BIBLIOGRAFÍA**

------------------------------------------------------------------------

International Organization for Standardization. (2022). ISO/IEC
27001:2022 -- Information security, cybersecurity and privacy protection
--- Information security management systems --- Requirements. ISO.

International Organization for Standardization. (2018). ISO/IEC
27005:2018 -- Information technology --- Security techniques ---
Information security risk management. ISO.

OWASP Foundation. (2021). OWASP Top Ten 2021. Open Web Application
Security Project. https://owasp.org/www-project-top-ten/

Tenable, Inc. (2024). Nessus Professional -- Vulnerability Scanner.
https://www.tenable.com/products/nessus

Greenbone Networks. (2023). OpenVAS -- Open Vulnerability Assessment
Scanner. https://www.openvas.org/

Rapid7. (2024). Metasploit Framework -- Penetration Testing Software.
https://www.metasploit.com/

Ministerio Secretaría General de la Presidencia de Chile. (2008). Ley N°
19.628 sobre protección de la vida privada. Biblioteca del Congreso
Nacional de Chile. https://www.bcn.cl/leychile/navegar?idNorma=141599

Ministerio de Hacienda, Chile. (2023). Norma Técnica de Seguridad de la
Información del Estado (NTSI). División de Gobierno Digital.
https://www.digital.gob.cl

CSIRT de Gobierno de Chile. (2024). Alertas y recomendaciones de
ciberseguridad. https://www.csirt.gob.cl/

Verizon. (2024). 2024 Data Breach Investigations Report (DBIR). Verizon
Business. https://www.verizon.com/business/resources/reports/dbir/

Stallings, W., & Brown, L. (2018). Computer security: Principles and
practice (4.a ed.). Pearson Education.

Kim, D., & Solomon, M. G. (2021). Fundamentals of information systems
security (4.a ed.). Jones & Bartlett Learning.
