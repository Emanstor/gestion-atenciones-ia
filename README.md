# 🚀 Sistema de Control y Gestión Inteligente de Atenciones Profesionales mediante IA Conversacional

**Trabajo Integrador Final | Tesis de Carrera**  
**Integrantes:** Emanuel López & Santino Del Corro  
**Tutora Académica:** María Candela Grosso  

---

## 📋 Descripción del Proyecto

Plataforma web pensada para profesionales independientes y comercios de proximidad (barberías, consultorios, centros de estética, etc.) que automatiza la administración de turnos. El sistema integra un **Asistente Virtual de IA Conversacional** que interpreta Lenguaje Natural (PLN) para agilizar la interacción con el cliente, delegando la validación y el control operativo al Backend.

---

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
