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

	if (ofGetMousePressed()) {
		float x = ofGetMouseX();
		float y = ofGetMouseY();
		float radius = ofRandom(8, 22);

		ofColor color;
		color.setHsb(ofRandom(255), 200, 255);

		float opacity = 180;

		strokes.enqueue(x, y, radius, color, opacity);
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
		float alpha = ofMap(index, 0, strokes.size - 1, 40, 255, true);

		ofColor drawColor = current->color;
		drawColor.a = alpha;

		ofSetColor(drawColor);
		ofDrawCircle(current->x, current->y, current->radius);

		current->next;
		index++;
	}

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

	if (ofGetMousePressed()) {
		float x = ofGetMouseX();
		float y = ofGetMouseY();
		float radius = ofRandom(8, 22);

		ofColor color;
		color.setHsb(ofRandom(255), 200, 255);

		float opacity = 180;

		strokes.enqueue(x, y, radius, color, opacity);
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
		float alpha = ofMap(index, 0, strokes.size - 1, 40, 255, true);

		ofColor drawColor = current->color;
		drawColor.a = alpha;

		ofSetColor(drawColor);
		ofDrawCircle(current->x, current->y, current->radius);

		current->next;
		index++;
	}

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




