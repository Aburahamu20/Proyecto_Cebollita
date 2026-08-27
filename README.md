# Informe Técnico Final de Proyecto IoT

## Sistema Automatizado de Control Ambiental, Humedad y Riego para Invernaderos

**Instituto Profesional Duoc UC**  
Escuela de Informática y Telecomunicaciones  
Sede Plaza Vespucio

### Integrantes del equipo

1. Abraham Castro Romero
2. Sebastian Fuentes Cortes
3. Lisandra Gonzalez Hernandez
4. Felipe Murua Lobos
5. Barbara Saavedra Fernandez

### Información académica

- **Asignatura:** SIY6122 - Problemáticas Globales
- **Docente:** Marco Antonio Perelli
- **Plataforma:** Arduino Uno + Tinkercad
- **Asistente IA:** Gemini AI
- **Lugar y año:** Santiago, Chile — 2026

---

## 1. Introducción y objetivos del proyecto

El presente proyecto aborda la automatización del microclima en un invernadero mediante el desarrollo de un prototipo IoT basado en la plataforma Arduino.

El objetivo principal es mantener las condiciones ambientales óptimas de temperatura, humedad del suelo e iluminación para maximizar el rendimiento agrícola y reducir el desperdicio de agua y energía.

### 1.1 Objetivos específicos

- Monitorear continuamente la temperatura ambiental, el nivel de humedad del suelo y la intensidad lumínica.
- Implementar un control térmico activo mediante ventilación cuando la temperatura sea superior a 25 °C.
- Activar la calefacción cuando la temperatura sea inferior a 13 °C.
- Automatizar la bomba de agua para mantener el nivel de humedad entre 40 % y 70 %.
- Prevenir tanto el estrés hídrico como la saturación del suelo.
- Controlar el sistema de iluminación suplementaria LED según las condiciones de luz ambiental.
- Garantizar la ejecución de un ciclo continuo de lectura y control cada 5 segundos.

---

## 2. Descripción y análisis del diagrama de flujo

La arquitectura lógica del sistema está modelada mediante un diagrama de flujo estructurado en tres bloques secuenciales de toma de decisiones y un temporizador global de control.

### 2.1 Etapa de temperatura

1. Se realiza la lectura del sensor de temperatura.
2. Si la temperatura es superior a 25 °C, se enciende el ventilador.
3. Si la temperatura es inferior a 13 °C, se enciende la luz térmica o calefactor.
4. Si la temperatura se encuentra entre 13 °C y 25 °C, se apagan el ventilador y el calefactor.

### 2.2 Etapa de humedad del suelo

1. Se realiza la lectura del sensor de humedad del suelo.
2. Si la humedad es inferior al 40 %, se enciende la bomba de agua o sistema de riego.
3. Si la humedad es superior al 70 %, se apaga el riego y se enciende el ventilador para evacuar la humedad.
4. Si la humedad se encuentra entre 40 % y 70 %, se mantiene apagado el sistema de riego.

### 2.3 Etapa de iluminación

1. Se realiza la lectura de la fotocelda o sensor de luz.
2. Si el nivel de luz ambiental es bajo, se encienden los LED suplementarios.
3. Si existe suficiente iluminación, se apagan los LED.

### 2.4 Temporización

El sistema espera 5 segundos antes de regresar al inicio del ciclo de lectura y control.

> La imagen correspondiente al diagrama de flujo fue omitida de este repositorio.

---

## 3. Arquitectura de hardware y mapeo de pines

| Componente o actuador | Conexión Arduino | Tipo de entrada/salida | Función en el sistema |
|---|---|---|---|
| Sensor de temperatura TMP36 | Pin analógico A0 | Entrada analógica | Medición de la temperatura ambiental en grados Celsius |
| Sensor de humedad del suelo | Pin analógico A1 | Entrada analógica | Medición del porcentaje de humedad del suelo entre 0 % y 100 % |
| Sensor de luz LDR | Pin analógico A2 | Entrada analógica | Detección del nivel de luminosidad ambiental |
| Ventilador o motor DC | Pin digital 3 | Salida digital PWM | Refrigeración cuando T > 25 °C o disipación cuando H > 70 % |
| Calefactor o luz térmica | Pin digital 4 | Salida digital | Calefacción del ambiente cuando T < 13 °C |
| Bomba de agua o riego | Pin digital 5 | Salida digital | Suministro de agua cuando H < 40 % |
| Iluminación LED | Pin digital 6 | Salida digital | Iluminación de apoyo cuando el nivel de luz es bajo |

---

## 4. Código fuente C++ implementado para Arduino

El siguiente programa escrito en C++ traduce cada bloque de decisión y acción del diagrama de flujo para su simulación directa en Tinkercad:

