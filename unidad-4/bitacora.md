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
	Node * next;

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
	Node * front;
	Node * rear;
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
	Node * newNode = new Node(x, y, radius, color, opacity);

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

	Node * temp = front;
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

	Node * current = strokes.front;
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

	ofSetColor(255);
	ofDrawBitmapString("maxSize = " + ofToString(strokes.maxSize), 20, 20);
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

## Bitácora de reflexión

### Actividad 04





