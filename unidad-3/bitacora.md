# Unidad 3

[Actividades](https://juanferfranco.github.io/computacionales-2026-10/units/unit3/)

## Bitácora de proceso de aprendizaje

### Actividad 01



### Actividad 02

``` c++
#include <iostream>

using namespace std;

// Función que modifica el parámetro pasado por valor
void swapPorValor(int a, int b) {

	int c = b;
	b = a;
	a = c;

}

// Función que modifica el parámetro pasado por referencia
void swapPorReferencia(int& a, int& b) {

	int c = b;
	b = a;
	a = c;
}

// Función que modifica el parámetro utilizando punteros
void swapPorPuntero(int* a, int* b) {

	int c = *b;
	*b = *a;
	*a = c;

}

int main() {
	int a = 10;
	int b = 20;

	swapPorValor(a, b);

	cout << "a: " << a << endl;
	cout << "b: " << b << endl;

	swapPorReferencia(a, b);

	cout << "a: " << a << endl;
	cout << "b: " << b << endl;

	swapPorPuntero(&a, &b);

	cout << "a: " << a << endl;
	cout << "b: " << b << endl;

	return 0;
}
```

### Actividad 03



### Actividad 04

<details>
	<summary><b>Experimento 1: modificar el segmento de texto</b></summary><br>
	
``` c++
#include <iostream>
#include <cstdlib>

using namespace std;


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 1
    ***********************************************************/

    void* ptr = reinterpret_cast<void*>(&main);
    cout << "Voy a modificar la memoria en la dirección: " << ptr << endl;
    *reinterpret_cast<int*>(ptr) = 0;

    /********************************************************/

    return 0;
}
```

- Al ejecutar el programa, este normalmente se detiene con un error (segmentation fault o access violation). Es decir, el programa se cae antes de terminar correctamente.
- Porque el código intenta modificar la memoria donde está almacenada la función main. Esa zona pertenece al segmento de código (Text), que en los sistemas modernos está protegida como solo lectura y ejecutable, pero no escribible.
- Cuando el programa ejecuta esta línea:

- `*reinterpret_cast<int*>(ptr) = 0;`

- está intentando escribir un valor entero directamente sobre las instrucciones compiladas de main. El sistema operativo detecta que se está intentando escribir en una región protegida de memoria y genera una excepción, finalizando el programa.

El segmento de texto no puede modificarse porque está protegido por el sistema operativo para evitar errores y posibles ataques. Este experimento demuestra que el código ejecutable vive en una región de memoria distinta y protegida frente a escritura.

</details>

<details>
	<summary><b>Experimento 2: modificar el segmento de datos (constante global)</b></summary><br>
	
``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Constante global
const char* const mensaje_ro = "Hola, memoria de solo lectura";


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;


    /**********************************************************
        EXPERIMENTO 2
    ***********************************************************/

    char* ptr = (char*)&mensaje_ro;
    cout << "Voy a modificar la memoria en la dirección: " << ptr << endl;
    *ptr = 0;

    /********************************************************/

    return 0;
}
```
- el programa se cae cuando se intenta realizar la linea `*ptr = 0;`, generando una excepcion por infraccion de acceso de escritura; porque `ptr` apunta a la dirección 0x7FF76587AC10, que es &mensaje_ro —memoria de solo lectura— y la escritura en ella falla.
-  mensaje_ro está declarado como const char* const mensaje_ro = "Hola...". Eso significa que tanto el puntero como los datos apuntados son constantes; el compilador ubica ese objeto en una sección de solo lectura. En el código se hace (char*)&mensaje_ro y se asigna a char* ptr, luego se escribe *ptr = 0. Eso intenta modificar la memoria donde se almacena la variable mensaje_ro (dirección 0x7FF76587AC10 según el depurador), no el contenido de la cadena, y como esa región es de solo lectura el sistema lanza la excepción.

El programa falla porque intenta modificar una constante global, y esa zona de memoria está protegida como solo lectura para evitar cambios accidentales.

</details>

<details>
	<summary><b>Experimento 3: modificar el segmento de datos (variables globales)</b></summary><br>

``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Variables globales
int global_inicializada = 42;
int global_no_inicializada;


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 3
    ***********************************************************/

    cout << "global_inicializada: " << global_inicializada << endl;
    cout << "global_no_inicializada: " << global_no_inicializada << endl;


    global_inicializada = 69;
    global_no_inicializada = 666;

    cout << "global_inicializada: " << global_inicializada << endl;
    cout << "global_no_inicializada: " << global_no_inicializada << endl;

    /********************************************************/

    return 0;
}
```
- El programa se ejecuta normalmente, primero imprime: `global_inicializada: 42` y `global_no_inicializada: 0`. Luego, después de modificarlas, imprime: `global_inicializada: 69` y `global_no_inicializada: 666`, y aca no ocurre ningun error
- Esto ocurre porque las variables globales sí pueden modificarse durante la ejecución del programa.
	- `global_inicializada` empieza en 42 porque fue declarada con un valor inicial.
	- `global_no_inicializada` aparece como 0 porque las variables globales que no se inicializan explícitamente se inicializan automáticamente en 0.

   Ambas están en la zona de datos globales, que es una región de memoria escribible, por eso se pueden cambiar sin problema.

