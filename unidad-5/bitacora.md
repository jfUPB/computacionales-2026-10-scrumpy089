# Unidad 5

[ACTIVIDADES](https://juanferfranco.github.io/computacionales-2026-10/units/unit5/)

## Bitácora de proceso de aprendizaje

### Actividad 01



### Actividad 02



### Actividad 03



### Actividad 04



### Actividad 05



## Bitácora de aplicación 

### Actividad 06

FASE 1
<details>
  <summary><b>ofApp.h</b></summary>

``` c++
#pragma once
#include "ofMain.h"
#include <vector>

class Particle {
public:
	virtual ~Particle() { }
	virtual void update(float dt) = 0;
	virtual void draw() = 0;
	virtual bool isDead() const = 0;
	virtual bool shouldExplode() const { return false; }
	virtual glm::vec2 getPosition() const { return glm::vec2(0, 0); }
	virtual ofColor getColor() const { return ofColor(255); }
};

class RisingParticle : public Particle {
protected:
	glm::vec2 position;
	glm::vec2 velocity;
	ofColor color;
	float lifetime;
	float age;
	bool exploded;

public:
	RisingParticle(const glm::vec2 & pos, const glm::vec2 & vel,
		const ofColor & col, float life)
		: position(pos)
		, velocity(vel)
		, color(col)
		, lifetime(life)
		, age(0)
		, exploded(false) { }

	void update(float dt) override {
		position += velocity * dt;
		age += dt;
		velocity.y += 9.8f * dt * 8;
		float explosionThreshold = ofGetHeight() * 0.15f + ofRandom(-30, 30);
		if (position.y <= explosionThreshold || age >= lifetime) {
			exploded = true;
		}
	}

	void draw() override {
		ofSetColor(color);
		ofDrawCircle(position, 10);
	}

	bool isDead() const override { return exploded; }
	bool shouldExplode() const override { return exploded; }
	glm::vec2 getPosition() const override { return position; }
	ofColor getColor() const override { return color; }
};

class ZigZagParticle : public Particle {
private:
	glm::vec2 position;
	glm::vec2 velocity;
	ofColor color;
	float lifetime;
	float age;
	bool exploded;
	float frequency;
	float amplitude;
	float baseX;

public:
	ZigZagParticle(const glm::vec2 & pos, const glm::vec2 & vel,
		const ofColor & col, float life)
		: position(pos)
		, velocity(vel)
		, color(col)
		, lifetime(life)
		, age(0)
		, exploded(false)
		, frequency(ofRandom(6.0f, 10.0f))
		, amplitude(ofRandom(40.0f, 90.0f))
		, baseX(pos.x) { }

	void update(float dt) override {
		age += dt;
		position.y += velocity.y * dt;
		position.x = baseX + sin(age * frequency) * amplitude;
		velocity.y += 9.8f * dt * 7;
		float explosionThreshold = ofGetHeight() * 0.18f + ofRandom(-20, 20);
		if (position.y <= explosionThreshold || age >= lifetime) {
			exploded = true;
		}
	}

	void draw() override {
		ofSetColor(color);
		ofDrawTriangle(position.x, position.y - 12,
			position.x - 8, position.y + 8,
			position.x + 8, position.y + 8);
	}

	bool isDead() const override { return exploded; }
	bool shouldExplode() const override { return exploded; }
	glm::vec2 getPosition() const override { return position; }
	ofColor getColor() const override { return color; }
};

class SpiralParticle : public Particle {
private:
	glm::vec2 center;
	glm::vec2 velocity;
	ofColor color;
	float lifetime;
	float age;
	bool exploded;
	float angle;
	float radius;
	float angularSpeed;

public:
	SpiralParticle(const glm::vec2 & pos, const glm::vec2 & vel,
		const ofColor & col, float life)
		: center(pos)
		, velocity(vel)
		, color(col)
		, lifetime(life)
		, age(0)
		, exploded(false)
		, angle(0)
		, radius(0)
		, angularSpeed(ofRandom(4.0f, 8.0f)) { }

	void update(float dt) override {
		age += dt;
		center += velocity * dt;
		velocity.y += 9.8f * dt * 6;
		angle += angularSpeed * dt;
		radius += 40.0f * dt;
		float explosionThreshold = ofGetHeight() * 0.2f + ofRandom(-25, 25);
		if (center.y <= explosionThreshold || age >= lifetime) {
			exploded = true;
		}
	}

	void draw() override {
		glm::vec2 drawPos = center + glm::vec2(cos(angle), sin(angle)) * radius;
		ofSetColor(color);
		ofDrawCircle(drawPos, 7);
		ofDrawCircle(center, 3);
	}

	bool isDead() const override { return exploded; }
	bool shouldExplode() const override { return exploded; }
	glm::vec2 getPosition() const override { return center; }
	ofColor getColor() const override { return color; }
};

class ExplosionParticle : public Particle {
protected:
	glm::vec2 position;
	glm::vec2 velocity;
	ofColor color;
	float age;
	float lifetime;
	float size;

public:
	ExplosionParticle(const glm::vec2 & pos, const glm::vec2 & vel,
		const ofColor & col, float life, float sz)
		: position(pos)
		, velocity(vel)
		, color(col)
		, age(0)
		, lifetime(life)
		, size(sz) { }

	void update(float dt) override {
		position += velocity * dt;
		age += dt;
		float alpha = ofMap(age, 0, lifetime, 255, 0, true);
		color.a = alpha;
	}

	bool isDead() const override { return age >= lifetime; }
};

class CircularExplosion : public ExplosionParticle {
public:
	CircularExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.2f, ofRandom(16, 32)) {
		float angle = ofRandom(0, TWO_PI);
		float speed = ofRandom(80, 200);
		velocity = glm::vec2(cos(angle), sin(angle)) * speed;
	}

	void draw() override {
		ofSetColor(color);
		ofDrawCircle(position, size);
	}
};

class RandomExplosion : public ExplosionParticle {
public:
	RandomExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.5f, ofRandom(16, 32)) {
		velocity = glm::vec2(ofRandom(-200, 200), ofRandom(-200, 200));
	}

	void draw() override {
		ofSetColor(color);
		ofDrawRectangle(position.x, position.y, size, size);
	}
};

class StarExplosion : public ExplosionParticle {
public:
	StarExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.3f, ofRandom(20, 40)) {
		float angle = ofRandom(0, TWO_PI);
		float speed = ofRandom(90, 180);
		velocity = glm::vec2(cos(angle), sin(angle)) * speed;
	}

	void draw() override {
		ofSetColor(color);
		int rays = 5;
		float outerRadius = size;
		float innerRadius = size * 0.5f;
		ofPushMatrix();
		ofTranslate(position);
		for (int i = 0; i < rays; i++) {
			float theta = ofMap(i, 0, rays, 0, TWO_PI);
			float xOuter = cos(theta) * outerRadius;
			float yOuter = sin(theta) * outerRadius;
			float xInner = cos(theta + PI / rays) * innerRadius;
			float yInner = sin(theta + PI / rays) * innerRadius;
			ofDrawLine(0, 0, xOuter, yOuter);
			ofDrawLine(xOuter, yOuter, xInner, yInner);
		}
		ofPopMatrix();
	}
};

class RingExplosion : public ExplosionParticle {
public:
	RingExplosion(const glm::vec2 & pos, const ofColor & col)
		: ExplosionParticle(pos, glm::vec2(0, 0), col, 1.4f, ofRandom(10, 18)) {
		float angle = ofRandom(0, TWO_PI);
		float speed = ofRandom(140, 220);
		velocity = glm::vec2(cos(angle), sin(angle)) * speed;
	}

	void draw() override {
		ofNoFill();
		ofSetColor(color);
		ofSetLineWidth(2);
		ofDrawCircle(position, size);
		ofFill();
	}
};

class ofApp : public ofBaseApp {
public:
	void setup();
	void update();
	void draw();
	void mousePressed(int x, int y, int button);
	void keyPressed(int key);

	std::vector<Particle *> particles;
	~ofApp();

private:
	void createRisingParticle();
	void createZigZagParticle();
	void createSpiralParticle();
};
```
</details>


<details>
  <summary><b>ofApp.cpp</b></summary>
  
``` c++
#include "ofApp.h"

void ofApp::setup() {
	ofSetFrameRate(60);
	ofBackground(0);
}

void ofApp::update() {
	float dt = ofGetLastFrameTime();

	for (int i = 0; i < particles.size(); i++) {
		particles[i]->update(dt);
	}

	for (int i = particles.size() - 1; i >= 0; i--) {
		if (particles[i]->shouldExplode()) {
			int explosionType = (int)ofRandom(4);
			int numParticles = (int)ofRandom(20, 30);

			for (int j = 0; j < numParticles; j++) {
				if (explosionType == 0) {
					particles.push_back(new CircularExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				} else if (explosionType == 1) {
					particles.push_back(new RandomExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				} else if (explosionType == 2) {
					particles.push_back(new StarExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				} else {
					particles.push_back(new RingExplosion(
						particles[i]->getPosition(), particles[i]->getColor()));
				}
			}

			delete particles[i];
			particles.erase(particles.begin() + i);
		} else if (particles[i]->isDead()) {
			delete particles[i];
			particles.erase(particles.begin() + i);
		}
	}
}

void ofApp::draw() {
	for (int i = 0; i < particles.size(); i++) {
		particles[i]->draw();
	}
}

void ofApp::createRisingParticle() {
	float minX = ofGetWidth() * 0.35f;
	float maxX = ofGetWidth() * 0.65f;
	float spawnX = ofRandom(minX, maxX);
	glm::vec2 pos(spawnX, ofGetHeight());
	glm::vec2 target(ofGetWidth() / 2.0f + ofRandom(-300, 300),
		ofGetHeight() * 0.10f + ofRandom(-30, 30));
	glm::vec2 direction = glm::normalize(target - pos);
	glm::vec2 vel = direction * ofRandom(250, 350);
	ofColor col;
	col.setHsb(ofRandom(255), 220, 255);
	float lifetime = ofRandom(1.5f, 3.5f);
	particles.push_back(new RisingParticle(pos, vel, col, lifetime));
}

void ofApp::createZigZagParticle() {
	float spawnX = ofRandom(ofGetWidth() * 0.25f, ofGetWidth() * 0.75f);
	glm::vec2 pos(spawnX, ofGetHeight());
	glm::vec2 vel(0, ofRandom(-320, -260));
	ofColor col;
	col.setHsb(ofRandom(255), 200, 255);
	float lifetime = ofRandom(1.6f, 3.0f);
	particles.push_back(new ZigZagParticle(pos, vel, col, lifetime));
}

void ofApp::createSpiralParticle() {
	float spawnX = ofRandom(ofGetWidth() * 0.3f, ofGetWidth() * 0.7f);
	glm::vec2 pos(spawnX, ofGetHeight());
	glm::vec2 target(ofGetWidth() / 2.0f + ofRandom(-200, 200),
		ofGetHeight() * 0.15f + ofRandom(-20, 20));
	glm::vec2 direction = glm::normalize(target - pos);
	glm::vec2 vel = direction * ofRandom(220, 300);
	ofColor col;
	col.setHsb(ofRandom(255), 180, 255);
	float lifetime = ofRandom(1.8f, 3.2f);
	particles.push_back(new SpiralParticle(pos, vel, col, lifetime));
}

void ofApp::mousePressed(int x, int y, int button) {
	int type = (int)ofRandom(3);

	if (type == 0) {
		createRisingParticle();
	} else if (type == 1) {
		createZigZagParticle();
	} else {
		createSpiralParticle();
	}
}

void ofApp::keyPressed(int key) {
	if (key == ' ') {
		for (int i = 0; i < 1000; i++) {
			int type = (int)ofRandom(3);
			if (type == 0) {
				createRisingParticle();
			} else if (type == 1) {
				createZigZagParticle();
			} else {
				createSpiralParticle();
			}
		}
	}

	if (key == '1') {
		createRisingParticle();
	}

	if (key == '2') {
		createZigZagParticle();
	}

	if (key == '3') {
		createSpiralParticle();
	}

	if (key == 's') {
		ofSaveScreen("screenshot_" + ofToString(ofGetFrameNum()) + ".png");
	}
}

ofApp::~ofApp() {
	for (int i = 0; i < particles.size(); i++) {
		delete particles[i];
	}
	particles.clear();
}

```
</details>

---

FASE 2

<details>
	<summary><b>Evidencia 1 — Herencia en memoria</b></summary>

<img width="964" height="662" alt="image" src="https://github.com/user-attachments/assets/8932898f-6ffe-4eea-83c6-516539656e70" /><br>

Se colocó un breakpoint justo después de crear una instancia de `SpiralParticle` y agregarla al vector `particles`, porque en ese momento el objeto ya está construido y puede inspeccionarse su organización en memoria

En el depurador se observa que el vector almacena un puntero de tipo `Particle*`, pero el objeto real es `SpiralParticle`. Al expandir el objeto se pueden ver atributos propios como `center`, `velocity`, `angle`, `radius` y `angularSpeed`

Esto demuestra la herencia en memoria porque el objeto puede manejarse mediante un puntero a la clase base `Particle`, pero conserva internamente la estructura y atributos específicos de la clase derivada `SpiralParticle`

</details>

---

<details>
	<summary><b>Evidencia 2 — La _vtable de tu nuevo tipo</b></summary>

<img width="976" height="577" alt="image" src="https://github.com/user-attachments/assets/744d5723-d54d-4c60-9c9d-a3646b9b00fb" />

<img width="899" height="552" alt="image" src="https://github.com/user-attachments/assets/8cc1c019-9c68-498c-9d0c-0dfbf65d94ae" />

Detuve la ejecución dentro del método `update()` en el momento en que se crean las partículas de explosión (`CircularExplosion` y `RingExplosion`). Elegí este punto porque en ese momento los objetos ya fueron creados dinámicamente y se encuentran almacenados en el vector `particles`, lo que permite inspeccionar su estructura interna

En las capturas se observa el vector `particles`, que almacena punteros de tipo `Particle*`, pero cuyos objetos reales corresponden a diferentes tipos derivados como CircularExplosion y RingExplosion, al expandir cada objeto en el depurador se puede observar el puntero _vfptr, que corresponde al puntero a la tabla virtual del objeto. Dentro de esta tabla aparecen las funciones virtuales que el objeto ejecutará:

Para CircularExplosion:

- destructor → CircularExplosion
- update → ExplosionParticle::update
- draw → CircularExplosion::draw
- isDead → ExplosionParticle::isDead
- shouldExplode → Particle::shouldExplode
- getPosition → Particle::getPosition
- getColor → Particle::getColor

Para RingExplosion:

- destructor → RingExplosion
- update → ExplosionParticle::update
- draw → RingExplosion::draw
- isDead → ExplosionParticle::isDead
- shouldExplode → Particle::shouldExplode
- getPosition → Particle::getPosition
- getColor → Particle::getColor

Se observa que ambas clases comparten varias funciones heredadas, pero la función `draw()` apunta a implementaciones distintas dependiendo del tipo real del objeto

Esta evidencia demuestra comprensión del polimorfismo porque muestra que aunque ambos objetos están almacenados como `Particle*`, cada uno posee su propia tabla virtual. La vtable muestra que el método virtual `draw()` apunta a` CircularExplosion::draw` en un caso y a `RingExplosion::draw` en el otro

</details>

---

<details>
	<summary><b>Evidencia 3 — Polimorfismo en tiempo de ejecución</b></summary>

<img width="1094" height="618" alt="image" src="https://github.com/user-attachments/assets/3a7cb858-636b-4b2f-9d8e-d726b9849ab4" />
<img width="958" height="739" alt="image" src="https://github.com/user-attachments/assets/3765b5bb-37e9-480b-95d1-727c6f91904e" />

Se colocó un breakpoint en la llamada al método virtual `draw()` dentro del ciclo que recorre el vector de partículas. Este punto fue elegido porque es donde ocurre el despacho dinámico de las funciones virtuales, permitiendo verificar qué implementación del método se ejecuta realmente

En la captura se observa que el objeto actual (`this`) corresponde a un objeto de tipo `StarExplosion`, al utilizar el depurador (Step Into (F10)), la ejecución entra directamente al método `StarExplosion::draw()` y no al método `draw()` de la clase base `Particle` ni de alguna otra clase derivada
También se puede observar en la ventana de variables que el objeto mantiene la jerarquía StarExplosion -> ExplosionParticle -> Particle

Esta evidencia demuestra el polimorfismo en tiempo de ejecución porque muestra que, aunque el objeto se maneja mediante un puntero `Particle*`, el método ejecutado corresponde al tipo dinámico real (`StarExplosion`). Esto confirma que el despacho dinámico funciona correctamente y que el programa ejecuta la implementación correcta del método virtual dependiendo del tipo real del objeto

</details>

---

<details>
	<summary><b>Evidencia 4 — Encapsulamiento en el contexto de herencia</b></summary>

<img width="807" height="692" alt="image" src="https://github.com/user-attachments/assets/852881f5-1488-43ce-9118-4dc3619b8216" />

Se colocó un breakpoint dentro del método `draw()` de la clase `RingExplosion`, ya que este punto permite inspeccionar el objeto actual (`this`) y observar los atributos heredados de las clases base

En la captura se observa la jerarquía del objeto:
- StarExplosion
	- ExplosionParticle
		- Particle

Dentro de esta jerarquía se pueden observar los atributos heredados desde `ExplosionParticle`, como:

- position
- velocity
- color
- age
- lifetime
- size

Estos atributos son accesibles desde `StarExplosion` porque fueron declarados como `protected`

Esta evidencia demuestra comprensión del encapsulamiento porque muestra que las subclases pueden acceder a los atributos heredados definidos como `protected` o `public`, mientras que los atributos privados no serían accesibles directamente desde la subclase

</details>

---

<details>
	<summary><b>Evidencia 5 — Ciclo de vida completo de tu partícula</b></summary>

<img width="1193" height="758" alt="image" src="https://github.com/user-attachments/assets/90ffaf69-18be-4817-9eaa-b6430eaf473a" />
<img width="863" height="470" alt="image" src="https://github.com/user-attachments/assets/b50af65b-c62d-4826-962f-1137d57165a3" />
<img width="937" height="397" alt="image" src="https://github.com/user-attachments/assets/fcdb83b6-b194-40d5-a47e-f2d566fc3883" />
<img width="1085" height="414" alt="image" src="https://github.com/user-attachments/assets/b0bef5ce-6def-4db1-9e6f-6ff40e4a0192" />

Se colocaron breakpoints en tres momentos del ciclo de vida de una partícula: durante su creación cuando se agrega al vector `particles`, durante su actualización en el método `update()`, y durante su eliminación cuando se detecta que ha muerto y se libera su memoria

En la primera captura se observa la creación de la partícula mediante `new` y su inserción en el vector particles. El depurador muestra el tamaño del vector y el tipo dinámico del objeto creado

En la segunda captura se observa el objeto durante su ejecución en el método `update()`. Se puede ver cómo cambian variables como la posición, la velocidad y la edad (`age`), demostrando que el objeto mantiene estado y evoluciona en el tiempo

En las ultimas capturas se observa cómo el objeto es eliminado cuando cumple su condición de muerte (`isDead()`), donde se ejecuta `delete` y posteriormente se elimina del vector. El tamaño del vector disminuye y el puntero deja de existir

Esta evidencia demuestra comprensión del ciclo de vida de los objetos dinámicos porque muestra las tres etapas fundamentales: creación en memoria dinámica, modificación del estado durante la ejecución y liberación correcta de memoria al eliminar el objeto. Esto confirma que la implementación gestiona correctamente la memoria y evita objetos persistentes innecesarios
	
</details>

---

<details>
	<summary><b>Evidencia 6 — Sin fugas de memoria</b></summary>

<img width="1427" height="797" alt="image" src="https://github.com/user-attachments/assets/1bc550ec-e35e-4920-937f-1f7573692d0c" />
<img width="1455" height="846" alt="image" src="https://github.com/user-attachments/assets/c9c1e0b3-4f1e-4354-9368-4e5ee6996568" />
<img width="1451" height="672" alt="image" src="https://github.com/user-attachments/assets/95865463-419b-4df4-bf35-28e44ad63d9d" />

Se colocaron breakpoints en las instrucciones `delete particles[i];` y `particles.erase(particles.begin() + i);`, porque estas dos líneas son las responsables de liberar la memoria dinámica y retirar el puntero del contenedor

En la primera captura se observa que el vector particles todavía contiene un puntero válido a la partícula que va a eliminarse. En ese momento el tamaño del vector incluye todavía ese elemento, luego, al ejecutar `delete particles[i];`, se libera la memoria dinámica ocupada por el objeto. Después, en la instrucción `particles.erase(particles.begin() + i);`, el puntero correspondiente se elimina del vector. En la ultima captura se observa que el tamaño del vector disminuye y que el elemento ya no se encuentra almacenado en la posición anterior

Esta evidencia demuestra que no hay fuga de memoria porque la implementación realiza correctamente las dos acciones necesarias al destruir una partícula: primero libera la memoria reservada dinámicamente con `delete` y luego elimina el puntero del vector con `erase`. Si faltara cualquiera de estas dos operaciones, habría un problema: o fuga de memoria, o un puntero colgante dentro del contenedor

</details>

---

<details>
	<summary><b>Evidencia 7 — Prueba de condición límite</b></summary>

<img width="434" height="270" alt="image" src="https://github.com/user-attachments/assets/f983ebf3-5371-4e95-a984-6062737a44ff" />
<img width="946" height="508" alt="image" src="https://github.com/user-attachments/assets/9fc6c713-e442-4aeb-93e2-0f79be19cf89" />
<img width="933" height="347" alt="image" src="https://github.com/user-attachments/assets/6c388125-32cc-4d38-b986-77b5c22354d3" />
<img width="1230" height="510" alt="image" src="https://github.com/user-attachments/assets/54437113-f86e-49bd-9301-d2743524b7de" />
<img width="1181" height="460" alt="image" src="https://github.com/user-attachments/assets/2015b90c-7043-4835-b833-a5924ac697e5" />

Se diseñó una prueba de estrés generando una gran cantidad de partículas al mismo tiempo mediante la tecla espacio, la cual crea aproximadamente 2000 partículas. Esta prueba se eligió para verificar si el sistema puede manejar una gran cantidad de objetos dinámicos sin fallos o problemas de memoria

Se colocó un breakpoint durante el proceso de actualización del vector de partículas para observar el tamaño del vector y verificar el comportamiento del sistema bajo carga alta

En la captura se observa que el vector particles contiene una gran cantidad de elementos (más de 2000), a pesar de esta cantidad, el sistema continúa ejecutando correctamente los métodos `update()` y `draw()` sin errores, también se puede observar que conforme las partículas mueren, el tamaño del vector disminuye progresivamente, lo que demuestra que los objetos se eliminan correctamente

Esta prueba demuestra que el sistema maneja correctamente una condición límite donde se crean muchas partículas simultáneamente. La aplicación continúa funcionando correctamente, las partículas se actualizan sin errores y la memoria se libera conforme las partículas mueren. Esto demuestra que la implementación es robusta y maneja correctamente escenarios extremos
	
</details>

## Bitácora de reflexión

### Actividad 07
