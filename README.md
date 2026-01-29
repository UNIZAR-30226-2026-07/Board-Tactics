# Board Tactics

> **Wargame Táctico por Turnos con Niebla de Guerra.**
> Proyecto de la asignatura Proyecto Software (UNIZAR).

![Status](https://img.shields.io/badge/Status-Development-blue)
![Stack](https://img.shields.io/badge/Stack-Java%20%7C%20React%20%7C%20Flutter-orange)
![License](https://img.shields.io/badge/License-Private-red)

## Descripción

**Board Tactics** es un sistema de estrategia multijugador síncrono sobre una matriz discreta.
A diferencia del clásico "Hundir la Flota", aquí los jugadores se mueven, tienen habilidades (AP) y gestionan combustible (MP). El servidor es la única fuente de verdad y gestiona la **Niebla de Guerra** (el cliente solo recibe información de lo que ven sus unidades).

---

## Stack Tecnológico

Hemos optado por una arquitectura de **Monolito Modular** para facilitar el desarrollo en equipo.

| Capa | Tecnología |
| :--- | :--- |
| **Backend** | Java 21 + Spring Boot 3 (WebSockets & REST) |
| **Frontend Web** | React + TypeScript + Vite |
| **Mobile** | Flutter (Dart) |
| **Base de Datos** | PostgreSQL 16 |
| **Infraestructura** | Docker Compose |

---

## Quick Start

Si eres nuevo en el equipo, sigue estos pasos para tener el **Walking Skeleton** funcionando en 5 minutos.

### Prerrequisitos

* [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Corriendo)
* Git

### Instalación

1. **Clonar el repositorio:**

    ```bash
    git clone https://github.com/TU-ORGANIZACION/tactical-grid-engine.git
    cd tactical-grid-engine
    ```

2. **Configurar Variables de Entorno:**

    ```bash
    cp .env.example .env
    # (Opcional) Edita .env si tienes conflictos de puertos
    ```

3. **Levantar el Entorno (La Magia):**

    ```bash
    docker-compose up --build
    ```

    *Esto levantará la Base de Datos, el Backend y el Frontend Web automáticamente.*

4. **Acceder:**

   * **Web:** [http://localhost:5173](http://localhost:5173)
   * **API/Swagger:** [http://localhost:8080/swagger-ui.html](http://localhost:8080/swgger-ui.html)
   * **Base de Datos:** `localhost:5432` (User: `admin` / Pass: `admin123`)

---

## Estructura del Proyecto

```text
tactical-grid-engine/
├── .github/             # Pipelines de CI/CD
├── backend/             # Código Fuente Java (Spring Boot)
│   └── src/main/java/com/unizar/tacticalengine
│       ├── modules/     # AUTH, GAME, ENGINE (Separación por Features)
│       └── shared/      # Configuración global
├── frontend/            # Cliente Web (React)
├── mobile/              # Cliente Móvil (Flutter)
├── docs/                # 📚 DOCUMENTACIÓN MAESTRA
└── docker-compose.yml   # Orquestador
```

---

## Documentación

Antes de escribir una sola línea de código, **LEE ESTO**:

1. **[Reglas del Juego (RULES)](docs/RULES.md):** La lógica matemática del motor (Movimiento, Daño, Visión).
2. **[Flujo de Trabajo (WORKFLOW)](docs/WORKFLOW_STRATEGY.md):** Cómo usar Git, ramas y mensajes de commit.
3. **[Guía de Contribución (CONTRIBUTING)](CONTRIBUTING.md):** Cómo abrir Pull Requests y reportar Bugs.
4. **[Arquitectura (ARCHITECTURE)](docs/ARCHITECTURE_ELECTION.md):** Por qué usamos Spring y cómo se organizan los módulos.
5. **[API Standard (API)](docs/API_DESIGN_STANDARD_STRATEGY.md):** Contratos JSON y WebSockets.

---

## Gestión del Equipo

* **Metodología:** GitFlow Simplificado (`main`, `develop`, `feature/...`).
* **Issues:** Todo trabajo debe estar reflejado en una Issue de GitHub.
* **Testing:**
  * Backend: `mvn test` (Obligatorio para lógica Core).
  * Frontend: `npm test`.

### Miembros del Equipo

* Usuario 1 (@github) - Rol
* Usuario 2 (@github) - Rol
* Usuario 3 (@github) - Rol
* Usuario 4 (@github) - Rol
* Usuario 5 (@github) - Rol
* Usuario 6 (@github) - Rol
* Usuario 7 (@github) - Rol
* Usuario 8 (@github) - Rol
