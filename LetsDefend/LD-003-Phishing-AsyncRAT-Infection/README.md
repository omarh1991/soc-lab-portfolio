# LD-003 - Campaña de Phishing con Infección por AsyncRAT

## Resumen Ejecutivo

Durante esta investigación se analizó una alerta de phishing relacionada con un correo electrónico malicioso que simulaba una promoción de café gratuita.

El objetivo fue validar la alerta, identificar indicadores de compromiso (IOC), determinar el impacto del incidente y aplicar medidas de contención para evitar una posible propagación del malware.

---

## Habilidades Demostradas

- Respuesta a Incidentes (Incident Response)
- Análisis de Phishing
- Análisis de Malware
- Inteligencia de Amenazas (Threat Intelligence)
- Análisis de IOC
- Análisis de Logs
- Threat Hunting
- Mapeo MITRE ATT&CK

---

## Resumen de la Alerta

| Campo | Valor |
|---------|---------|
| Event ID | 257 |
| Regla | SOC282 - Phishing Alert - Deceptive Mail Detected |
| Destinatario | felix@letsdefend.io |
| Remitente | free@coffeeshoop.com |
| SMTP IP | 103.80.134.63 |
| Asunto | Free Coffee Voucher |

---

## Proceso de Investigación

### 1. Validación de la Alerta

La alerta identificó un correo electrónico sospechoso entregado al buzón del usuario.

Información relevante:

- Remitente: free@coffeeshoop.com
- Destinatario: felix@letsdefend.io
- SMTP IP: 103.80.134.63
- Asunto: Free Coffee Voucher

El correo utilizaba técnicas comunes de ingeniería social mediante mensajes de urgencia como:

- "Hurry"
- "This offer expires soon"

Estas expresiones buscan presionar al usuario para que actúe sin analizar cuidadosamente el contenido del mensaje.

---

### 2. Análisis del Correo Electrónico

El correo contenía un archivo sospechoso:

```text
free-coffee.zip
```

URL identificada:

```text
https://files-ld.s3.us-east-2.amazonaws.com/59cbd215-76ea-434d-93ca-4d6aec3bac98-free-coffee.zip
```

---

### 3. Inteligencia de Amenazas

La URL fue analizada mediante VirusTotal.

Resultados obtenidos:

- Diversos motores antivirus clasificaron la URL como maliciosa.
- Reportes comunitarios la relacionaban con actividad malware.

Posteriormente se realizó un análisis dinámico utilizando Any.Run, donde se identificó una variante del malware AsyncRAT.

---

### 4. Verificación de Entrega

La revisión de los registros de seguridad de correo mostró:

```text
Device Action: Allowed
```

Esto confirmó que el correo malicioso fue entregado correctamente al usuario.

---

### 5. Análisis de Interacción del Usuario

Los registros Proxy evidenciaron que el usuario accedió a la URL maliciosa.

Durante la investigación se confirmó la ejecución del archivo:

```text
Coffee.exe
```

en el equipo afectado.

---

### 6. Actividad de Comando y Control (C2)

Se identificó comunicación hacia la siguiente dirección IP:

```text
37.120.233.226
```

Los registros demostraron tráfico de red hacia esta infraestructura inmediatamente después de la ejecución del malware.

---

## Indicadores de Compromiso (IOC)

| Tipo | Valor |
|---------|---------|
| SMTP IP | 103.80.134.63 |
| URL | free-coffee.zip |
| IP C2 | 37.120.233.226 |
| Archivo | Coffee.exe |
| Malware | AsyncRAT |

---

## Evaluación del Ataque

### Hallazgos

- El correo de phishing fue entregado exitosamente.
- El usuario abrió el archivo malicioso.
- Se confirmó la ejecución del malware.
- Se observó comunicación con infraestructura C2.
- Existía riesgo de robo de credenciales y acceso remoto al sistema.

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

| Táctica | Técnica |
|---------|---------|
| Initial Access | T1566 - Phishing |
| Execution | T1059 - Command and Scripting Interpreter |
| Execution | T1204 - User Execution |
| Discovery | T1087 - Account Discovery |
| Discovery | T1007 - System Service Discovery |

---

## Acciones de Contención

- Aislar el equipo comprometido.
- Eliminar el correo malicioso del buzón.
- Bloquear los IOC identificados.
- Restablecer credenciales potencialmente comprometidas.
- Revisar el sistema en busca de persistencia.
- Realizar concienciación de seguridad al usuario afectado.

---

## Lecciones Aprendidas

- Los correos con ofertas atractivas y mensajes urgentes deben considerarse sospechosos.
- El análisis dinámico mediante sandbox permite comprender mejor el comportamiento del malware.
- La detección temprana reduce significativamente el impacto de campañas de phishing.
- La formación continua de los usuarios es una de las mejores medidas preventivas.

---

## Conclusión

La investigación confirmó una campaña de phishing exitosa que culminó con la ejecución de una variante de AsyncRAT en el equipo de la víctima.

El análisis de los registros de correo, tráfico web y actividad del sistema permitió identificar la infección, la comunicación con infraestructura de comando y control (C2) y el potencial compromiso de credenciales.

La aplicación inmediata de medidas de contención fue fundamental para limitar el impacto y prevenir movimientos posteriores del atacante dentro del entorno.
