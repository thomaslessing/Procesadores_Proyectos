# Pràctica 7 - BUSES DE COMUNICACIÓN III

Aquest codi té com a objectiu demostrar el funcionament del bus I2S (Inter-Integrated Circuit Sound) per reproduir un fitxer d'àudio a través d'un dispositiu utilitzant una placa Arduino compatible amb ESP8266.
Es necessiten diverses llibreries de l'ESP8266Audio Library per gestionar el bus I2S i la reproducció d'àudio. Aquestes llibreries són importades al principi del codi:
```cpp
#include "Arduino.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
```
Es declaren diversos objectes per gestionar la reproducció d'àudio. S'utilitza un objecte de tipus "AudioFileSourcePROGMEM" anomenat "in" per especificar el fitxer d'àudio a reproduir. També s'utilitza un objecte de tipus "AudioGeneratorAAC" anomenat "aac" per a la generació d'àudio i un objecte de tipus "AudioOutputI2S" anomenat "out" per enviar l'àudio a través del bus I2S:
```cpp
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
```
A la funció setup(), s'inicialitza la comunicació sèrie (Serial) per a mostrar informació per la terminal. A continuació, s'inicialitzen els objectes "in", "aac" i "out". Es configura el guany del canal d'àudio amb "out->SetGain(0.125)" i es defineixen els pins de sortida del bus I2S amb "out->SetPinout(26, 25, 22)". Finalment, s'inicia la reproducció de l'àudio amb "aac->begin(in, out)":
```cpp
void setup(){

  Serial.begin(115200);
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);}
  ```

A la funció loop(), es comprova si la reproducció d'àudio està en curs amb "aac->isRunning()". Si està en curs, es crida a "aac->loop()" per avançar en la reproducció. Si la reproducció ha finalitzat, s'atura la reproducció amb "aac->stop()", es mostra el missatge "Sound Generator" per la comunicació sèrie i es fa una pausa de 1 segon amb "delay(1000)":
```cpp
void loop(){
  if (aac->isRunning()) {
    aac->loop();
    } else {

      aac -> stop();
      Serial.printf("Sound Generator\n");
      delay(1000);
  }
}
```

## Conclusions Pràctica 7

Aquest codi demostra com utilitzar el bus I2S per reproduir un fitxer d'àudio en un dispositiu compatible amb ESP8266. L'objectiu és comprendre el funcionament del bus I2S i realitzar una pràctica d'àudio digital utilitzant aquesta tecnologia

