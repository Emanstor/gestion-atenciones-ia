# 🚀 Sistema de Control y Gestión Inteligente de Atenciones Profesionales mediante IA Conversacional

**Trabajo Integrador Final | Tesis de Carrera**  
**Integrantes:** Emanuel López & Santino Del Corro  
**Tutora Académica:** María Candela Grosso  

---

## 📋 Descripción del Proyecto

Plataforma web diseñada para profesionales independientes y comercios de proximidad (barberías, consultorios, centros de estética, etc.) que automatiza la administración de turnos. El sistema integra un **Asistente Virtual de IA Conversacional** que interpreta Lenguaje Natural (PLN) para agilizar la interacción con el cliente, delegando la validación y el control operativo al Backend.

---

## 🎯 Circuito Principal (Flujo del Sistema)

1. **Acceso e Identificación (Seguridad):**
   - El cliente ingresa a la WebApp.
   - En su primer ingreso, se registra indicando su **Nombre, Apellido, Número de Teléfono, Contraseña** y, de forma opcional, un **Email** para recibir anuncios.
   - Para proteger sus datos e historial en accesos posteriores, el inicio de sesión se realiza únicamente mediante su **Número de Teléfono y Contraseña**.
   - En dispositivos móviles, la sesión se mantiene guardada para agilizar el uso cotidiano.

2. **Canal Principal (IA Conversacional):**
   - **Solicitud:** El cliente pide un turno por el Chat Web en lenguaje natural (ej: *"¿Tenés lugar para un corte mañana a las 16hs?"*).
   - **Procesamiento:** La IA interpreta el mensaje y extrae los datos clave (servicio, fecha, horario).
   - **Validación:** La IA envía la solicitud estructurada (JSON) al Backend. El Backend consulta la disponibilidad en la Base de Datos (MySQL) y aplica las reglas de negocio.
   - **Confirmación:** El Backend devuelve la respuesta y la IA redacta un mensaje amigable confirmando la cita.

3. **Módulo de Fallback (Contingencia ante Fallos de IA):**
   Si la IA no comprende la solicitud tras múltiples intentos o la API no responde, el sistema **nunca bloquea al usuario** y despliega 3 opciones de escape instantáneas:
   - **1. Reserva Manual:** Grilla horaria/calendario interactivo para seleccionar turno disponible directamente en el Backend.
   - **2. Contacto por WhatsApp:** Enlace directo con mensaje prellenado para comunicarse con el local.
   - **3. Atención Presencial:** Ficha con dirección física, horarios de atención y enlace a Google Maps.

---

## 🏗️ Arquitectura y Responsabilidades

Para garantizar la estabilidad y evitar inconsistencias, las responsabilidades están estrictamente delimitadas:

- 🤖 **IA (Interpretador Conversacional):** Su función se limita a la interfaz conversacional. Interpreta la intención del usuario, extrae parámetros estructurados y redacta la respuesta final. No ejecuta reglas de negocio ni accede directamente a la base de datos.
- ⚙️ **Backend (Lógica de Negocio):** Es el núcleo operativo (Node.js/Express). Valida los datos recibidos de la IA, consulta la disponibilidad real, aplica restricciones de agenda, registra/cancela turnos y maneja la persistencia.
- 🗄️ **Base de Datos (Persistencia):** Almacena la información del sistema (MySQL): usuarios, servicios, agendas y estados de los turnos.
- 💻 **Frontend (Interfaz de Usuario):** Interfaz web (React) para que el cliente interactúe con el chat/fallback y para que el profesional administre su agenda.

---

## 👤 Funcionalidades por Rol

### 🔹 Cliente
- Registro inicial (Nombre, Apellido, Teléfono, Contraseña, Email opcional) e inicio de sesión seguro (Teléfono + Contraseña).
- Solicitar nuevos turnos mediante el chat conversacional asistido por IA.
- Módulo de contingencia (reserva manual, WhatsApp o mapa) ante fallos de IA.
- Consultar y cancelar turnos activos desde su perfil.

