# Pràctica 1
## Codi de la pràctica 1

```cpp

#include <Arduino.h>
//pin del LED
#define PIN 14
//set up
void setup() {
  Serial.begin(115200);
  pinMode(PIN, OUTPUT);
}
//loop
void loop() {

  digitalWrite(PIN, HIGH);
  Serial.println("ON");
  delay(500);
  digitalWrite(PIN, LOW);
  Serial.println("OFF");
  delay(500);
}
```

Aquest codi utilitza el llenguatge de programació Arduino per controlar un LED connectat al pin 14 d'una placa Arduino. El codi està dividit en dues parts principals: el setup i el loop.

```cpp
//set up
void setup() {
  Serial.begin(115200);
  pinMode(PIN, OUTPUT);
}
``` 

En el setup, s'estableixen les configuracions inicials. S'inicia la comunicació sèrie amb una velocitat de 115200 bps mitjançant la crida a Serial.begin(115200). Això permetrà enviar i rebre dades des del microcontrolador Arduino al monitor sèrie de l'entorn de desenvolupament Arduino. 

```cpp 
#define PIN 14
``` 
A continuació, es configura el pin 14 com a sortida mitjançant pinMode (PIN, OUTPUT), indicant que el pin s'utilitzarà per enviar senyals de sortida.

```cpp
//loop
void loop() {

  digitalWrite(PIN, HIGH);
  Serial.println("ON");
  delay(500);
  digitalWrite(PIN, LOW);
  Serial.println("OFF");
  delay(500);
}
```

En el loop, s'executa un cicle infinit. Dins d'aquest cicle, primer s'activa el LED fent servir digitalWrite (PIN, HIGH), que estableix el pin 14 a l'estat alt, el que encén el LED. 
A continuació, es mostra el missatge "ON" a través de Serial.println("ON"), que s'envia al monitor sèrie. Després d'un retard de 500 mil·lisegons (mitjançant delay(500)), es desactiva el LED fent servir digitalWrite (PIN, LOW), que estableix el pin 14 a l'estat baix, apagant el LED. 
A continuació, es mostra el missatge "OFF" a través de Serial.println ("OFF") i hi ha un altre retard de 500 mil·lisegons abans que el cicle es repeteixi.

## Conclusions de la pràctica 1

Per tant, aquest codi fa que el LED connectat al pin 14 s'encengui i s'apagui alternativament cada 500 mil·lisegons, mentre s'envien els missatges "ON" i "OFF" al monitor sèrie. Aquesta és una pràctica bàsica per aprendre a utilitzar els pins, les sortides i el cicle loop en Arduino.


