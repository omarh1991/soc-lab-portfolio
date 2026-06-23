# LD-004 - Lumma Stealer mediante ClickFix Phishing

## Resumen Ejecutivo

Durante esta investigación se analizó una alerta de seguridad relacionada con una campaña de phishing que utilizaba la técnica ClickFix para distribuir el malware Lumma Stealer mediante DLL Side-Loading.

El objetivo fue validar la alerta, identificar indicadores de compromiso (IOC), determinar el impacto del incidente y aplicar medidas de contención para evitar la exfiltración de información sensible.

---

## Habilidades Demostradas

* Respuesta a Incidentes (Incident Response)
* Análisis de Phishing
* Análisis de Malware
* Threat Hunting
* Inteligencia de Amenazas
* Análisis de Logs
* Investigación de Command and Control (C2)
* Mapeo MITRE ATT&CK

---

## Resumen de la Alerta

| Campo            | Valor                                                            |
| ---------------- | ---------------------------------------------------------------- |
| Event ID         | 316                                                              |
| Regla            | SOC338 - Lumma Stealer - DLL Side-Loading via Click Fix Phishing |
| Usuario afectado | Dylan                                                            |
| Remitente        | [update@windows-update.site](mailto:update@windows-update.site)  |
| SMTP IP          | 132.232.40.201                                                   |
| Asunto           | Upgrade your system to Windows 11 Pro for FREE                   |

---

## Proceso de Investigación

### 1. Validación de la Alerta

La alerta identificó un correo sospechoso enviado al usuario Dylan desde una dirección que simulaba ser una actualización legítima de Windows.

Información identificada:

* Remitente: [update@windows-update.site](mailto:update@windows-update.site)
* Destinatario: [Dylan@letsdefend.io](mailto:Dylan@letsdefend.io)
* SMTP IP: 132.232.40.201
* Fecha: 13 de marzo de 2025
* Asunto: Upgrade your system to Windows 11 Pro for FREE

Durante el análisis se identificaron indicadores típicos de phishing, incluyendo mensajes de urgencia y ofertas llamativas diseñadas para presionar al usuario a actuar rápidamente.

---

### 2. Análisis del Correo Electrónico

El correo contenía una URL sospechosa:

```text
https://www.windows-update.site/
```

La página imitaba una actualización legítima de Windows con el objetivo de engañar al usuario y facilitar la ejecución de código malicioso.

---

### 3. Inteligencia de Amenazas

La URL fue analizada mediante VirusTotal.

Resultados obtenidos:

* 11 motores antivirus detectaron la URL como maliciosa.
* Clasificación: Phishing / Fraud.
* Reportes asociados a campañas de distribución de malware.

Posteriormente se realizó un análisis dinámico mediante Any.Run.

La simulación confirmó la distribución de Lumma Stealer utilizando técnicas de DLL Side-Loading.

---

### 4. Verificación de Entrega

La revisión de los registros de correo mostró:

```text
Device Action: Allowed
```

Esto confirmó que el correo fue entregado correctamente al usuario y no fue bloqueado por los controles de seguridad.

---

### 5. Verificación de Interacción del Usuario

El análisis de los registros Proxy confirmó que el usuario accedió a la URL maliciosa.

URL visitada:

```text
https://www.windows-update.site/
```

La investigación permitió confirmar que la víctima interactuó con el contenido malicioso.

---

### 6. Ejecución de Malware

Durante la revisión de eventos del sistema se observó la ejecución del proceso:

```text
mshta.exe
```

Este proceso fue utilizado para descargar y ejecutar contenido malicioso desde infraestructura controlada por el atacante.

---

### 7. Actividad de Command and Control (C2)

Se identificó comunicación con la siguiente infraestructura maliciosa:

```text
https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4
```

Dirección IP asociada:

```text
172.67.139.19
```

Los registros evidenciaron conexiones hacia esta infraestructura inmediatamente después de la ejecución de mshta.exe.

---

## Indicadores de Compromiso (IOC)

| Tipo    | Valor                                                  |
| ------- | ------------------------------------------------------ |
| SMTP IP | 132.232.40.201                                         |
| URL     | https://www.windows-update.site/                       |
| URL C2  | https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4 |
| IPv4 C2 | 172.67.139.19                                          |
| Proceso | mshta.exe                                              |
| Malware | Lumma Stealer                                          |

---

## Evaluación del Ataque

### Hallazgos

* Correo de phishing entregado exitosamente.
* Usuario accedió al enlace malicioso.
* Ejecución de mshta.exe confirmada.
* Comunicación con infraestructura C2 observada.
* Riesgo elevado de robo de credenciales e información sensible.

### Dirección del Tráfico

```text
Internet → Endpoint
Endpoint → Infraestructura C2
```

### Estado del Ataque

```text
Exitoso
```

---

## Mapeo MITRE ATT&CK

| Táctica        | Técnica                                   |
| -------------- | ----------------------------------------- |
| Initial Access | T1566 - Phishing                          |
| Execution      | T1059 - Command and Scripting Interpreter |
| Execution      | T1204 - User Execution                    |
| Discovery      | T1087 - Account Discovery                 |
| Discovery      | T1007 - System Service Discovery          |

---

## Acciones de Contención

1. Aislar el endpoint afectado de la red.
2. Eliminar el correo malicioso del buzón del usuario.
3. Bloquear los IOC identificados.
4. Restablecer credenciales potencialmente comprometidas.
5. Revisar el sistema en busca de mecanismos de persistencia.
6. Incrementar el monitoreo de actividad sospechosa asociada al usuario afectado.

---

## Lecciones Aprendidas

* Los correos que simulan actualizaciones de software son una técnica frecuente de phishing.
* Los mensajes de urgencia continúan siendo altamente efectivos para engañar a los usuarios.
* El análisis mediante sandbox permite validar rápidamente amenazas desconocidas.
* La monitorización temprana reduce significativamente el impacto de campañas de robo de información.

---

## Conclusión

La investigación confirmó una campaña de phishing exitosa que condujo a la ejecución de Lumma Stealer mediante técnicas de DLL Side-Loading.

El análisis de los registros de correo, actividad web, ejecución de procesos y comunicaciones C2 permitió confirmar el compromiso del equipo afectado y el riesgo potencial de exfiltración de credenciales e información sensible.

La aplicación inmediata de medidas de contención resultó fundamental para limitar el impacto y prevenir actividades posteriores por parte del atacante.
