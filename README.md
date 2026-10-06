# 🚀 Sistema de Control y Gestión Inteligente de Atenciones Profesionales mediante IA Conversacional

**Trabajo Integrador Final | Tesis de Carrera**  
**Integrantes:** Emanuel López & Santino Del Corro  
**Tutora Académica:** María Candela Grosso  

---

## 📋 Descripción del Proyecto

Plataforma web pensada para profesionales independientes y comercios de proximidad (barberías, consultorios, centros de estética, etc.) que automatiza la administración de turnos. El sistema integra un **Asistente Virtual de IA Conversacional** que interpreta Lenguaje Natural (PLN) para agilizar la interacción con el cliente, delegando la validación y el control operativo al Backend.

---
# 🎯 Circuito Principal (Flujo del Sistema)

### Acceso e Identificación (Seguridad)
* **Ingreso:** El cliente ingresa a la WebApp.
* **Autenticación:** Para proteger sus datos e historial, se identifica mediante su **Número de Teléfono** y **Contraseña**.
* **Persistencia:** En dispositivos móviles, la sesión permanece activa para agilizar el uso cotidiano.

---

### Canal Principal (IA Conversacional)
1. **Solicitud:** El cliente pide un turno por el Chat Web en lenguaje natural *(ej: "¿Tenés lugar para un corte mañana a las 16hs?")*.
2. **Procesamiento:** La IA interpreta el mensaje y extrae los datos clave (servicio, fecha, horario deseado).
3. **Validación:** La IA envía la solicitud estructurada (`JSON`) al Backend. El Backend consulta la disponibilidad en la Base de Datos (**MySQL**) y aplica las reglas de negocio.
4. **Respuesta:** El Backend devuelve los horarios disponibles o la confirmación del turno.
5. **Confirmación:** La IA redacta una respuesta amigable al cliente y el Backend registra la cita en el sistema.

---

### Módulo de Fallback (Contingencia ante Fallos de la IA)
Si la IA no logra comprender la solicitud o el servicio de la API no está disponible, el sistema **nunca bloquea la reserva** y activa 3 opciones de escape instantáneas:

1. 📅 **Reserva Manual:** Calendario e interfaz con grilla de turnos libres para agendar directamente en el Backend.
2. 💬 **Contacto por WhatsApp:** Redirección al WhatsApp del local con un mensaje prellenado de ayuda.
3. 📍 **Atención Presencial:** Muestra dirección física, horarios de atención y enlace a Google Maps.

---

# 🏗️ Arquitectura y Responsabilidades

Para garantizar la estabilidad y evitar inconsistencias, las responsabilidades están estrictamente delimitadas:

* 🤖 **IA (Interpretador Conversacional):** Su función se limita a la interfaz conversacional. Interpreta la intención del usuario, extrae parámetros estructurados y redacta la respuesta final. *No ejecuta reglas de negocio ni accede directamente a la base de datos.*
* ⚙️ **Backend (Lógica de Negocio):** Es el núcleo operativo (**Node.js / Express**). Valida los datos recibidos de la IA, consulta la disponibilidad real, aplica restricciones de agenda, registra/cancela turnos y maneja la persistencia.
* 🗄️ **Base de Datos (Persistencia):** Almacena la información del sistema (**MySQL**): usuarios, servicios, agendas y estados de los turnos.
* 💻 **Frontend (Interfaz de Usuario):** Interfaz web (**React**) para que el cliente interactúe con el chat/fallback y para que el profesional administre su agenda.

---

# 👤 Funcionalidades por Rol

### 🔹 Cliente
* Registro e inicio de sesión mediante **Teléfono + Contraseña**.
* Solicitar nuevos turnos mediante el chat conversacional asistido por IA.
* Módulo de contingencia (reserva manual, WhatsApp o ubicación) ante fallos de la IA.
* Consultar el estado de sus turnos activos o pasados.
* Cancelar turnos previamente agendados.

### 🔹 Profesional (Administrador)
* Gestionar servicios (alta, baja y modificación de precios/duración).
* Configurar horarios de atención y días de disponibilidad.
* Visualizar y gestionar la agenda completa de atenciones en su panel (**Dashboard**).
* Métrica simple de ocupación (turnos agendados vs. horarios libres).

---

# 📌 Alcance del MVP y Reglas de Negocio

### 🟢 Incluido en el MVP
* **Modelo Unipersonal / Mono-profesional:** El sistema está diseñado para la gestión de un negocio operado por un único profesional/dueño.
* **Autenticación diferenciada:** Clientes (Teléfono + Clave) y Profesional (Credenciales Admin).
* **Chat conversacional:** Con procesamiento de lenguaje natural y Módulo de Fallback triple.
* **Reglas mínimas de reserva:**
  * **Confirmación:** Un turno se confirma solo si hay disponibilidad real en la Base de Datos.
  * **Cancelación:** El cliente o el profesional pueden cancelar un turno cambiando su estado a `CANCELADO` y liberando la agenda.
  * **Reprogramación:** Se gestiona mediante la cancelación del turno actual y la creación de una nueva reserva.
* **Dashboard del Profesional:** Vista rápida de turnos del día, próximos turnos de la semana y métricas simples.

### 🔴 Fuera del MVP (Para futuras versiones)
* Pasarelas de pago online o cobro de señas (ej. Mercado Pago).
* Integración directa con la API oficial de WhatsApp (en el MVP se utiliza enlace directo `wa.me`).
* Gestión multiespacio o de múltiples empleados por cuenta.

---

# ☁️ Estrategia de Deploy (Publicación)

El proyecto será desplegado en entornos de producción utilizando los siguientes servicios:

* **Frontend:** Vercel / Netlify
* **Backend:** Render / Railway
* **Base de Datos:** Aiven / PlanetScale (MySQL)

---

# 🛠️️ Stack Tecnológico

| Capa | Tecnología |
| :--- | :--- |
| **Frontend** | React.js |
| **Backend** | Node.js con Express |
| **Base de Datos** | MySQL |
| **IA** | API de LLM (OpenAI / Groq) con soporte para *Function Calling* |

---

# 👥 Equipo de Desarrollo

| Nombre | Rol |
| :--- | :--- |
| **Emanuel López** | Desarrollador Full Stack |
| **Santino Del Corro** | Desarrollador Full Stack |

> 🎓 *Proyecto desarrollado como Trabajo Integrador Final para la carrera.*

## 📐 Diagrama de Flujo del Sistema (Circuito Principal y Contingencia)

```mermaid
flowchart LR
    subgraph S1 ["1. Acceso y Seguridad"]
        direction TB
        A([Cliente entra a WebApp]) --> B{¿Sesión activa?}
        B -- No --> C{¿Registrado?}
        C -- No --> D[Registro: Teléfono y Clave]
        C -- Sí --> F[Login: Teléfono y Clave]
        B -- Sí --> E[Chat WebApp]
        D --> E
        F --> E
    end

    subgraph S2 ["2. Reserva por IA"]
        direction TB
        E --> G[Solicita turno por Chat]
        G --> H[IA valida con Backend]
        H --> I{¿IA activa y hay cupo?}
        I -- Sí --> J[Backend reserva en DB]
        J --> K[IA confirma reserva]
        I -- No / Error --> L[Alerta: IA No Disponible]
    end

    subgraph S3 ["3. Módulo de Fallback"]
        direction TB
        L --> M1["1. Reserva Manual - Calendario"]
        L --> M2["2. Contacto por WhatsApp"]
        L --> M3["3. Dirección y Google Maps"]
        M1 --> N[Reserva directa en DB]
    end

    S1 --> S2
    S2 --> S3
