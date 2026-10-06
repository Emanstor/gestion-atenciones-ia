# 🚀 Sistema de Control y Gestión Inteligente de Atenciones Profesionales mediante IA Conversacional

**Trabajo Integrador Final | Tesis de Carrera**  
**Integrantes:** Emanuel López & Santino Del Corro  
**Tutora Académica:** María Candela Grosso  

---

## 📋 Descripción del Proyecto

Plataforma web diseñada para profesionales independientes y comercios de proximidad (barberías, consultorios, centros de estética, etc.) que automatiza la administración de turnos. El sistema integra un **Asistente Virtual de IA Conversacional** que interpreta Lenguaje Natural (PLN) para agilizar la interacción con el cliente, delegando la validación y el control operativo al Backend.

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

Backend: Node.js, Express.js

Base de Datos: MySQL

IA: API LLM (OpenAI / Groq) con soporte para Function Calling
