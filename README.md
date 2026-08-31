# 🚀 Sistema de Control y Gestión Inteligente de Atenciones Profesionales mediante IA Conversacional

> **Trabajo Integrador Final** | Tesis de Carrera  
> **Integrantes:** Emanuel López & Santino Del Corro  
> **Tutora Académica:** María Candela Grosso  

---

## 📋 Descripción del Proyecto

Plataforma web pensada para profesionales independientes y comercios de proximidad que automatiza la administración de turnos. El sistema integra un **Asistente Virtual de IA Conversacional** que interpreta Lenguaje Natural (PLN) para agilizar la interacción con el cliente, delegando la validación y el control operativo al Backend.

---

## 🎯 Circuito Principal (Flujo del Sistema)

1. **Solicitud:** El cliente pide un turno por chat en lenguaje natural (ej: *"¿Tenés lugar para un corte el viernes a la tarde?"*).
2. **Procesamiento:** La IA interpreta el mensaje y extrae los datos clave (`servicio`, `fecha`, `horario deseado`).
3. **Validación:** La IA envía la solicitud estructurada (JSON) al Backend. El Backend consulta la disponibilidad en la Base de Datos y aplica las reglas de negocio.
4. **Respuesta:** El Backend devuelve los horarios disponibles o la confirmación del turno.
5. **Confirmación:** La IA redacta una respuesta amigable al cliente y el Backend registra la cita en el sistema.

---

## 🏗️ Arquitectura y Responsabilidades

Para garantizar la estabilidad y evitar inconsistencias, las responsabilidades están estrictamente delimitadas:

* 🤖 **IA (Interpretador Conversacional):** Su función se limita a la interfaz conversacional. Interpreta la intención del usuario, extrae parámetros estructurados y redacta la respuesta final. **No ejecuta reglas de negocio ni accede directamente a la base de datos.**
* ⚙️ **Backend (Lógica de Negocio):** Es el núcleo operativo (Node.js/Express). Valida los datos recibidos de la IA, consulta la disponibilidad real, aplica restricciones de agenda, registra/cancela turnos y maneja la persistencia.
* 🗄️ **Base de Datos (Persistencia):** Almacena la información del sistema (MySQL): usuarios, servicios, agendas y estados de los turnos.
* 💻 **Frontend (Interfaz de Usuario):** Interfaz web (React) para que el cliente interactúe con el chat y para que el profesional administre su agenda.

---

## 📌 Alcance del MVP (Mínimo Producto Viable)

### 🟢 Incluido en el MVP
* Autenticación básica para Clientes y Profesional (Admin).
* Chat conversacional para solicitud, consulta de disponibilidad y reserva de turnos.
* Módulo de fallback (soporte por interfaz gráfica simple si la IA no logra interpretar la solicitud).
* Panel de administración (Dashboard) para el profesional: gestión de horarios de atención, listado de servicios y visualización de agenda.

### 🔴 Fuera del MVP (Para futuras versiones)
* Pasarelas de pago online o cobro de señas (Mercado Pago).
* Integración directa con la API oficial de WhatsApp.
* Gestión multiespacio o de múltiples empleados por cuenta.

---

## 🛠️ Stack Tecnológico

* **Frontend:** React.js
* **Backend:** Node.js con Express
* **Base de Datos:** MySQL
* **IA:** API de LLM (OpenAI / Groq) con soporte para *Function Calling*

---

## 👥 Equipo de Desarrollo

| Nombre | Rol |
| :--- | :--- |
| **Emanuel López** | Desarrollador Full Stack |
| **Santino Del Corro** | Desarrollador Full Stack |

---
*Proyecto desarrollado como Trabajo Integrador Final para la carrera.*
