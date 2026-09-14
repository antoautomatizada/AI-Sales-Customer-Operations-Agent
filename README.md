# AI Sales & Customer Operations Agent

Sistema de automatización inteligente para la gestión de leads, interacciones y respuestas comerciales mediante **n8n, Airtable, GPT-4o-mini, Gmail y Telegram**.

El proyecto integra automatización de procesos, análisis mediante inteligencia artificial, CRM, prevención de duplicados, manejo de errores y un circuito **Human-in-the-loop (HITL)** para decisiones que requieren aprobación humana.

---

## 🎯 Objetivo del proyecto

Automatizar el ciclo completo de atención de contactos recibidos por correo electrónico:

**Recepción → Validación → CRM → Historial → Análisis IA → Scoring → Decisión → Respuesta → Registro**

El workflow permite procesar nuevos leads, reconocer contactos existentes, registrar cada interacción, analizar el contenido mediante IA y responder automáticamente cuando corresponde.

Cuando una interacción requiere intervención humana, el sistema genera una alerta mediante Telegram y espera una decisión antes de enviar una respuesta.

---

## 🏗️ Arquitectura tecnológica

| Componente              | Tecnología  | Función                                                                |
| ----------------------- | ----------- | ---------------------------------------------------------------------- |
| Orquestación            | n8n Cloud   | Automatización y coordinación del workflow                             |
| CRM / Base de datos     | Airtable    | Leads, interacciones, errores y KPIs                                   |
| Inteligencia Artificial | GPT-4o-mini | Clasificación, intención, prioridad, scoring y generación de respuesta |
| Entrada                 | Gmail       | Recepción de emails                                                    |
| Comunicación HITL       | Telegram    | Solicitud de aprobación humana                                         |
| Salida                  | Gmail       | Envío de respuestas                                                    |
| Dashboard               | Airtable    | Monitoreo de KPIs y errores                                            |

---

## 🔄 Flujo principal

```text
Gmail Trigger
     ↓
Validación de correo relevante
     ↓
Prevención de duplicados
     ↓
Búsqueda de Lead en CRM
     ↓
Lead existente / Nuevo Lead
     ↓
Registro de interacción
     ↓
Preparación del contexto
     ↓
AI Agent — GPT-4o-mini
     ↓
Actualización de CRM
     ↓
¿Requiere revisión humana?
     ↓
   ┌───────────────┐
   │               │
   NO              SÍ
   ↓               ↓
Respuesta       Telegram
automática          ↓
   ↓           Aprobación /
   ↓           Rechazo
   ↓               ↓
   └───────→ Gmail
             ↓
        Registro final
```

---

## 🤖 Inteligencia Artificial

El proyecto utiliza **GPT-4o-mini** como modelo único para el procesamiento de las interacciones.

El agente analiza:

* Categoría del contacto.
* Intención.
* Prioridad.
* Lead Score.
* Necesidad de revisión humana.
* Respuesta comercial.

La salida se estructura mediante un **Structured Output Parser**, evitando depender de texto libre para las decisiones posteriores del workflow.

Se aplica minimización de contexto y solamente se envían a la IA los datos necesarios para realizar el análisis.

---

## 🗄️ Estructura del CRM

### Leads

Contiene la información principal de cada contacto y su estado dentro del ciclo comercial.

### Interactions

Registra cada interacción recibida, su análisis, respuesta generada, estado de revisión y trazabilidad mediante Gmail Message ID.

### Errores

Centraliza los errores producidos durante las ejecuciones para facilitar su monitoreo y resolución.

### Dashboard KPIs

Consolida indicadores de operación y permite visualizar el estado general del sistema.

---

## 🔐 Seguridad y resiliencia

El workflow incorpora mecanismos para mejorar la confiabilidad y seguridad del proceso:

* Prevención de procesamiento duplicado mediante **Gmail Message ID**.
* Validación de datos antes de continuar el procesamiento.
* Minimización de datos enviados al modelo de IA.
* Rutas específicas para el manejo de errores.
* Registro de errores en Airtable.
* Human-in-the-loop para decisiones críticas.
* Registro de decisiones humanas.
* Estados de procesamiento y trazabilidad de las interacciones.

