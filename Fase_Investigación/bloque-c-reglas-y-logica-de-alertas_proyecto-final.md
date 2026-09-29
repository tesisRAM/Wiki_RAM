# BLOQUE C — REGLAS Y LÓGICA DE ALERTAS

## 1. Propósito del bloque

El Bloque C especifica la lógica que utilizará el motor de detección de la plataforma central para transformar los eventos sintéticos generados por las sedes hospitalarias simuladas en alertas útiles para ejercicios de vigilancia de resistencia antimicrobiana (RAM).

El bloque conecta cuatro elementos:

**datos generados → interpretación microbiológica → contexto epidemiológico/calidad/operación → alerta**

El sistema no diagnostica pacientes, no recomienda tratamientos y no pretende representar la situación epidemiológica real de hospitales de Bogotá. Las alertas se generan sobre datos sintéticos y escenarios controlados.

---

# 2. Fuentes y papel de cada una

## 2.1 Propuesta del proyecto

La propuesta establece que cada agente representa una sede hospitalaria hipotética y genera eventos sintéticos configurables. La plataforma central valida, armoniza y transforma esos eventos a un modelo canónico que conserva su procedencia. Sobre esos datos funciona un motor extensible de detección.

Las variables se agrupan en:

- Institucionales
- Contextuales
- Microbiológicas
- Temporales
- De calidad
- De transmisión

Cada variable debe tener dominio, fuente, regla de generación y relación con el escenario.

## 2.2 CLSI/EUCAST 2025

Los documentos técnicos de CLSI/EUCAST sirven principalmente para interpretar los resultados microbiológicos.

Se utilizarán como referencia para:

- Interpretación S/I/R según el estándar vigente.
- Cambios de puntos de corte.
- Identificación de resultados que requieren pruebas complementarias.
- Escenarios relacionados con resistencia a carbapenémicos.
- Interpretación de pruebas de carbapenemasa.
- Diferenciación entre puntos de corte clínicos y valores epidemiológicos.

En Enterobacterales resistentes a carbapenémicos, el material revisado indica que debe evaluarse la presencia de carbapenemasa mediante métodos fenotípicos y/o moleculares. Entre los métodos descritos están Carba-NP y mCIM/eCIM.

La lógica del sistema debe conservar la versión del estándar utilizada para interpretar el resultado.

**Importante:** CLSI/EUCAST define el significado microbiológico de un resultado; no define por sí solo que exista un brote o una alerta epidemiológica.

## 2.3 WHONET

WHONET sirve como referencia para la estructura y análisis de los datos microbiológicos.

Entre las variables relevantes están:

- Identificador del paciente sintético.
- Edad o rango de edad.
- Sexo cuando sea pertinente.
- Institución.
- Ubicación/servicio.
- Tipo de muestra.
- Microorganismo.
- Fecha.
- Antimicrobiano.
- Resultado de susceptibilidad.
- Método.
- MIC/CIM o diámetro de halo cuando aplique.
- Campos complementarios para mecanismos o pruebas.

También se consideran campos como APB y EDTA para escenarios de vigilancia de resistencia.

El manual de WHONET revisado corresponde a una versión anterior, por lo que se utilizará como referencia de estructura y flujo de trabajo, no como autoridad para reemplazar los puntos de corte clínicos actuales.

---

# 3. Arquitectura lógica del motor de alertas

El procesamiento propuesto es:

```text
AGENTE HOSPITALARIO
       |
       v
EVENTO SINTÉTICO
       |
       v
VALIDACIÓN DE CALIDAD
       |
       +---- inválido/duplicado/incompleto ----> ALERTA DE CALIDAD
       |
       v
NORMALIZACIÓN
       |
       v
INTERPRETACIÓN MICROBIOLÓGICA
       |
       v
IDENTIFICACIÓN DE FENOTIPO / SEÑAL
       |
       +---- señal de vigilancia
       |
       v
CONTEXTO TEMPORAL Y EPIDEMIOLÓGICO
       |
       +---- cambio respecto a línea base
       +---- nuevo microorganismo relevante
       +---- cambio de perfil de resistencia
       +---- escenario controlado de brote
       |
       v
MOTOR DE REGLAS
       |
       +---- EPIDEMIOLÓGICA
       +---- CALIDAD
       +---- OPERACIONAL
       |
       v
ALERTA
       |
       v
DASHBOARD + TRAZABILIDAD
```

---

# 4. Tipos de alertas

## 4.1 Alertas epidemiológicas

