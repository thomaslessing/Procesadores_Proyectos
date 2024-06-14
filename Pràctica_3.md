# Pràctica 4 - Sistemes Operatius en Temps Real

## 4a

En aquest codi, es crea un sistema amb múltiples tasques per a comprendre el funcionament d'un sistema operatiu en temps real. Aquí s'explica el funcionament del codi:
En el setup(), es configura la comunicació sèrie i es crea una nova tasca utilitzant la funció xTaskCreate(). Els paràmetres de la funció són els següents:
"anotherTask": És la funció que s'executarà com a tasca.
"another Task": És el nom que se li dóna a la tasca.
10000: És el tamany de la pila de la tasca en bytes.
NULL: És el paràmetre de la tasca.
1: És la prioritat de la tasca. Un nombre més gran indica una prioritat més alta.
NULL: És el manegador de la tasca.

```cpp 
void setup()
{
    Serial.begin(115200);
    /* we create a new task here */
    xTaskCreate(anotherTask, "another Task", 10000, NULL, 1, NULL); /* Task handle to keep track of created task */
}
```

En el loop(), es mostra per la comunicació sèrie el missatge "this is ESP32 Task" amb un retard de 1000ms entre iteracions.

```cpp
void loop()
{
    Serial.println("this is ESP32 Task");
    delay(1000);
}
```




La funció "anotherTask" és una tasca infinita que mostra el missatge "this is another Task" per la comunicació sèrie amb un retard de 1000ms entre iteracions. Aquesta tasca està dissenyada per funcionar en paral·lel amb la tasca principal (loop()).
```cpp 
void anotherTask( void * parameter )
{
    /* loop forever */
    for(;;)
    {
        Serial.println("this is another Task");
        delay(1000);
    }
    /* delete a task when finish,
    this will never happen because this is infinity loop */
    vTaskDelete( NULL );
}
```

## Conclusions Pràctica 4a

Amb aquest codi, es crea un sistema amb dues tasques funcionant simultàniament. La tasca principal (loop()) i la tasca addicional (anotherTask) s'executaran en paral·lel, permetent veure com es divideix el temps d'ús de la CPU entre les dues tasques. Això permet comprendre el funcionament d'un sistema operatiu en temps real, on múltiples tasques poden funcionar de manera concurrent.














# 4b

En aquest codi es realitza un programa que utilitza dues tasques sincronitzades per encendre i apagar dos LEDs. Aquí s'explica el funcionament del codi:
En el setup(), es configura la comunicació sèrie i es defineixen els pins dels LEDs com a sortida. A continuació, es creen dues tasques utilitzant la funció xTaskCreate(). Els paràmetres de la funció són els següents:
LED_ON: És la funció que s'executarà com a tasca per encendre el LED.
"ON": És el nom que se li dóna a la tasca.
10000: És el tamany de la pila de la tasca en bytes.
NULL: És el paràmetre de la tasca.
1: És la prioritat de la tasca. Un nombre més gran indica una prioritat més alta.
NULL: És el manegador de la tasca.
Aquest procés es repeteix per crear la segona tasca (LED_OFF).

```cpp 
void setup(){

Serial.begin(115200);
pinMode(LED, OUTPUT);
pinMode(LED2, OUTPUT);
xTaskCreate(LED_ON, "ON", 10000, NULL, 1, NULL);
xTaskCreate(LED_OFF, "OFF", 10000, NULL, 1, NULL);
}
```
La funció LED_ON és una tasca infinita que encén i apaga el primer LED de manera repetida. S'encén el LED, s'imprimeix "a" per la comunicació sèrie, es fa una pausa de 1 segon, s'apaga el LED i es torna a fer una pausa de 1 segon abans de repetir el bucle.
```cpp 
void loop(){}

void LED_ON (void * pvParameters){
    for(;;){
        digitalWrite(LED, HIGH);
        Serial.println("a");
        vTaskDelay(1000);
        digitalWrite(LED, LOW);
        vTaskDelay(1000);
    }
}
```

La funció LED_OFF és una tasca infinita que encén i apaga el segon LED de manera repetida. S'encén el LED, s'imprimeix "b" per la comunicació sèrie, es fa una pausa de 500ms, s'apaga el LED i es torna a fer una pausa de 500ms abans de repetir el bucle.
```cpp 
void LED_OFF (void * pvParameters){
    for(;;){
        digitalWrite(LED2, HIGH);
        Serial.println("b");
        vTaskDelay(500);
        digitalWrite(LED2, LOW);
        vTaskDelay(500);
    }
}
```

## Conclusions Pràctica 4b

Amb aquest codi, es creen dues tasques sincronitzades que funcionen en paral·lel. La primera tasca (LED_ON) s'encarrega d'encendre i apagar un LED cada segon, mentre que la segona tasca (LED_OFF) s'encarrega d'encendre i apagar un altre LED cada 500ms. Les tasques estan sincronitzades en el temps, ja que el retard en cada bucle està ajustat perquè coincideixin els enceniments i apagaments dels dos LEDs. Això permet crear un programa que controla múltiples tasques de manera coordinada en un sistema en temps real