A diferencia del segmento de código o de las constantes de solo lectura, las variables globales sí pueden modificarse durante la ejecución porque están almacenadas en una zona de memoria que permite escritura.

</details>

<details>
	<summary><b>Experimento 4: modificar la variable local estática de una función por fuera de ella</b></summary>

``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Función de ejemplo que muestra la dirección de su variable local estática
void funcionConStatic() {
    static int var_estatica = 100;
    cout << "Dirección de var_estatica (static): " << &var_estatica << endl;
}


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 4
    ***********************************************************/

    var_estatica = 42;

    cout << "var_estatica: " << var_estatica << endl;

    /********************************************************/
    return 0;
}
```

- El programa directamente no compila. El compilador muestra un error diciendo que var_estatica no está declarada en este ámbito.
- Porque `var_estatica` fue declarada dentro de la función `funcionConStatic()`, aunque sea `static` (lo que significa que vive durante toda la ejecución del programa), su alcance sigue siendo local a esa función.

¿Qué pasa con las variables cada que entras y sales de la función?
- Las variables locales normales (no estáticas) se crean cada vez que entras a la función y se destruyen cuando sales de ella

¿Qué pasa con las variables locales estáticas?
- Las variables locales estáticas se crean una sola vez y conservan su valor entre llamadas a la función

</details>

<details>
	<summary><b>Experimento 5: variables locales estática vs no estática</b></summary>

``` c++
#include <iostream>
#include <cstdlib>

using namespace std;

// Función de ejemplo que muestra la dirección de su variable local estática
void funcionConStatic() {
    static int var_estatica = 100;
    cout << "var_estatica: " << var_estatica << endl;
    var_estatica++;
}

void funcionSinStatic() {
    int var_no_estatica = 100;
    cout << "var_no_estatica: " << var_no_estatica << endl;
    var_no_estatica++;
}


int main() {
    // Variable local (stack)
    int a = 10;
    int b = 20;

    /**********************************************************
        EXPERIMENTO 5
    ***********************************************************/

    for (int i = 0; i < 5; i++) {
        cout << "Iteración " << i << endl;
        funcionSinStatic();
        funcionConStatic();
    }

    /********************************************************/

    return 0;
}
```
- cada vez que se imprime la funcion `funcionSinStatic()` siempre da el valor de 100 mientras que `funcionConStatic()`, cada vez que se itera se le está sumando +1
- esto pasa porque `var_no_estatica` se crea desde cero cada vez que entras a funcionSinStatic(), Entonces siempre empieza en 100, la incrementas, pero al salir de la función se pierde ese cambio. Y `var_estatica` se crea una sola vez (la primera vez que llamas funcionConStatic()) y luego recuerda su valor, así que cada vez que vuelves a entrar, continúa desde donde quedó.

¿Ves alguna diferencia entre locales estáticas y no estáticas?
- Las locales no estática se “reinicia” en cada llamada y las locales estática conserva su valor entre llamadas.

</details>

<details>
	<summary><b>Experimento 6: modificar el segmento de heap</b></summary>

``` c++
#include <iostream>
using namespace std;

int main() {
    // Tamaño del arreglo dinámico
    int tam = 5;

    // Asignar memoria en el Heap para un arreglo de enteros
    int* arrayHeap = new int[tam];

    // Inicializar y mostrar los valores y direcciones de memoria
    for (int i = 0; i < tam; i++) {
        arrayHeap[i] = (i + 1) * 10;
        cout << "arrayHeap[" << i << "] = " << arrayHeap[i]
            << " en dirección " << (arrayHeap + i) << endl;
    }

    // Liberar la memoria asignada en el Heap
    delete[] arrayHeap;

    /**********************************************************
        EXPERIMENTO 6
    ***********************************************************/

    cout << arrayHeap[0] << endl;


    /********************************************************/


    return 0;
}
```

- suelta error porque se intenta usar un puntero que no tiene contenido ya que inmediatamente antes fue eliminado

¿Qué diferencias notas entre el comportamiento y la gestión del Heap en comparación con el Stack?

- Stack: las variables se crean y se borran solas al entrar/salir de funciones (automático, rápido).
- Heap: tú pides memoria con new y tú mismo debes devolverla con delete (manual). Si la liberas y la vuelves a usar, pasan errores como este.

¿Qué consecuencias tendría no liberar la memoria reservada con new?

- Se produce una fuga de memoria, provocando que el software consuma cada vez más recursos con el tiempo. Esto suele derivar en un rendimiento lento, inestabilidad o el bloqueo del 		sistema  

¿Por qué es importante usar delete[] al liberar memoria asignada para un arreglo?
- Porque `new int[tam]` reserva un arreglo (varios elementos), delete[] le dice al sistema: “voy a liberar todo el arreglo correctamente”.
Si usas delete en vez de `delete[]`, estás liberando “como si fuera un solo elemento” y no todo el conjunto
	
</details>

### Actividad 05

Explica qué ocurre al copiar un objeto en C++ y en C#. ¿Qué diferencias encuentras?

- en C++ se crea una nueva direccion con el mismo contenido del de la original dentro del stack permitiendo su uso independiente teniendo como base el contenido de la original. En cambio, en C#, se crea un puntero con la direccion del objeto creado en el stack y este ultimo, el objeto, se guarda en el heap, y, al crear la copia de esa forma, lo que hace es guardar la informacion que contiene original (el puntero), o sea la direccion del objeto y no el contenido de dicha direccion, finiquitando asi con 2 punteros hacia el mismo objeto

¿Qué es copia en C++ y en C#? ¿Es una copia independiente de original?

- en C++ es lo que su nombre lo indica, y en C# es un nuevo puntero
- en C++ si, en C# no

### Actividad integradora de investigación

<details>
<summary><b>Codigo</b></summary>
	
``` c++
#include <iostream>

