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

## Bitácora de aplicación 



## Bitácora de reflexión

