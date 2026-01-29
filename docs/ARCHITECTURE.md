# 🏛️ DECISIÓN ARQUITECTÓNICA

Este documento formaliza las decisiones técnicas tomadas para el desarrollo del proyecto.

---

## 0. CONTEXTO Y RESTRICCIONES (Pre-Requisitos)

* **Equipo:** 8 Desarrolladores (Estudiantes de Ingeniería). Perfil técnico: Java/Spring Lovers.
* **Tiempo:** ~150 horas/persona. (Deadline estricto).
* **Restricción:** Obligatoriedad de 2 clientes distintos (Web + Móvil).
* **Complejidad:** Alta en lógica algorítmica (Core), Media en concurrencia (Turnos síncronos).

---

## 1. NIVEL SISTEMA: Topología Física

> **DECISIÓN:** **Monolito Modular**

**Justificación:**

* **Evita la complejidad distribuida:** No queremos gestionar latencias de red ni transacciones distribuidas entre microservicios.
* **Facilita el refactor:** Mover código entre módulos es copiar-pegar archivos, no reconfigurar redes.
* **Despliegue atómico:** Todo el backend se despliega en un solo contenedor Docker. Fácil para el profesor y para nosotros.

---

## 2. NIVEL DATOS: Estrategia de Estado

### 2.1. Propiedad del Dato

> **DECISIÓN:** **Shared Database (Lógica)**

Aunque usamos un Monolito, lógicamente intentaremos que el módulo `Game` no haga JOINs con tablas de `Auth`. Pero físicamente, todo vive en una única instancia de PostgreSQL para simplificar backups y gestión.

### 2.2. Modelo de Persistencia

> **DECISIÓN:** **Híbrido (State Oriented + In-Memory)**

* **Datos Fríos (Usuarios, Historial):** `State Oriented` en PostgreSQL. Persistencia clásica ACID.
* **Datos Calientes (Partida en Curso):** `In-Memory` (Java `ConcurrentHashMap` o Redis).
  * *Razón:* El estado del tablero cambia cada segundo. Escribir en disco cada movimiento es lento e innecesario hasta que la partida termina.

---

## 3. NIVEL ESTRUCTURAL: Organización Lógica

> **DECISIÓN:** **Arquitectura de Capas Modular (Package by Feature)**

Organizamos el código verticalmente por dominio, y dentro de cada dominio, usamos capas estándar de Spring.

**Esquema de Carpetas:**

```text
src/main/java/com/engine
├── modules
│   ├── auth        (Login, Register)
│   ├── matchmaking (Colas, Lobby)
│   └── engine      (Lógica pura de matrices)
└── shared          (Config, Utils)
```

**Justificación vs Hexagonal:**

* La Arquitectura Hexagonal pura requiere demasiado *boilerplate* (mappers, puertos) para el tiempo disponible.
* "Package by Feature" ofrece el 80% de los beneficios de desacoplamiento con el 20% del esfuerzo.

---

## 4. NIVEL IMPLEMENTACIÓN: Patrones Tácticos

* **Comunicación Cliente-Servidor:**
  * **REST (JSON):** Para acciones atómicas (Login, Unirse a Cola).
  * **WebSockets (STOMP):** Para el bucle de juego (Eventos de movimiento, Chat).
* **Manejo de Errores:**
  * **Global Exception Handler:** Uso de `@ControllerAdvice` de Spring para capturar errores y devolver JSONs estandarizados (RFC 7807).

---

## 5. ATRIBUTOS DE CALIDAD (Prioridades)

1. **Mantenibilidad:** Código limpio y tipado fuerte (Java/TS/Dart) para que 8 personas se entiendan.
2. **Corrección (Correctness):** El motor no puede permitir movimientos ilegales. Tests unitarios masivos en el Core.
3. *Sacrificamos:* Escalabilidad Masiva (No necesitamos soportar 1 millón de usuarios).

---

## 6. STACK TECNOLÓGICO (Glosario Final)

| Capa | Tecnología | Justificación |
| :--- | :--- | :--- |
| **Lenguaje Backend** | **Java 21** | LTS más reciente, rendimiento y familiaridad. |
| **Framework Backend** | **Spring Boot 3** | Ecosistema robusto, WebSockets fáciles. |
| **Base de Datos** | **PostgreSQL 16** | Relacional, robusta, soporte JSONB si hace falta. |
| **Frontend Web** | **React + TypeScript + Vite** | Desarrollo rápido, tipado seguro. |
| **Frontend Móvil** | **Flutter (Dart)** | Multiplataforma real, tipado fuerte similar a Java. |
| **Infraestructura** | **Docker Compose** | Entorno reproducible para los 8 devs. |
