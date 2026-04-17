# Unidad 6

[ACTIVIDADES](https://juanferfranco.github.io/computacionales-2026-10/units/unit6/)

## Bitácora de proceso de aprendizaje

### Actividad 01

¿Qué observas en la aplicación al presionar las teclas a, r, s, n?

la letra `a` atrae las particulas donde esta el cursor, `r` repele las particulas al rededor del cursor, `s` detiene todo moviento de particulas y `n` las devuelve a su estado normal 

¿Qué diferencias notas entre los tipos de partículas?

las rojas tienen una mayor cantidad que las otras y en general son mas pequeñas, las verdes son las mas rapidas y las azules son mas grandes que las otras y son mas lentas

Formula una hipótesis: ¿Cómo crees que el código organiza la comunicación entre las teclas, las partículas y el cambio de comportamiento?


### Actividad 02

<details>
<summary><b>Investigación del patrón Observer</b></summary>
  
#### Coloca un breakpoint dentro de `Subject::notify`. Cuando se dispare al presionar una tecla, observa en el depurador el vector `observers`: ¿Cuántos elementos tiene? ¿Qué tipo de objetos son? ¿A qué direcciones de memoria apuntan?

<img width="1355" height="826" alt="image" src="https://github.com/user-attachments/assets/aef7e221-6607-4382-b906-f767c7505361" />

  tiene 115 elementos que puede referirse a las 100 particulas tipo `star`, 5 particulas tipo `shooting_star` y 10 particulas tipo `planet`

  el tipo almacenado es `Observer*`, pero los objetos reales que apunta cada puntero son instancias de `Particle`, porque `Particle` hereda de `Observer`

  Cada entrada del vector guarda una dirección distinta, porque cada partícula se crea dinámicamente con `new` dentro de `ParticleFactory::createParticle()`

#### Ahora coloca un breakpoint en `Particle::onNotify`. ¿Cuál es la dirección de memoria del objeto `this` que recibe la llamada? ¿Puedes encontrar esa misma dirección en el vector `observers` que observaste antes? ¿Qué concluyes sobre cómo el `Subject` sabe a quién notificar?

<img width="1313" height="548" alt="image" src="https://github.com/user-attachments/assets/c40fe29a-4d5e-4de7-b6b4-a1543bb41123" />

  la dirección de memoria del objeto `this` es 0x0000024078b0e6e0 
  
  Sí, en la primera captura (vector `observers`) aparece exactamente qie [0] es 0x0000024078b0e6e0 

  El `Subject` sabe a quién notificar porque mantiene un vector de punteros a los observadores. Cada puntero representa la dirección de memoria de un objeto Particle. Al recorrer este vector y llamar `observer->onNotify(event)`, el `Subject` invoca directamente el método sobre cada objeto usando su dirección en memoria, lo que permite la comunicación sin conocer detalles internos de cada observador.
</details> 



<details>
<summary><b>Investigación del patrón State</b></summary>
  
#### Coloca un breakpoint en `Particle::setState`. Observa en memoria el puntero `state` antes y después de la asignación: ¿Qué dirección tenía? ¿Qué dirección tiene ahora? ¿qué le pasó al objeto de estado anterior?

Antes
<img width="1187" height="521" alt="image" src="https://github.com/user-attachments/assets/ce1e1244-f40d-4914-821c-2281553d17ef" />

Despues
<img width="1187" height="475" alt="image" src="https://github.com/user-attachments/assets/9c300cdc-e395-4feb-9c23-b3382d2a8b91" />

`state` contaba con la direccion de memoria `0x000001b32bda52a0` correspondiente a un objeto `NormalState`, al mismo tiempo, el parámetro `newState` apuntaba a la dirección `0x000001b338268a30` correspondiente al objeto `AttrackState`, despues `state` cambió la direccion por la de `newState`

Antes de reemplazarlo, el estado anterior ejecutó `onExit(this)` y luego fue destruido con `delete state`;. Esto significa que el objeto `NormalState` que estaba en la dirección `0x000001b32bda52a0` fue liberado de memoria y dejó de existir. Luego el puntero state fue actualizado para apuntar al nuevo objeto de estado

#### En la unidad anterior estudiaste la `_vtable` y el despacho dinámico. Ahora conéctalo con el patrón `State`: coloca breakpoints en `NormalState::update` y en `AttractState::update`. Presiona n y luego a. ¿A cuál llega el depurador primero en cada caso? ¿Cómo demuestra esto que el patrón State usa polimorfismo para cambiar el comportamiento en tiempo de ejecución?

<img width="1478" height="439" alt="image" src="https://github.com/user-attachments/assets/0a31521a-d3de-4d6a-b16b-6556ee0226d8" />

  sin presionar ninguna tecla incluso cuando el programa apenas esta iniciando el depurador para en `NormalState::update` 

  al presionar la tecla `a` el depurador llega a `AttractState::update`

esto demuestra que el patrón `State` utiliza polimorfismo porque la llamada `state->update(this)` es siempre la misma, pero el método que se ejecuta cambia según el objeto al que apunta `state`. Al presionar `n`, el puntero `state` apunta a un objeto `NormalState` y se ejecuta `NormalState::update`. Al presionar `a`, el puntero cambia a un objeto `AttractState` y se ejecuta `AttractState::update`
</details>


<details>
<summary><b>Investigación del patrón Factory</b></summary>

####  Coloca un breakpoint en ParticleFactory::createParticle. Observa el parámetro type y el objeto Particle* que se retorna. Luego, en ofApp::setup, observa el vector particles justo después de las llamadas a la factory. ¿Qué relación tienen las direcciones de los objetos creados con los elementos del vector?

<img width="1367" height="773" alt="image" src="https://github.com/user-attachments/assets/f3b265b2-fa57-4126-b93e-e19e46d833bb" />

<img width="1413" height="740" alt="image" src="https://github.com/user-attachments/assets/b07a2c24-9913-48f4-81d6-6a708e7ee212" />

Al inspeccionar ParticleFactory::createParticle, se observa que la factory crea un objeto Particle dinámicamente con new y retorna un puntero a su dirección de memoria. Luego, en ofApp::setup, ese puntero se guarda en la variable p y se inserta en el vector particles mediante push_back(p). Por tanto, los elementos del vector particles son exactamente las mismas direcciones de memoria de los objetos creados por la factory. El vector no almacena copias de las partículas, sino punteros a ellas. Esto demuestra que la aplicación organiza sus partículas creadas dinámicamente dentro de una colección de punteros para poder actualizarlas y dibujarlas después

</details>

### Actividad 03



## Bitácora de aplicación 

### Actividad 04

<details>
<summary><b>Fase 1</b></summary><br>



---

<details>
<summary><b>ofApp.h</b></summary>

``` cpp
#pragma once

#include "ofMain.h"
#include <string>
#include <vector>

class Observer {
public:
	virtual ~Observer() = default;
	virtual void onNotify(const std::string & event) = 0;
};

class Subject {
public:
	void addObserver(Observer * observer);
	void removeObserver(Observer * observer);

protected:
	void notify(const std::string & event);

private:
	std::vector<Observer *> observers;
};

class Particle;

class State {
public:
	virtual ~State() = default;
	virtual void update(Particle * particle) = 0;
	virtual void onEnter(Particle * particle) { }
	virtual void onExit(Particle * particle) { }
};

class Particle : public Observer {
public:
	Particle();
	~Particle() override;

	Particle(const Particle &) = delete;
	Particle & operator=(const Particle &) = delete;

	void update();
	void draw();
	void onNotify(const std::string & event) override;

	void setState(State * newState);

	ofVec2f position;
	ofVec2f velocity;
	float size;
	ofColor color;

private:
	void keepInsideWindow();
	State * state;
};

class NormalState : public State {
public:
	void update(Particle * particle) override;
	void onEnter(Particle * particle) override;
};

class AttractState : public State {
public:
	void update(Particle * particle) override;
};

class RepelState : public State {
public:
	void update(Particle * particle) override;
};

class StopState : public State {
public:
	void update(Particle * particle) override;
};

class OrbitState : public State {
public:
	void update(Particle * particle) override;
};

class ParticleFactory {
public:
	static Particle * createParticle(const std::string & type);
};

class ofApp : public ofBaseApp, public Subject {
public:
	~ofApp() override;
	void setup() override;
	void update() override;
	void draw() override;
	void keyPressed(int key) override;

private:
	std::vector<Particle *> particles;
};

```
</details>

---

<details>
<summary><b>ofApp.cpp</b></summary>

``` cpp
#include "ofApp.h"
#include <algorithm>

void Subject::addObserver(Observer * observer) {
	if (!observer) return;
	if (std::find(observers.begin(), observers.end(), observer) == observers.end()) {
		observers.push_back(observer);
	}
}

void Subject::removeObserver(Observer * observer) {
	if (!observer) return;
	observers.erase(std::remove(observers.begin(), observers.end(), observer), observers.end());
}

void Subject::notify(const std::string & event) {
	for (Observer * observer : observers) {
		observer->onNotify(event);
	}
}

Particle::Particle()
	: state(nullptr) {
	position = ofVec2f(ofRandomWidth(), ofRandomHeight());
	velocity = ofVec2f(ofRandom(-0.5f, 0.5f), ofRandom(-0.5f, 0.5f));
	size = ofRandom(2.0f, 5.0f);
	color = ofColor(255);

	state = new NormalState();
	state->onEnter(this);
}

Particle::~Particle() {
	if (state) {
		state->onExit(this);
		delete state;
		state = nullptr;
	}
}

void Particle::setState(State * newState) {
	if (state) {
		state->onExit(this);
		delete state;
	}
	state = newState;
	if (state) {
		state->onEnter(this);
	}
}

void Particle::update() {
	if (state) {
		state->update(this);
	}
	keepInsideWindow();
}

void Particle::draw() {
	ofPushStyle();
	ofSetColor(color);
	ofDrawCircle(position, size);
	ofPopStyle();
}

void Particle::onNotify(const std::string & event) {
	if (event == "attract") {
		setState(new AttractState());
	} else if (event == "repel") {
		setState(new RepelState());
	} else if (event == "stop") {
		setState(new StopState());
	} else if (event == "normal") {
		setState(new NormalState());
	} else if (event == "orbit") {
		setState(new OrbitState());
	}
}

void Particle::keepInsideWindow() {
	const float W = static_cast<float>(ofGetWidth());
	const float H = static_cast<float>(ofGetHeight());

	if (position.x < 0.0f) {
		position.x = 0.0f;
		velocity.x *= -1.0f;
	} else if (position.x > W) {
		position.x = W;
		velocity.x *= -1.0f;
	}
	if (position.y < 0.0f) {
		position.y = 0.0f;
		velocity.y *= -1.0f;
	} else if (position.y > H) {
		position.y = H;
		velocity.y *= -1.0f;
	}
}

void NormalState::onEnter(Particle * particle) {
	particle->velocity.set(ofRandom(-0.5f, 0.5f), ofRandom(-0.5f, 0.5f));
}

void NormalState::update(Particle * particle) {
	particle->position += particle->velocity;
}

static void steer(Particle * particle, const ofVec2f & toward, float accel, float vmax, float posScale) {
	ofVec2f dir = toward - particle->position;
	float len = dir.length();
	if (len > 1e-6f) {
		dir /= len;
		particle->velocity += dir * accel;
	}
	particle->velocity.limit(vmax);
	particle->position += particle->velocity * posScale;
}

void AttractState::update(Particle * particle) {
	ofVec2f mouse(ofGetMouseX(), ofGetMouseY());
	steer(particle, mouse, 0.05f, 3.0f, 0.2f);
}

void RepelState::update(Particle * particle) {
	ofVec2f mouse(ofGetMouseX(), ofGetMouseY());
	ofVec2f away = particle->position - mouse;
	float len = away.length();
	if (len > 1e-6f) {
		away /= len;
		particle->velocity += away * 0.05f;
	}
	particle->velocity.limit(3.0f);
	particle->position += particle->velocity * 0.2f;
}

void StopState::update(Particle * particle) {
	particle->velocity *= 0.80f;
	if (particle->velocity.lengthSquared() < 1e-4f) {
		particle->velocity.set(0.0f, 0.0f);
	}
	particle->position += particle->velocity;
}

void OrbitState::update(Particle * particle) {
	ofVec2f center(ofGetWidth() * 0.5f, ofGetHeight() * 0.5f);
	ofVec2f dir = particle->position - center;
	float len = dir.length();
	if (len < 1e-6f) {
		dir.set(1.0f, 0.0f);
		len = 1.0f;
	}
	dir /= len;
	ofVec2f tangent(-dir.y, dir.x);
	particle->velocity = tangent * 2.5f;
	particle->position += particle->velocity;
}

Particle * ParticleFactory::createParticle(const std::string & type) {
	Particle * particle = new Particle();
	if (type == "star") {
		particle->size = ofRandom(2.0f, 4.0f);
		particle->color = ofColor(255, 0, 0);
	} else if (type == "shooting_star") {
		particle->size = ofRandom(3.0f, 6.0f);
		particle->color = ofColor(0, 255, 0);
		particle->velocity *= 3.0f;
	} else if (type == "planet") {
		particle->size = ofRandom(5.0f, 8.0f);
		particle->color = ofColor(0, 0, 255);
	} else if (type == "comet") {
		particle->size = ofRandom(6.0f, 7.0f);
		particle->color = ofColor(255, 140, 0);
		particle->velocity *= 6.0f;
	}
	return particle;
}

ofApp::~ofApp() {
	for (Particle * p : particles) {
		removeObserver(p);
		delete p;
	}
	particles.clear();
}

void ofApp::setup() {
	ofBackground(0);
	particles.reserve(100 + 5 + 10 + 8);

	for (int i = 0; i < 100; ++i) {
		Particle * p = ParticleFactory::createParticle("star");
		particles.push_back(p);
		addObserver(p);
	}
	for (int i = 0; i < 5; ++i) {
		Particle * p = ParticleFactory::createParticle("shooting_star");
		particles.push_back(p);
		addObserver(p);
	}
	for (int i = 0; i < 10; ++i) {
		Particle * p = ParticleFactory::createParticle("planet");
		particles.push_back(p);
		addObserver(p);
	}
	for (int i = 0; i < 8; ++i) {
		Particle * p = ParticleFactory::createParticle("comet");
		particles.push_back(p);
		addObserver(p);
	}
}

void ofApp::update() {
	for (Particle * p : particles) {
		p->update();
	}
}

void ofApp::draw() {
	for (Particle * p : particles) {
		p->draw();
	}
}

void ofApp::keyPressed(int key) {
	switch (key) {
	case 's':
		notify("stop");
		break;
	case 'a':
		notify("attract");
		break;
	case 'r':
		notify("repel");
		break;
	case 'n':
		notify("normal");
		break;
	case 'o':
		notify("orbit");
		break;
	default:
		break;
	}
}

```
</details>

---

</details>

<details>
<summary><b>Fase 2</b></summary>

---

<details>
<summary><b>Evidencia 1 — Tu nueva partícula en la Factory</b></summary>
  
<img width="1348" height="774" alt="image" src="https://github.com/user-attachments/assets/9ab99e2e-7c19-4901-b956-4b4d3432f799" />

Ese punto es revelador porque ahí se ve si la factory realmente reconoció el nuevo tipo y si configuró correctamente el objeto recién creado

En el depurador se observa que el parámetro `type` vale `"comet"`, por lo que se ejecuta la nueva rama de la factory. El puntero `particle` apunta a un objeto recién creado en memoria, y sus atributos muestran la configuración diferencial del nuevo tipo: tamaño mayor, color naranja y una velocidad amplificada respecto a una partícula normal

La captura evidencia que la creación del objeto no ocurre dispersa por el programa, sino centralizada en `ParticleFactory::createParticle`. Además, muestra que la nueva partícula no solo existe nominalmente, sino que realmente sale de la factory con propiedades propias

</details>  

---

<details>
<summary><b>Evidencia 2 — Tu nuevo estado en la _vtable</b></summary><br>

<img width="482" height="155" alt="image" src="https://github.com/user-attachments/assets/e03a7bc2-5b2c-48b3-8841-4030f5384c5a" />


NormalState
<img width="1175" height="547" alt="image" src="https://github.com/user-attachments/assets/fdf10cc7-07f9-4005-a0cc-64812a51d791" />


OrbitState
<img width="1142" height="614" alt="image" src="https://github.com/user-attachments/assets/7bbe35d3-2ccc-4cd0-beb7-9a0e6f475f0f" />


La comparación muestra que ambos objetos comparten la misma interfaz base `State`, pero no comparten la misma tabla virtual, en `NormalState`, sus metodos sobreescritos/propios son el destructor, `NormalState::update` y `NormalState::onEnter`, mientras que en `OrbitState`, con las implementaciones del nuevo estado, la `vtable` quedaria con el destructor, `OrbitState::update`, `State::onEnter` y `State::onExit`

Esto demuestra que el patrón State usa polimorfismo porque el puntero state sigue siendo de tipo base `State*`, pero al cambiar el objeto al que apunta también cambia su `vtable`. Gracias a eso, la misma llamada:

``` cpp
state->update(this);
```
ejecuta una función distinta según el estado activo

</details>

---

<details>
<summary><b>Evidencia 3 — La cadena Observer → State completa</b></summary>

Esos puntos son reveladores porque permiten seguir el recorrido completo del evento desde la entrada del usuario hasta el cambio del estado interno

en `keyPressed`: `key = 'o'`
<img width="975" height="510" alt="image" src="https://github.com/user-attachments/assets/e8eff64b-e1fd-4d70-af44-044c408eae46" />

en `notify`: `event = "orbit"`
<img width="1237" height="620" alt="image" src="https://github.com/user-attachments/assets/cf6833de-93e6-429e-9b70-abeee84e5958" />

en `onNotify`: `event = "orbit"`
<img width="1229" height="605" alt="image" src="https://github.com/user-attachments/assets/e0fd56fb-6fad-44f5-a3b2-4fffcbe9a75a" />
<img width="1215" height="754" alt="image" src="https://github.com/user-attachments/assets/15da4af1-f0a7-4f97-8567-b46109907460" />

en `setState`: `newState = State* {OrbitState}`
<img width="1060" height="643" alt="image" src="https://github.com/user-attachments/assets/f3bbd692-a15a-4ad4-9c86-90f9165f3043" />
<img width="1026" height="545" alt="image" src="https://github.com/user-attachments/assets/371b6cec-0adf-4b4c-ab6d-9b6c03c743cd" />
<img width="1020" height="545" alt="image" src="https://github.com/user-attachments/assets/095f5655-07fd-4a31-966a-7a4621fbc186" />
<img width="1035" height="610" alt="image" src="https://github.com/user-attachments/assets/8d56eb7c-0ad7-4560-a909-609823798d0c" />


Al presionar la tecla `o`, `keyPressed` dispara `notify("orbit")`. Ese evento llega al `Subject`, que recorre su vector de observadores y llama `onNotify(event)` sobre cada partícula. Dentro de `Particle::onNotify`, el evento `"orbit"` activa la rama correspondiente y ejecuta `setState(new OrbitState())`. Finalmente, en `setState`, el puntero `state` deja de apuntar al estado anterior y pasa a apuntar al nuevo objeto `OrbitState`

La evidencia muestra que no es una función aislada, sino una cadena completa de colaboración entre patrones. `Observer` transporta el evento desde la entrada del usuario hasta cada partícula, y `State` usa ese evento para reemplazar el objeto de estado activo, cambiando el comportamiento en tiempo de ejecución.

</details>

---

<details>
<summary><b>Evidencia 4 — Decisión de diseño justificada</b></summary>

Hice que `OrbitState` heredara directamente de `State` y no de `AttractState` o `NormalState`, porque `OrbitState` no es una variante pequeña de otro estado, sino un comportamiento propio

<img width="1028" height="740" alt="image" src="https://github.com/user-attachments/assets/fc0280a0-7c5a-4c4e-ab33-3276afcb3148" />
<img width="1029" height="744" alt="image" src="https://github.com/user-attachments/assets/92da6064-a2f5-4b7d-88f5-468c9e64f9e1" />

Antes de la asignación, la partícula tiene un estado activo (`NormalState`) y simultáneamente se ha creado un nuevo objeto (`OrbitState`) que será el siguiente estado. Ambos existen en memoria como objetos independientes, cada uno con su propia `vtable`, lo que implica comportamientos distintos.

Durante la ejecución de `setState`, el estado anterior es finalizado con `onExit` y luego destruido con `delete`. Posteriormente, el puntero `state` se reasigna para apuntar al nuevo objeto `OrbitState`.

Después de la asignación, `state` deja de referenciar al objeto anterior y pasa a apuntar exclusivamente al nuevo estado, que será el encargado de definir el comportamiento de la partícula en las siguientes actualizaciones.

La decisión de diseño fue implementar el cambio de comportamiento mediante la creación de un nuevo objeto de estado y la sustitución del puntero state, en lugar de modificar atributos internos de la partícula o usar condicionales

Esta es la mejor opción porque:

- mantiene separados los comportamientos de cada estado
- evita estructuras condicionales complejas dentro de `Particle`
- facilita la extensión del sistema, agregar nuevos estados sin modificar código existente
- aprovecha el polimorfismo y el despacho dinámico para cambiar comportamiento en tiempo de ejecución

</details>

</details>

## Bitácora de reflexión
