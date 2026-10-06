🚀 Sistema de Control y Gestión Inteligente de Atenciones Profesionales mediante IA Conversacional

Trabajo Integrador Final | Tesis de Carrera

Integrantes: Emanuel López & Santino Del Corro

Tutora Académica: María Candela Grosso

Institución: Universidad Tecnológica Nacional (UTN)

📋 Descripción del Proyecto

Plataforma web pensada para profesionales independientes y comercios de proximidad que automatiza la administración de turnos. El sistema integra un Asistente Virtual de IA Conversacional que interpreta Lenguaje Natural (PLN) para agilizar la interacción con el cliente, delegando la validación, persistencia y control operativo al Backend.

🎯 Circuito Principal y Diagrama de Flujo

El flujo de atención está estructurado en tres grandes bloques para garantizar accesibilidad, velocidad y cero bloqueos en la reserva:

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
        C -- No --> D[Registro: Teléfono + Clave]
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


🔐 Autenticación y Registro de Usuarios

Para equilibrar la usabilidad en dispositivos móviles con la seguridad de la información:

Cliente Final: Se identifica mediante un registro ágil con Nombre, Apellido, Teléfono y Contraseña. El número de teléfono actúa como identificador único para asociar el historial de turnos. En navegadores móviles, la sesión permanece guardada para evitar logueos innecesarios, pero exige contraseña ante accesos en nuevos dispositivos.

Profesional / Administrador: Utiliza un acceso mediante credenciales tradicionales (Email/Usuario y Contraseña) con rol ADMIN para acceder al Dashboard de gestión.

🛠️ Módulo de Fallback (Contingencia ante Fallo de la IA)

Si la API de la IA experimenta caídas, no logra interpretar la solicitud tras reintentos o el usuario prefiere una alternativa tradicional, la reserva jamás se bloquea. El sistema despliega un mensaje amigable junto con tres vías de acción directa:

📅 Reserva Manual (Calendario): Una interfaz gráfica estática expuesta por el Backend con la grilla de turnos libres en tiempo real para agendar con un solo clic.

💬 Contacto Directo por WhatsApp: Redirección automática al WhatsApp del comercio con un mensaje prellenado ("Hola, tuve un problema para agendar mi turno por la web y necesito ayuda.").

📍 Atención Presencial e Información de Contacto: Ficha informativa con la dirección física del local, horarios comerciales y enlace directo a Google Maps.

🏗️ Arquitectura y Responsabilidades

🤖 IA (Interpretador Conversacional): Interfaz conversacional. Interpreta la intención del cliente en lenguaje natural, extrae parámetros estructurados (JSON) y redacta respuestas. No ejecuta reglas de negocio ni accede a la base de datos.

⚙️ Backend (Lógica de Negocio - Node.js/Express): Núcleo del sistema. Valida datos, consulta disponibilidad real, aplica reglas de reserva, maneja la persistencia y sirve las vistas de contingencia.

🗄️ Base de Datos (MySQL): Persistencia de usuarios, servicios, agendas y estados de los turnos.

💻 Frontend (React.js): WebApp responsive con chat conversacional para el cliente y panel de administración para el profesional.

📌 Alcance del MVP y Reglas de Negocio

🟢 Incluido en el MVP

Enfoque Unipersonal / Mono-profesional: El sistema está acotado a comercios de proximidad o profesionales independientes operados por un único prestador.

Flujo Conversacional: Solicitud, consulta y cancelación de turnos vía chat.

Autenticación diferida por rol: Cliente (Teléfono + Clave) vs Administrador (Panel de gestión).

Gestión de Turnos: Confirmación condicional a disponibilidad, cancelación con liberación de horario y reprogramación (cancelación + alta nueva).

Módulo de Fallback Triple: Contingencia automática visual ante fallo o degradación de la IA.

Dashboard del Profesional: Configuración de agenda, abm de servicios y vista semanal de atenciones.

🔴 Fuera del MVP (Futuras versiones)

Pasarelas de pago online o cobro de señas (Mercado Pago).

Integración con la API oficial de WhatsApp Business (se utiliza enlace directo wa.me).

Gestión multi-empleado o multi-sucursal.

☁️ Estrategia de Deploy

Frontend: Vercel / Netlify

Backend: Render / Railway

Base de Datos: Aiven / PlanetScale (MySQL)

🛠️ Stack Tecnológico

Frontend: React.js, TailwindCSS

Backend: Node.js, Express.js

Base de Datos: MySQL

IA: API LLM (OpenAI / Groq) con soporte para Function Calling
