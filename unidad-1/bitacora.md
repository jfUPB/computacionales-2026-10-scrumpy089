# Unidad 1

[Actividades](https://juanferfranco.github.io/computacionales-2026-10/units/unit1/)

## Bitácora de proceso de aprendizaje

### Actividad 01

**¿Qué sucede?**

El programa carga dos valores en los registros A y D, realiza una suma entre ellos y guarda un valor en la dirección de memoria 16, al final entra en un ciclo infinito


**¿Qué valor se almacena en la dirección de memoria 16?**

Se almacena el valor numero 3


**¿Por qué crees que es ese valor?**

Porque en la linea de código aparece "M=D" siendo "M" refiriendose a la memoria RAM y "D" a un registro que fue almacenado con dicho valor anteriormente


**¿Qué instrucciones se ejecutan en cada ciclo Fetch-Decode-Execute?**

- Se guarda el valor 1 en el registro A
- Hace que el registro D tenga el mismo valor del registro A
- Se sobreescribe en el registro A el valor 2
- Se suman los valores de los registros D y A, y el resultado se guarda en el registro D
- Le da un nuevo valor al registro A
- Almacena el valor que tiene el registro D, 3, en la direccion de memoria del valor del registro A (Address)
- Almacena el valor del numero del ROM que tenga la etiqueta "END"
- Genera un valor que, al final no se guarda y despues hace un salto a la memoria ROM que dice en el registro A


**¿Qué cambios observas en el contenido de la memoria y los registros?**

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

## Bitácora de aplicación 



## Bitácora de reflexión

