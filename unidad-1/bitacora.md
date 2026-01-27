# Unidad 1

[Actividades](https://juanferfranco.github.io/computacionales-2026-10/units/unit1/)

## Bitácora de proceso de aprendizaje

### Actividad 01

**_¿Qué sucede?_**

El programa carga dos valores en los registros A y D, realiza una suma entre ellos y guarda un valor en la dirección de memoria 16, al final entra en un ciclo infinito


**_¿Qué valor se almacena en la dirección de memoria 16?_**

Se almacena el valor numero 3


**_¿Por qué crees que es ese valor?_**

Porque en la linea de código aparece "M=D" siendo "M" refiriendose a la memoria RAM y "D" a un registro que fue almacenado con dicho valor anteriormente


**_¿Qué instrucciones se ejecutan en cada ciclo Fetch-Decode-Execute?_**

- Se guarda el valor 1 en el registro A
- Hace que el registro D tenga el mismo valor del registro A
- Se sobreescribe en el registro A el valor 2
- Se suman los valores de los registros D y A, y el resultado se guarda en el registro D
- Le da un nuevo valor al registro A
- Almacena el valor que tiene el registro D, 3, en la direccion de memoria del valor del registro A (Address)
- Almacena el valor del numero del ROM que tenga la etiqueta "END"
- Genera un valor que, al final no se guarda y despues hace un salto a la memoria ROM que dice en el registro A


**_¿Qué cambios observas en el contenido de la memoria y los registros?_**

El registro A y D cambian durante la ejecución del programa, y como resultado final el valor 3 queda almacenado en la dirección de memoria 16, los registros PC y A quedan con el valor 7 y el registro queda con el valor de 3. El programa termina en un bucle infinito al ejecutar "0;JMP" hacia la etiqueta END

**RETO**
``` assembly
@10
D=A
@5
D=D+A
@20
M=D
@END
(END)
0;JMP

```
<img width="1246" height="568" alt="image" src="https://github.com/user-attachments/assets/2ec0926a-d318-4a9f-a2a1-b8d59fcba866" />


### Actividad 02

**_Identifica una instrucción que use la ALU y explica qué hace_**

ALU (Arithmetic Logic Unit) es la parte de la CPU que suma, resta, niega, compara o hace operaciones logicas(AND, OR, etc.)

Una instrucción que usa la ALU es:
```
D=D-A
```
Esta instrucción utiliza la ALU para realizar una resta entre los valores almacenados en los registros D y A
El resultado de la operación se guarda nuevamente en el registro D

**_¿Para qué sirve el registro PC?_**

El registro PC (Program Counter) se encarga de almacenar la dirección de la siguiente instrucción que debe ejecutar la CPU, normalmente el PC avanza de forma secuencial, pero cuando el programa encuentra una instrucción de salto (JMP, JNE, JLE, JGE), el PC cambia su valor y el programa continúa desde otra parte del código

**_¿Cuál es la diferencia entre @i y @READKEYBOARD?_**

- @i hace referencia a una variable almacenada en la memoria RAM, que en este programa se utiliza como un puntero para recorrer la memoria de la pantalla
- @READKEYBOARD hace referencia a una etiqueta del programa, es decir, a una posición del código a la cual se puede saltar para repetir el proceso de lectura del teclado

**_Describe qué se necesita para leer el teclado y mostrar información en la pantalla_**

Para leer el teclado, el programa accede a la dirección de memoria @KBD y obtiene su valor, el cual indica si una tecla está presionada o no, para mostrar información en la pantalla, el programa escribe valores en la memoria que comienza en @SCREEN, además, se necesita un bucle infinito que permita revisar constantemente el estado del teclado y condiciones que determinen si se debe escribir o borrar información en la pantalla

**_Identifica un bucle en el programa y explica su funcionamiento_**

```
@READKEYBOARD
0;JMP
```
Este salto incondicional hace que el programa vuelva constantemente a la etiqueta READKEYBOARD, esta condición se utiliza para detectar si una tecla está presionada y decidir si se debe dibujar información en la pantalla


### Actividad 3

```
@5
D=M
@10
D=D-A
@MENOR
D;JLT

@7
M=0
@FINAL
0;JMP

(MENOR)
@7
M=1

@FINAL
(FINAL)
0;JMP

```


## Bitácora de aplicación 



## Bitácora de reflexión



