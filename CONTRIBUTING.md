# 🤝 GUÍA DE CONTRIBUCIÓN

**Proyecto:** `Tactical Grid Engine`
**Ubicación:** `Raíz del repositorio (CONTRIBUTING.md)`

Bienvenido al repositorio del proyecto. Dado que somos 8 personas tocando el mismo código, estas normas no son burocracia, son **supervivencia**.

---

## 1. ANTES DE EMPEZAR

> **ESTADO DEL PROYECTO:** **Internal Driven / Equipo Cerrado**
> Este repositorio es para el desarrollo de la asignatura. Todo el trabajo debe estar reflejado en Issues antes de convertirse en código.

---

## 2. CÓMO REPORTAR Y PEDIR TRABAJO (Issues)

No escribas código si no hay una **Issue** creada. Esto permite al profesor ver la gestión del proyecto.

### 2.1. Tipos de Issues principales

* **🐛 Bug Report:** Algo no funciona.
  * *Obligatorio:* Pasos para reproducirlo. "No me va el login" no sirve. "Al pulsar Login con usuario vacío explota" sí sirve.
* **✨ Feature Request:** Algo nuevo que implementar.
  * *Obligatorio:* Vincular a qué parte de la Arquitectura afecta (Backend, Core, Frontend).

---

## 3. PREPARACIÓN DEL ENTORNO (Setup)

El proyecto funciona como un **Monorepo**.

### 3.1. Requisitos

* **Docker & Docker Compose:** Obligatorio para levantar la infraestructura completa (DB + Back + Front).
* **Git:** Configurado con tu nombre real y email de la universidad (o el que uses en GitHub).

### 3.2. Instalación Rápida

```bash
# 1. Clonar
git clone https://github.com/TU-ORGANIZACION/tactical-grid-engine.git

# 2. Configurar entorno (copiar .env de ejemplo)
cp .env.example .env

# 3. Levantar todo el sistema
docker-compose up --build
```

---

## 4. FLUJO DE DESARROLLO (Tu día a día)

1.  **Sincronízate:** Antes de empezar, bájate los últimos cambios de integración.
    ```bash
    git checkout develop
    git pull origin develop
    ```
2.  **Crea tu rama:**
    ```bash
    git checkout -b feature/mi-nueva-tarea
    ```
3.  **Programa y commitea:**
    *   Usa **Conventional Commits** (ver `docs/WORKFLOW_STRATEGY.md`).
    *   Ej: `git commit -m "feat(ui): add ship placement grid"`
4.  **Sube tus cambios:**
    ```bash
    git push origin feature/mi-nueva-tarea
    ```
5.  **Abre Pull Request (PR):**
    *   Ve a GitHub.
    *   Abre la PR contra `develop`.
    *   Pide revisión a un compañero.

---

## 5. ESTÁNDARES DE CALIDAD

Tu código será rechazado en la PR si:
1.  **Contiene archivos basura:** `.DS_Store`, `__pycache__`, `node_modules`. (Revisa tu `.gitignore`).
2.  **No compila:** Si rompes la build, bloqueas a los otros 7 compañeros.
3.  **Código comentado/muerto:** No dejes bloques de código comentado "por si acaso". Git ya guarda el historial.
4.  **Variables en Español/Spanglish:** El código debe estar en **INGLÉS** (`ship.move()`, no `barco.mover()`). Los comentarios pueden estar en español si es necesario para aclarar conceptos complejos.

---

## 6. DUDAS Y CONFLICTOS

*   Si tienes un conflicto de merge (Git conflict) que no sabes resolver, **PIDE AYUDA** antes de forzarlo. Borrar el trabajo de un compañero por error es fácil si no se tiene cuidado.
*   Usa el grupo de Discord/WhatsApp para avisar: *"Chicos, voy a tocar el Core de Movimiento, cuidado con los conflictos"*.
```

### Feedback del Sistema
He generado los documentos basándome en tus plantillas pero tomando las decisiones técnicas necesarias para el **Tactical Grid Engine**:
1.  **GitFlow Simplificado** en lugar de GitHub Flow (más seguro para juniors).
2.  **Inglés en el código** (estándar industrial y evita el spanglish doloroso).
3.  **Docker** como herramienta central de entorno (imprescindible para que 8 personas tengan el mismo entorno).

ESTADO: Documentos de Gobernanza generados.
SIGUIENTE PASO: Definir **ArchitectureElection.md** con el stack tecnológico exacto (Lenguajes/Frameworks) para poder rellenar ese `docker-compose.yml`.