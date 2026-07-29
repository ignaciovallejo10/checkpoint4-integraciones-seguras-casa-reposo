# Pre entrega 4 - Integraciones Seguras Casa Reposo

## Descripción

Este proyecto corresponde a la **Pre entrega 4 de la carrera AI Automation Avanzado de Coderhouse**.

Se desarrolló un workflow en **n8n** que integra un agente de IA con herramientas reales de negocio mediante conexiones seguras, implementando controles de seguridad, prevención de errores y gobernanza operativa.

---

# Objetivo

Diseñar una automatización que permita:

- Recibir consultas desde Gmail.
- Bloquear respuestas automáticas para evitar bucles infinitos.
- Analizar la consulta mediante un Agente IA.
- Buscar el contacto en HubSpot antes de crear uno nuevo.
- Evitar contactos duplicados.
- Crear un borrador en Gmail (Human-in-the-Loop).
- Notificar al equipo mediante Slack utilizando un payload reducido.

---

# Arquitectura del Workflow

Gmail Trigger

↓

IF – Bloquear respuestas automáticas

↓

Set – Limpieza de payload

↓

AI Agent (Gemini)

↓

HubSpot – Buscar contacto por email

↓

IF – ¿Existe el contacto?

├── Sí → Actualizar contacto (Upsert)
└── No → Crear nuevo contacto (Upsert)

↓

Gmail – Create Draft (Human in the Loop)

↓

Set – Preparar payload para Slack

↓

Slack – Notificación al equipo

---

# Integraciones utilizadas

- Gmail
- Google Gemini
- HubSpot CRM
- Slack
- n8n

---

# Medidas de Seguridad Implementadas

## Prevención de bucles infinitos

Se implementó un nodo IF inmediatamente después del Gmail Trigger para bloquear correos automáticos como:

- Auto Reply
- Out of Office
- Undeliverable
- Delivery Status
- No Reply

---

## Prevención de duplicados

Antes de crear un contacto nuevo se realiza una búsqueda en HubSpot utilizando el correo electrónico.

Si el contacto existe:

- Se actualiza.

Si no existe:

- Se crea un nuevo registro.

Esto evita errores 409 por duplicados.

---

## Human in the Loop (HITL)

La respuesta generada por IA no se envía automáticamente.

El workflow crea únicamente un borrador en Gmail para que un operador humano revise y apruebe el contenido antes del envío.

---

## Limpieza de Payload

Antes de enviar información a Slack se utiliza un nodo Set para conservar únicamente los campos necesarios.

Campos enviados:

- sender_name
- sender_email
- subject
- draftId

Con esto se evita enviar payloads innecesarios o datos pesados.

---

# Tecnologías utilizadas

- n8n
- Gmail API
- HubSpot CRM
- Slack API
- Google Gemini

---

# Archivo incluido

checkpoint4_ignacio_vallejo.json

Este archivo contiene el workflow completo listo para ser importado en n8n.

---

# Autor

Ignacio Vallejo

Coderhouse – AI Automation Avanzado
