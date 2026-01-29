# 📐 REGLAS Y FISICAS DEL JUEGO

Este documento define las reglas lógicas y matemáticas del motor, independientes de la representación visual ("Theme").

---

## 1. EL ESPACIO (GRID)

El mundo de juego se representa mediante una matriz discreta $M$ de dimensiones $W \times H$ (Ancho x Alto).

### 1.1. Sistema de Coordenadas

* Origen $(0,0)$: Esquina superior izquierda.
* Eje $X$: Incrementa hacia la derecha.
* Eje $Y$: Incrementa hacia abajo.
* Una celda se define por la tupla $C(x,y)$ donde $x \in [0, W-1]$ y $y \in [0, H-1]$.

### 1.2. Tipos de Celda (Terreno)

Cada celda tiene una propiedad de transitabilidad intrínseca:

* **TRAVERSABLE:** Coste de entrada estándar ($C=1$). (Ej: Suelo normal).
* **DIFFICULT:** Coste de entrada aumentado ($C>1$). (Ej: Terreno lento).
* **BLOCKED:** Coste infinito. No se puede entrar ni atravesar. (Ej: Muros, Montañas).

---

## 2. LAS ENTIDADES

Objetos dinámicos que interactúan en la matriz.

### 2.1. Propiedades Físicas

* **Pivot Point $(P_x, P_y)$:** Coordenada principal de la entidad.
* **Footprint (Huella):** Matriz local que define qué celdas ocupa la entidad relativa a su pivote y orientación.
  * *Ejemplo:* Una entidad de $1\times2$ ocupa $(P_x, P_y)$ y $(P_x, P_y+1)$ si mira al Sur.
* **Orientation ($\vec{v}$):** Vector unitario cardinal.
  * NORTH $(0, -1)$, EAST $(1, 0)$, SOUTH $(0, 1)$, WEST $(-1, 0)$.

### 2.2. Propiedades de Estado

* **Integrity (HP):** Valor numérico. Si llega a 0, la entidad es destruida (removida de la matriz o convertida en obstáculo `DEBRIS`).
* **Resources:**
  * **MP (Movement Points):** Recurso consumible para traslación/rotación.
  * **AP (Action Points):** Recurso consumible para habilidades activas.

---

## 3. MECÁNICA DE MOVIMIENTO

El movimiento es discreto, síncrono y validado por el servidor.

### 3.1. Traslación

Desplazamiento del Pivote en la dirección de la Orientación actual.

* **Input:** `MOVE_FORWARD(n_steps)`
* **Coste:** $\sum_{i=1}^{n} CosteCelda(C_i)$.
* **Validación:** Para cada paso, todas las celdas del *Footprint* de la entidad deben caer en celdas `TRAVERSABLE` y no ocupadas por otras entidades.

### 3.2. Rotación

Cambio del vector de Orientación en 90 grados.

* **Input:** `ROTATE_CW` (Horario) o `ROTATE_CCW` (Antihorario).
* **Pivote:** La rotación ocurre alrededor del *Pivot Point*.
* **Validación:** El *Footprint* rotado no debe colisionar con celdas `BLOCKED` u otras entidades.
  * *Nota:* Entidades largas pueden "atascarse" en pasillos estrechos si no tienen espacio para girar.

---

## 4. SISTEMA DE INFORMACIÓN

El motor gestiona información incompleta ("Niebla de Guerra").

### 4.1. Sensores y Rango

Cada entidad tiene un atributo `VisionRange` (R).

* La visibilidad se calcula mediante algoritmo de **Raycasting** o **Shadowcasting** desde el Pivote.
* Celdas `BLOCKED` interrumpen la línea de visión (LOS).

### 4.2. Estado del Cliente

El cliente solo recibe información de:

1. Celdas estáticas del mapa (siempre conocidas o descubiertas).
2. Entidades propias.
3. Entidades enemigas que se encuentren dentro del conjunto de celdas visibles $\cup Visibilidad(Entidad_i)$.

---

## 5. BUCLE DE JUEGO (GAME LOOP)

1. **Fase de Inicio:** Regeneración de MP/AP para el Jugador Activo.
2. **Fase de Acción:**
    * Jugador envía `RequestAction(Type, Params)`.
    * Motor evalúa: `CanExecute(State, Action) -> Boolean`.
    * Si `True`: `ApplyAction(State, Action) -> NewState`.
    * Si `False`: Devuelve `Error(Reason)`.
3. **Resolución:** Si la acción provoca daño/cambio, se evalúan condiciones de muerte/victoria.
4. **Notificación:** Se emiten eventos `STATE_UPDATE` a los clientes conectados (filtrando información oculta).
