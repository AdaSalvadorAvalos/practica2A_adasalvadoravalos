# Practice 2-A: Button Interrupt on ESP32 (English Version)

## Materials
- ESP32
- Push Button

## Introduction
In this practice, I learned about the processor's ISR (Interrupt Service Routine) and how, using hardware—specifically by pressing buttons—we can trigger an interrupt.

## Code Explanation (with comments explaining line by line):
```cpp
#include <Arduino.h>

struct Button { // Struct for the button
  const uint8_t PIN;
  uint32_t numberKeyPresses;
  bool pressed;
};

// Select the pin where the button is connected
Button button1 = {18, 0, false};

void IRAM_ATTR isr() { // Function for a short interrupt
  button1.numberKeyPresses += 1;
  button1.pressed = true; // Track how many times it has been pressed
}

void setup() {
  Serial.begin(115200);
  pinMode(button1.PIN, INPUT_PULLUP);
  attachInterrupt(button1.PIN, isr, FALLING); 
  // Set up an interrupt based on the pin:
  // button1.PIN = pin that triggers the interrupt
  // isr = function to call when interrupt occurs
  // FALLING = triggers when pin goes from HIGH to LOW
}

void loop() {
  if (button1.pressed) { // If the button was pressed
      Serial.printf("Button 1 has been pressed %u times\n", button1.numberKeyPresses); // Print to the terminal
      button1.pressed = false;
  }

  // Stop monitoring after one minute
  static uint32_t lastMillis = 0;
  if (millis() - lastMillis > 60000) {
    lastMillis = millis();
    detachInterrupt(button1.PIN); // Stop monitoring the pin
    Serial.println("Interrupt Detached!");
  }
}  
```

## How to Run

1. **Install PlatformIO**
   - Install [VS Code](https://code.visualstudio.com/)
   - Install the [PlatformIO extension](https://platformio.org/install/ide?install=vscode)

2. **Create a New Project**
   - Open PlatformIO in VS Code
   - Create a new project and select your board (e.g., ESP32 Dev Module)
   - Choose **Arduino framework**

3. **Add the Code**
   - Replace the contents of `src/main.cpp` with the code provided above

4. **Connect the Hardware**
   - Connect the push button to **GPIO 18**
   - Connect the ESP32 board to your computer via USB

5. **Build and Upload**
   - Click the **Build (✓)** button to compile the code
   - Click the **Upload (→)** button to flash the code to your ESP32

6. **Observe the Output**
   - Open the Serial Monitor at **115200 baud**
   - Press the button and see the number of presses printed
   - After one minute, the interrupt is detached automatically


# Práctica 2-A: Interrupción con Pulsador en ESP32 (Versión en Español)
## Materiales
- ESP32
- Pulsador

## Presentación

En esta práctica he entendido el ISR del procesador y cómo, mediante el hardware, es decir, apretando los pulsadores, podemos realizar una interrupción.

## Explicación del código (con comentarios línea a línea):
```cpp
#include <Arduino.h>

struct Button { //struct del pulsador
  const uint8_t PIN;
  uint32_t numberKeyPresses;
  bool pressed;
};
 // seleccionamos el pin donde está el pulsador
Button button1 = {18, 0, false};

void IRAM_ATTR isr() { //funcion para que la interrupción sea corta 
  button1.numberKeyPresses += 1;
  button1.pressed = true; //mira las veces que ha pulsado
}

void setup() {
  Serial.begin(115200);
  pinMode(button1.PIN, INPUT_PULLUP);
  attachInterrupt(button1.PIN, isr, FALLING); //interrupción en base al pin por pin, button1.PIN= clavija que hace la interrupción y debe monitorizar, 
  //isr= funcion que llama para hacer la interrupción, FALLING=los disparadores interrumpen cuando el pin va de HIGH a LOW
}

void loop() {
  if (button1.pressed) { //si ha sido pulsado
      Serial.printf("Button 1 has been pressed %u times\n", button1.numberKeyPresses); //lo enseña por la terminal
      button1.pressed = false;
  }

  //estamos diciendo que pare después de un minuto -> millis() - lastMillis > 60000
  static uint32_t lastMillis = 0;
  if (millis() - lastMillis > 60000) {
    lastMillis = millis();
    detachInterrupt(button1.PIN); //es para que deje de monitorizar un pin, el de parar
     Serial.println("Interrupt Detached!");
  }
}  
```
## Cómo Ejecutar

1. **Instalar PlatformIO**
   - Instala [VS Code](https://code.visualstudio.com/)
   - Instala la [extensión PlatformIO](https://platformio.org/install/ide?install=vscode)

2. **Crear un Nuevo Proyecto**
   - Abre PlatformIO en VS Code
   - Crea un nuevo proyecto y selecciona tu placa (ejemplo: ESP32 Dev Module)
   - Elige el **framework Arduino**

3. **Agregar el Código**
   - Reemplaza el contenido de `src/main.cpp` con el código proporcionado arriba

4. **Conectar el Hardware**
   - Conecta el pulsador al **GPIO 18**
   - Conecta la placa ESP32 a tu computadora mediante USB

5. **Compilar y Subir**
   - Haz clic en el botón **Build (✓)** para compilar el código
   - Haz clic en el botón **Upload (→)** para cargar el código en tu ESP32

6. **Observar el Resultado**
   - Abre el Monitor Serial a **115200 baudios**
   - Pulsa el botón y observa en la terminal cuántas veces se ha presionado
   - Después de un minuto, la interrupción se desactiva automáticamente

### Recursos
- Video explicativo en español sobre las conexiones del pulsador: [Ver video](assets/practica2avideo.mp4)