# Unidad 6

[ACTIVIDADES](https://juanferfranco.github.io/computacionales-2026-10/units/unit6/)

## Bitácora de proceso de aprendizaje

### Actividad 01

¿Qué observas en la aplicación al presionar las teclas a, r, s, n?

la letra `a` atrae las particulas donde esta el cursor, `r` repele las particulas al rededor del cursor, `s` detiene todo moviento de particulas y `n` las devuelve a su estado normal 

¿Qué diferencias notas entre los tipos de partículas?

las rojas tienen una mayor cantidad que las otras y en general son mas pequeñas, las verdes son las mas rapidas y las azules son mas grandes que las otras y son mas lentas

Formula una hipótesis: ¿Cómo crees que el código organiza la comunicación entre las teclas, las partículas y el cambio de comportamiento?


### Actividad 02

#### Investigación del patrón Observer
- Coloca un breakpoint dentro de `Subject::notify`. Cuando se dispare al presionar una tecla, observa en el depurador el vector `observers`: ¿Cuántos elementos tiene? ¿Qué tipo de objetos son? ¿A qué direcciones de memoria apuntan?

<img width="1355" height="826" alt="image" src="https://github.com/user-attachments/assets/aef7e221-6607-4382-b906-f767c7505361" />

  tiene 115 elementos que puede referirse a las 100 particulas tipo `star`, 5 particulas tipo `shooting_star` y 10 particulas tipo `planet`

  el tipo almacenado es `Observer*`, pero los objetos reales que apunta cada puntero son instancias de `Particle`, porque `Particle` hereda de `Observer`

  Cada entrada del vector guarda una dirección distinta, porque cada partícula se crea dinámicamente con `new` dentro de `ParticleFactory::createParticle()`

-  Ahora coloca un breakpoint en `Particle::onNotify`. ¿Cuál es la dirección de memoria del objeto `this` que recibe la llamada? ¿Puedes encontrar esa misma dirección en el vector `observers` que observaste antes? ¿Qué concluyes sobre cómo el `Subject` sabe a quién notificar?

<img width="1313" height="548" alt="image" src="https://github.com/user-attachments/assets/c40fe29a-4d5e-4de7-b6b4-a1543bb41123" />

  la dirección de memoria del objeto `this` es 0x0000024078b0e6e0 
  
  Sí, en la primera captura (vector `observers`) aparece exactamente qie [0] es 0x0000024078b0e6e0 

  El `Subject` sabe a quién notificar porque mantiene un vector de punteros a los observadores. Cada puntero representa la dirección de memoria de un objeto Particle. Al recorrer este vector y llamar `observer->onNotify(event)`, el `Subject` invoca directamente el método sobre cada objeto usando su dirección en memoria, lo que permite la comunicación sin conocer detalles internos de cada observador.

#### Investigación del patrón State

- Coloca un breakpoint en `Particle::setState`. Observa en memoria el puntero `state` antes y después de la asignación: ¿Qué dirección tenía? ¿Qué dirección tiene ahora? ¿qué le pasó al objeto de estado anterior?

Antes
<img width="1187" height="521" alt="image" src="https://github.com/user-attachments/assets/ce1e1244-f40d-4914-821c-2281553d17ef" />

Despues
<img width="1187" height="475" alt="image" src="https://github.com/user-attachments/assets/9c300cdc-e395-4feb-9c23-b3382d2a8b91" />

`state` contaba con la direccion de memoria `0x000001b32bda52a0` correspondiente a un objeto `NormalState`, al mismo tiempo, el parámetro `newState` apuntaba a la dirección `0x000001b338268a30` correspondiente al objeto `AttrackState`, despues `state` cambió la direccion por la de `newState`

Antes de reemplazarlo, el estado anterior ejecutó `onExit(this)` y luego fue destruido con `delete state`;. Esto significa que el objeto `NormalState` que estaba en la dirección `0x000001b32bda52a0` fue liberado de memoria y dejó de existir. Luego el puntero state fue actualizado para apuntar al nuevo objeto de estado

-  En la unidad anterior estudiaste la `_vtable` y el despacho dinámico. Ahora conéctalo con el patrón `State`: coloca breakpoints en `NormalState::update` y en `AttractState::update`. Presiona n y luego a. ¿A cuál llega el depurador primero en cada caso? ¿Cómo demuestra esto que el patrón State usa polimorfismo para cambiar el comportamiento en tiempo de ejecución?

<img width="1511" height="491" alt="image" src="https://github.com/user-attachments/assets/2bffa0ef-adb0-4647-92d5-630bae02d39e" />

<img width="1478" height="439" alt="image" src="https://github.com/user-attachments/assets/0a31521a-d3de-4d6a-b16b-6556ee0226d8" />

  sin presionar ninguna tecla incluso cuando el programa apenas esta iniciando el depurador para en `NormalState::update` 

  al presionar la tecla `a` el depurador llega a `AttractState::update`

## Bitácora de aplicación 


## Bitácora de reflexión
