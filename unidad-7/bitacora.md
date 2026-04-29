# Unidad 7

[ACTIVIDADES](https://juanferfranco.github.io/computacionales-2026-10/units/unit7/)


## Bitácora de proceso de aprendizaje

### Actividad 01



### Actividad 02

.lib → usado en tiempo de compilación para enlazar el programa.
.dll → usado en tiempo de ejecución para que el programa funcione.

`opengl32.lib` (de Windows)

Es una biblioteca de enlace estático incluida con Windows.
Permite enlazar contra la API base de OpenGL en Windows y usar funciones básicas de OpenGL 1.1.
Es necesaria para enlazar el uso básico de OpenGL en Windows, aunque las funciones modernas las implementen los drivers.

`GLFW`

Biblioteca multiplataforma para crear ventanas, manejar el teclado, el mouse y gestionar el contexto OpenGL.
Requiere dos archivos:
glfw3.lib: le dice al compilador dónde están las funciones de GLFW.
glfw3.dll: contiene el código real que se usa en tiempo de ejecución.

`GLAD`

Es un cargador de funciones de OpenGL.
Las funciones modernas de OpenGL (3.3, 4.6) no están en opengl32.lib: están implementadas por los drivers de la GPU.
GLAD obtiene esas funciones desde el driver usando wglGetProcAddress y las hace disponibles en tu código.

`GLM` (opcional)

Biblioteca de matemáticas para gráficos: vectores, matrices, transformaciones.
Es solo código fuente (.hpp), no requiere .lib ni .dll.


Conexión entre todos

GLFW crea la ventana y el contexto.
opengl32.lib permite enlazar contra la API base de OpenGL en Windows.
GLAD carga las funciones modernas del driver de tu GPU.
GLM te ayuda a hacer matemáticas para animaciones o transformaciones.

---

Lo primero que entra en juego es GLFW. `GLFW básicamente es el que me crea la ventana del programa y me prepara el contexto donde OpenGL va a trabajar`. También es el que escucha cosas como el teclado, el mouse o cuando cierro la ventana. Sin GLFW realmente no tendría dónde mostrar el triángulo, porque OpenGL necesita una superficie donde renderizar.

Luego está `opengl32.lib`, que es una librería que ya trae Windows. Es el primer `puente entre el proyecto y OpenGL`, porque cuando Visual Studio compila necesita saber que existen funciones de OpenGL y esa librería le permite enlazar con esa API básica. El problema es que esa librería solo llega hasta funciones viejas, entonces por sí sola no alcanza para hacer OpenGL moderno.

Para resolver el problema de opengl32.lib con OpenGL moderno esta `GLAD`, las funciones nuevas de OpenGL n oestan guardadas directamente en windowssino dentro de los drivers de la tarjeta grafica. asi que `GLAD` lo que hace es buscar dentro de esos drivers las funciones modernas de OpenGL y cargarlas para que se puedan usar en el codigo.

O sea que realmente los que tienen el poder gráfico son los drivers de la GPU. `Ellos son los que saben ejecutar instrucciones avanzadas` de OpenGL porque están conectados con la tarjeta gráfica.

Por otro lado está `GLM`, que no es obligatorio para que el programa abra, pero sí ayuda mucho porque trae todas las matemáticas que se usan en gráficos: vectores, matrices, movimientos, rotaciones, escalas, cámaras, etc. Es como una caja de herramientas matemática para no tener que hacer todas esas cuentas a mano.

La cadena de funcion seria:
primero `GLFW` me da una ventana y un contexto (las herramientas),
después `opengl32.lib` permite que el proyecto se conecte con OpenGL básico,
luego `GLAD` carga las funciones modernas que están escondidas en los drivers,
los `drivers de la GPU` son los que realmente ejecutan esas órdenes gráficas,
y `GLM` me ayuda a hacer los cálculos para mover o transformar objetos.

### Actividad 03

Un contexto OpenGL es una estructura de datos interna que contiene:

El estado actual de OpenGL (colores, shaders, buffers, matrices, etc.).
Los recursos que vas a usar (texturas, VBOs, VAOs, etc.).
La conexión con la ventana donde se dibujarán los gráficos.
La versión de OpenGL que estás usando (por ejemplo, 4.6 Core).

¿Qué es OpenGL? 
OpenGL es una interfaz (API): un conjunto de funciones que tú como programador usas para enviar instrucciones a la GPU. OpenGL no dibuja directamente. En cambio, traduce tus comandos en operaciones que la GPU ejecuta.

¿Y qué hace la GPU?

- Toma los datos de entrada que le pasas (vértices, texturas, shaders…).
- Ejecuta los shaders: pequeños programas que definen cómo transformar esos datos en píxeles.
- Dibuja en el framebuffer, que es memoria de video (RAM de la GPU).

En otras palabras:

- Tú escribes código OpenGL en C++.
- OpenGL lo convierte en instrucciones que la GPU entiende.
- La GPU hace el trabajo pesado en paralelo, pintando los píxeles en el framebuffer.

¿Qué es el viewport?
El viewport define qué parte del framebuffer se usará para dibujar. Se mide en píxeles y normalmente coincide con el tamaño completo del framebuffer.

---

con:

``` cpp
// 9) Configura el viewport
glViewport(0, 0, bufferWidth, bufferHeight);
```
<img width="402" height="432" alt="image" src="https://github.com/user-attachments/assets/77f8e71e-851f-4025-93cb-36be047f5953" />

ahora con:

``` cpp
glViewport(0, bufferHeight/2, bufferWidth/2, bufferHeight/2);
```
<img width="402" height="432" alt="image" src="https://github.com/user-attachments/assets/6697a3d5-2251-4539-bbf7-871f9a20b122" />

la ventana es el mismo tamaño pero la zona en la que se dibuja es un cuarto de esta

---

|Concepto|¿Qué es?|¿Por qué es importante?|
|---|---|---|
|GLFW|Biblioteca para crear la ventana y manejar eventos.|Nos evita programar código específico de cada sistema operativo.|
|Contexto OpenGL|Entorno donde OpenGL guarda todo su estado.|Sin él, no se pueden ejecutar funciones de OpenGL.|
|Framebuffer|Memoria donde OpenGL dibuja cada cuadro.|Es lo que finalmente se muestra en pantalla.|
|Viewport|	Área del framebuffer donde se dibuja|Define el espacio visible. Debe ajustarse al tamaño del framebuffer.|

---

1. ¿Qué es el contexto OpenGL?

El contexto OpenGL es el entorno interno donde OpenGL guarda todo lo que necesita para trabajar: configuraciones, shaders, buffers, colores y recursos gráficos. Yo lo entendí como el espacio activo donde las instrucciones gráficas tienen efecto. Sin ese contexto, llamar funciones de OpenGL sería como dar órdenes en el vacío porque no habría ningún lugar asociado para ejecutarlas.

2. ¿Cuál es el rol de GLFW y qué ventaja tiene usarla?

GLFW cumple el papel de crear la ventana, generar el contexto OpenGL y manejar eventos del teclado, mouse y tamaño de pantalla. La ventaja es que simplifica mucho el proceso porque evita que uno tenga que usar directamente funciones específicas de Windows, Linux o Mac. Básicamente hace portable la creación del entorno gráfico.

3. ¿Por qué OpenGL necesita un contexto?

Porque OpenGL no dibuja por sí mismo de manera aislada; necesita saber en qué ventana trabajar, qué versión está usando y qué recursos tiene disponibles. La analogía del taller de arte me ayudó a verlo así: OpenGL es el artista, pero necesita un estudio con herramientas antes de empezar a pintar.

4. ¿Qué es el framebuffer y a qué recuerda de las primeras unidades?

El framebuffer es una memoria donde se almacenan temporalmente los píxeles que la GPU dibuja antes de mostrarlos en pantalla. Me recuerda cuando habia que reservar espacio en la memoria en assembly

5. ¿Qué relación hay entre viewport y framebuffer?

El framebuffer es toda la superficie de memoria donde se puede dibujar, mientras que el viewport define qué parte de esa superficie se va a usar realmente para renderizar. Es como si el framebuffer fuera una hoja completa y el viewport fuera el marco exacto donde quiero pintar.

6. ¿Qué rol juegan los drivers de la GPU y la GPU?

Los drivers contienen la implementación real de muchas funciones modernas de OpenGL. OpenGL desde C++ solo manda instrucciones, pero quien hace el trabajo pesado es la GPU ejecutando esas órdenes a través de sus drivers. En otras palabras, OpenGL da las órdenes y la GPU produce la imagen.

7. ¿Por qué activar VSync?

El VSync sincroniza la actualización de la ventana con la frecuencia del monitor. Esto evita que se vea el efecto de tearing, que ocurre cuando partes de dos cuadros diferentes aparecen mezcladas en pantalla. Si la imagen es estática casi no se nota, pero si hay movimiento la animación puede verse cortada o poco fluida.

8. ¿Qué es OpenGL Legacy y qué diferencia tiene con OpenGL moderno?

OpenGL Legacy es la versión antigua donde muchas cosas se hacían con funciones fijas como `glBegin()` y `glEnd()`, dejando que OpenGL manejara internamente gran parte del pipeline. En OpenGL moderno el programador tiene más control porque debe definir shaders, buffers y estados manualmente. Es más complejo, pero también mucho más flexible y eficiente.

9. ¿Qué es el shader program y por qué es importante?

El shader program es el programa que corre dentro de la GPU para procesar la información gráfica. Está formado por shaders compilados y enlazados, normalmente un vertex shader y un fragment shader. Es importante porque en OpenGL moderno nada puede dibujarse sin indicar cómo la GPU debe transformar vértices y colorear fragmentos.

10. ¿Qué creo que hacen setupTriangle(), VAO y VBO?

Intuitivamente entendí que `setupTriangle()` prepara los datos del triángulo y los manda a la GPU. Dentro de esa función seguramente se crea un VBO, que es el bloque de memoria donde se guardan los vértices, y un VAO, que es como la configuración que le dice a OpenGL cómo interpretar esos datos cuando vaya a dibujar.

11. Uso repetido del shader program y del VAO dentro del game loop

Si el programa solamente va a dibujar un único objeto y nunca cambia ni el shader ni el VAO, no seria necesario volverlos a llamar en cada iteración del game loop, porque OpenGL conserva el último estado activo hasta que otro lo modifique. Los casos que seria util hacerlo seria cuando el programa sea mas grande y ya no solo dibujaria un triangulo sino distintos objetos

12. ¿Por qué es importante glfwSwapBuffers()?

Porque OpenGL no dibuja directamente sobre la imagen visible, sino sobre un buffer trasero. `glfwSwapBuffers()` intercambia ese buffer oculto con el buffer que sí se muestra en pantalla. Si no se llama, la GPU seguiría dibujando internamente pero nunca veríamos la actualización visual, o la ventana quedaría congelada y con parpadeos.


### Actividad 04



### Actividad 05

<details>
<summary>Codigo</summary>

``` cpp
#include <iostream>
#include <glad/glad.h>
#include <GLFW/glfw3.h>


// Callback: ajusta el viewport cuando cambie el tamaño de la ventana
void framebuffer_size_callback(GLFWwindow* window, int width, int height) {
	glViewport(0, 0, width, height);
}

// Procesa entrada simple: cierra con ESC
void processInput(GLFWwindow* window) {
	if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
		glfwSetWindowShouldClose(window, true);
}

// Tamaño de las ventanas
const unsigned int SCR_WIDTH = 400;
const unsigned int SCR_HEIGHT = 400;

// Fuentes de los shaders
const char* vertexShaderSrc = R"glsl(
    #version 460 core

    layout(location = 0) in vec3 aPos;

    // Uniform enviado desde C++ para mover el triángulo
    uniform vec2 offset;

    void main() {
        vec3 newPos = aPos;

        // Se modifica la posición del triángulo usando el mouse
        newPos.x += offset.x;
        newPos.y += offset.y;

        gl_Position = vec4(newPos, 1.0);
    }
)glsl";

const char* fragmentShaderSrc = R"glsl(
    #version 460 core

    out vec4 FragColor;

    // Uniform enviado desde C++ para cambiar el color
    uniform vec4 ourColor;

    void main() {
        FragColor = ourColor;
    }
)glsl";

// IDs globales
unsigned int VAO, VBO;
unsigned int shaderProg;

// Compila y linkea un programa de shaders, retorna su ID
unsigned int buildShaderProgram() {
	int success;
	char log[512];

	unsigned int vs = glCreateShader(GL_VERTEX_SHADER);
	glShaderSource(vs, 1, &vertexShaderSrc, nullptr);
	glCompileShader(vs);
	glGetShaderiv(vs, GL_COMPILE_STATUS, &success);
	if (!success) {
		glGetShaderInfoLog(vs, 512, nullptr, log);
		std::cerr << "ERROR VERTEX SHADER:\n" << log << "\n";
	}

	unsigned int fs = glCreateShader(GL_FRAGMENT_SHADER);
	glShaderSource(fs, 1, &fragmentShaderSrc, nullptr);
	glCompileShader(fs);
	glGetShaderiv(fs, GL_COMPILE_STATUS, &success);
	if (!success) {
		glGetShaderInfoLog(fs, 512, nullptr, log);
		std::cerr << "ERROR FRAGMENT SHADER:\n" << log << "\n";
	}

	unsigned int prog = glCreateProgram();
	glAttachShader(prog, vs);
	glAttachShader(prog, fs);
	glLinkProgram(prog);
	glGetProgramiv(prog, GL_LINK_STATUS, &success);
	if (!success) {
		glGetProgramInfoLog(prog, 512, nullptr, log);
		std::cerr << "ERROR LINKING PROGRAM:\n" << log << "\n";
	}

	glDeleteShader(vs);
	glDeleteShader(fs);
	return prog;
}

// Crea un VAO/VBO con los datos de un triángulo
void setupTriangle() {
	float vertices[] = {
		-0.5f, -0.5f, 0.0f,
		 0.5f, -0.5f, 0.0f,
		 0.0f,  0.5f, 0.0f
	};

	glGenVertexArrays(1, &VAO);
	glGenBuffers(1, &VBO);

	glBindVertexArray(VAO);
	glBindBuffer(GL_ARRAY_BUFFER, VBO);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
	glEnableVertexAttribArray(0);
	glBindVertexArray(0);
}


int main()
{
	// 1) Inicializar GLFW
	if (!glfwInit()) {
		std::cerr << "Fallo al inicializar GLFW\n";
		return -1;
	}

	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 6);
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

	// 2) Crear ventana
	GLFWwindow* mainWindow = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "Ventana", nullptr, nullptr);
	if (!mainWindow) {
		std::cerr << "Error creando ventana1\n";
		glfwTerminate();
		return -1;
	}

	// 3) Lee el tamaño del framebuffer
	int bufferWidth, bufferHeight;
	glfwGetFramebufferSize(mainWindow, &bufferWidth, &bufferHeight);

	// 4) Callbacks 
	glfwSetFramebufferSizeCallback(mainWindow, framebuffer_size_callback);

	// 5) Cargar GLAD y recursos en contexto de window1
	glfwMakeContextCurrent(mainWindow);

	if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
		std::cerr << "Fallo al cargar GLAD (contexto1)\n";
		return -1;
	}

	// 6) Habilita el V-Sync
	glfwSwapInterval(1);

	// 7) Compila y linkea shaders
	shaderProg = buildShaderProgram();

	// 8) Genera el contenido a mostrar
	setupTriangle();

	// 9) Configura el viewport
	glViewport(0, 0, bufferWidth, bufferHeight);

	// Activamos el shader program para poder buscar los uniforms
	glUseProgram(shaderProg);

	// Obtenemos la ubicación de los uniforms dentro del shader
	int offsetLocation = glGetUniformLocation(shaderProg, "offset");
	int colorLocation = glGetUniformLocation(shaderProg, "ourColor");

	// 10) Loop principal
	while (!glfwWindowShouldClose(mainWindow))
	{
		// 11) Manejo de eventos
		glfwPollEvents();

		// 12) Procesa la entrada
		processInput(mainWindow);

		// 13) Configura el color de fondo y limpia el framebuffer
		glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
		glClear(GL_COLOR_BUFFER_BIT);

		// 14) Indica a OpenGL que use el shader program
		glUseProgram(shaderProg);

		// Dibuja el triángulo
		double xpos, ypos;
		glfwGetCursorPos(mainWindow, &xpos, &ypos);

		// Normalizo las coordenadas del mouse
		float x = (float)xpos / (float)SCR_WIDTH;
		if (x < 0) x = 0;
		if (x > 1) x = 1;

		float y = (float)ypos / (float)SCR_HEIGHT;
		if (y < 0) y = 0;
		if (y > 1) y = 1;

		// Envio el color y la posición del triángulo
		glUniform4f(colorLocation, x, y, 0.0f, 1.0f);

		// Envio el offset del triángulo normalizado a NDC
		glUniform2f(offsetLocation, x * 2 - 1, 1 - y * 2);

		// 15) Activa el VAO y dibuja el triángulo
		glBindVertexArray(VAO);
		glDrawArrays(GL_TRIANGLES, 0, 3);

		// 16) Intercambia buffers y muestra el contenido
		glfwSwapBuffers(mainWindow);
	}

	// 17) Limpieza
	glfwMakeContextCurrent(mainWindow);
	glDeleteVertexArrays(1, &VAO);
	glDeleteBuffers(1, &VBO);
	glDeleteProgram(shaderProg);

	glfwDestroyWindow(mainWindow);
	glfwTerminate();

	return 0;
}
```
</details>


<img width="402" height="432" alt="image" src="https://github.com/user-attachments/assets/930e6193-0620-433f-9507-1efc0d5a80a9" />

**Normalización de las coordenadas del mouse**
La función glfwGetCursorPos() entrega la posición del mouse en píxeles dentro de la ventana. Como OpenGL no trabaja directamente con píxeles, fue necesario convertir esos valores a un rango entre 0 y 1 dividiendo entre el ancho y alto de la ventana

```cpp
float x = (float)xpos / (float)SCR_WIDTH;
float y = (float)ypos / (float)SCR_HEIGHT;
```

Con esto obtuve una posición relativa del cursor sin importar el tamaño de la ventana. Además, estos valores normalizados también sirven para controlar el color porque OpenGL maneja los componentes RGB en el rango 0 a 1

**Normalización a coordenadas NDC**
OpenGL posiciona los objetos usando coordenadas NDC, donde el espacio visible va de -1 a 1 en ambos ejes. Por eso fue necesario convertir los valores anteriores con:

```cpp
glUniform2f(offsetLocation, x * 2 - 1, 1 - y * 2);
```

La fórmula x * 2 - 1 transforma el eje horizontal de 0..1 a -1..1, y 1 - y * 2 hace lo mismo con el eje vertical pero además invierte la dirección, ya que en la ventana el eje Y crece hacia abajo y en OpenGL crece hacia arriba.


## Bitácora de aplicación 

### Actividad 06





## Bitácora de reflexión
