# 📡 ESTRATEGIA DE DISEÑO DE API

**Propósito:** Definir los contratos de comunicación entre el Backend (Core), Frontend Web.

---

## 1. NIVEL ESTRATÉGICO: Paradigma de Comunicación

Este proyecto es híbrido. Usaremos dos canales diferenciados:

| Canal | Tecnología | Uso | Justificación |
| :--- | :--- | :--- | :--- |
| **Gestión (Lobby)** | **RESTful (JSON)** | Login, Registro, Tienda, Historial, Configuración. | Es estándar, fácil de cachear y depurar. |
| **Juego (In-Game)** | **WebSockets** | Movimiento, Chat, Actualización de estado en tiempo real. | La latencia HTTP es inaceptable para un juego multijugador fluido. |

---

## 2. NIVEL TÁCTICO: Estándares de Formato

### 2.1. Serialización y Naming

* **Formato:** `JSON` estricto (`application/json`).
* **Nomenclatura (Keys):** `camelCase` para todo (`playerId`, `shipPosition`).
* *Razón:* Coherencia con JavaScript/TypeScript en el Frontend.

### 2.2. Formatos de Datos Críticos

* **Fechas:** `ISO8601 UTC` (Ej: `2023-10-27T10:00:00Z`). El cliente la convierte a hora local.
* **Coordenadas:** Objetos explícitos, no arrays mágicos.
  * ✅ `{ "x": 10, "y": 5 }`
  * ❌ `[10, 5]`
* **Moneda:** Enteros (Ej: `credits: 1500`). Nada de floats para dinero.

---

## 3. NIVEL DE INTERACCIÓN (REST)

### 3.1. Estilo de URL

Usaremos nombres en plural y jerarquía lógica.

* `GET /api/v1/matches` (Listar partidas)
* `POST /api/v1/matches` (Crear partida)
* `GET /api/v1/matches/{matchId}` (Detalle partida)
* `GET /api/v1/users/{userId}/fleet` (La flota de un usuario)

### 3.2. Códigos de Estado

No nos volveremos locos con todos los códigos HTTP. Usaremos estos 5:

* `200 OK`: Éxito (lectura/escritura).
* `201 Created`: Éxito al crear recurso.
* `400 Bad Request`: El cliente envió datos mal (validación).
* `401 Unauthorized`: No estás logueado.
* `404 Not Found`: Recurso no existe.
* `500 Internal Server Error`: El servidor explotó (Bug nuestro).

---

## 4. NIVEL DE RESPUESTA: Envelope

Usaremos un **Wrapped Pattern** ligero para facilitar metadatos futuros.

**Respuesta de Éxito:**

```json
{
  "data": { ...objeto o array... },
  "meta": { "serverTime": "..." } // Opcional
}
```

**Respuesta de Error (Standard):**

```json
{
  "error": {
    "code": "GRID_COLLISION",
    "message": "Target cell [10,2] is occupied by an obstacle.",
    "details": { "x": 10, "y": 2 }
  }
}
```

---

## 5. DOCUMENTACIÓN Y CONTRATO

* **Enfoque:** `Code-First` con anotaciones.
* **Herramienta:** **OpenAPI 3.0 (Swagger)** generado automáticamente desde el código del Backend.
* **Regla:** Si un endpoint no está en el Swagger, **no existe** para el Frontend.

---

## 6. SEGURIDAD DE API

* **Autenticación:** `Bearer JWT` (JSON Web Token) en el Header `Authorization`.
* **WebSockets:** El Token se envía en el handshake inicial (`?token=...`).