Detectan cambios relevantes en los datos de resistencia o en los microorganismos de vigilancia.

Ejemplos:

- Aumento de resistencia respecto a la línea base de una sede/servicio.
- Cambio del perfil de resistencia de un microorganismo.
- Aparición de un microorganismo de interés que no se encontraba previamente en el escenario.
- Incremento controlado de un fenotipo de vigilancia.
- Aparición de casos relacionados temporal y espacialmente en un escenario configurado como brote.

La propuesta del proyecto establece que las alertas epidemiológicas pueden señalar aumentos simulados en combinaciones microorganismo–antimicrobiano.

## 4.2 Alertas de calidad

Detectan problemas en los datos.

Ejemplos:

- Campo obligatorio faltante.
- Valor inválido.
- Microorganismo no reconocido.
- Antimicrobiano incompatible con el modelo.
- Registro duplicado.
- Resultado de susceptibilidad incompatible con el método.
- Inconsistencia temporal.
- Evento sin procedencia identificable.

Estas alertas no significan que exista resistencia ni brote. Indican que la información necesita revisión.

## 4.3 Alertas operacionales

Detectan problemas en la comunicación o ejecución de los agentes.

Ejemplos:

- Retraso en la transmisión.
- Interrupción de un agente.
- Pérdida de eventos.
- Falta de reporte durante un periodo configurado.
- Error de comunicación entre agente y plataforma central.

---

# 5. Reglas microbiológicas

## R-MIC-01 — Interpretación de susceptibilidad

Todo resultado S/I/R deberá interpretarse utilizando la versión del estándar microbiológico configurada para el escenario.

**Entrada:** microorganismo + antimicrobiano + resultado + método + estándar.

**Salida:** interpretación normalizada.

**Condición:** si el resultado no puede interpretarse con el estándar configurado, generar una alerta de calidad o estado pendiente de validación.

---

## R-MIC-02 — Fenotipo de vigilancia

El sistema deberá identificar los fenotipos de vigilancia definidos por la configuración validada del proyecto.

Los documentos revisados incluyen, entre otros:

- Staphylococcus aureus resistente a oxacilina.
- Staphylococcus epidermidis resistente a oxacilina en los servicios pediátricos/neonatales indicados.
- Enterococcus faecalis/faecium resistente a vancomicina.
- Enterobacterales de interés resistentes a antimicrobianos definidos para vigilancia.
- Pseudomonas aeruginosa resistente a antimicrobianos definidos para vigilancia.
- Acinetobacter baumannii resistente a imipenem o meropenem.

La lista definitiva deberá quedar versionada y validada con el profesional de microbiología/epidemiología antes de fijarla como regla del sistema.

---

## R-MIC-03 — Resistencia a carbapenémicos

Cuando un Enterobacterales presente resistencia a carbapenémicos definidos en la configuración, el sistema deberá registrar una señal para evaluación de carbapenemasa.

La alerta del sistema no debe asumir automáticamente un mecanismo específico.

Debe distinguir:

1. Resistencia observada.
2. Prueba de carbapenemasa solicitada.
3. Resultado de prueba.
4. Mecanismo confirmado, si existe.
5. Estado pendiente.

---

## R-MIC-04 — Pruebas de carbapenemasa

Para escenarios configurados con pruebas fenotípicas, podrán registrarse:

- Carba-NP.
- mCIM.
- eCIM.
- APB.
- EDTA.

La lógica debe permitir estados como:

```text
NO REALIZADA
PENDIENTE
NEGATIVA
POSITIVA
INCONCLUSIVA
CONFIRMADA POR MÉTODO COMPLEMENTARIO
```

El resultado de una prueba no deberá convertirse automáticamente en una afirmación clínica que no esté respaldada por la configuración validada.

---

## R-MIC-05 — Señales KPC/MBL

Los escenarios de prueba pueden representar combinaciones de resultados descritas por el material técnico de CLSI/EUCAST/LNR.

Por ejemplo:

- mCIM positivo + eCIM negativo → señal compatible con carbapenemasa de serina, que requiere interpretación adicional.
- mCIM positivo + eCIM positivo → señal compatible con metalo-beta-lactamasa.

Estas señales deben conservarse como resultados microbiológicos y no como diagnóstico clínico.

---

## R-MIC-06 — Cambios de puntos de corte

Si cambia el estándar o un punto de corte:

1. Se registra la versión del estándar.
2. Se conserva el resultado original.
3. Se almacena la interpretación utilizada.
4. Se permite reprocesar el escenario si se requiere.
5. Se evita comparar directamente resultados interpretados bajo reglas incompatibles sin marcar la diferencia.

Esto es especialmente importante para escenarios donde CLSI 2025 modificó puntos de corte.

---

# 6. Reglas de línea base

La línea base representa el comportamiento esperado **dentro del escenario simulado**, no la situación real de Bogotá.

Debe poder definirse por:

- Sede.
- Servicio.
- Microorganismo.
- Antimicrobiano.
- Periodo.
- Población configurada.

Ejemplo:

```text
Sede A
Servicio: UCI
Microorganismo: K. pneumoniae
Antimicrobiano: meropenem
Periodo: mensual
Línea base: comportamiento histórico del escenario
```

La regla compara el comportamiento actual contra esa línea base.

## R-BASE-01 — Cambio respecto a línea base

```text
SI comportamiento_actual
   supera la condición configurada
   respecto a la línea_base
ENTONCES generar señal epidemiológica
```

La condición numérica exacta no deberá inventarse. Debe quedar como parámetro configurable y ser validada por el equipo del proyecto y el experto del dominio.

---

# 7. Reglas de cambio de perfil

Una alerta no depende únicamente de que exista un resultado resistente.

Debe poder detectarse un cambio en la combinación de resistencias.

Ejemplo:

```text
Periodo anterior:
K. pneumoniae → R a A + S a B

Periodo actual:
K. pneumoniae → R a A + R a B
```

Esto puede generar una señal de:

**CAMBIO DE PERFIL DE RESISTENCIA**

La comparación debe considerar:

- microorganismo;
- servicio;
- periodo;
- antimicrobianos;
- número de casos;
- distribución de S/I/R;
- configuración del escenario.

---

# 8. Reglas de nuevo microorganismo

## R-EPI-01

Si aparece por primera vez un microorganismo definido como relevante dentro del ámbito configurado:

```text
SI casos_actuales > 0
Y casos_históricos = 0
Y microorganismo ∈ lista_de_vigilancia
ENTONCES generar señal de nuevo microorganismo
```

Debe registrarse:

- sede;
- servicio;
- fecha;
- muestra;
- microorganismo;
- primer evento detectado;
- procedencia;
- escenario que originó el evento.

---

# 9. Reglas de pseudobrote

El sistema debe contemplar escenarios donde el aumento observado no representa un aumento real del fenómeno simulado.

Ejemplos:

- Cambio controlado de sensibilidad de detección.
- Cambio de definición.
- Modificación del estándar.
- Incremento artificial del número de muestras.
- Cambio de servicio reportante.
- Duplicación de registros.
- Cambio en la frecuencia de transmisión.

Por esto:

```text
AUMENTO OBSERVADO
       |
       v
¿Cambio en el proceso de generación/transmisión?
       |
      SÍ
       |
       v
POSIBLE PSEUDOBROTE
       |
       v
ALERTA PARA REVISIÓN
```

El sistema debe distinguir entre:

**evento detectado** ≠ **brote confirmado**

---

# 10. Reglas de calidad

## R-CAL-01 — Campos obligatorios

Si falta un campo obligatorio:

```text
SI campo_obligatorio = NULL
ENTONCES ALERTA_CALIDAD
```

## R-CAL-02 — Duplicados

Si dos eventos tienen una combinación de identificadores definida como duplicada:

```text
SI evento_A ≈ evento_B
ENTONCES ALERTA_CALIDAD_DUPLICADO
```

La definición exacta de duplicado debe quedar en el diccionario de datos.

## R-CAL-03 — Valores inválidos

Ejemplos:

- edad fuera del dominio configurado;
- fecha inválida;
- microorganismo inexistente en el vocabulario;
- antimicrobiano no reconocido;
- resultado diferente de S/I/R cuando ese campo exige categorías;
- método incompatible.

## R-CAL-04 — Procedencia

Todo evento aceptado debe conservar:

```text
agente_origen
sede
fecha_recepcion
fecha_evento
escenario
identificador_evento
```

Esto permite rastrear una alerta hasta los casos sintéticos que la originaron.

---

# 11. Reglas operacionales

## R-OP-01 — Retraso

```text
SI fecha_recepcion - fecha_evento
   > retraso_configurado
ENTONCES ALERTA_OPERACIONAL
```

## R-OP-02 — Silencio de agente

