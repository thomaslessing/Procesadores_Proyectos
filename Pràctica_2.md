# Pràctica 2
## 2a

Aquest codi implementa l'ús d'interrupcions en Arduino per gestionar una pulsació de botó. A continuació, s'explica el codi pas a pas:

Es defineix una estructura anomenada "Button" que conté tres variables: el número de pin del botó (PIN), el nombre de vegades que s'ha premut el botó (numberKeyPresses) i un indicador de si el botó està premut (pressed).

```cpp
struct Button {
  const uint8_t PIN;
  uint32_t numberKeyPresses;
  bool pressed;
};
```


Es declara una instància de la estructura Button anomenada "button1" amb els valors específics (pin 18, 0 pulsacions i no premut).

```cpp
Button button1 = {18, 0, false};
```

Es defineix la funció d'interrupció "isr()" (Interrupt Service Routine). Quan es produeix una interrupció, aquesta funció s'executa. En aquest cas, incrementa el nombre de pulsacions del botó en 1 i estableix l'indicador de "pressed" a true.

```cpp
void IRAM_ATTR isr() {
  button1.numberKeyPresses += 1;
  button1.pressed = true;
}
```



En el setup(), s'estableix la comunicació sèrie per a la sortida de dades, s'inicialitza el pin del botó com a INPUT_PULLUP (entrada amb resistència pull-up interna) i s'adjunta la interrupció al pin del botó amb attachInterrupt(). Aquesta funció indica que s'ha de cridar la funció "isr()" quan es produeixi una interrupció en el flanc de baixada del senyal (FALLING).
```cpp
void setup() {
  Serial.begin(115200);
  pinMode(button1.PIN, INPUT_PULLUP);
  attachInterrupt(button1.PIN, isr, FALLING);
}
```

En el loop(), es comprova si el botó ha estat premut llegint l'indicador "pressed". Si és true, s'imprimeix per la comunicació sèrie el nombre de pulsacions i s'estableix l'indicador a false.
```cpp 
void loop() {
  if (button1.pressed) {
    Serial.printf("Button 1 has been pressed %u times\n", button1.numberKeyPresses);
    button1.pressed = false;
  }
  ```

Després, s'implementa una condició per a desconectar la interrupció després d'un minut d'inactivitat del botó. Es crea una variable "lastMillis" per a mantenir el seguiment del temps transcorregut. Si la diferència entre l'hora actual (obtinguda amb millis()) i la darrera vegada que es va desconnectar la interrupció és superior a 60000ms (1 minut), es desconnecta la interrupció amb detachInterrupt() i es mostra per la comunicació sèrie el missatge "Interrupt Detached!".
```cpp 
//Detach Interrupt after 1 Minute
static uint32_t lastMillis = 0;
if (millis() - lastMillis > 60000) {
  lastMillis = millis();
  detachInterrupt(button1.PIN);
  Serial.println("Interrupt Detached!");
  }
}
```
## Conclusions Pràctica 2a

Aquest codi demostra l'ús de les interrupcions per capturar les pulsacions d'un botó i gestionar-les eficientment, evitant la necessitat d'estar constantment revisant l'estat del botó en el bucle principal. També il·lustra com utilitzar la funció detachInterrupt() per desconnectar una interrupció després d'un període determinat.



# 2b

En aquest codi, s'utilitzen les interrupcions de temporitzador per a controlar el flux del programa. Aquí s'explica el funcionament del codi:

Es declaren dues variables: "interruptCounter" (de tipus volatile int) i "totalInterruptCounter" (de tipus int). "interruptCounter" s'utilitza per comptar les interrupcions que es produeixen, mentre que "totalInterruptCounter" registra el nombre total d'interrupcions.
```cpp 
volatile int interruptCounter;
int totalInterruptCounter;
```

Es crea un punter "timer" per al temporitzador i una variable "timerMux" per a sincronitzar les interrupcions.
```cpp 
hw_timer_t * timer = NULL;
portMUX_TYPE timerMux = portMUX_INITIALIZER_UNLOCKED;
```
Es defineix la funció d'interrupció "onTimer()", que s'executarà quan es produeixi una interrupció de temporitzador. Aquesta funció incrementa el "interruptCounter" per indicar que s'ha produït una interrupció.
```cpp 
void IRAM_ATTR onTimer() {
  portENTER_CRITICAL_ISR(&timerMux);
  interruptCounter++;
  portEXIT_CRITICAL_ISR(&timerMux);
}
```
En el setup(), s'inicialitza la comunicació sèrie i es configuren els paràmetres del temporitzador. S'adjunta la funció d'interrupció "onTimer()" al temporitzador i es configura l'alarma perquè es produeixi cada 1 segon. Finalment, s'habilita l'alarma del temporitzador.
```cop
void setup() {
 
  Serial.begin(115200);
 
  timer = timerBegin(0, 80, true);
  timerAttachInterrupt(timer, &onTimer, true);
  timerAlarmWrite(timer, 1000000, true);
  timerAlarmEnable(timer);
 
}
```

En el loop(), es comprova si s'ha produït una interrupció (si "interruptCounter" és més gran que 0). Si hi ha hagut una interrupció, es decrementa "interruptCounter" i s'incrementa "totalInterruptCounter". Després, s'imprimeix per la comunicació sèrie el nombre total d'interrupcions.

```cpp
void loop() {
 
  if (interruptCounter > 0) {
 
    portENTER_CRITICAL(&timerMux);
    interruptCounter--;
    portEXIT_CRITICAL(&timerMux);
 
    totalInterruptCounter++;
 
    Serial.print("An interrupt as occurred. Total number: ");
    Serial.println(totalInterruptCounter);
  }
}
```




## Conclusions Pràctica 2b

Aquest codi utilitza les interrupcions de temporitzador per controlar el flux d'execució del programa i comptar el nombre d'interrupcions que es produeixen. Això permet gestionar de manera eficient tasques que es repeteixen a intervals fixos de temps.




