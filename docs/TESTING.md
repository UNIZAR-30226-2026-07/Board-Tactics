# 🧪 GUÍA ESTRATEGIA DE TESTING

**Propósito:** Definir cómo validamos la corrección lógica del motor matricial y la estabilidad del sistema distribuido.

---

## 1. NIVEL ESTRATÉGICO: Topología (La Pirámide)

Dada la naturaleza crítica del motor (cálculos de colisiones, pathfinding, visión), la carga de pruebas se distribuye así:

1. **Unit Tests (70% - Backend):**
    * **Foco:** Lógica pura. Validar que una entidad 1x3 no puede rotar si choca con un obstáculo en (x+1, y).
    * **Velocidad:** Deben ejecutarse en milisegundos. Sin I/O (Base de datos ni Red).

2. **Integration Tests (20% - API & DB):**
    * **Foco:** Contratos de comunicación. Validar que el endpoint `POST /move` recibe el JSON correcto y persiste el nuevo estado en la BD.

3. **E2E / Smoke Tests (10% - Cliente):**
    * **Foco:** Flujo completo. Abrir el juego, loguearse, mover una entidad y ver que se actualiza.

---

## 2. NIVEL TÁCTICO: Organización Física

### 2.1. Relación Código-Test

Mantendremos una estructura espejo para facilitar la localización de tests.

* **Código Fuente:** `src/core/physics/collision.ts` (o `.py`/`.java`)
* **Test Unitario:** `tests/core/physics/test_collision.ts`

### 2.2. Gestión de Datos

No usaremos datos aleatorios en tests lógicos. Usaremos **Escenarios Deterministas**.

* *Fixture:* `scenario_corridor_blocked`. (Un pasillo estrecho con un obstáculo al final).
* *Fixture:* `scenario_open_field`. (Matriz vacía 10x10).

---

## 3. NIVEL IMPLEMENTACIÓN: Tipos de Pruebas

### 3.1. Pruebas del Motor (Unitarias)

Deben probar los límites matemáticos ("Edge Cases").

* **Boundary Testing:** Intentar mover una entidad a la coordenada (-1, 0) o (N+1, N).
* **Collision Testing:** Intentar mover una entidad a una celda ocupada por otra entidad.
* **Resource Testing:** Intentar ejecutar una acción que cuesta 5 AP teniendo solo 4 AP.

### 3.2. Pruebas de API (Integración)

Se levanta una instancia del servidor (o un mock de alto nivel).

* **Auth:** Intentar acceder a un endpoint de juego sin Token.
* **State Persistence:** Realizar un movimiento, reiniciar el servicio simulado y verificar que la posición persiste.

---

## 4. GOBERNANZA Y CALIDAD

### 4.1. Definition of Done (DoD)

Una funcionalidad del Core no se considera terminada sin:

* [ ] Test de "Camino Feliz" (Funciona como se espera).
* [ ] Test de "Camino Triste" (Falla controladamente ante inputs ilegales).
* [ ] Coverage mínimo del 80% en módulos matemáticos.

### 4.2. Ejecución Continua (CI)

* GitHub Actions ejecutará la suite completa en cada **Pull Request**.
* Política de **"Broken Build"**: Si los tests fallan en `develop`, es prioridad absoluta arreglarlo antes de seguir programando nuevas features.