```text
SI agente_configurado = ACTIVO
Y no recibe eventos durante el periodo esperado
ENTONCES ALERTA_OPERACIONAL
```

## R-OP-03 — Interrupción

Si el agente deja de transmitir durante una simulación configurada:

```text
estado_agente = INTERRUMPIDO
→ ALERTA_OPERACIONAL
```

---

# 12. Niveles de análisis

El motor puede analizar las alertas en tres niveles:

### Nivel paciente/caso

Se refiere a un evento sintético individual.

Ejemplo:

> K. pneumoniae + meropenem R en un caso de UCI.

### Nivel hospital/sede

Agrupa eventos de una sede hipotética.

Ejemplo:

> Incremento de K. pneumoniae resistente a meropenem en la UCI de la Sede A.

### Nivel agregado

Combina información de varias sedes simuladas.

Ejemplo:

> Incremento del mismo patrón en varias sedes durante el mismo periodo.

Estos niveles permiten probar escenarios de diferente escala sin afirmar que representan hospitales reales.

---

# 13. Prioridad de las alertas

El proyecto puede implementar una prioridad interna para ordenar el trabajo del usuario, pero esta escala debe considerarse una **decisión de diseño del proyecto**, no una clasificación oficial mientras no haya sido validada.

Propuesta configurable:

```text
ALTA
→ requiere revisión prioritaria

MEDIA
→ requiere seguimiento

BAJA
→ requiere revisión rutinaria
```

La prioridad deberá depender de parámetros documentados, por ejemplo:

- magnitud del cambio;
- número de eventos;
- servicio;
- microorganismo;
- resistencia observada;
- alcance de la señal;
- persistencia temporal;
- calidad de la evidencia.

No se debe presentar esta escala como normativa oficial.

---

# 14. Cápsula de alerta

Toda alerta deberá contener información suficiente para ser interpretada y rastreada.

```text
ID de alerta
Tipo de alerta
Prioridad interna
Fecha de detección

Sede
Servicio
Periodo

Microorganismo
Muestra
Antimicrobiano
Resultado S/I/R
Método
MIC/CIM/halo, si aplica

Prueba complementaria
Resultado
Mecanismo confirmado, si aplica

Línea base utilizada
Condición que activó la regla
Número de eventos afectados

Escenario de simulación
Agente origen
Eventos relacionados

Estado:
- Nueva
- En revisión
- Confirmada
- Cerrada

Observaciones
```

---

# 15. Flujo completo del motor

```text
1. RECIBIR EVENTO
        ↓
2. VALIDAR ESTRUCTURA
        ↓
3. VALIDAR CALIDAD
        ↓
4. NORMALIZAR DATOS
        ↓
5. INTERPRETAR SUSCEPTIBILIDAD
        ↓
6. IDENTIFICAR FENOTIPO / SEÑAL
        ↓
7. EVALUAR PRUEBAS COMPLEMENTARIAS
        ↓
8. COMPARAR CON LÍNEA BASE
        ↓
9. ANALIZAR TIEMPO / SERVICIO / SEDE
        ↓
10. EVALUAR PSEUDOBROTE
        ↓
11. APLICAR REGLAS
        ↓
12. GENERAR ALERTA
        ↓
13. ASIGNAR PRIORIDAD INTERNA
        ↓
14. GUARDAR TRAZABILIDAD
        ↓
15. MOSTRAR EN DASHBOARD
```

---

# 16. Escenarios sintéticos para verificar las reglas

El sistema deberá permitir configurar escenarios cuya verdad de referencia sea conocida.

## Escenario 1 — Normal

Los agentes generan casos dentro de los parámetros esperados.

**Esperado:** ninguna alerta epidemiológica.

## Escenario 2 — Incremento de resistencia

Se incrementa artificialmente la frecuencia de resistencia de una combinación microorganismo–antimicrobiano.

**Esperado:** alerta epidemiológica.

## Escenario 3 — Nuevo microorganismo

Se introduce un microorganismo relevante que no había aparecido en el periodo anterior.

**Esperado:** señal de nuevo microorganismo.

## Escenario 4 — Cambio de perfil

Se modifica el patrón de resistencia de una población simulada.

**Esperado:** alerta de cambio de perfil.

## Escenario 5 — Datos incompletos

Se eliminan campos obligatorios.

**Esperado:** alerta de calidad.

## Escenario 6 — Duplicados

Se transmiten registros repetidos.

**Esperado:** alerta de calidad.

## Escenario 7 — Retraso