---

## 👤 Human-in-the-loop

Cuando el análisis de IA determina que una interacción requiere revisión humana:

**AI Agent → Requiere revisión → Telegram → Decisión humana**

La persona responsable puede:

* **Aprobar:** se envía la respuesta mediante Gmail.
* **Rechazar:** la decisión queda registrada y no se envía la respuesta.

Este mecanismo evita que determinadas respuestas sensibles sean enviadas automáticamente sin supervisión.

---

## 📊 Dashboard y monitoreo

https://airtable.com/appcMsxBd3bBeUQ8t/shrYkBqc0Ifgr2Idf/tblAIs9KB8n83D6ZU/viwuXJ65Kpi4AIAhE

El proyecto incluye un dashboard de control con indicadores operativos:

* Total de Leads.
* Total de Interacciones.
* Respuestas Enviadas.
* Total de Errores.
* Tasa de Errores.

Los KPIs se actualizan automáticamente mediante un workflow independiente de n8n.

---

## 🧪 Testing y resiliencia

Se realizaron pruebas sobre diferentes escenarios:

* Nuevo lead.
* Lead existente.
* Correo irrelevante.
* Interacción con información incompleta.
* Aprobación mediante HITL.
* Rechazo mediante HITL.
* Error de la API/modelo de IA.
* Prevención de duplicados.

Las pruebas permitieron verificar tanto el funcionamiento normal como diferentes escenarios de error y recuperación.

---

## 📁 Documentación del proyecto

### 01 — Arquitectura

Diagrama completo de la arquitectura y flujo del sistema.

### 02 — Datos

Manual operativo de datos, estructura del CRM y esquemas de integración.

### 03 — Costos

Matriz de costos y decisión tecnológica utilizada para seleccionar el stack.

### 04 — Seguridad

Documento de seguridad, minimización de datos, resiliencia, manejo de errores y HITL.

### 05 — Dashboard

Documentación del dashboard ejecutivo y sus indicadores.

### 06 — Workflows

Archivos JSON exportados desde n8n para permitir la revisión y reproducción de los workflows.

### 07 — Evidencias

Capturas de pantalla que documentan la configuración, ejecución, pruebas, IA, Airtable, dashboard y circuito HITL.

---

## 📂 Estructura del repositorio

```text
AI-Sales-Customer-Operations-Agent
│
├── README.md
│
├── 01_Arquitectura
│   └── Diagrama_Arquitectura.pdf
│
├── 02_Datos
│   └── Manual_Operativo_de_Datos.pdf
│
├── 03_Costos
│   └── Matriz_Costos_y_Decision_Tecnologica.pdf
│
├── 04_Seguridad
│   └── Seguridad_y_Resiliencia.pdf
│
├── 05_Dashboard
│   └── Dashboard_Ejecutivo.pdf
│
├── 06_Workflows
│   ├── AI_Sales_Customer_Operations_Agent.json
│   └── Actualizar_Dashboard_KPIs.json
│
└── 07_Evidencias
    ├── 01_Workflow_Completo.png
    ├── 02_AI_Agent_GPT_4o_mini.png
    ├── 03_Structured_Output_Parser.png
    ├── 04_Error_Handling.png
    ├── 05_HITL_Aprobacion.png
    ├── 06_Airtable_Leads.png
    ├── 07_Airtable_Interactions.png
    ├── 08_Airtable_Errores.png
    ├── 09_Dashboard_Control.png
    ├── 10_Dashboard_KPIs.png
    ├── 11_Workflow_Dashboard_KPIs.png
    ├── 12_Gmail_Respuesta_Real.png
    └── 13_Telegram_HITL.png
```

---

## 🛠️ Stack

**n8n · Airtable · GPT-4o-mini · Gmail · Telegram**

Proyecto académico desarrollado como demostración de automatización inteligente aplicada a operaciones comerciales y atención de clientes.
