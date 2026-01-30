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

```assembly
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

``` assembly
@READKEYBOARD
0;JMP
```
Este salto incondicional hace que el programa vuelva constantemente a la etiqueta READKEYBOARD, esta condición se utiliza para detectar si una tecla está presionada y decidir si se debe dibujar información en la pantalla


### Actividad 3

Escribe un programa que compare el valor almacenado en la dirección de memoria 5 con el valor 10. Si el valor es menor que 10, guarda el valor 1 en la dirección 7. Si el valor es mayor o igual a 10, guarda el valor 0 en la dirección 7.



``` assembly
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

### Actividad 04

Crea un programa que use un ciclo para sumar los números del 1 al 5 y guarde el resultado en la dirección de memoria 12

``` Assembly
// Inicializo dandole valor a las variables "fijas"
D=A
// @i siendo el "contador" iniciando en 1
@i 
M=D

// Apartado donde inicia el ciclo
(CICLO)
// Ingreso el valor del contador en el registro D para comparar si ya se completo todo el ciclo de la suma
@i
D=M
@6
D=A-D
@SUMADO
// Tiene que dar 0 para salir del ciclo
D;JEQ

// Se le agrega el valor del contator a la variable de la sumatoria
@i
D=M
// Variable donde se va a guardar la sumatoria de los numeros
@sum
M=D+M
// Incremento del contador
@i
M=M+1
// Donde vuelve a empezar el ciclo
@CICLO
0;JMP

// Al ya haber salido del ciclo/terminado de sumar los numeros, se guarda el resultado en la direccion de memoria 12
(SUMADO)
@sum
D=M
@12
M=D
@FINAL
(FINAL)
0;JMP
```
<img width="1111" height="801" alt="image" src="https://github.com/user-attachments/assets/3f3f6482-9a6c-44df-ac1b-fbf4299435a4" />


## Bitácora de reflexión

### Actividad 05

1. **_Describe con tus palabras las tres fases del ciclo Fetch-Decode-Execute. ¿Qué rol juega el Program Counter (PC) en este ciclo?_**

El Fetch es cuando la CPU busca y obtiene la instrucción a relizar 
Decode es cuando la decodifica o interpreta la intrucción para saber que operación debe realizar
Execute es cuando la CPU ejecuta la instrucción recibida
El program Counter es, digamos, el orden en el que la CPU lee las instrucciones, que estan guardadas en la ROM, es el que lleva el control del orden de ejecución de las instrucciones

2. **_¿Cuál es la diferencia fundamental entre una instrucción-A (que empieza con @) y una instrucción-C (que involucra D, M, A, etc.) en el lenguaje ensamblador de Hack? Da un ejemplo de cada una_**

Las intrucciones tipo A se encargan principalmente en guardar / establecer valores o hacer referencia a una localizacion, mientras que las instrucciones de tipo C se encargan de hacer calculos con los valores guardados, asignaciones y saltos usando los registros A, D, M y la ALU

Ejemplos:
- tipo A:
```asm
@10
```

- tipo C:
```asm
M=D+A
```

3. **_Explica la función de los siguientes componentes del computador Hack: el registro D, el registro A y la ALU_**

- El registro A es un registro que guarda direcciones de memoria o valores inmediatos y tambien determina que posicion de la RAM se accede mediante M
- El registro D guarda datos temporales, resultados de operaciones, sirve para comparar valores o transportar información
- La ALU (Arithmetic Logic Unit) es la unidad encargada de realizar operaciones aritmeticas y logicas en la computadora

4. **_¿Cómo se implementa un salto condicional en Hack? Describe un ejemplo (p. ej., saltar si el valor de D es mayor que cero)_**

Un salto se puede utilizar usando las etiquetas, sirviendo como puntos de referencia para que el CPU sepa hacia donde realizar el salto y una instrucción de salto (JLT, JGT, JEQ, etc.), en el que sus condiciones se realiza evaluando el valor del registro D con respecto a cero

EJ:
(saltar si un valor es menor que 10)
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

5. **_¿Cómo se implementa un loop en el computador Hack? Describe un ejemplo (p. ej., un loop que decremente un valor hasta que llegue a cero)_**

Se implemente utilizando una etiqueta que marca el inicio del ciclo y un salto incondicional (JMP) que hace que el programa vuelva a esa etiqueta. Tiene una condición de salida basada en una comparación/resta

```asm
(LOOP)
@i
D=M
@END
D;JEQ      // si i == 0, sale del loop

@i
M=M-1      // i = i - 1, en el caso que tenga otro valor
@LOOP
0;JMP

(END)
0;JMP
```

6. **_¿Cuál es la diferencia entre la instrucción D=M y la instrucción M=D?_**

D=M copia el valor que esta en la memoria RAM[A] al registro D, mientras que M=D esta registrando un valor en el registro D en la posicion de memoria RAM[A]

7. **_Describe brevemente qué se necesita para leer un valor del teclado (KBD) y para “pintar” un pixel en la pantalla (SCREEN)_**

Para leer el teclado, se debe acceder a la dirección de memoria @KBD y leer su valor con D=M, donde compara registros y si un valor es distinto de cero estaria indicando que una tecla está presionada
Para pintar un pixel en la pantalla, se debe escribir un valor en la memoria que comienza en @SCREEN. Al escribir -1 se encienden los píxeles correspondientes y al escribir 0 se apagan




