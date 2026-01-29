# ⚙️ GUÍA FLUJO DE INGENIERÍA (WORKFLOW)

**Propósito:** Estandarizar el Ciclo de Vida del Desarrollo de Software (SDLC) para el equipo de 8 personas.
**Objetivo:** Evitar conflictos de código ("Merge Hell") y asegurar que la rama principal siempre sea demostrable.

---

## 0. NIVEL PRE-REQUISITOS: Contexto del Equipo

* **Tamaño del Equipo:** 8 Desarrolladores (Divididos en Backend, Frontend Web, Móvil, Core).
* **Frecuencia de Despliegue:**
  * `[x]` **High:** Integración continua semanal (Sprints).
* **Tolerancia a Bugs:**
  * **Baja en el Core:** El motor matemático no puede fallar.
  * **Media en UI:** Errores visuales son aceptables en fases tempranas.

---

## 1. NIVEL ESTRATÉGICO: Gestión de Ramas (Git Strategy)

Hemos elegido **GitFlow Simplificado**. Necesitamos un lugar seguro (`main`) y un lugar de caos controlado (`develop`).

| Rama | Propósito | Regla de Oro |
| :--- | :--- | :--- |
| **`main`** | Producción / Demo. | **INTOCABLE**. Solo recibe merges desde `develop` o `hotfix`. Lo que hay aquí SIEMPRE compila y arranca. |
| **`develop`** | Integración. | Aquí se mezclan las funcionalidades terminadas. Es la base para crear nuevas ramas. |
| **`feature/nombre-desc`** | Trabajo diario. | Se crea desde `develop`. Vida corta (< 3 días). |
| **`hotfix/nombre-error`** | Urgencias. | Se crea desde `main` para arreglar bugs críticos en una entrega. |

---

## 2. NIVEL TÁCTICO: Historia y Commits

### 2.1. Convención de Mensajes (Atomic Commits)

Seguimos **Conventional Commits** rigurosamente.

* **Formato:** `<tipo>(<alcance>): <descripción>`

**Tipos permitidos:**

* `feat`: Nueva funcionalidad (ej: `feat(engine): add collision detection`).
* `fix`: Corrección de error (ej: `fix(auth): resolve jwt timeout`).
* `docs`: Cambios en documentación (ej: `docs(api): update swagger`).
* `style`: Formato, puntos y comas (no afecta lógica).
* `refactor`: Cambio de código que no altera el comportamiento.
* `test`: Añadir o corregir tests.
* `chore`: Tareas de mantenimiento (build, deps).

### 2.2. Estrategia de Merge

Usaremos **Squash & Merge** en las Pull Requests (PRs).

* *Razón:* Al terminar una feature con 15 micro-commits, queremos que en `develop` aparezca como **UN solo commit** limpio y descriptivo.

---

## 3. NIVEL DE CALIDAD: Code Review & Gates

### 3.1. Política de Pull Requests (PRs)

* **Origen:** Tu rama `feature/...`
* **Destino:** `develop` (Nunca a `main` directamente).
* **Tamaño Máximo:** Intentar no superar 400 líneas. Si es más grande, divídela.
* **Aprobaciones:** Mínimo **1 revisor** obligatorio (preferiblemente del mismo subsistema).
* **Checks Obligatorios:**
  * `[x]` El proyecto compila.
  * `[x]` Linter pasa sin errores (Eslint/Pylint/Sonar).

### 3.2. Etiqueta de Revisión

* **Nitpick:** "Esto es mejorable, pero te lo apruebo".
* **Blocker:** "Esto rompe la arquitectura o tiene un bug, no apruebo".

---

## 4. NIVEL DE AUTOMATIZACIÓN: CI Pipeline (GitHub Actions)

1. **Lint Check:** Se ejecuta en cada Push.
2. **Unit Tests:** Se ejecutan al abrir una PR.
3. **Build:** Se verifica que Docker puede construir la imagen sin fallos.

---

## 5. NIVEL DE ENTREGA: Versionado

Usaremos **SemVer** (`Major.Minor.Patch`) para las entregas al profesor/hitos.

* **v0.0.0:** Prototipo funcional (Fase 1) - separado en otro repositorio.
* **v0.1.0:** Multijugador básico (Fase 2).
* **v1.0.0:** Entrega Final.

---

## 6. DEFINICIÓN DE HECHO (Definition of Done - DoD)

Una tarea **NO** está terminada hasta que:

1. [ ] El código está en `develop`.
2. [ ] Funciona en el entorno local de otra persona.
3. [ ] No rompe el linter.
4. [ ] (Opcional/Deseable) Tiene tests unitarios básicos.
