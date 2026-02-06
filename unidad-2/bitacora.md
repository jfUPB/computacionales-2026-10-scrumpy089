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
### Actividad 07

## Bitácora de aplicación 

### Actividad 08

## Bitácora de reflexión

### Actividad 09


