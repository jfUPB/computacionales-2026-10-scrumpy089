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
### Actividad 06
### Actividad 07

## Bitácora de aplicación 

### Actividad 08

## Bitácora de reflexión

### Actividad 09

