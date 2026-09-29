# Documentos y datos para alertas hospitalarias de resistencia antimicrobiana

**Investigación técnica para tesis de Ingeniería de Sistemas**  
**Destinatario:** Andrés Ortiz  
**Fecha de consulta:** 27 de septiembre de 2026  
**Prioridad:** Colombia y Bogotá; referencias internacionales para completar vacíos.

## 1. Resultado principal y alcance de la evidencia

La entrada de un sistema hospitalario de alertas no debería reducirse a una fotografía o PDF del antibiograma. El modelo necesita relacionar **paciente, episodio de atención, muestra, aislamiento, pruebas de susceptibilidad y versiones del resultado**. Para alertas sobre tratamiento debe incorporar prescripción y administración; para señales epidemiológicas, ubicación e historia temporal. Esta es una propuesta de ingeniería derivada de las fuentes, no una especificación existente del Hospital Universitario San Ignacio (HUSI).

Se distinguen cuatro niveles:

- **CO:** dato, documento o procedimiento explícitamente documentado por una fuente colombiana. No significa que todos los hospitales lo capturen en el mismo sistema.
- **INT:** referencia internacional original. No se presenta como obligación colombiana.
- **H:** implementación que solo puede verificarse con el hospital.
- **DISEÑO:** recomendación propuesta para el software; requiere validación institucional.

La revisión priorizó textos originales de Ministerio de Salud, INS, Secretaría Distrital de Salud (SDS), HUSI, OMS, OPS, WHONET, CLSI, EUCAST, CDC y publicaciones científicas. Se verificó la fecha dentro de los documentos cuando estuvo disponible: la fecha que muestra un buscador puede ser de indexación y no de publicación. Es una revisión documental técnica, no una revisión sistemática clínica con protocolo PRISMA.

**Límites explícitos:** no se obtuvo un informe clínico actual del HUSI, una exportación de su laboratorio ni su diccionario de datos. La SDS tiene un documento indexado como `Lineam_RAM_2026.pdf`, pero su descarga devolvió HTTP 403: no se atribuyen requisitos a su contenido no leído. El enlace del protocolo INS de resistencia bacteriana de 2022 devolvió 404 en descarga directa. Se empleó el instructivo WHONET del INS de junio de 2022, que sí pudo descargarse, y los criterios de remisión de marzo de 2026. [S03–S05, S09]

## 2. Qué documentos intervienen

Este inventario separa productos que suelen confundirse. Los responsables y usos constituyen una síntesis funcional; la asignación exacta de cargos y permisos es H.

| Documento o registro | Contenido y función | Productor principal | Destinatario o usuario | Base |
|---|---|---|---|---|
| Orden de examen y solicitud microbiológica | Qué estudiar, a quién y con qué muestra/contexto | Médico solicitante; toma de muestras completa datos | Microbiología | INT, S15; CO, S07 |
| Registro de recepción y procesamiento | Identificación, recepción, calidad y seguimiento de la muestra | Laboratorio | Personal técnico y calidad | INT, S15 |
| Informe individual de microbiología | Resultado del cultivo/identificación y, cuando corresponde, susceptibilidad | Microbiología autorizada | Equipo tratante; PROA; control de infecciones según hallazgo | CO, S01; INT, S12–S15 |
| Antibiograma individual | Respuesta de un aislamiento frente a antimicrobianos | Microbiología | Médico e infectología/PROA | CO, S03; INT, S12–S14 |
| Informe de mecanismo de resistencia | Prueba adicional, técnica, resultado y alcance confirmatorio | Laboratorio local, LSP o INS | Microbiología, PROA y vigilancia | CO, S03 |
| Registro de comunicación de resultado crítico | A quién se avisó y cuándo | Laboratorio | Solicitante y responsables definidos | INT, S15; CO, S06 |
| Exportación del equipo/LIS y base WHONET | Registros estructurados de aislamientos y sensibilidad | Laboratorio; transformación técnica | Analistas y vigilancia | CO, S04; INT, S16 |
| Antibiograma acumulado o perfil institucional | Estadísticas de susceptibilidad por población y periodo | Microbiología con PROA/epidemiología | Prescriptores, PROA y dirección | INT, S19 |
| Prescripción, preautorización y seguimiento PROA | Indicación, tratamiento y revisión profesional | Médico, infectología y farmacia | Tratante, farmacia y PROA | CO, S02 |
| Registro de administración y consumo | Exposición individual o cantidades agregadas | Enfermería/farmacia | PROA y vigilancia del consumo | CO, S10; INT, S23 |
| Investigación de IAAS/brote | Casos, relación epidemiológica y clasificación | Control de infecciones/epidemiología | Hospital, entidad territorial e INS | CO, S01, S06 |
| Ficha de remisión y orden SIVILAB-LabMuestras | Identifica el aislamiento enviado a referencia | UPGD y LSP; recepción INS | LSP y Laboratorio Nacional de Referencia | CO, S03 |

**No son equivalentes:** informe clínico, archivo WHONET, ficha de remisión de aislamiento y notificación de un evento de vigilancia. Tampoco debe suponerse que todos los antibiogramas individuales se transmiten como una misma ficha de Sivigila.

## 3. Formato real del antibiograma

### 3.1 Antibiograma individual

Es el resultado de susceptibilidad de **un aislamiento**, no una propiedad permanente del paciente ni de toda la especie bacteriana. Un mismo cultivo puede contener varios aislamientos; cada uno requiere sus resultados asociados. WHONET documenta tanto el registro por aislamiento como exportaciones con una fila por antibiótico. [S14, S16]

Se localizaron tres ejemplos verificables:

**A. Colombia: ficha oficial INS, versión 08.** Está reproducida en el anexo 1 del documento de marzo de 2026, página 26 del PDF —numeración impresa 25—. Su bloque de susceptibilidad contiene método, antibiótico, resultado con unidades e interpretación. Tiene apartados diferentes para paciente, aislamiento y confirmación. Es una **ficha de remisión**, no una plantilla nacional única del informe clínico. [S03]

**B. Informe de ejemplo de ARUP Laboratories, código 2008476.** Publicado por el propio laboratorio; usa identificadores ficticios. Presenta paciente, episodio, médico, muestra rectal, cultivo, organismo, susceptibilidad por antibiótico y fechas de verificación. El ejemplo muestra *E. coli*: ciprofloxacina con CIM ≥4 µg/mL y clasificación resistente; meropenem con CIM ≤0,5 µg/mL y clasificación susceptible. Son valores de ese ejemplo, no puntos de corte ni recomendaciones de tratamiento. [S12]

**C. Informe clínico imprimible de WHONET.** El tutorial oficial muestra institución, paciente, ubicación, muestra, organismo, antibióticos, categoría y diámetro en milímetros. Es una demostración de formato de 2006; sus resultados y reglas no deben implementarse como criterios de 2026. [S14]

Enlaces para inspeccionar los originales:

