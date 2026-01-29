# Idea de proyecto

Batalla naval

Flota conformada por unidades hasta X puntos de armamento (final 1º iteracion - inicio 2º iteracion)

Unidades se pueden configurar con distintos tipos de armamento en cada casilla del barco (2º iteracion)

Cada jugador puede emplazar sus unidades en una zona inicial determinada (1º iteracion)

Durante su turno, cada jugador dispondra de una cierta cantidad de combustible, acumulable entre rondas, que pueden utilizar para mover (trasladando o rotando) sus unidades

- Primera iteracion, solo depende de la posicion inicial, direccion y obstaculos
- Segunda iteracion, pathfinding esquivando islas

Durante su turno, cada jugador dispondra de una cierta cantidad de municion, no acumulable entre rondas, que pueden utilizar para atacar con cada tipo de arma (1º iteracion)

Cada tipo de barco/clase de barco tiene caracteristicas especiales (1º/2º iteracion)

- Lancha 1x1
    - mayor velocidad y puede atravesar aguas poco profundas
    - Escudo Ligero
- Carguero 2x1
    - Aumenta la regeneracion de combustible
    - Escudo ligero
- Submarino 3x1
    - No se puede detectar con LOS, solo mediante radar
    - Escudo medio
- Acorazado 4x1
    - Escudo pesado
    - Lento, puede liberar minas subacuaticas
- Gran buque/Portaviones 3x2
    - Gran potencia de fuego, distancia de ataque aumentada, muy lento
    - Escudo medio

Por cada casilla golpeada de un barco, aumenta el coste de movimiento por casilla (2 iteracion)

El mapa tendra obstáculos (2º iteracion)

- Islas: Bloquean disparos y movimientos
- Rocas: Bloquean movimiento de todos los barcos
- Aguas poco profundas: Bloquean movimiento de todos los barcos a Excepcion de las lanchas

Clases de armas (1º iteracion):

- Ametralladoras: Daño ligero
- Cañones: Daño medio
- Artilleria: Daño pesado
- Torpedos: Se mueven en una direccion a lo largo de varios turnos sin ser detectados (Daño pesado)
- Minas: Permanecen en el lugar en el que se colocan hasta contactar con un barco o ser disparadas. Son visibles para el jugador que las coloca, pero invisibles para el rival. Tienen fuego amigo. Daño pesado
- Radar (Instantaneo o a distancia): Detectan barcos, proyectiles ocultos y submarinos. Sin daño
    - Revela la posicion del objeto pero no lo que es. Puede durar varios turnos

Dentro de cada clase de arma habra armas especificas con diferentes daños y costes (2º iteracion)

(opcional) Para cada clase de barco puede haber distintos cascos o barcos especificos con estadisticas diferentes (2º iteracion)