Un agente transmite los eventos fuera del tiempo configurado.

**Esperado:** alerta operacional.

## Escenario 8 — Interrupción

Un agente deja de transmitir.

**Esperado:** alerta operacional.

## Escenario 9 — Pseudobrote

Se aumenta el número aparente de casos mediante una modificación controlada del proceso de generación/transmisión.

**Esperado:** el sistema detecta la anomalía y conserva el contexto para diferenciarla de un aumento epidemiológico simulado.

## Escenario 10 — Carbapenemasa pendiente

Se genera resistencia a carbapenémicos y la prueba complementaria queda pendiente.

**Esperado:** señal microbiológica con estado PENDIENTE, sin afirmar un mecanismo no confirmado.

---

# 17. Métricas de verificación

Como el escenario generado tiene una verdad de referencia conocida, se podrán calcular:

### Aciertos

Alertas que el sistema debía generar y efectivamente generó.

### Omisiones

Eventos que debían producir alerta pero no fueron detectados.

### Falsas alertas

Alertas generadas cuando el escenario no las contemplaba.

### Tiempo de procesamiento

Tiempo entre recepción del evento y generación de la alerta.

### Escalabilidad

Comportamiento al aumentar el número de agentes concurrentes.

Estas métricas están alineadas con el planteamiento de verificación de la propuesta.

---

# 18. Trazabilidad de una alerta

Cada alerta deberá poder responder:

```text
¿Por qué se generó?
       ↓
¿Qué regla se activó?
       ↓
¿Qué datos la activaron?
       ↓
¿De qué agente llegaron?
       ↓
¿De qué sede simulada?
       ↓
¿A qué escenario pertenecían?
       ↓
¿Qué estándar/regla microbiológica se utilizó?
       ↓
¿Qué eventos originales están relacionados?
```

Esto es fundamental para que el sistema sea verificable.

---

# 19. Separación de responsabilidades

| Componente | Responsabilidad |
|---|---|
| Agente hospitalario | Generar y transmitir eventos sintéticos |
| Plataforma central | Recibir, validar y normalizar |
| CLSI/EUCAST | Referencia para interpretación microbiológica |
| WHONET | Referencia para estructura/análisis de datos |
| Motor de reglas | Detectar señales y generar alertas |
| Línea base | Representar comportamiento esperado del escenario |
| Dashboard | Presentar alertas e indicadores |
| Usuario de vigilancia | Revisar los resultados |
| Experto de dominio | Validar variables, reglas y parámetros |

---

# 20. Lo que NO debe hacer el motor

El sistema no deberá:

- Diagnosticar pacientes.
- Recomendar antibióticos.
- Determinar tratamientos.
- Afirmar que un hospital real tiene un brote.
- Utilizar datos personales reales.
- Presentar los datos sintéticos como estadísticas reales de Bogotá.
- Convertir automáticamente una resistencia en un brote.
- Afirmar un mecanismo de resistencia sin el resultado que lo respalde.
- Tratar una prioridad interna del proyecto como una categoría normativa oficial.

---

# 21. Información que debe quedar pendiente de validación

Antes de implementar las reglas definitivas se debe validar con el profesional de microbiología/epidemiología:

1. Lista definitiva de microorganismos de vigilancia.
2. Lista definitiva de antimicrobianos.
3. Versión de CLSI/EUCAST que utilizará el sistema.
4. Variables obligatorias.
5. Definición de duplicado.
6. Periodo utilizado para línea base.
7. Fórmulas o umbrales de cambio.
8. Definición operacional de pseudobrote.
9. Tratamiento de resultados pendientes.
10. Reglas específicas para carbapenemasas.
11. Prioridad interna de alertas.
12. Frecuencia de procesamiento y reporte.
13. Reglas específicas de agregación entre sedes.

---

# 22. Resumen conceptual

La lógica completa del proyecto puede resumirse así:

```text
DATOS SINTÉTICOS
      ↓
VALIDACIÓN
      ↓
NORMALIZACIÓN
      ↓
INTERPRETACIÓN MICROBIOLÓGICA
      ↓
CONTEXTO
      ↓
COMPARACIÓN CON LÍNEA BASE
      ↓
REGLAS
      ↓
ALERTA
      ↓
TRAZABILIDAD
      ↓
DASHBOARD
```

La frase guía del Bloque C es:

> **“El sistema no decide clínicamente; detecta y documenta señales configuradas sobre datos sintéticos para apoyar ejercicios de vigilancia.”**
