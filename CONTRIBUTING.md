# 🤝 GUÍA DE CONTRIBUCIÓN

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

## 3. PREPARACIÓN DEL ENTORNO

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

## 4. FLUJO DE DESARROLLO

1. **Sincronízate:** Antes de empezar, bájate los últimos cambios de integración.

    ```bash
    git checkout develop
    git pull origin develop
    ```

2. **Crea tu rama:**

    ```bash
    git checkout -b feature/mi-nueva-tarea
    ```

3. **Programa y commitea:**
    * Usa **Conventional Commits** (ver `docs/WORKFLOW.md`).
    * Ej: `git commit -m "feat(ui): add placement grid"`

4. **Sube tus cambios:**

    ```bash
    git push origin feature/mi-nueva-tarea
    ```

5. **Abre Pull Request (PR):**
    * Ve a GitHub.
    * Abre la PR contra `develop`.
    * Pide revisión a un compañero.

---

## 5. ESTÁNDARES DE CALIDAD

Tu código será rechazado en la PR si:

1. **Contiene archivos basura:** `.DS_Store`, `__pycache__`, `node_modules`. (Revisa tu `.gitignore`).
2. **No compila:** Si rompes la build, bloqueas a los otros 7 compañeros.
3. **Código comentado/muerto:** No dejes bloques de código comentado "por si acaso".
4. **Variables en Español/Spanglish:** El código debe estar en **INGLÉS** (`ship.move()`, no `barco.mover()`). Los comentarios pueden estar en español si es necesario para aclarar conceptos complejos.
5. **Evitar cualquier Code Smell:** No es nada deseable ni God Objects/Methods/Folders, codigo duplicado, codigo no autoexplicativo (los comentarios solo para contratos en los headers de funciones o archivos generalmente), mucha complejidad (codigo hardcodeado, mucho anidamiento...)

---

## 6. DUDAS Y CONFLICTOS

* Si tienes un conflicto de merge (Git conflict) que no sabes resolver, **PIDE AYUDA** antes de forzarlo. Borrar el trabajo de un compañero por error es fácil si no se tiene cuidado.
* Usa el grupo de Discord/WhatsApp para avisar.