### 🔹 Profesional / Administrador
- Configurar servicios (alta, baja, modificación de precios y duración).
- Configurar horarios de atención y días de disponibilidad.
- Dashboard de gestión: vista de turnos del día, próximos turnos y métricas de ocupación.

---

## 📌 Alcance del MVP y Reglas de Negocio

### 🟢 Incluido en el MVP
- **Alcance Unipersonal:** El sistema está diseñado para la gestión de un negocio operado por un único profesional/dueño (mono-profesional).
- **Autenticación:** Roles diferenciados para Clientes (Registro completo e inicio con Teléfono + Clave) y Profesional (Credenciales Admin).
- **Chat conversacional:** Con procesamiento de lenguaje natural y Módulo de Contingencia triple.
- **Reglas de reserva:** Confirmación en tiempo real según disponibilidad en MySQL, cancelación y reprogramación.
- **Dashboard del Profesional:** Vista rápida de agenda y métricas simples de ocupación.

### 🔴 Fuera del MVP (Para futuras versiones)
- Pasarelas de pago online o cobro de señas (Mercado Pago).
- Integración directa con la API oficial de WhatsApp (en el MVP se usa link directo `wa.me`).
- Gestión multiespacio o de múltiples empleados por cuenta.

---

## ☁️ Estrategia de Deploy (Publicación)

- **Frontend:** Vercel / Netlify
- **Backend:** Render / Railway
- **Base de Datos:** Aiven / PlanetScale (MySQL)

---

## 🛠️ Stack Tecnológico

- **Frontend:** React.js
- **Backend:** Node.js con Express
- **Base de Datos:** MySQL
- **IA:** API de LLM (OpenAI / Groq) con soporte para Function Calling

---

## 👥 Equipo de Desarrollo

| Nombre | Rol |
| :--- | :--- |
| **Emanuel López** | Desarrollador Full Stack |
| **Santino Del Corro** | Desarrollador Full Stack |

*Proyecto desarrollado como Trabajo Integrador Final para la carrera.*

---

## 📐 Diagrama de Flujo del Sistema (Circuito Principal y Contingencia)

```mermaid
flowchart LR
    %% ESTILOS VISUALES
    classDef default fill:#1f2937,stroke:#4b5563,color:#fff,stroke-width:1px;
    classDef highlight fill:#1e3a8a,stroke:#3b82f6,color:#fff,stroke-width:2px;
    classDef fallback fill:#581c87,stroke:#9333ea,color:#fff,stroke-width:2px;

    %% SUBGRAPH 1: AUTENTICACIÓN
    subgraph S1 ["1. Acceso y Seguridad"]
        direction TB
        A([Cliente entra a WebApp]) --> B{¿Sesión activa?}
        B -- No --> C{¿Registrado?}
        C -- No --> D["Registro: Nombre, Apellido,<br>Teléfono, Clave, Email (opcional)"]
        C -- Sí --> F[Login: Teléfono + Clave]
        B -- Sí --> E[Pantalla Chat WebApp]
        D --> E
        F --> E
    end

    %% SUBGRAPH 2: CANAL IA
    subgraph S2 ["2. Reserva por IA"]
        direction TB
        E --> G[Cliente pide turno por Chat]
        G --> H[IA valida con Backend]
        H --> I{¿IA activa y hay cupo?}
        I -- Sí --> J[Backend reserva en DB] --> K[IA confirma reserva]
        I -- No / Error --> L[Alerta: IA No Disponible]
    end

    %% SUBGRAPH 3: FALLBACK TRIPLE
    subgraph S3 ["3. Módulo de Contingencia"]
        direction TB
        L --> M1["📅 1. Reserva Manual (Calendario)"]
        L --> M2["💬 2. Contacto por WhatsApp"]
        L --> M3["📍 3. Dirección y Google Maps"]
        M1 --> N[Reserva directa en DB]
    end

    %% CONEXIONES PRINCIPALES
    S1 ==> S2
    S2 ==> S3

    class S1 highlight;
    class S2 highlight;
    class S3 fallback;
