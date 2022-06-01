# Práctica 2: ejercicio A

## MATERIALES
- ESP32
- Pulsador

## Presentación: 
En esta práctica he entendido el ISH del procesador y como mediante el hardware, es decir, aprentado los pulsadores podemos realizar una interrupción.


## Explicación y comentario del código:
```
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
Observar el vídeo para entender las salidas
