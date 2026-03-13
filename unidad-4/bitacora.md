# Unidad 4

[ACTIVIDADES](https://juanferfranco.github.io/computacionales-2026-10/units/unit4/)

## Bitácora de proceso de aprendizaje

### Actividad 01

`ofApp.h` se declaran las clases, `ofApp.cpp` se definen las clases

### Actividad 02



## Bitácora de aplicación 

### Actividad 03

<details>
  <summary><b>ofApp.h</b></summary>

``` c++
#pragma once
#include "ofMain.h"

// Nodo de la cola
struct Node {
	float x, y;
	float radius;
	ofColor color;
	float opacity;
	Node* next;

	Node(float _x, float _y, float _radius, ofColor _color, float _opacity)
		: x(_x)
		, y(_y)
		, radius(_radius)
		, color(_color)
		, opacity(_opacity)
		, next(nullptr) {
	}
};

// Implementación manual de una cola (FIFO)
class BrushQueue {
public:
	Node* front;
	Node* rear;
	int size;
	int maxSize;

	BrushQueue(int _maxSize);
	~BrushQueue();

	void enqueue(float x, float y, float radius, ofColor color, float opacity);
	void dequeue();
	void clear();
	bool isEmpty();
};

// Constructor
BrushQueue::BrushQueue(int _maxSize)
	: front(nullptr)
	, rear(nullptr)
	, size(0)
	, maxSize(_maxSize) { }

// Destructor
BrushQueue::~BrushQueue() {
	clear();
}

// Agregar un nuevo trazo al final de la cola
void BrushQueue::enqueue(float x, float y, float radius, ofColor color, float opacity) {
	Node* newNode = new Node(x, y, radius, color, opacity);

	if (isEmpty()) {
		front = rear = newNode;
	} else {
		rear->next = newNode;
		rear = newNode;
	}

	size++;

	if (size > maxSize) {
		dequeue();
	}
}

// Eliminar el trazo más antiguo
void BrushQueue::dequeue() {
	if (isEmpty()) {

		return;
	}

	Node* temp = front;
	front = front->next;
	delete temp;
	size--;

	if (front == nullptr) {
		rear = nullptr;
	}
}

// Eliminar todos los trazos
void BrushQueue::clear() {
	while (!isEmpty()) {
		dequeue();
	}
}

// Verificar si la cola está vacía
bool BrushQueue::isEmpty() {
	return front == nullptr;
}

class ofApp : public ofBaseApp {
public:
	BrushQueue strokes; // Cola de trazos
	float backgroundHue = 0;

	ofApp()
		: strokes(50) { } // Tamaño máximo de la cola

	void setup();
	void update();
	void draw();
	void keyPressed(int key);
};

```

</details>


<details>
  <summary><b> ofApp.cpp </b></summary>

``` c++
#include "ofApp.h"

//--------------------------------------------------------------
void ofApp::setup() {
	ofBackground(0);
	//ofSetCircleResolution(40);
}

//--------------------------------------------------------------
void ofApp::update() {
	backgroundHue += 0.2;
	if (backgroundHue > 255) backgroundHue = 0;

	// TODO: agregar un nuevo trazo si el mouse está presionado.
	// Usa strokes.enqueue(x, y, radius, color, opacity);

	static int lastX = -1;
	static int lastY = -1;

	int x = ofGetMouseX();
	int y = ofGetMouseY();

	if (x != lastX || y != lastY) {
		float radius = ofRandom(8, 22);

		ofColor color;
		color.setHsb(ofRandom(255), 200, 255);

		float opacity = 180;

		strokes.enqueue(x, y, radius, color, opacity);

		lastX = x;
		lastY = y;
	}
}

//--------------------------------------------------------------
void ofApp::draw() {
	// Fondo con gradiente dinámico
	ofColor color1, color2;
	color1.setHsb(backgroundHue, 150, 240);
	color2.setHsb(fmod(backgroundHue + 128, 255), 150, 240);
	ofBackgroundGradient(color1, color2, OF_GRADIENT_LINEAR);

	// TODO: dibujar los trazos almacenados en la cola.
	// Recorre los nodos desde strokes.front hasta nullptr y usa ofDrawCircle().

	Node* current = strokes.front;
	int index = 0;

	while (current != nullptr) {
		float alpha;

		if (strokes.size <= 1) {
			alpha = 255;
		} else {
			alpha = ofMap(index, 0, strokes.size - 1, 40, 255, true);
		}

		ofColor drawColor = current->color;
		drawColor.a = alpha;

		ofSetColor(drawColor);
		ofDrawCircle(current->x, current->y, current->radius);

		current = current->next;
		index++;
	}

	//ofSetColor(255);
	//ofDrawBitmapString("maxSize = " + ofToString(strokes.maxSize), 20, 20);
}

//--------------------------------------------------------------
void ofApp::keyPressed(int key) {
	if (key == 'c') {
		// TODO: limpiar la cola de trazos.

		strokes.clear();
	}

	else if (key == 'a') {
		// TODO: alternar entre 50 y 100 trazos.

		if (strokes.maxSize == 50) {
			strokes.maxSize = 100;
		} else {
			strokes.maxSize = 50;
		}
		while (strokes.size > strokes.maxSize) {
			strokes.dequeue();
		}

	} else if (key == 's') {
		// TODO: guardar el frame actual.

		ofSaveFrame();
	}
}
```
  
</details>

#### Evidencia 1: inserción del primer nodo en una cola vacía (enqueue)

<img width="1161" height="648" alt="image" src="https://github.com/user-attachments/assets/fd93fc4b-d4b7-4e73-b19f-4ad7a4e5f62c" /><br>

En la captura se observa que al insertar el primer nodo de la cola, los punteros `front` y `rear` apuntan a la misma dirección de memoria, correspondiente al nodo recién creado (`newNode`) También se observa que el puntero `next` es nulo, lo que indica que no existen más elementos en la cola
Esto demuestra que la función enqueue() maneja correctamente el caso en que la cola está vacía, estableciendo el primer nodo como el frente y el final de la estructura FIFO

#### Evidencia 2: mantenimiento del orden FIFO al insertar más nodos (enqueue)

Size: 1
<img width="1169" height="654" alt="image" src="https://github.com/user-attachments/assets/b7106415-f636-4ca3-a3ee-16298b66035a" /><br>

Size: 2
<img width="1157" height="651" alt="image" src="https://github.com/user-attachments/assets/d5a66d45-e952-4dee-83fb-680ecc94d01e" /><br>

Size: 3
<img width="1155" height="645" alt="image" src="https://github.com/user-attachments/assets/89e58afc-971f-451c-af5d-c30146de442f" /><br>

Size: 4
<img width="1161" height="648" alt="image" src="https://github.com/user-attachments/assets/3a096f79-07be-42c4-a5dc-7203cc699598" /><br>

Se observa que cada nuevo nodo se agrega al final de la estructura mediante el puntero `rear`, mientras que `front` sigue apuntando al primer nodo insertado. Además, se evidencia que cada nodo mantiene un enlace al siguiente mediante next
Esto demuestra que la cola mantiene el orden FIFO, ya que los nodos más antiguos permanecen en el frente mientras los nuevos se agregan al final

#### Evidencia 3: comportamiento de eliminación y prevención de fugas (dequeue)

Antes
<img width="1161" height="579" alt="image" src="https://github.com/user-attachments/assets/ee703a8b-8c2d-424d-9866-4dfcf9575a89" /><br>

Despues
<img width="1157" height="582" alt="image" src="https://github.com/user-attachments/assets/fececfae-f92c-4b53-9005-533b831da3c8" /><br>

Se observa que la variable `temp` almacena el nodo más antiguo de la cola. Luego el puntero `front` se actualiza al siguiente nodo y el nodo antiguo es eliminado mediante `delete`
Esto demuestra que la eliminación respeta el orden FIFO y que la memoria del nodo eliminado se libera correctamente, evitando fugas de memoria

#### Evidencia 4: control del tamaño máximo de la cola (maxSize)

<img width="1161" height="620" alt="image" src="https://github.com/user-attachments/assets/d679796f-22c7-4328-95cd-56c7f52eecd9" /><br>

<img width="1160" height="626" alt="image" src="https://github.com/user-attachments/assets/884f6736-fd5b-4ce5-9295-010b0d4c74b4" /><br>

En la captura se observa que el tamaño de la cola supera el valor máximo permitido (maxSize). En ese momento el programa ejecuta `dequeue()` para eliminar el nodo más antiguo
Esto demuestra que el programa controla correctamente el tamaño máximo de la cola y evita que crezca indefinidamente


#### Evidencia 5: recorrido de la cola sin destruirla (draw)

Primera pasada
<img width="1155" height="588" alt="image" src="https://github.com/user-attachments/assets/6ed1ab2c-813a-44dd-b682-fc7fd0fdc1f9" /><br>

Segunda pasada
<img width="1154" height="622" alt="image" src="https://github.com/user-attachments/assets/026ce5ea-10c3-4895-a4e8-c79f41201a5c" /><br>

cuarta pasada
<img width="1158" height="505" alt="image" src="https://github.com/user-attachments/assets/8effd1c6-6cbd-4f6c-97b3-b570650eb7fc" /><br>

Se observa que el puntero `current` recorre todos los nodos de la cola, sin embargo, los punteros `front` y `rear` permanecen sin cambios y el tamaño de la cola no se modifica
Esto demuestra que la función `draw()` solo recorre la estructura para dibujar los trazos, sin modificar ni destruir la cola


#### Evidencia 6: limpieza total de la memoria (clear)

Antes
<img width="1154" height="527" alt="image" src="https://github.com/user-attachments/assets/463f432f-fec9-4c7b-8962-880e66297d51" /><br>
<img width="1153" height="441" alt="image" src="https://github.com/user-attachments/assets/6806869f-c049-4622-bca2-838cb261e625" /><br>

Despues
<img width="1156" height="464" alt="image" src="https://github.com/user-attachments/assets/ecd7fa33-2c51-4749-a6ff-f71c86f1c204" /><br>

Se observa que la función `clear()` elimina todos los nodos llamando repetidamente a `dequeue()` hasta que la cola queda vacía
Esto demuestra que la estructura libera completamente la memoria utilizada y deja la cola en estado inicial, evitando fugas de memoria


## Bitácora de reflexión

### Actividad 04






