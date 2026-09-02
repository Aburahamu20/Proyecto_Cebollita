<div align="center">

# Proyecto Cebollita — Fase 1

## Invernadero inteligente con micro:bit

![Badge](https://img.shields.io/badge/Asignatura-SIY6122-0078D4?style=for-the-badge)
![Badge](https://img.shields.io/badge/Evaluación-EP1-28a745?style=for-the-badge)
![Badge](https://img.shields.io/badge/Fase-Diseño%20conceptual-f0ad4e?style=for-the-badge)
![Badge](https://img.shields.io/badge/Plataforma-Tinkercad-e63946?style=for-the-badge)

| Campo | Detalle |
|:---|:---|
| **Institución** | Instituto Profesional Duoc UC |
| **Sede** | Plaza Norte |
| **Asignatura** | SIY6122 - Problemáticas Globales y Prototipado |
| **Evaluación** | Experiencia Práctica 1 (EP1) |
| **Sección** | 002V |
| **Experiencia** | Programando dispositivos para el IoT |
| **Proyecto** | Invernadero inteligente — Proyecto Cebollita |
| **Dispositivo obligatorio** | micro:bit |
| **Lenguaje de la futura implementación** | Python |
| **Plataforma** | Tinkercad Circuits |
| **Integrantes** | Abraham Castro Romero · Sebastian Fuentes Cortes · Lisandra Gonzalez Hernandez · Felipe Murua Lobos · Barbara Saavedra Fernandez |
| **Docente** | Marcos Antonio Perelli Henriquez |
| **Fecha** | Septiembre 2026 |

</div>

---

## 📋 Contenido

- [1. Alcance de la Fase 1](#1-alcance-de-la-fase-1)
- [2. Análisis del problema](#2-análisis-del-problema)
- [3. Solución propuesta](#3-solución-propuesta)
- [4. Entradas del sistema](#4-entradas-del-sistema)
- [5. Estados y umbrales](#5-estados-y-umbrales)
- [6. Salidas e interacción manual](#6-salidas-e-interacción-manual)
- [7. Regla de prioridad](#7-regla-de-prioridad)
- [8. Diseño lógico previo](#8-diseño-lógico-previo)
- [9. Pseudocódigo conceptual](#9-pseudocódigo-conceptual)
- [10. Revisión de requisitos](#10-revisión-de-requisitos)
- [11. Próxima fase](#11-próxima-fase)

---

## 1. Alcance de la Fase 1

Esta fase corresponde al análisis y diseño conceptual del primer prototipo. Su objetivo es definir cómo deberá comportarse el sistema antes de construir el circuito o escribir el programa.

En esta etapa:

- Se analiza el requerimiento del cliente.
- Se definen las entradas y salidas.
- Se proponen los estados `NORMAL`, `ADVERTENCIA` y `CRÍTICO`.
- Se establecen umbrales iniciales de temperatura, iluminación y humedad del suelo.
- Se define una interacción manual con el usuario.
- Se establece una regla de prioridad.
- Se elabora el diagrama y pseudocódigo previos.

> **Importante:** esta fase no incluye todavía el circuito final, código Python ni resultados de pruebas. Esos elementos deberán desarrollarse y comprobarse en las fases siguientes.

---

## 2. Análisis del problema

Una persona debe supervisar constantemente las condiciones ambientales del invernadero para detectar situaciones que puedan afectar el cultivo. Este método depende de la observación humana permanente y puede provocar que un problema sea detectado demasiado tarde.

El cliente necesita un primer prototipo de bajo costo que sea capaz de:

1. Observar la temperatura, el nivel de iluminación y, como función adicional, la humedad del suelo.
2. Evaluar automáticamente las condiciones ambientales.
3. Clasificar la situación en uno de tres estados.
4. Informar claramente el estado al encargado.
5. Permitir que el usuario consulte manualmente las mediciones.
6. Resolver correctamente la aparición simultánea de dos condiciones desfavorables.

Durante esta primera versión se trabajará con una sola planta y con las condiciones generales de su entorno.

---

## 3. Solución propuesta

Se propone utilizar una placa `micro:bit` dentro de Tinkercad Circuits. El dispositivo medirá la temperatura, el nivel de iluminación y la humedad del suelo, analizará las tres mediciones y mostrará en su matriz LED el estado general del invernadero.

La humedad se incorporará como una función adicional mediante una entrada analógica. Si Tinkercad no dispone del sensor específico, durante la simulación se podrá representar su lectura con un potenciómetro conectado al pin `P0`, utilizando una escala normalizada de 0 % a 100 %.

La solución tendrá tres estados:

- 😊 **NORMAL:** las condiciones no requieren intervención.
- ⚠️ **ADVERTENCIA:** existe una condición que debe observarse.
- ❌ **CRÍTICO:** existe una condición que requiere atención inmediata.

Los botones de la micro:bit permitirán consultar manualmente las mediciones sin detener la supervisión automática.

---

## 4. Entradas del sistema

| Entrada | Origen | Uso |
|:---|:---|:---|
| **Temperatura** | Sensor de temperatura disponible en la micro:bit | Detectar frío o calor fuera del rango definido |
| **Iluminación** | Lectura de luz de la micro:bit | Detectar iluminación insuficiente |
| **Humedad del suelo** | Sensor analógico o potenciómetro de simulación conectado a `P0` | Detectar tierra seca o exceso de humedad |
| **Botón A** | Interacción manual | Cambiar entre temperatura, iluminación y humedad |
| **Botón B** | Interacción manual | Mostrar el estado general y la condición que lo provoca |
| **Botones A+B** | Interacción manual | Mostrar consecutivamente las tres mediciones |

La temperatura, la iluminación y la humedad serán evaluadas continuamente, aunque el usuario no presione ningún botón.

---

## 5. Estados y umbrales

Los siguientes valores son umbrales iniciales para la simulación. El equipo deberá revisarlos y justificar su elección antes de considerarlos definitivos.

### 5.1 Temperatura

| Estado | Condición inicial propuesta |
|:---|:---|
| **NORMAL** | Entre 18 °C y 25 °C |
| **ADVERTENCIA** | Entre 14 °C y 17 °C, o entre 26 °C y 29 °C |
| **CRÍTICO** | Menor a 14 °C o igual/superior a 30 °C |

### 5.2 Iluminación

Para el prototipo se utilizará la escala de iluminación entregada por la micro:bit.

| Estado | Condición inicial propuesta |
|:---|:---|
| **NORMAL** | Nivel igual o superior a 120 |
| **ADVERTENCIA** | Nivel entre 60 y 119 |
| **CRÍTICO** | Nivel inferior a 60 |

### 5.3 Humedad del suelo

La lectura analógica se convertirá a una escala de 0 % a 100 % para facilitar su interpretación.

| Estado | Condición inicial propuesta |
|:---|:---|
| **NORMAL** | Entre 40 % y 70 % |
| **ADVERTENCIA** | Entre 25 % y 39 %, o entre 71 % y 85 % |
| **CRÍTICO** | Menor a 25 % o superior a 85 % |

### 5.4 Interpretación general

| Estado general | Significado |
|:---|:---|
| **NORMAL** | La temperatura, la iluminación y la humedad están dentro de los rangos aceptables |
| **ADVERTENCIA** | Al menos una medición está en advertencia y ninguna es crítica |
| **CRÍTICO** | Al menos una medición se encuentra en nivel crítico |

---

## 6. Salidas e interacción manual

### 6.1 Información automática

La matriz LED de la micro:bit mostrará permanentemente el estado general:

| Estado | Representación propuesta |
|:---|:---|
| **NORMAL** | Cara feliz o símbolo de aprobación |
| **ADVERTENCIA** | Signo de exclamación |
| **CRÍTICO** | Una X intermitente |

### 6.2 Interacción manual

| Acción del usuario | Respuesta del sistema |
|:---|:---|
| Presionar el botón A | Cambiar entre la temperatura, la iluminación y la humedad actuales |
| Presionar el botón B | Mostrar el estado general y la condición que lo provoca |
| Presionar A+B | Mostrar consecutivamente las tres mediciones |

Después de mostrar el dato solicitado, la micro:bit volverá automáticamente al indicador del estado general.

---

## 7. Regla de prioridad

La prioridad general será:

```text
CRÍTICO  >  ADVERTENCIA  >  NORMAL
```

Las reglas serán las siguientes:

1. Si cualquiera de las tres mediciones está en estado `CRÍTICO`, el sistema completo será `CRÍTICO`.
2. Si no existe una condición crítica, pero al menos una medición está en `ADVERTENCIA`, el sistema será `ADVERTENCIA`.
3. El sistema solamente será `NORMAL` cuando las tres mediciones sean normales.
4. Si varias condiciones presentan simultáneamente el mismo nivel desfavorable, el orden de información será: temperatura, humedad del suelo e iluminación.

Esta regla evita que una condición menos grave oculte un problema crítico.

---

## 8. Diseño lógico previo

```text
┌──────────────────────────────────────────────┐
│            INICIAR EL SISTEMA                │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ LEER TEMPERATURA, ILUMINACIÓN Y HUMEDAD       │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ ¿ALGUNA CONDICIÓN ESTÁ EN NIVEL CRÍTICO?     │
└───────────────┬───────────────────┬──────────┘
                │ SÍ                │ NO
                ▼                   ▼
┌──────────────────────────┐  ┌─────────────────────────────────────┐
│ ESTADO GENERAL: CRÍTICO  │  │ ¿ALGUNA CONDICIÓN ESTÁ EN          │
│ Mostrar X intermitente   │  │ NIVEL DE ADVERTENCIA?              │
└─────────────┬────────────┘  └────────────┬──────────────┬─────────┘
              │                           │ SÍ           │ NO
              │                           ▼              ▼
              │             ┌────────────────────────┐ ┌──────────────────────┐
              │             │ ESTADO: ADVERTENCIA    │ │ ESTADO: NORMAL       │
              │             │ Mostrar exclamación    │ │ Mostrar cara feliz   │
              │             └────────────┬───────────┘ └──────────┬───────────┘
              │                          │                        │
              └──────────────────────────┴────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────┐
│ ¿EL USUARIO PRESIONÓ UN BOTÓN?               │
└───────────────┬───────────────────┬──────────┘
                │ SÍ                │ NO
                ▼                   │
┌──────────────────────────────────┐│
│ MOSTRAR EL DATO SOLICITADO       ││
│ A: cambiar entre mediciones      ││
│ B: estado y causa                ││
│ A+B: mostrar las tres mediciones ││
└────────────────┬─────────────────┘│
                 │                  │
                 └─────────┬────────┘
                           │
                           ▼
┌──────────────────────────────────────────────┐
│ ESPERAR UN INTERVALO BREVE Y REPETIR         │
└──────────────────────┬───────────────────────┘
                       │
                       └──────────────► VOLVER A LEER
                                        LAS CONDICIONES
```

---

## 9. Pseudocódigo conceptual

```text
INICIAR sistema

REPETIR continuamente:
    LEER temperatura
    LEER iluminación
    LEER humedad del suelo

    CLASIFICAR estado de temperatura
    CLASIFICAR estado de iluminación
    CLASIFICAR estado de humedad

    SI temperatura es CRÍTICA
       O iluminación es CRÍTICA
       O humedad es CRÍTICA:
        estado general = CRÍTICO

    SINO, SI temperatura está en ADVERTENCIA
          O iluminación está en ADVERTENCIA
          O humedad está en ADVERTENCIA:
        estado general = ADVERTENCIA

    SINO:
        estado general = NORMAL

    MOSTRAR símbolo correspondiente al estado general

    SI se presiona el botón A:
        CAMBIAR entre temperatura, iluminación y humedad

    SI se presiona el botón B:
        MOSTRAR estado general y causa

    SI se presionan A y B:
        MOSTRAR las tres mediciones consecutivamente

    ESPERAR un intervalo breve
FIN REPETIR
```

---

## 10. Revisión de requisitos

| Requisito | Cómo se considera en la Fase 1 | Estado |
|:---|:---|:---:|
| R1: utilizar micro:bit | Se definió como dispositivo del prototipo | ✅ Diseñado |
| R2: utilizar Python | Se estableció para la futura implementación | ⏳ Fase 2 |
| R3: temperatura e iluminación | Ambas entradas están incluidas | ✅ Diseñado |
| Función extra: humedad del suelo | Se añadió una entrada analógica y sus umbrales | ✅ Diseñado |
| R4: tres estados | Se definieron NORMAL, ADVERTENCIA y CRÍTICO | ✅ Diseñado |
| R5: cambio automático | La lógica evalúa continuamente las mediciones | ✅ Diseñado |
| R6: identificar estado | Se definieron símbolos en la matriz LED | ✅ Diseñado |
| R7: interacción manual | Se asignaron funciones a los botones A, B y A+B | ✅ Diseñado |
| R8: regla de prioridad | Se definió CRÍTICO > ADVERTENCIA > NORMAL | ✅ Diseñado |
| R9: simulación en Tinkercad | Todavía no ejecutada | ⏳ Fase 2 |
| R10: realizar pruebas | Todavía no ejecutadas | ⏳ Fase posterior |

---

## 11. Próxima fase

En la Fase 2 se deberá:

1. Crear el circuito con micro:bit en Tinkercad Circuits.
2. Confirmar cómo se simulan la temperatura, la iluminación y la humedad del suelo.
3. Convertir este diseño lógico en un programa Python.
4. Verificar que la matriz LED muestre correctamente los tres estados.
5. Probar la consulta de las tres mediciones con los botones A, B y A+B.
6. Corregir errores antes de completar la tabla oficial de pruebas.
7. Registrar el prompt principal y las consultas posteriores realizadas a la IA.

> Los umbrales y el comportamiento propuestos en esta fase deben ser revisados por todo el equipo antes de comenzar la programación.

---

<div align="center">

**DUOC UC — Sede Plaza Norte**

**SIY6122 · Problemáticas Globales y Prototipado · EP1 · Fase 1**

</div>