```cpp
/*
 * PROYECTO IoT: CONTROL DE INVERNADERO AUTOMATIZADO
 * Asignatura: SIY6122 - Problemáticas Globales | Duoc UC
 */

// Definición de pines de entrada (sensores)
const int PIN_TEMP = A0;       // Sensor TMP36
const int PIN_HUMEDAD = A1;    // Sensor de humedad del suelo
const int PIN_LDR = A2;        // Fotoresistencia

// Definición de pines de salida (actuadores)
const int PIN_VENTILADOR = 3;   // Ventilador
const int PIN_CALEFACTOR = 4;   // Calefactor o luz térmica
const int PIN_BOMBA_RIEGO = 5;  // Bomba de agua
const int PIN_LEDS_LUZ = 6;     // Iluminación LED

void setup() {
  pinMode(PIN_VENTILADOR, OUTPUT);
  pinMode(PIN_CALEFACTOR, OUTPUT);
  pinMode(PIN_BOMBA_RIEGO, OUTPUT);
  pinMode(PIN_LEDS_LUZ, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  // ---------------------------------------------------------
  // 1. LECTURA Y CONTROL DE TEMPERATURA
  // ---------------------------------------------------------

  int lectTemp = analogRead(PIN_TEMP);

  // Conversión TMP36: (Vout - 0.5 V) * 100
  float tempC = ((lectTemp * (5.0 / 1023.0)) - 0.5) * 100.0;

  if (tempC > 25.0) {
    digitalWrite(PIN_VENTILADOR, HIGH);
    digitalWrite(PIN_CALEFACTOR, LOW);
  }
  else if (tempC < 13.0) {
    digitalWrite(PIN_CALEFACTOR, HIGH);
    digitalWrite(PIN_VENTILADOR, LOW);
  }
  else {
    // Rango normal: entre 13 °C y 25 °C
    digitalWrite(PIN_VENTILADOR, LOW);
    digitalWrite(PIN_CALEFACTOR, LOW);
  }

  // ---------------------------------------------------------
  // 2. LECTURA Y CONTROL DE HUMEDAD DEL SUELO Y RIEGO
  // ---------------------------------------------------------

  int lecturaHumedad = analogRead(PIN_HUMEDAD);
  int humedadPct = map(lecturaHumedad, 0, 1023, 0, 100);

  if (humedadPct < 40) {
    digitalWrite(PIN_BOMBA_RIEGO, HIGH);
  }
  else if (humedadPct > 70) {
    digitalWrite(PIN_BOMBA_RIEGO, LOW);
    digitalWrite(PIN_VENTILADOR, HIGH);
  }
  else {
    // Rango normal: entre 40 % y 70 %
    digitalWrite(PIN_BOMBA_RIEGO, LOW);
  }

  // ---------------------------------------------------------
  // 3. LECTURA Y CONTROL DE ILUMINACIÓN
  // ---------------------------------------------------------

  int nivelLuz = analogRead(PIN_LDR);
  int umbralLuzBaja = 400;

  if (nivelLuz < umbralLuzBaja) {
    digitalWrite(PIN_LEDS_LUZ, HIGH);
  }
  else {
    digitalWrite(PIN_LEDS_LUZ, LOW);
  }

  // ---------------------------------------------------------
  // 4. TEMPORIZADOR DEL CICLO
  // ---------------------------------------------------------

  delay(5000);
}
```

---

## 5. Matriz de validación y pruebas del sistema

| Caso de prueba | Valores de entrada | Resultado esperado | Estado |
|---|---|---|---|
| Caso 1: Temperatura alta | Temp = 28 °C, Hum = 50 %, Luz = 600 | Ventilador ON, calefactor OFF, riego OFF y LED OFF | PASÓ |
| Caso 2: Temperatura baja | Temp = 10 °C, Hum = 50 %, Luz = 600 | Calefactor ON, ventilador OFF, riego OFF y LED OFF | PASÓ |
| Caso 3: Humedad baja | Temp = 20 °C, Hum = 30 %, Luz = 600 | Bomba de riego ON, ventilador OFF y calefactor OFF | PASÓ |
| Caso 4: Humedad alta | Temp = 20 °C, Hum = 85 %, Luz = 600 | Bomba de riego OFF y ventilador ON para secado | PASÓ |
| Caso 5: Oscuridad | Temp = 20 °C, Hum = 50 %, Luz = 200 | LED ON y los demás actuadores OFF | PASÓ |
| Caso 6: Estado normal | Temp = 20 °C, Hum = 55 %, Luz = 700 | Todos los actuadores OFF | PASÓ |

---

## 6. Conclusiones y próximos pasos

El prototipo desarrollado cumple íntegramente con la lógica de control especificada en el diagrama de flujo.

La combinación del control de temperatura, humedad del suelo e iluminación permite mantener un microclima automatizado y óptimo.

### 6.1 Eficiencia de control

El uso de una temporización de 5 segundos evita sobrecargas de procesamiento y fluctuaciones bruscas en las lecturas de los sensores.

### 6.2 Automatización integral

La integración simultánea de ventilación, calefacción y riego automatizado reduce considerablemente la intervención humana.

### 6.3 Proyección IoT futura

Como siguiente fase, se propone añadir un módulo de comunicación inalámbrica ESP8266 o ESP32 para transmitir la telemetría a una base de datos en la nube.

Esto permitiría visualizar las métricas del invernadero en tiempo real mediante un dashboard interactivo.
