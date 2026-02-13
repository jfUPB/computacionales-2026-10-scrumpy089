# Unidad 2

[Actividades](https://juanferfranco.github.io/computacionales-2026-10/units/unit2/)

## Bitácora de proceso de aprendizaje

### Actividad 01

<img width="286" height="227" alt="image" src="https://github.com/user-attachments/assets/7f26536a-e31c-4aeb-80c7-de6b5c0dc9c0" />

``` asm
(START)
@SCREEN
M=1

@FIN
(FIN)
0;JMP
```

### Actividad 02

<img width="273" height="218" alt="image" src="https://github.com/user-attachments/assets/adbfe880-701b-4382-81e5-1c267c63c692" />

``` asm
(START)
@SCREEN
M=-1

@FIN
(FIN)
0;JMP
```

### Actividad 03

<img width="499" height="831" alt="image" src="https://github.com/user-attachments/assets/f47350b0-70ce-4d2b-9077-58989bb72336" />
<img width="309" height="668" alt="image" src="https://github.com/user-attachments/assets/139c3651-1adc-480c-909a-fbcda643f410" />

``` asm
(START)
//i = SCREEN
@SCREEN
D=A
@i
M=D
// Inicio la pantalla pintando una linea
@SCREEN
M=-1

// Detecta si se esta presionando "d" o "i"
(LOOP)
@KBD
D=M
@100
D=D-A
@DERECHA
D;JEQ

@KBD
D=M
@105
D=D-A
@IZQUIERDA
D;JEQ
@LOOP
0;JMP

// Si se presiona "d" se moverá a la derecha
(DERECHA)
@i
A=M
M=0
A=A+1
M=-1
D=A
@i
M=D

@LOOP
0;JMP

// Si se presiona "i" se moverá a la izquierda
(IZQUIERDA)
@i
A=M
M=0
A=A-1
M=-1
D=A
@i
M=D

@LOOP
0;JMP

@FIN
(FIN)
0;JMP

```


### Actividad 04
### Actividad 05

<img width="987" height="208" alt="image" src="https://github.com/user-attachments/assets/5df22ab1-e764-4c93-893f-2993b27774bb" />

### Traducción

``` asm
// int a = 10;
@10 
D=A
@a
M=D
// int* p;
//p = &a;
@a
D=A
@p
M=D
// *p = 20;
@20
D=A
// tener A el valor almacenado en p
@p
A=M
M=D
```
<img width="976" height="164" alt="image" src="https://github.com/user-attachments/assets/5781ef68-2916-495e-965b-7e98ab0c349f" />

### Traducción

``` asm
// int a = 10;
@10
D=A
@a
M=D
// int b = 5;
@5
D=A
@b
M=D
// int *p;
//p = &a;
@a
D=A
@p
M=D
//b = *p;
@p // A=contenido de p, pero p contiene la dirección A
A=M // A =16
D=M // D = contenido de la direción 16 --> 10
@b // A = la dirección de b --> 17
M=D // Guardando en la 17, que es la b, el 10 que tengo en D
```


### Actividad 06

``` asm
@1
D=A
@16
M=D

@20
D=A
@17
M=D

@13
D=A
@18
M=D

@24
D=A
@19
M=D

@55
D=A
@20
M=D

@96
D=A
@21
M=D

@87
D=A
@22
M=D

@87
D=A
@22
M=D

@83
D=A
@23
M=D

@98
D=A
@24
M=D

@102
D=A
@25
M=D

// j = 0
@R0
M=0
// sum = 0
@R1
M=0
// ptr = 16
@16
D=A
@R2
M=D

(LOOP)
@R0
D=M
@10
D=D-A
@END
D;JEQ

// D= *ptr
@R2
A=M
D=M

// sum = sum+D
@R1
M=M+D

// ptr++
@R2
M=M+D

// j++
@R0
M=M+1

@LOOP
0;JMP

(END)
@END
0;JMP
```

### Actividad 07
<img width="897" height="390" alt="image" src="https://github.com/user-attachments/assets/4617eef2-c3ed-4ecf-8151-4bf977ca42ee" />

``` asm
(start)

//int result = 0;

@result
M=0
// inicializa y habilita la RAM 16 para almacenar el valor 0, dandole el nombre de result 

/*
int main()
{
  result = sum(3, 4);
  std::cout << "The sum: " << result << std::endl;
}
*/

// Load sum arguments
@3
D=A
@R0
M=D
// Ingresa el valor de 3 y la guarda en la dirección RAM 0

@4
D=A
@R1
M=D
// Ingresa el valor de 4 y la guarda en la dirección RAM 1
}
// Save return address
@returnFromSum
D=A
@R15
M=D
// returnFromSum es el puntero de una etiqueta, se guarda la dirección de dicha etiqueta en la RAM 15 (@R15), que se usará mas adelante

// call sum
@sum
0;JMP
// sum es el puntero de una etiqueta, con el que se saltará a la operación para sumar los valores que hay en @R0 y @R1

// return after sum
// and store result
(returnFromSum)
@R0
D=M
@result
M=D
// se copia el valor de la suma para despues, almacenarla en la "variable" correspondiente al resultado
@fin
(fin)
0;JMP

/*
int sum(int a, int b)
{
    return a + b;
}
*/

(sum)
@R0
D=M
// Se copia el valor almacenado en RAM 0 en D
@R1
// Se instancia la direccion de @R1 para después hacer referencia el contenido de RAM 1
D=D+M
// se realiza la suma del valor de @R0 (que esta en D) y @R1 (que se llamó la dirección de la posición de RAM 1 para que estuviera en A, para que despues el programa a que direccion (Address) de memoria buscar). guardandola en D
@R0
M=D
// Devuelve la suma a @R0
@R15
// Se llama la direccón de la etiqueta previamente almacenada para salir del proceso de la suma 
A=M
0;JMP
```

## Bitácora de aplicación 

### Actividad 08

<img width="1019" height="496" alt="image" src="https://github.com/user-attachments/assets/f7c596b1-d9c5-45a0-a417-19db2653ae84" />

``` asm
@10
D=A
@16
M=D    // RAM[16] = a = 10

@20
D=A
@17
M=D    // RAM[17] = b = 20

@16
D=A
@R0
M=D    //R0 = &a (16)

@17
D=A
@R1
M=D    //R1 = &b (17)

@retornoDespuesCambio
D=A
@R15
M=D

@Cambio
0;JMP

(retornoDespuesCambio)
@0
D=A
@R0
M=D

@END
(END)
0;JMP

(Cambio)

@R0
A=M
D=M
@13
M=D

@R1
A=M
D=M
@R0
A=M
M=D

@13
D=M
@R1
A=M
M=D

@R15
A=M
0;JMP
```

<img width="1117" height="807" alt="image" src="https://github.com/user-attachments/assets/02bf16d4-b5e2-4356-98ce-843d04dfd208" />

En la captura se puede ver que:

<img width="1021" height="499" alt="image" src="https://github.com/user-attachments/assets/e98a1768-c123-436c-8eb1-692047c6fea9" />

``` asm
// ---------- main: inicializar arr[] ----------
@10
D=A
@16
M=D          // arr[0]=10

@15
D=A
@17
M=D          // arr[1]=15

@2
D=A
@18
M=D          // arr[2]=2

@3
D=A
@19
M=D          // arr[3]=3

@50
D=A
@20
M=D          // arr[4]=50

// ---------- llamar calSum(arr, 5) ----------
@16
D=A
@R0
M=D          // R0 = &arr[0] = 16

@5
D=A
@R1
M=D          // R1 = arrSize = 5

@RET_AFTER_CALSUM
D=A
@R15
M=D          // guardar return address

@CALSUM
0;JMP        // saltar a función

(RET_AFTER_CALSUM)
// return 0;
@0
D=A
@R2
M=D         

(END)
@END
0;JMP


// =====================================================
// int calSum(int* parr, int arrSize)
// Entrada: R0=parr, R1=arrSize
// Salida:  R0=sum
// R13=i, R14=sum
// =====================================================
(CALSUM)
// i = 0
@R13
M=0

// sum = 0
@R14
M=0

(CALSUM_LOOP)
// if (i == arrSize) goto CALSUM_END
@R13
D=M
@R1
D=D-M
@CALSUM_END
D;JEQ

// D = *(parr + i)
@R0
D=M          // D = parr
@R13
A=D+M        // A = parr + i
D=M          // D = *(parr+i)

// sum = sum + D
@R14
M=D+M

// i++
@R13
M=M+1

@CALSUM_LOOP
0;JMP

(CALSUM_END)
// return sum en R0
@R14
D=M
@R0
M=D

// volver a la dirección guardada en R15
@R15
A=M
0;JMP
```


## Bitácora de reflexión

### Actividad 09








