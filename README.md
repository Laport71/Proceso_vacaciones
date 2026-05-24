# Proceso_vacaciones
Desarrollo de una solución automatizada para el proceso de solicitud de vacaciones.
🏖️ Sistema Inteligente de Automatización Administrativa — Gestión de Vacaciones

Sistema automatizado para la gestión de solicitudes de vacaciones mediante inteligencia artificial, desarrollado con n8n, OpenAI y Google Sheets. El proceso completo — desde la recepción del mail hasta la notificación de respuesta — ocurre sin intervención humana.

---

## 📋 Descripción

El sistema recibe solicitudes de vacaciones por correo electrónico, las interpreta mediante IA (OpenAI GPT-4.1-mini), verifica la disponibilidad de días del empleado en Google Sheets y responde automáticamente con la aprobación o rechazo. También notifica a Recursos Humanos y actualiza los registros correspondientes.

---

## 🛠️ Tecnologías

| Componente        | Tecnología               |
|-------------------|--------------------------|
| Automatización    | n8n                      |
| Inteligencia Artificial | OpenAI GPT-4.1-mini |
| Base de Datos     | Google Sheets            |
| Trigger           | Gmail Trigger            |
| Notificaciones    | Gmail API                |
| Modelado de Procesos | BPMN 2.0 en Bizagi    |
| Control de Versiones | GitHub + PAT          |

---

## 📁 Estructura del Repositorio

```
Proceso_vacaciones/
│
├── README.md
├── workflow-n8n.json
├── docs/
│   ├── BPMN-ASIS.png
│   ├── BPMN-TOBE.png
│   ├── capturas/
│   └── informe.pdf
│
├── database/
│   ├── empleados.xlsx
│   └── solicitudes.xlsx
│
└── prompts/
    └── prompt-openai.txt
```

---

## ⚙️ Cómo Ejecutar

### 1. Requisitos previos
- Cuenta activa en [n8n](https://n8n.io/) (self-hosted o cloud)
- Cuenta de Gmail configurada con acceso a la API
- Google Sheets con las hojas `Empleados` y `Solicitudes`
- API Key de OpenAI

### 2. Variables necesarias

Configurar las siguientes credenciales en n8n:

| Variable            | Descripción                                      |
|---------------------|--------------------------------------------------|
| `GMAIL_CREDENTIAL`  | Credencial OAuth2 de Gmail en n8n                |
| `OPENAI_API_KEY`    | Clave de API de OpenAI                           |
| `SHEETS_ID`         | ID del Google Spreadsheet utilizado como base de datos |
| `MAIL_RRHH`         | Dirección de correo del área de Recursos Humanos |
| `MAIL_BOT`          | Dirección receptora de solicitudes (`lauraportels@gmail.com`) |

### 3. Importar el workflow
1. Abrí n8n y dirigite a **Workflows → Import**
2. Seleccioná el archivo `workflow-n8n.json`
3. Configurá las credenciales según la tabla anterior
4. Activá el workflow

---

## 📨 Ejemplo de Flujo

### El empleado envía un mail: 
Debe figurar la palabra vacaciones y la fecha de comienzo y fin del periodo
seleccionado. Debe usar formato dd/mm/aaaa para fechas.

**Para:** lauraportels@gmail.com  
**Asunto:** Pedido de vacaciones  
**Mensaje:**
```
Hola RRHH,
Quiero solicitar vacaciones desde el 01/07/2026 hasta el 07/07/2026.
Gracias.
```

### El sistema responde automáticamente:

```
✅ Tu solicitud de vacaciones fue APROBADA.
Período: 01/07/2026 al 07/07/2026 (7 días)
Días restantes disponibles: 8
```

o bien:

```
❌ Tu solicitud de vacaciones fue RECHAZADA.
Días solicitados: 7 | Días disponibles: 3
Por favor, revisá tu saldo de vacaciones.
```

---

## 🔄 Flujo del Proceso (Resumen)

1. Empleado envía mail con su solicitud de vacaciones
2. Gmail Trigger detecta el nuevo correo
3. OpenAI analiza el texto e identifica fechas y palabras clave
4. El sistema consulta los días disponibles en la hoja `Empleados`
5. Se toma la decisión: **Aprobado** o **Rechazado**
6. Se notifica al empleado por mail
7. Si es aprobado: se notifica a RRHH, se actualizan los días disponibles y se registra en la hoja `Solicitudes`

---

## 📸 Capturas

Las capturas del workflow, el diagrama BPMN y el funcionamiento en tiempo real se encuentran en `docs/capturas/`.

---

## 📊 Diccionario de Datos

### Hoja: `Empleados`

| Variable | Tipo | Descripción |
|---|---|---|
| `legajo` | Integer | Número identificador único del empleado |
| `nombre` | String | Nombre completo del empleado |
| `email` | String | Correo electrónico del empleado |
| `dias_disponibles` | Integer | Saldo actual de días de vacaciones |
| `dias_tomados` | Integer | Días de vacaciones ya utilizados |
| `fecha_vacacional` | String | Rango de fechas del período vacacional (ej: 24/09/2026-30/09/2026) |

### Hoja: `Solicitudes`

| Variable | Tipo | Descripción |
|---|---|---|
| `id_solicitud` | Integer | Identificador único de la solicitud |
| `legajo` | Integer | Legajo del empleado solicitante |
| `fecha_desde` | Date | Fecha de inicio de las vacaciones solicitadas |
| `fecha_hasta` | Date | Fecha de fin de las vacaciones solicitadas |
| `cantidad_dias` | Integer | Total de días solicitados |
| `estado` | String | Resultado de la solicitud (`APROBADO` / `RECHAZADO`) |
| `motivo` | String | Motivo o detalle adicional de la solicitud |
| `fecha_envio` | Date | Fecha en que se realizó la solicitud |

## 👥 Autores

Desarrollado como proyecto integrador para la materia **Organización Empresarial** — Tecnicatura Universitaria en Programación, UTN.