- [Ficha INS 2026, anexo 1](https://www.ins.gov.co/BibliotecaDigital/criterios-para-envio-de-aislamientos-bacterianos-y-levaduras-del-genero-candida-y-generos-relacionados-recuperados-en-iaas-para-confirmacion-de-mecanismos-de-ra-2026.pdf#page=26).
- [Informe publicado por ARUP](https://ltd.aruplab.com/api/ltd/examplereport?report=2008476%2C%20Positive.pdf).
- [Imagen original del informe demostrativo WHONET](https://whonet.org/WebDocs/WHONET%203.Data%20entry_files/image010.jpg).

### 3.2 Cómo interpretar sus campos

| Elemento | Significado técnico | Consecuencia para el software |
|---|---|---|
| Microorganismo | Género/especie identificada | La interpretación depende del organismo |
| Antimicrobiano | Sustancia o combinación evaluada | Conservar código y nombre; no confundir componentes |
| CIM o MIC | Concentración mínima que inhibe crecimiento bajo el método utilizado | Guardar valor, comparador y unidades |
| Diámetro del halo | Medición de difusión en disco, en mm | No mezclar con CIM |
| Categoría interpretativa | Resultado categórico bajo un estándar | Conservar la categoría original |
| Método | Procedimiento con que se obtuvo la medición | Permite evaluar comparabilidad y procedencia |
| Norma y versión | Reglas y puntos de corte aplicados | Evita reinterpretaciones históricas silenciosas |
| Comentario | Aclaración del laboratorio | No sustituirlo por una inferencia automática |

Estas definiciones se apoyan en WHONET/BacLink, CLSI y EUCAST; la tercera columna es DISEÑO. [S16, S18, S20–S21]

**S/I/R no significa lo mismo bajo todos los estándares.** EUCAST define I como susceptible con mayor exposición; desaconseja agrupar I y R como si ambas fueran resistencia. CLSI conserva consideraciones propias para I y categorías como SDD. El sistema debe admitir las categorías emitidas por el laboratorio sin forzarlas a un booleano sensible/resistente. [S20–S21]

CLSI M100, edición 36, fue publicado el 26 de enero de 2026; su página identifica correcciones de julio de 2026. Esta investigación verificó la edición y su función, no transcribió todas las tablas de pago. Antes de programar umbrales hay que confirmar la edición realmente implementada en el hospital y sus actualizaciones. [S18]

**Quién lo genera y quién lo recibe.** El profesional de microbiología identifica el organismo y su perfil; el instrumento produce mediciones que requieren el proceso de revisión y autorización del laboratorio. El receptor asistencial es el equipo solicitante/tratante; los hallazgos seleccionados también alimentan PROA y control de infecciones. No se encontró evidencia pública que permita fijar el canal, firma, cargo validador o tiempo de entrega del HUSI. [S01, S15]

### 3.3 Antibiograma acumulado

Es una tabla poblacional: organismo, número de aislamientos y proporciones de susceptibilidad por antimicrobiano, con periodo y población definidos. CLSI M39 orienta su construcción para apoyar principalmente la terapia empírica. Advierte que la selección del primer aislamiento por paciente/especie/periodo puede ocultar resistencia emergente relevante para otros análisis. [S19]

**Decisión de diseño:** conservar todos los resultados originales y aplicar deduplicación en cada análisis, no eliminarlos de la base clínica. Registrar el denominador efectivo por combinación organismo-antimicrobiano y documentar exclusiones. Una tasa institucional no sustituye el resultado de un paciente ni demuestra que dos aislamientos pertenezcan al mismo brote.

## 4. PROA en Colombia: marco identificado y datos utilizados

La **Resolución 2471 del 9 de diciembre de 2022**, con su anexo técnico, es el marco nacional identificado para implementar IAAS y PROA. La búsqueda no encontró una norma posterior que la sustituyera; la Resolución 914 de 2025 todavía la cita. Esto documenta continuidad normativa, sin convertir una búsqueda negativa en certificación jurídica absoluta. [S01, S11]

El anexo de 2471 asigna a microbiología informes individuales y agregados, perfiles de susceptibilidad y avisos sobre multirresistencia o patrones inusuales. Incluye antibiogramas estratificados, reporte selectivo y alarmas en historia clínica con comunicación al PROA. Entre los hallazgos sugeridos figuran bacteriemias, BLEE, AmpC, resistencia a meticilina, KPC, determinados perfiles de *P. aeruginosa* y cultivos positivos de sitios estériles. Véanse hojas 17–18 y 28. **No constituye una tabla universal e inmutable de reglas informáticas.** [S01]

El documento técnico complementario de Minsalud/ACIN localizado está fechado **julio de 2019**. No debe citarse como una guía nueva de 2025 por la fecha del buscador. Incluye un formato propuesto de prescripción/preautorización y ajustes basados en contexto clínico, peso, función renal y CIM. También contempla DDD, DOT y seguimiento de ajustes según microbiología. Véanse páginas 44–47 y 52–54 del PDF. [S02]

Para ingeniería, conviene separar estas necesidades:

| Necesidad del PROA | Entradas que deben vincularse | Producto de software propuesto |
|---|---|---|
| Revisar un resultado resistente relevante | Resultado validado, especie, muestra, paciente y tratamiento activo | Caso para revisión por PROA |
| Revisar pertinencia del antimicrobiano | Indicación, diagnóstico, tratamiento, microbiología y guía local | Lista de revisión con evidencia |
| Evaluar oportunidad del estudio | Hora de toma y primera administración | Indicador temporal con datos faltantes visibles |
| Evaluar el cambio terapéutico | Resultado, recomendación y decisión médica | Trazabilidad de aceptación/rechazo |
| Describir comportamiento institucional | Aislamientos, población, periodo y criterios analíticos | Perfil estratificado reproducible |
| Medir consumo | Cantidad consumida o días de administración, servicio y denominador | Indicadores de consumo separados de resistencia |

La tabla anterior es **DISEÑO**, derivado de S01, S02 y S10; no describe pantallas ya existentes en el HUSI. DDD es una unidad agregada de consumo; no equivale a la dosis real prescrita a un paciente. DOT contabiliza días de terapia por agente y requiere una definición operacional; no se obtiene de la sola tabla de susceptibilidad. [S10, S23]

## 5. Informe completo de bacteriología/microbiología

**Puede existir informe microbiológico sin antibiograma.** El producto depende de la prueba: cultivo sin crecimiento, flora/crecimiento mixto, organismo sin estudio de susceptibilidad, identificación molecular o resultado preliminar. El reporte selectivo significa que algunos antibióticos estudiados no se muestran; no debe confundirse con no haberlos estudiado. CDC documenta ambas situaciones. [S22]

Como referencia internacional, la OMS incluye en el informe identificación del laboratorio y paciente, solicitante, muestra, fecha de toma, pruebas/resultados, comentarios pertinentes, responsable autorizado y fecha/hora de liberación. También contempla comunicación de resultados críticos y conservación de informes revisados. No se encontró un formulario colombiano único que obligue a una diagramación idéntica para todos los laboratorios. [S15]

Según el tipo de muestra pueden agregarse tinción/Gram, cuantificación del crecimiento y pruebas adicionales. La guía IDSA/ASM de 2024 distingue requerimientos por síndrome, sitio y material; por tanto, sería incorrecto exigir idénticos campos analíticos a un urocultivo y a un hemocultivo. [S24]

**Estados que el modelo debe soportar —DISEÑO—:** recibido, en proceso, preliminar, final, corregido, anulado y rechazado, mapeados a los estados reales del LIS. La emisión de una alerta basada en preliminares debe ser una política explícita: un Gram preliminar no acredita por sí solo una resistencia específica.

Hay que conservar la distinción entre:

- **Resultado observado:** medición o detección efectuada.
- **Interpretación del laboratorio:** categoría o comentario liberado.
- **Inferencia del sistema:** regla aplicada con su versión.
- **Confirmación de referencia:** resultado posterior del LSP/INS.

Esta separación es una decisión de trazabilidad. Una sospecha fenotípica y la detección de un gen no deben ocupar un único campo booleano llamado “resistencia confirmada”.

## 6. Matriz de datos relevantes

**Cómo leerla:** RM = reporte microbiológico; AI = antibiograma individual; FI = ficha INS; W = registro/exportación WHONET; HC = historia clínica; RX = prescripción/preautorización; ADM = administración; AA = antibiograma acumulado. L = laboratorio; T = tratante; P = PROA; E = epidemiología/control de infecciones; R = LSP/INS.

La columna “fuente/alcance” acredita la existencia o fundamento del dato. **Los usos son propuestas analíticas; el productor consignado es su origen funcional habitual, que debe confirmarse localmente.** Un dato presente en una ficha no implica que esté en el PDF clínico ni en una interfaz automática. Los campos compuestos se separan en variables durante el diseño físico.

### 6.1 Identidad, atención y muestra

| N.º | Dato | Qué significa | Documento/registro | Quién lo genera | Quién lo usa | Fuente/alcance |
|---|---|---|---|---|---|---|
| 1 | Identificador del paciente | Enlace estable de resultados | W, RM | Admisiones; L lo incorpora | L/T/P/E | S04, CO |
| 2 | Nombre | Identificación humana asistencial | FI, RM | Admisiones | L/T | S03, CO |
| 3 | Tipo y número de documento | Identidad administrativa | FI | Admisiones; UPGD diligencia | L/R | S03, CO |
| 4 | Fecha de nacimiento/edad | Contexto demográfico; incluir unidad de edad | W | Admisiones | P/E | S04, CO |
| 5 | Sexo registrado | Variable demográfica | W | Admisiones | E | S04, CO |
| 6 | IPS y laboratorio procesador | Diferencia remitente de ejecutor | FI | UPGD | L/R | S03, CO |
| 7 | Departamento/municipio | Localización institucional del envío | FI | UPGD | R | S03, CO |
| 8 | Servicio y tipo de localización | UCI, hospitalización u otra categoría | W | Atención; L incorpora | P/E | S04, CO |
| 9 | Identificador del episodio | Distingue ingresos de una persona | RM/HC | Admisiones | T/P/E | S12, INT; H |
| 10 | Fecha de ingreso | Contextualiza la muestra en la estancia | W/HC | Admisiones | E | S17, INT; H |
| 11 | Número de muestra | Identifica material recibido | W/RM | L | L/T/E | S17, INT; H |
| 12 | Fecha/hora de toma | Momento de recolección | RM | Personal recolector | L/P/E | S12, INT; fecha también S04, CO |
| 13 | Tipo de muestra | Sangre, orina, tejido, etc. | W/RM | Solicitante/recolector | L/P/E | S04, CO |
| 14 | Sitio anatómico | Precisión adicional al material | RM | Solicitante/recolector | L/T | S13, INT; H |
| 15 | Técnica/origen de obtención | Diferencia métodos relevantes de recolección | FI/solicitud | Recolector | L/R | S03, CO |
| 16 | Recepción de muestra | Momento de llegada al laboratorio | RM/LIS | L | L/calidad | S13, INT; H |
| 17 | Calidad o rechazo | Limitación preanalítica y motivo | RM/recepción | L | L/T | S15, INT; H |
| 18 | Finalidad clínica o tamizaje | Distingue diagnóstico de búsqueda de colonización | Solicitud/W | Solicitante/E | E/P | S25, INT; H |

### 6.2 Microbiología y susceptibilidad

| N.º | Dato | Qué significa | Documento/registro | Quién lo genera | Quién lo usa | Fuente/alcance |
|---|---|---|---|---|---|---|
| 19 | Resultado del cultivo | Crecimiento o conclusión microbiológica | RM | L | T/P/E | S12, S22, INT; H |
| 20 | Tinción/Gram | Hallazgo microscópico, si aplica | RM | L | T/L | S24, INT; H |
| 21 | Recuento/crecimiento | Cuantificación y unidad, cuando corresponda | RM | L | T/L | S24, INT; H |
| 22 | Género/especie | Identidad del microorganismo | FI/W/AI | L | T/P/E/R | S03, S04, CO |
| 23 | Identificador del aislamiento | Separa organismos de una muestra | LIS/modelo | L o integración | Todos | S16; DISEÑO/H |
| 24 | Método de identificación | Técnica usada para identificar | RM | L | L/R | S13, INT; H |
| 25 | Antimicrobiano | Agente estudiado | AI/W | L | T/P/E | S16, INT; S03, CO |
| 26 | Método de susceptibilidad | Difusión, dilución u otro método registrado | AI/exportación | L | L/P | S16, INT |
| 27 | Valor de CIM | Medición de concentración | AI/W | L/equipo | L/T/P/E | S16, INT |
| 28 | Comparador de CIM | ≤, ≥, <, > o igualdad | AI/exportación | L/equipo | L/integración | S16, INT |
| 29 | Unidad de CIM | Unidad de concentración | AI | L/equipo | L/integración | S16, INT |
| 30 | Diámetro del halo | Medición en milímetros | AI/W | L | L/P/E | S14, INT |
| 31 | Interpretación liberada | S, I, R, SDD u otra categoría aplicable | AI | L | T/P/E | S20–S21, INT |
| 32 | Estándar/edición | Referencia interpretativa aplicada | Configuración/LIS | L | L/integración | S18; DISEÑO/H |
| 33 | Estado probado/no probado/suprimido | Explica ausencia de un resultado visible | Equipo/LIS | L | P/E/integración | S22, INT; S06, CO |
| 34 | Comentario interpretativo | Aclaración emitida por laboratorio | AI/RM | L | T/P | S22, INT |
| 35 | Mecanismo por confirmar | Motivo de estudio de referencia | FI | UPGD | R/E | S03, CO |
| 36 | Prueba confirmatoria y resultado | Evidencia adicional, positiva/negativa según prueba | FI/RM | UPGD/LSP/INS | L/R/P/E | S03, CO |
| 37 | Genes/blancos evaluados | Alcance de la prueba molecular | FI/RM molecular | L/R | L/P/E | S03, CO |
| 38 | Fecha/hora de liberación | Disponibilidad del resultado autorizado | RM | L/LIS | T/P/E | S15, INT; H |
| 39 | Revisor/autorizador | Responsable de validar el informe | RM | L | Calidad/T | S15, INT; H |
| 40 | Estado y versión del resultado | Distingue final, preliminar y correcciones | LIS/RM | L | Todos | S15; DISEÑO/H |

### 6.3 Clínica, PROA, vigilancia y control de calidad

| N.º | Dato | Qué significa | Documento/registro | Quién lo genera | Quién lo usa | Fuente/alcance |
|---|---|---|---|---|---|---|
| 41 | Diagnóstico y grado de certeza | Motivo infeccioso sospechado/confirmado | RX/HC | T | P/farmacia | S02, CO |
| 42 | Indicación terapéutica/profilaxis | Finalidad del medicamento | RX | T | P/farmacia | S02, CO |
| 43 | Fármaco, dosis, frecuencia y duración | Régimen solicitado/actual | RX | T | P/farmacia | S02, CO |
| 44 | Razón del cambio | Justificación de ajuste o adición | RX | T | P | S02, CO |
| 45 | Peso y función renal | Contexto de evaluación posológica | HC/laboratorio | Equipo asistencial | T/P/farmacia | S02, CO |
| 46 | Prescriptor y visto bueno PROA | Responsables y fecha de autorización | RX | T/P | Farmacia/auditoría | S02, CO |
| 47 | Administración efectiva | Agente, fecha y dosis realmente administrada | ADM | Enfermería | P/farmacia | S23; INT/H |
| 48 | Gramos consumidos | Numerador del consumo agregado | Registro de consumo | Farmacia | P/E | S10, CO |
| 49 | Camas/días-cama y servicio | Denominador y estrato de consumo | Censo hospitalario | Gestión hospitalaria | P/E | S10, CO |
| 50 | Clasificación de IAAS | Asociación a dispositivo/sitio según evaluación | W/vigilancia | E; L incorpora | E/R | S04, CO |
| 51 | Origen de infección | Comunitario/hospitalario según definición usada | Vigilancia/HC | E | E | S26, INT; H |
| 52 | Sospecha de brote | Señal para investigación | FI | UPGD/E | R/E | S03, CO |
| 53 | Evolución del paciente | Desenlace consignado | FI/HC | Atención; UPGD incorpora | E/R | S03, CO |
| 54 | Orden de remisión y resultado de referencia | Seguimiento externo del aislamiento | FI/SIVILAB | UPGD/LSP/INS | L/R/E | S03, CO |
| 55 | Periodo, estrato y población | Alcance del resumen estadístico | AA | Analista L/E/P | P/T | S19, INT |
| 56 | Número analizado y porcentajes | Denominadores/resultados por combinación | AA | Analista | P/E/T | S19, INT |
| 57 | Versión de deduplicación y exclusiones | Define qué registros participan | Configuración analítica | Analista | P/E/calidad | S19; DISEÑO |
| 58 | Resultado de control de calidad | Evidencia técnica; separado de pacientes | Registros de calidad | L | L/calidad | S27, INT |
| 59 | Comunicación crítica y receptor | Traza la entrega de un hallazgo | Registro de aviso | L | T/E/calidad | S15, INT |
| 60 | Regla, evidencia y seguimiento de alerta | Explica qué se activó y su resolución | Sistema propuesto | Motor y responsables | P/E/L | DISEÑO; H |

**No son todos campos obligatorios para todo resultado.** Los números 20–21 dependen de la muestra; 35–37 de las pruebas efectuadas; 41–47 del alcance clínico; 48–49 de indicadores de consumo; 52–54 del proceso de referencia. “No aplica”, “no realizado”, “pendiente”, “no disponible” y “negativo” deben conservar significados diferentes.

## 7. Entradas necesarias para el software

### 7.1 Núcleo mínimo para alertas microbiológicas

Propuesta DISEÑO:

1. Identificador del hospital/laboratorio y del paciente.
2. Identificador de muestra y de aislamiento.
3. Tipo de muestra, fecha/hora de toma y ubicación disponible.
4. Microorganismo, código de origen y normalización.
5. Antimicrobiano, método y resultado interpretado.
6. Medición original, unidad y comparador cuando existan.
7. Estado del resultado y fecha/hora de validación o disponibilidad.
8. Fuente, identificador externo y versión de cada resultado.
9. Pruebas confirmatorias y su estado, si se realizaron.
10. Catálogo institucional de reglas, responsables y prioridad.

Para una regla que use la categoría validada puede admitirse una entrada sin CIM, señalando esa limitación. **Sin medición, método o estándar suficiente no se debe recalcular una categoría.** Si falta el enlace seguro con el paciente, el registro debe pasar a una cola de conciliación en vez de generar un aviso clínico dirigido.

### 7.2 Datos adicionales según la alerta

| Alerta propuesta | Datos adicionales indispensables | Qué puede afirmar | Qué no puede afirmar por sí sola |
|---|---|---|---|
| Fenotipo resistente prioritario | Regla por especie/agente y resultado validado | Cumple criterio configurado | Mecanismo molecular no probado |
| Resultado inusual o inconsistente | Método, estándar, patrón completo y controles disponibles | Requiere revisión del laboratorio | Que el paciente debe cambiar tratamiento |
| Posible discordancia con tratamiento | Episodio, prescripción activa, administración y contexto infeccioso | Existe discrepancia para revisión | Fracaso terapéutico confirmado |
| Nuevo perfil en el mismo paciente | Historia de aislamientos y reglas comparables | Cambio observado entre resultados | Evolución del mismo clon |
| Agrupación de casos | Pacientes únicos, tiempo, unidades y línea basal | Señal epidemiológica | Brote o transmisión confirmados |
| Resultado crítico sin gestión | Responsable, aviso, acuse y tiempos | Hay pendiente de atención | Que el equipo clínico desconoce el caso |
| Posible pérdida de resultados en interfaz | Exportación de origen, destino y política de supresión | Diferencia que debe conciliarse | Que todo dato oculto deba liberarse al médico |

Los umbrales, ventanas, destinatarios y tiempos deben acordarse con microbiología, PROA y control de infecciones. No se propone un número universal de casos para declarar brote.

### 7.3 Caso colombiano concreto: resistencia a ceftazidima-avibactam

El comunicado técnico del INS sobre CZA documenta notificación inmediata a vigilancia/control de infecciones y situaciones en que el resultado puede suprimirse en sistemas automatizados. También indica cómo completar el registro cuando la CIM no pasa a WHONET. [S06]

Para el sistema, esto justifica un caso de prueba de **integridad de la interfaz**: identificar el resultado de origen, comprobar si llega al destino y conservar si fue medido, confirmado, suprimido o añadido manualmente. Los criterios de remisión deben contrastarse con el documento más reciente de 2026; no se debe fijar la versión 06 de la ficha mencionada en un comunicado anterior como formato vigente. [S03, S06]

### 7.4 Arquitectura de datos recomendada

Entidades propuestas: `Paciente`, `Episodio`, `UbicacionTemporal`, `Solicitud`, `Muestra`, `Aislamiento`, `ResultadoAST`, `PruebaMecanismo`, `VersionInforme`, `Prescripcion`, `Administracion`, `Regla`, `Alerta` y `GestionAlerta`.

Relaciones fundamentales:

- Un paciente tiene múltiples episodios y muestras.
- Una muestra puede tener cero, uno o varios aislamientos.
- Un aislamiento tiene múltiples pruebas, incluso para el mismo antimicrobiano con distintos métodos o fechas.
- Un informe puede corregirse; una alerta debe mantener vínculo con la versión que la originó.
- Una alerta puede tener varias comunicaciones, revisiones y decisiones.

Una tabla plana de una fila por paciente perdería información. Un modelo relacional puede exportar después una vista compatible con WHONET. BacLink admite tanto organización por aislamiento como por resultado de antibiótico, lo que aporta una referencia de interoperabilidad, no una prueba de compatibilidad automática con el HUSI. [S16]

**Contrato de integración propuesto:** código original, valor original, código normalizado, unidad, comparador, método, estado, identificadores de relación y marcas temporales. HL7 v2/FHIR o CSV son opciones a negociar; no se encontró evidencia de cuál está habilitada por el hospital. Debe importarse de manera idempotente y registrar correcciones sin duplicar alertas.

### 7.5 Calidad, denominadores y evaluación

Controles propuestos:

- Validar unidades y comparadores: `≤0,5` no equivale a `0,5` exacto ni a dato ausente.
- Distinguir muestras clínicas, tamizajes, ambientales y control de calidad.
- Conciliar nombres locales de microorganismos, servicios y fármacos.
- No asumir que “sin resultado” significa susceptible.
- No convertir “gen no detectado” en “todos los mecanismos descartados”.
- Retener negativos en el repositorio de origen si se quieren calcular positividad y calidad diagnóstica; una exportación de vigilancia puede excluirlos.
- No deducir infección, colonización o contaminación exclusivamente a partir de crecimiento.
- Separar cambio de resistencia de cambio en panel, punto de corte o población muestreada.

El instructivo INS de 2022 describe depuración de negativos en la base de vigilancia y controles sobre muestra/localización. Por ello, dicha base puede ser insuficiente para reconstruir todos los exámenes solicitados. [S04] CDC documenta además que contaminación de hemocultivos puede producir falsos positivos y tratamientos innecesarios. [S28]

Para evaluar la tesis, medir contra casos revisados por profesionales: completitud por campo, exactitud de vinculación, concordancia de reglas, alertas falsas, duplicados, latencia desde liberación, tiempo hasta revisión y proporción gestionada. Si se evalúan brotes, usar revisión epidemiológica como referencia; una agrupación estadística no es una etiqueta clínica definitiva. Para un prototipo de investigación, usar datos desidentificados y un identificador estable autorizado; para operar asistencialmente, el hospital necesita resolver la identidad bajo sus permisos. Esta es una recomendación de diseño y gobernanza, no una autorización de acceso.

## 8. Lo que está confirmado, lo internacional y lo pendiente del HUSI

| Tema | Confirmado para Colombia | Referencia internacional | Pendiente del HUSI |
|---|---|---|---|
| PROA | Marco 2471/2022 y documentación técnica [S01–S02] | Apoyo a stewardship [S23] | Programa, responsables y procedimientos vigentes |
| Susceptibilidad | Ficha INS con resultado e interpretación [S03] | Reportes originales y estándares [S12–S21] | Plantilla, paneles y edición implementada |
| Exportación | Instructivo INS para WHONET/BacLink [S04] | Esquemas de exportación [S16] | LIS, equipo, interfaces, frecuencia y permisos |
| Confirmación | Circuito y ficha del INS 2026 [S03] | Métodos referenciales [S18] | Pruebas locales y tiempos externos |
| Alertas | Avisos PROA y alerta específica CZA [S01, S06] | Alertas WHONET/SaTScan [S25] | Qué funciona hoy y quién recibe/gestiona |
| Epidemiología | Seguimiento SDS documentado en 2025 [S08] | GLASS y calidad regional [S26–S27] | Definiciones, ubicación temporal y datos accesibles |

El HUSI publica un documento histórico que describe antibiograma automatizado, referencia a CLSI y uso de LabPro/WHONET. Su portafolio de 2019 incluye antibiograma por CIM automatizada. **Esto acredita una oferta y descripción histórica, no confirma su infraestructura, versión, plantilla o flujo de septiembre de 2026.** [S07]

Hay además un antecedente científico directamente pertinente: un estudio colombiano publicado en *Infectio* en 2017, con autoras afiliadas al HUSI, evaluó SaTScan-WHONET con datos históricos. En ese estudio, el análisis prospectivo simulado no superó a la vigilancia activa en oportunidad de detección. No se extrapola ese resultado a la operación actual del hospital. [S29]

WHONET ya documenta alertas microbiológicas, análisis de perfiles y detección de agrupaciones con SaTScan. Por tanto, la tesis no debería justificar su novedad afirmando que esas capacidades no existen. Una contribución defendible sería demostrar una necesidad local en integración oportuna, trazabilidad, gestión de alertas o vinculación clínica y resolverla de forma medible. La necesidad concreta permanece H. [S25]

Otro estudio colombiano, publicado en *Biomédica* en 2023, analizó información de veinte instituciones de doce ciudades entre 2018 y 2021. Es evidencia de uso real de datos microbiológicos para vigilancia, pero no sirve como descripción de la resistencia actual del HUSI ni de Colombia en 2026. [S30]

## 9. Fuentes originales y localizadores

Todos los enlaces se consultaron o intentaron consultar el 27-09-2026. “Accesible” indica lectura del texto, PDF o sección pertinente; no significa validación de cada página. Las fuentes con acceso limitado se señalan expresamente.

| ID | Fuente y enlace directo | Localizador / valor probatorio |
|---|---|---|
| S01 | Ministerio de Salud. [Resolución 2471 de 2022 y anexo](https://www.minsalud.gov.co/sites/rid/Lists/BibliotecaDigital/RIDE/DE/DIJ/resolucion-2471-de-2022.pdf) | 61 páginas. Hojas 17–18: microbiología; 28: alarmas. [Texto oficial compilado por Invima](https://normograma.invima.gov.co/normograma/compilacion/docs/resolucion_minsaludps_2471_2022.htm). |
| S02 | Minsalud/ACIN. [Lineamientos técnicos PROA](https://minsalud.gov.co/sites/rid/Lists/BibliotecaDigital/RIDE/VS/PP/ET/lineamientos-optimizacion-uso-antimicrobianos.pdf) | Portada: julio 2019. PDF pp. 44–47: prescripción; 52–54: indicadores. [Registro OPS](https://www.paho.org/es/documentos/lineamientos-tecnicos-para-implementacion-proa-escenario-hospitalario-ambulatorio). |
| S03 | INS. [Criterios de envío de aislamientos y levaduras, marzo 2026](https://www.ins.gov.co/BibliotecaDigital/criterios-para-envio-de-aislamientos-bacterianos-y-levaduras-del-genero-candida-y-generos-relacionados-recuperados-en-iaas-para-confirmacion-de-mecanismos-de-ra-2026.pdf) | PDF 36 páginas; anexo 1 en página 26, ficha versión 08, revisada visualmente. Secciones iniciales: remisión y SIVILAB. |
| S04 | INS. [Instructivo para el manejo del software WHONET](https://www.ins.gov.co/BibliotecaDigital/instructivo-para-el-manejo-del-software-whonet-en-la-vigilancia-de-la-resistencia-a-los-antimicrobianos.pdf) | Fecha interna 15-06-2022. §2.1.4 campos; §2.1.12 IAAS; páginas impresas 33–35 depuración; §4 exportación/BacLink. Descargado y leído localmente. |
| S05 | INS. [Protocolo resistencia bacteriana 2022](https://www.ins.gov.co/buscador-eventos/Lineamientos/Pro_Resistencia%20bacteriana%202022.pdf) | Enlace localizado pero descarga 404. No se presenta como documento íntegramente revisado. La copia [PRO-Resistencia-bacteriana](https://www.ins.gov.co/BibliotecaDigital/PRO-Resistencia-bacteriana.pdf) corresponde a 2018, no a una actualización de 2026. |
| S06 | INS. [Comunicado técnico: resistencia a ceftazidima-avibactam](https://www.ins.gov.co/BibliotecaDigital/comunicado-tecnico-vigilancia-intensificada-por-laboratorio-de-resistencia-a-ceftazidimaavibactam-mediada-por-betalactamasas-en-enterobacterales-en-colombia.pdf) | PDF pp. 5–7: confirmación, supresión de datos, comunicación y vigilancia. Contrastar remisión con S03. |
| S07 | HUSI. [Procedimientos del laboratorio clínico](https://www.husi.org.co/documents/10180/28381/PROCEDIMIENTOS%20LABORATORIO%20CLINICO%20HUSI.pdf/d890622c-0c58-4f61-9f75-f595b2a51fca) y [portafolio 2019](https://www.husi.org.co/documents/10180/0/PORTAFOLIO%20DE%20SERVICIOS%20LABORATORIO%20CLINICO%20HUSI%202019%20-para%20web.pdf/3723f05c-95e4-421d-b99d-f0d8eaa0b29b) | Primer PDF p. 4, microbiología; portafolio p. 14, código 901002. Evidencia histórica. |
| S08 | SDS. [Reporte de gestión, corte septiembre 2025](https://www.saludcapital.gov.co/DPYS/Seguimiento%20Proyectos%202013/Rep_trim_Segplan_2025/III_trim/Comp_Gest_trim_III_2025.pdf) | Sección programa IAAS/PROA/RAM: seguimiento WHONET, asistencia y brotes. Texto indexado verificable. |
| S09 | SDS. [Lineamiento RAM 2026](https://www.saludcapital.gov.co/DSP/Infecciones%20Asociadas%20a%20Atencin%20en%20Salud/Lineam_y_otros/Lineam_RAM_2026.pdf) | Documento localizado; acceso directo 403. Solicitar copia vigente. No sustenta requisitos específicos en este informe. |
| S10 | INS. [Novedades de lineamientos 2024](https://portalsivigila.ins.gov.co/Documentos%20compartidos/Novedades%20lineamientos%202024%20DVARSP.pdf) | Sección consumo: gramos, camas y días-cama. Complemento: [informe consumo noviembre 2024](https://www.ins.gov.co/buscador-eventos/Informesdeevento/CONSUMO%20DE%20ANTIBIOTICOS%20NOVIEMBRE%202024.pdf), contenido indexado; descarga limitada. No se infiere calendario operativo 2026. |
| S11 | Minsalud. [Resolución 914 de 2025](https://www.minsalud.gov.co/Normatividad_Nuevo/Resoluci%C3%B3n%20No%20914%20de%202025.pdf) | Considerandos, p. 2, cita 2471/2022. Su objeto es reprocesamiento de dispositivos, no reemplazar PROA. |
| S12 | ARUP Laboratories. [Informe de ejemplo 2008476](https://ltd.aruplab.com/api/ltd/examplereport?report=2008476%2C%20Positive.pdf) | Dos páginas. Datos de ejemplo; cultivo rectal, CIM y categorías. |
| S13 | ARUP Laboratories. [Informe de ejemplo H. pylori](https://www.aruplab.com/Testing-Information/resources/HotLines/Sample_Reports/Aug2024QHL/3017744_Antimicrobial%20Susceptibility%20-%20Helicobacter%20pylori_MA%20HPYL.pdf) | Sitio, origen, fechas y método de identificación; ejemplo publicado en 2024. |
| S14 | WHONET. [Tutorial de entrada de datos e informes clínicos](https://whonet.org/WebDocs/WHONET%203.Data%20entry.html) | Parte 4: clinical reports. Junio 2006; evidencia de estructura, no puntos de corte vigentes. |
| S15 | OMS. [LQSI: Information Management](https://extranet.who.int/lqsi/checklist/14) | Lista de campos del informe y procedimientos de revisión, autorización, comunicación y archivo. |
| S16 | WHONET. [BacLink: Laboratory information systems](https://whonet.org/WebDocs/BacLink%203.Laboratory%20information%20systems.html) | Parte 4: filas por aislamiento o antibiótico, mediciones, interpretaciones y métodos. |
| S17 | WHONET. [Manual para CAESAR, 01-11-2022](https://whonet.org/WebDocs/WHONET_for_CAESAR_Manual.2022-11-01.pdf) | §2.4.5 y configuración: variables demográficas y de muestra. Referencia internacional. |
| S18 | CLSI. [M100, edición 36, 2026](https://clsi.org/shop/standards/m100/) | Página oficial de edición, fecha y correcciones. Las tablas completas requieren acceso correspondiente. |
| S19 | CLSI. [M39, edición 5](https://clsi.org/shop/standards/m39/) y [muestra oficial del documento](https://clsi.org/media/vdojtv5x/m39ed5_sample.pdf) | Enero 2022. §1.2: finalidad y limitación del primer aislamiento; ejemplos de tablas y denominadores. Solo se afirma lo visible en la muestra pública. |
| S20 | EUCAST. [Definición de S, I y R](https://www.eucast.org/bacteria/clinical-breakpoints-and-interpretation/definition-of-s-i-and-r/) | Significado de I y tratamiento de categorías en vigilancia. |
| S21 | CLSI. [Re-exploring the intermediate interpretive category](https://clsi.org/resources/insights-blog/re-exploring-the-intermediate-interpretive-category/) | Explicación oficial de I, SDD e I con nota; no sustituye tablas M100 actuales. |
| S22 | CDC. [Selective Reporting of AST Results](https://www.cdc.gov/antibiotic-use/pdfs/Selective-Reporting-508.pdf) | Agosto 2020; distingue no realizar prueba, suprimir y reporte en cascada. |
| S23 | CDC. [Core Elements of Hospital Antibiotic Stewardship Programs](https://www.cdc.gov/antibiotic-use/hcp/core-elements/hospital.html) y [documento completo](https://www.cdc.gov/antibiotic-use/media/pdfs/hospital-core-elements-508.pdf) | Documento 2019, sección Tracking, p. 20: días de terapia y uso. Portal actualizado en 2025; referencia internacional, no norma colombiana. |
| S24 | IDSA/ASM. [Guía de uso del laboratorio de microbiología, 2024](https://www.idsociety.org/practice-guideline/laboratory-diagnosis-of-infectious-diseases/) | [Artículo original, DOI 10.1093/cid/ciae104](https://doi.org/10.1093/cid/ciae104). Tablas por muestra/síndrome. |
| S25 | WHONET. [Seminarios oficiales](https://whonet.org/webinars.html) | Sesiones enero/marzo 2024 de SaTScan y análisis: capacidades existentes y validación de señales. |
| S26 | OMS. [Manual GLASS-AMR 2023](https://www.who.int/publications/i/item/9789240076600) y [módulo GLASS-AMR](https://www.who.int/initiatives/glass/glass-routine-data-surveillance) | Manual del 31-08-2023 sustituye implementación temprana; módulo confirma variables microbiológicas, demográficas y epidemiológicas. No se presupone identidad con exportación colombiana. |
| S27 | OPS. [Monitoreo de calidad de vigilancia de resistencia](https://www.paho.org/es/documentos/instrumento-para-monitoreo-rapido-calidad-vigilancia-resistencia-antibioticos-epi-infotm) | Calidad de identificación y susceptibilidad. [Portal RAM](https://www.paho.org/es/ram-portal): información indexada sobre evaluación externa; descarga directa limitada. |
| S28 | CDC. [Prevención de contaminación de hemocultivos](https://www.cdc.gov/lab-quality/php/prevent-adult-blood-culture-contamination/index.html) | Actualización visible 31-03-2026; falsos positivos y calidad preanalítica. |
| S29 | Meneses-Ríos et al. [Evaluación SaTScan-WHONET en Colombia](https://revistainfectio.org/P_OJS/index.php/infectio/article/view/652) | *Infectio*, 2017;21(2). [DOI](https://doi.org/10.22354/in.v21i2.652). Antecedente con afiliaciones HUSI; resultados históricos. |
| S30 | De La Cadena et al. [Actualización de resistencia en instituciones colombianas](https://pubmed.ncbi.nlm.nih.gov/38109138/) | *Biomédica*, 2023;43:457–473. [Texto en SciELO](https://www.scielo.org.co/scielo.php?pid=S0120-41572023000400457&script=sci_arttext). Datos 2018–2021. |

## 10. Preguntas para la bacterióloga del Hospital San Ignacio

1. **¿Nos puede compartir informes actuales desidentificados** de hemocultivo, urocultivo y tamizaje, incluyendo un preliminar, un final, una corrección y una prueba de mecanismo?
2. **¿Qué equipo, LIS y versiones de CLSI/EUCAST usan hoy?** ¿Qué datos exportan a WHONET, con qué frecuencia y cuáles quedan suprimidos o se completan manualmente?
3. **¿Cómo relacionan paciente, ingreso, muestra y cada aislamiento?** ¿La ubicación corresponde a la toma de muestra o a la ubicación actual?
4. **¿Qué alertas ya existen y cuál es la dificultad concreta?** ¿Quién recibe, confirma, gestiona y cierra cada una, incluidos noches y fines de semana?
5. **¿Qué mecanismos confirman localmente y cuáles remiten al LSP/INS?** ¿Cómo incorporan después la respuesta y las correcciones al informe original?
6. **¿Podemos vincular microbiología con tratamiento y vigilancia?** Necesitamos saber si hay acceso autorizado a prescripción/administración, clasificación infección-colonización-contaminación y casos de referencia para evaluar el prototipo.