int contador_global = 100;

void ejecutarContador() {
    static int contador_estatico = 0;
    contador_estatico++;
    std::cout << "  -> Llamada a ejecutarContador. Valor de contador_estatico: " << contador_estatico << std::endl;
}

void sumaPorValor(int a) {
    a = a + 10;
    std::cout << "  -> Dentro de sumaPorValor, 'a' ahora es: " << a << std::endl;
}

void sumaPorReferencia(int& a) {
    a = a + 10;
    std::cout << "  -> Dentro de sumaPorReferencia, 'a' ahora es: " << a << std::endl;
}

void sumaPorPuntero(int* a) {
    *a = *a + 10;
    std::cout << "  -> Dentro de sumaPorPuntero, '*a' ahora es: " << *a << std::endl;
}

int main() {
    int val_A = 20;
    int val_B = 20;
    int val_C = 20;

    std::cout << "--- Experimento con paso de parámetros ---" << std::endl;
    std::cout << "Valor inicial de val_A: " << val_A << std::endl;
    sumaPorValor(val_A);
    std::cout << "Valor final de val_A: " << val_A << std::endl << std::endl;

    std::cout << "Valor inicial de val_B: " << val_B << std::endl;
    sumaPorReferencia(val_B);
    std::cout << "Valor final de val_B: " << val_B << std::endl << std::endl;

    std::cout << "Valor inicial de val_C: " << val_C << std::endl;
    sumaPorPuntero(&val_C);
    std::cout << "Valor final de val_C: " << val_C << std::endl << std::endl;

    std::cout << "--- Experimento con variables estáticas ---" << std::endl;
    ejecutarContador();
    ejecutarContador();
    ejecutarContador();

    return 0;
}
```
</details>

#### A) Predicción (sin ejecutar el código)

1. ¿Cuál será la salida final en la consola de este programa?

	- para `val_A` termina con valor de 20 porque se cambia es una copia y las otras 2 (`val_B` Y `val_C`) terminan con 30 porque se hacen por referencia y por puntero lo que cambian la original. Y se imprime `contador_estatico` con 1, 2 y 3

2. Escribe la salida completa que esperas.
	
	- 
	
3. Dibuja un mapa de memoria conceptual de este programa

	- SEGMENTO DE CÓDIGO

   		- main()
       	- ejecutarContador()
       	- sumaPorValor(int)
       	- sumarPorReferencia(int&)
       	- sumarPorPuntero(int*)
  
    - DATOS GLOBALES / ESTATICOS
  
    	- contador_global (global)
   		- contador_estatico (static dentro de una funcion)
  
	- HEAP
 
		- no hay (no se usa `new`)

	- STACK 
  
		- val_A
  		- val_B
    	- val_C 


#### B) Verificación y análisis (usando el depurador)

4. 


5. 


6. Explica con tus propias palabras el comportamiento de contador_estatico. ¿Por qué “recuerda” su valor entre llamadas a la función ejecutarContador? ¿En qué se diferencia de una variable local normal?

	- `contador_estatico` recuerda porque se le declara como estatico, se crean solo una vez y estos quedan guardados en la seccion de globales y estaticos, lo que perduran al cambiar de funciones (a diferencia de las variables locales normales que se crean al entrar y se borran al salir de las funciones), lo que permiten acumular cambios y duran hasta que termina el programa


### Actividad 06



### Actividad 07



### Actividad 08



### Actividad 09



### Actividad 10

## Bitácora de aplicación 

### Actividad Integradora



## Bitácora de reflexión

### Actividad 11





