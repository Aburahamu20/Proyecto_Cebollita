<div align="center">

# Informe EP1 — Proyecto IoT

## Sistema Automatizado de Control Ambiental, Humedad y Riego para Invernaderos

![Badge](https://img.shields.io/badge/Asignatura-SIY6122-0078D4?style=for-the-badge)
![Badge](https://img.shields.io/badge/Evaluación-EP1-28a745?style=for-the-badge)
![Badge](https://img.shields.io/badge/Sección-002V-e63946?style=for-the-badge)
![Badge](https://img.shields.io/badge/DUOC%20UC-Agosto%202026-1F4E79?style=for-the-badge)

---

| Campo | Detalle |
|:---|:---|
| **Institución** | Instituto Profesional Duoc UC |
| **Escuela** | Informática y Telecomunicaciones |
| **Sede** | Plaza Norte |
| **Asignatura** | SIY6122 - Problemáticas Globales y Prototipado |
| **Evaluación** | Experiencia Práctica 1 (EP1) |
| **Sección** | 002V |
| **Proyecto** | Sistema Automatizado de Control Ambiental, Humedad y Riego para Invernaderos |
| **Integrantes** | Abraham Castro Romero · Sebastian Fuentes Cortes · Lisandra Gonzalez Hernandez · Felipe Murua Lobos · Barbara Saavedra Fernandez |
| **Docente** | Marcos Antonio Perelli Henriquez |
| **Plataforma** | Arduino Uno + Tinkercad |
| **Asistentes IA** | Gemini AI · Claude · ChatGPT |
| **Fecha** | Agosto 2026 |

</div>

---

## 📋 Tabla de contenidos

- [1. Introducción y objetivos](#1-introducción-y-objetivos)
- [2. Funcionamiento del sistema](#2-funcionamiento-del-sistema)
- [3. Arquitectura de hardware](#3-arquitectura-de-hardware)
- [4. Código fuente Arduino](#4-código-fuente-arduino)
- [5. Validación y pruebas](#5-validación-y-pruebas)
- [6. Conclusiones y próximos pasos](#6-conclusiones-y-próximos-pasos)

---

## 1. Introducción y objetivos

> El proyecto desarrolla un prototipo IoT basado en Arduino para automatizar el control ambiental de un invernadero.

El presente proyecto aborda la automatización del microclima en un invernadero mediante el desarrollo de un prototipo IoT basado en la plataforma Arduino.

El objetivo principal es mantener condiciones ambientales óptimas de temperatura, humedad del suelo e iluminación, con la finalidad de maximizar el rendimiento agrícola y reducir el desperdicio de agua y energía.

### 1.1 Objetivos específicos

- ✔️ Monitorear continuamente la temperatura ambiental.
- ✔️ Medir el nivel de humedad presente en el suelo.
- ✔️ Detectar la intensidad de la iluminación ambiental.
- ✔️ Activar la ventilación cuando la temperatura sea superior a 25 °C.
- ✔️ Activar la calefacción cuando la temperatura sea inferior a 13 °C.
- ✔️ Mantener la humedad del suelo entre 40 % y 70 %.
- ✔️ Prevenir el estrés hídrico y la saturación del suelo.
- ✔️ Controlar automáticamente la iluminación LED suplementaria.
- ✔️ Ejecutar un ciclo continuo de lectura y control cada 5 segundos.

---

## 2. Funcionamiento del sistema

La arquitectura lógica del sistema se encuentra organizada en tres etapas secuenciales de toma de decisiones y un temporizador general.

### 2.1 Control de temperatura

El sensor TMP36 mide continuamente la temperatura ambiental.

| Condición | Acción |
|:---|:---|
| Temperatura superior a 25 °C | Encender ventilador y apagar calefactor |
| Temperatura inferior a 13 °C | Encender calefactor y apagar ventilador |
| Temperatura entre 13 °C y 25 °C | Apagar ventilador y calefactor |

### 2.2 Control de humedad del suelo

El sensor de humedad determina el porcentaje de agua presente en el suelo.

| Condición | Acción |
|:---|:---|
| Humedad inferior al 40 % | Encender bomba de riego |
| Humedad superior al 70 % | Apagar riego y encender ventilador |
| Humedad entre 40 % y 70 % | Mantener apagada la bomba de riego |

### 2.3 Control de iluminación

La fotoresistencia LDR mide la cantidad de luz ambiental.

| Condición | Acción |
|:---|:---|
| Nivel de luz inferior a 400 | Encender iluminación LED |
| Nivel de luz igual o superior a 400 | Apagar iluminación LED |

### 2.4 Temporización

Una vez completadas las lecturas y acciones, el sistema espera 5 segundos antes de comenzar nuevamente el ciclo.

### 2.5 Diagrama de flujo del sistema

> El siguiente diagrama representa el ciclo completo de lectura y control del invernadero. Fue recreado mediante caracteres de texto, por lo que no corresponde a una imagen y puede editarse directamente desde el repositorio.

```text
┌──────────────────────────────────────────┐
│            LEER TEMPERATURA              │
└────────────────────┬─────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────┐
│      ¿TEMPERATURA MAYOR A 25 °C?         │
└──────────┬───────────────────┬───────────┘
           │ SÍ                │ NO
           ▼                   ▼
┌────────────────────┐   ┌──────────────────────────────────────┐
│ Encender ventilador│   │   ¿TEMPERATURA MENOR A 13 °C?       │
└──────────┬─────────┘   └──────────┬───────────────┬───────────┘
           │                        │ SÍ            │ NO
           │                        ▼               ▼
           │             ┌───────────────────┐ ┌───────────────────────┐
           │             │ Encender luz      │ │ Apagar ventilador y   │
           │             │ térmica/calefactor│ │ calefactor            │
           │             └─────────┬─────────┘ └──────────┬────────────┘
           │                       │                      │
           └───────────────────────┴──────────────────────┘
                                   │
                                   ▼
                    ┌────────────────────────────┐
                    │ LEER HUMEDAD DEL SUELO     │
                    └──────────────┬─────────────┘
                                   │
                                   ▼
                    ┌────────────────────────────┐
                    │ ¿HUMEDAD MENOR AL 40 %?    │
                    └────────┬───────────┬───────┘
                             │ SÍ        │ NO
                             ▼           ▼
              ┌─────────────────────┐  ┌───────────────────────────┐
              │ Encender bomba de   │  │ ¿HUMEDAD MAYOR AL 70 %?  │
              │ agua/sistema de     │  └────────┬──────────┬───────┘
              │ riego               │           │ SÍ       │ NO
              └──────────┬──────────┘           ▼          ▼
                         │         ┌────────────────────┐ ┌──────────────┐
                         │         │ Apagar riego y     │ │ Apagar riego│
                         │         │ encender ventilador│ └──────┬───────┘
                         │         └──────────┬─────────┘        │
                         └────────────────────┴──────────────────┘
                                              │
                                              ▼
                              ┌──────────────────────────┐
                              │     CONTROL DE LUZ       │
                              └────────────┬─────────────┘
                                           │
                                           ▼
                              ┌──────────────────────────┐
                              │ ¿EL NIVEL DE LUZ ES BAJO?│
                              └────────┬──────────┬───────┘
                                       │ SÍ       │ NO
                                       ▼          ▼
                            ┌────────────────┐ ┌────────────────┐
                            │ Encender LEDS  │ │ Apagar LEDS    │
                            └───────┬────────┘ └───────┬────────┘
                                    │                  │
                                    └────────┬─────────┘
                                             │
                                             ▼
                              ┌──────────────────────────┐
                              │   ESPERAR 5 SEGUNDOS     │
                              └────────────┬─────────────┘
                                           │
                                           └──────────────► VOLVER A
                                                            LEER
                                                        TEMPERATURA
```

El sistema repite automáticamente este proceso cada 5 segundos, comenzando nuevamente con la lectura de la temperatura ambiental.

---

## 3. Arquitectura de hardware

### 3.1 Componentes utilizados

| Componente o actuador | Conexión Arduino | Tipo de entrada/salida | Función |
|:---|:---:|:---:|:---|
| Sensor de temperatura TMP36 | `A0` | Entrada analógica | Medición de la temperatura ambiental |
| Sensor de humedad del suelo | `A1` | Entrada analógica | Medición del porcentaje de humedad |
| Sensor de luz LDR | `A2` | Entrada analógica | Detección del nivel de iluminación |
| Ventilador o motor DC | `3` | Salida digital PWM | Refrigeración y evacuación de humedad |
| Calefactor o luz térmica | `4` | Salida digital | Calefacción del ambiente |
| Bomba de agua | `5` | Salida digital | Activación del sistema de riego |
| Iluminación LED | `6` | Salida digital | Iluminación ambiental suplementaria |

### 3.2 Distribución de pines

```text
Arduino Uno
│
├── A0 ── Sensor de temperatura TMP36
├── A1 ── Sensor de humedad del suelo
├── A2 ── Fotoresistencia LDR
├── D3 ── Ventilador
├── D4 ── Calefactor
├── D5 ── Bomba de riego
└── D6 ── Iluminación LED
```

---

## 4. Código fuente Arduino

> El siguiente programa escrito en C++ implementa la lógica de control ambiental para su simulación en Tinkercad y ejecución en Arduino Uno.

```cpp
/*
 * PROYECTO IoT: CONTROL DE INVERNADERO AUTOMATIZADO
 * Asignatura: SIY6122 - Problemáticas Globales y Prototipado
 * Evaluación: EP1
 * Sección: 002V
 * Duoc UC - Sede Plaza Norte
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
  // 2. LECTURA Y CONTROL DE HUMEDAD DEL SUELO
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

### 4.1 Resumen de variables

| Variable | Tipo | Función |
|:---|:---:|:---|
| `lectTemp` | `int` | Almacena la lectura analógica del sensor TMP36 |
| `tempC` | `float` | Guarda la temperatura convertida a grados Celsius |
| `lecturaHumedad` | `int` | Almacena la lectura del sensor de humedad |
| `humedadPct` | `int` | Guarda la humedad convertida a porcentaje |
| `nivelLuz` | `int` | Almacena la lectura de la fotoresistencia |
| `umbralLuzBaja` | `int` | Define el nivel mínimo de iluminación aceptable |

---

## 5. Validación y pruebas

> Se realizaron seis casos de prueba para comprobar el funcionamiento de los sensores y actuadores.

| Caso | Valores de entrada | Resultado esperado | Estado |
|:---|:---|:---|:---:|
| Temperatura alta | Temp = 28 °C, Hum = 50 %, Luz = 600 | Ventilador ON, calefactor OFF, riego OFF y LED OFF | ✅ PASÓ |
| Temperatura baja | Temp = 10 °C, Hum = 50 %, Luz = 600 | Calefactor ON, ventilador OFF, riego OFF y LED OFF | ✅ PASÓ |
| Humedad baja | Temp = 20 °C, Hum = 30 %, Luz = 600 | Bomba de riego ON, ventilador OFF y calefactor OFF | ✅ PASÓ |
| Humedad alta | Temp = 20 °C, Hum = 85 %, Luz = 600 | Bomba de riego OFF y ventilador ON | ✅ PASÓ |
| Oscuridad | Temp = 20 °C, Hum = 50 %, Luz = 200 | LED ON y los demás actuadores OFF | ✅ PASÓ |
| Estado normal | Temp = 20 °C, Hum = 55 %, Luz = 700 | Todos los actuadores OFF | ✅ PASÓ |

### 5.1 Resumen de resultados

```text
Pruebas ejecutadas: 6
Pruebas aprobadas:  6
Pruebas fallidas:   0
Resultado general:  SISTEMA VALIDADO
```

---

## 6. Conclusiones y próximos pasos

La implementación del sistema automatizado de control ambiental para invernaderos fue completada exitosamente.

Se logró:

- ✔️ Medir la temperatura ambiental mediante un sensor TMP36.
- ✔️ Automatizar la ventilación cuando la temperatura supera los 25 °C.
- ✔️ Automatizar la calefacción cuando la temperatura baja de 13 °C.
- ✔️ Controlar el riego según el porcentaje de humedad del suelo.
- ✔️ Activar la iluminación LED cuando existe poca luz ambiental.
- ✔️ Ejecutar continuamente el sistema con intervalos de 5 segundos.
- ✔️ Validar el funcionamiento mediante seis casos de prueba.

### 6.1 Eficiencia de control

El uso de una temporización de 5 segundos evita sobrecargas de procesamiento y fluctuaciones bruscas en las lecturas de los sensores.

### 6.2 Automatización integral

La integración simultánea de ventilación, calefacción, iluminación y riego automatizado reduce considerablemente la necesidad de intervención humana.

### 6.3 Proyección IoT futura

Como siguiente fase del proyecto, se propone incorporar un módulo de comunicación inalámbrica como un `ESP8266` o `ESP32`.

Este módulo permitiría:

- Enviar la telemetría del invernadero a una base de datos en la nube.
- Consultar la temperatura y humedad de manera remota.
- Visualizar métricas en tiempo real.
- Crear un dashboard interactivo.
- Generar alertas cuando se detecten condiciones críticas.
- Mantener un historial de las mediciones ambientales.

---

<div align="center">

**DUOC UC — Sede Plaza Norte**

**Problemáticas Globales y Prototipado — EP1 — Agosto 2026**

</div>
