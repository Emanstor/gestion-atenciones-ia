# 🚀 Sistema de Control y Gestión Inteligente de Atenciones Profesionales mediante IA Conversacional

**Trabajo Integrador Final | Tesis de Carrera**  
**Integrantes:** Emanuel López & Santino Del Corro  
**Tutora Académica:** María Candela Grosso  

---

## 📋 Descripción del Proyecto

Plataforma web diseñada para profesionales independientes y comercios de proximidad (barberías, consultorios, centros de estética, etc.) que automatiza la administración de turnos y consultas. El sistema integra un **Asistente Virtual de IA Conversacional** que interpreta Lenguaje Natural (PLN) para agilizar la interacción con el cliente (solicitud de turnos y consulta de precios), delegando la validación y el control operativo al Backend.

---

## 🎯 Circuito Principal (Flujo del Sistema)

1. **Acceso e Identificación (Seguridad):**
   - El cliente ingresa a la WebApp.
   - En su primer ingreso, se registra indicando su **Nombre, Apellido, Número de Teléfono, Contraseña** y, de forma opcional, un **Email** para recibir anuncios.
   - Para proteger sus datos e historial en accesos posteriores, el inicio de sesión se realiza únicamente mediante su **Número de Teléfono y Contraseña**.
   - En dispositivos móviles, la sesión se mantiene guardada para agilizar el uso cotidiano.

2. **Canal Principal (IA Conversacional):**
   - **Interacción:** El cliente se comunica por el Chat Web en lenguaje natural.
   - **Procesamiento e Intención:** La IA interpreta el mensaje, identificando la intención del usuario. Principalmente soporta dos flujos:
      - **A. Solicitud de Turno:** Extrae datos clave (servicio, fecha, horario).
      - **B. Consulta de Precios:** Identifica que el cliente desea saber el costo de un servicio específico (ej: *"¿Cuánto sale un corte?"*).
   - **Validación/Consulta:** La IA envía la solicitud estructurada (JSON) al Backend.
      - Para **turnos**, el Backend consulta la disponibilidad y aplica reglas de negocio.
      - Para **precios**, el Backend busca el servicio consultado en la base de datos y recupera su valor.
   - **Respuesta:** El Backend devuelve la información y la IA redacta un mensaje amigable confirmando la cita o informando el precio.

3. **Módulo de Fallback (Contingencia ante Fallos de IA):**
   Si la IA no comprende la solicitud tras múltiples intentos o la API no responde, el sistema **nunca bloquea al usuario** y despliega 3 opciones de escape instantáneas:
   - **1. Reserva Manual:** Grilla horaria/calendario interactivo para seleccionar turno disponible directamente en el Backend.
   - **2. Contacto por WhatsApp:** Enlace directo con mensaje prellenado para comunicarse con el local.
   - **3. Atención Presencial:** Ficha con dirección física, horarios de atención y enlace a Google Maps.

---

## 🏗️ Arquitectura y Responsabilidades

Para garantizar la estabilidad y evitar inconsistencias, las responsabilidades están estrictamente delimitadas:

- 🤖 **IA (Interpretador Conversacional):** Su función se limita a la interfaz conversacional. Interpreta la intención del usuario (reservar turno o consultar precio), extrae parámetros estructurados y redacta la respuesta final. No ejecuta reglas de negocio ni accede directamente a la base de datos.
- ⚙️ **Backend (Lógica de Negocio):** Es el núcleo operativo (Node.js/Express). Valida los datos recibidos de la IA, consulta disponibilidad real, busca información de servicios (precios/duración), aplica restricciones, registra/cancela turnos y maneja la persistencia.
- 🗄️ **Base de Datos (Persistencia):** Almacena la información del sistema (MySQL): usuarios, servicios (incluyendo sus precios y duración), agendas y estados de los turnos.
- 💻 **Frontend (Interfaz de Usuario):** Interfaz web (React) para que el cliente interactúe con el chat/fallback y para que el profesional administre su agenda y catálogo.

---

## 👤 Funcionalidades por Rol

### 🔹 Cliente
- Registro inicial (Nombre, Apellido, Teléfono, Contraseña, Email opcional) e inicio de sesión seguro (Teléfono + Contraseña).
- Solicitar nuevos turnos mediante el chat conversacional asistido por IA.
- Consultar precios de los servicios ofrecidos a través de la IA.
- Módulo de contingencia (reserva manual, WhatsApp o mapa) ante fallos de IA.
- Consultar y cancelar turnos activos desde su perfil.

### 🔹 Profesional / Administrador
- Configurar servicios: Alta, baja y modificación detallada (gestión de nombre, precio y duración).
- Configurar horarios de atención y días de disponibilidad.
- Dashboard de gestión: vista de turnos del día, próximos turnos y métricas de ocupación.

---

## 📌 Alcance del MVP y Reglas de Negocio

### 🟢 Incluido en el MVP
- **Alcance Unipersonal:** El sistema está diseñado para la gestión de un negocio operado por un único profesional/dueño (mono-profesional).
- **Autenticación:** Roles diferenciados para Clientes (Registro completo e inicio con Teléfono + Clave) y Profesional (Credenciales Admin).
- **Chat conversacional:** Con procesamiento de lenguaje natural capaz de gestionar **reservas de turnos** y responder a **consultas de precios** de los servicios registrados.
- **Módulo de Contingencia:** Opciones de escape (Fallback) ante fallos de comunicación.
- **Reglas de reserva:** Confirmación en tiempo real según disponibilidad en MySQL, cancelación y reprogramación.
- **Configuración de Servicios:** Gestión integral de los servicios prestados, incluyendo definición y actualización de precios y duración.
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

    %% SUBGRAPH 2: CANAL IA (RESERVA Y PRECIOS)
    subgraph S2 ["2. Interacción por IA"]
        direction TB
        E --> G[Cliente envía mensaje]
        G --> H["IA interpreta intención<br>(Reserva o Precio)"]
        H --> H1{¿Turno o Precio?}
        H1 -- Precio --> H2[Backend busca precio en DB] --> K1[IA informa precio]
        H1 -- Turno --> H3[IA valida datos de reserva]
        H3 --> I{¿IA activa y hay cupo?}
        I -- Sí --> J[Backend reserva en DB] --> K2[IA confirma reserva]
        I -- No / Error --> L[Alerta: IA No Disponible / Sin Cupo]
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
