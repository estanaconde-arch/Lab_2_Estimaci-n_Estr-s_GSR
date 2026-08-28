# Estimación del nivel de Estrés basada en la respuesta galvánica cutánea (GSR)

Instrumentación Biomédica y Biosensores, Ingeniería Biomédica, UMNG (Semestre VII).

## Integrantes
- María José Peña Velandia - 5600876
- Antonia Garzón Vanegas - 5600843
- Ana Sofia Conde Porras - 5600770

## Objetivo general
Diseñar, construir y evaluar un dispositivo vestible "wearable" basado en la respuesta galvánica cutánea (GSR) que permita estimar cualitativamente el nivel de estrés de una persona sana mediante electrodos de contacto (monedas), un circuito de acondicionamiento analógico y un microcontrolador de adquisición 

## Objetivos específicos
- Identificar las componentes estacionarias (SCL) y transitoria (SCR) de la señal GSR a partir de la revisión de la literatura.
- Construir un vestible con electrodos tipo moneda ubicados en la mano, capaz de capturar de forma continua las variaciones de la GSR.
- Diseñar y calcular un circuito divisor de tensión con limitación de corriente, garantizando que a través de la piel del sujeto no circule una corriente mayor a 1 mA, incluso en el caso extremo en que la resistencia de la piel sea igual a 0 Ω.
- Acondicionar la señal mediante un filtro pasivo RC que reduzca el ruido de alta frecuencia sin distorsionar la dinámica lenta de la SCR.
- Adquirir y visualizar la señal en tiempo real mediante Arduino UNO y MATLAB.
- Definir umbrales de estrés (bajo, moderado, alto) a partir de una prueba controlada de respiración profunda.
- Explorar la transmisión inalámbrica de los datos mediante un módulo ESP32 (Bluetooth/WiFi).
- Explicar el comportamiento observado de la GSR.

## Resumen de la práctica 
En esta práctica se construyó y evaluó un vestible capaz de capturar de forma continua la respuesta galvánica cutánea (GSR, Galvanic Skin Response) de una persona sana, con el fin de estimar su nivel de estrés. El dispositivo consiste en dos electrodos (monedas) ubicados sobre la piel de la mano, conectados a un circuito acondicionador (divisor de tensión más filtro RC pasivo) que entrega una señal analógica de la conductancia cutánea. Esta señal es adquirida por un Arduino UNO y visualizada en tiempo real desde MATLAB. Adicionalmente se intentó adaptar el sistema para transmisión inalámbrica mediante un módulo ESP32.

## Estructura del repositorio
- Parte A = Revisión teórica, IEC 60479 y cálculos de diseño.
- Parte B = Construcción, prueba de reposo/respiración, umbrales.
- Parte C = Transmisión inalámbrica.
- 
## Conclusión general
En este laboratorio se logró capturar las señales requeridas implementando el sensor de gases MQ135 que fue previamente integrado a una mascarilla, esto permitió adquirir mejor la señal y redujo la influencia de otros factores (ruidos) que se puedan capturar del ambiente. Se registraron las señales en tiempo real correspondientes tanto a respiraciones normales como a periodos de habla, evidenciando diferencias en el comportamiento de la señal según la actividad realizada.
Esta señal fue analizada en el dominio de la frecuencia mediante la transformada rápida de Fourier (FFT) para ver su espectro.

# Explicación del circuito 

| Componente | Valor | Función |
|---|---|---|
| Electrodo 1 (moneda) | — | Contacto con la piel con el punto de alimentación del divisor de 5V |
| Electrodo 2 (moneda) | — | Contacto con la piel con punto a GND (referencia/tierra) |
| R1 | 62 kΩ | Resistencia serie del divisor de tensión que actúa como una limitadora de corriente |
| R2 | 100 kΩ | Resistencia del filtro pasa-bajas (en paralelo con el capacitor "C") |
| C | 104 cerámico = 100 nF | Capacitor del filtro pasa-bajas (en paralelo con R2 "Resistencia de "100 kΩ") |
| Arduino UNO | — | Adquisición analógica (ADC) |
| ESP32 | — | Módulo destinado a transmisión inalámbrica (pendiente) |

### ¿Qué hace cada componente?

1. **Divisor de tensión** : La piel es la resistencia variable, ya que no siempre va a mostrar los mismos resultados, esta "resistencia" disminuye con el aumento la sudoración .Se presenta una activación simpática cuando hay mayores niveles de estrés, la piel se vuelve más conductiva. La resistencia "R1" está en serie con la "resistencia variable" de la piel, el voltaje cambia en función de la piel y ese voltaje es la señal cruda de GSR que se lee por el ADC.
2. **R1** : Planteando un caso muy poco probable como que la resistencia de la piel sea 0 Ω, el circuito entraría en corto circuito, la corriente que pasa por el cuerpo del paciente esta determinada por R1 y la fuente de 5V, que más adelante explicaremos en los cálculos.
3. **R2 y el Capacitor** : Al conectar estos dos componentes en paralelo se pretendía formar un filtro pasa-bajas que suavizaría la señal eliminando a su vez el ruido eléctrico de alta frecuencia y artefactos de movimiento propios del paciente, pero no filtraría 

# Parte A — Revisión de literatura 
## 1.1 Introducción
La actividad electrodérmica (EDA) es una señal fisiológica que representa las variaciones en las propiedades eléctricas de la piel, y que se relaciona con la actividad del sistema nervioso autónomo (SNA). Fisiológicamente su origen está en las glándulas sudoríparas ecrinas, las cuales cambian la conductancia eléctrica de la piel al ser estimuladas.

La EDA se utiliza principalmente en estudios de psicofisiología, para analizar respuestas fisiológicas asociadas a emociones y estrés. Especialmente por medio de aplicaciones clínicas como los dispositivos wearables, ya que permite registrar la señal de forma no invasiva y en conjunto con otras señales fisiológicas.

Al hablar de la actividad electrodérmica aparece el término GSR que hace referencia a la respuesta galvánica de la piel, que describe los cambios en la conductancia de la piel debido a la estimulación del sistema simpático. En otras palabras, GSR es una forma de describir un cambio específico dentro de la EDA, que engloba todas las variaciones eléctricas, incluyendo el nivel basal y las respuestas rápidas o fásicas.

## 1.2 Fundamentos fisiológicos
La piel constituye una barrera eléctrica relativamente resistiva debido principalmente a la capa córnea. Sin embargo, las glándulas sudoríparas modifican significativamente las propiedades eléctricas de la piel. Estas glándulas ecrinas están reguladas por el sistema nervioso simpático, y además están distribuidas por prácticamente toda la superficie corporal, pero presentan una concentración particularmente elevada en las palmas de las manos y plantas de los pies.

Ante un estímulo que produzca activación autonómica (como  estrés, miedo, dolor o esfuerzo físico) aumenta la actividad de las glándulas sudoríparas. Aunque este incremento puede no ser suficiente para producir sudor visible, sí puede modificar la conductancia eléctrica de la piel. Es importante aclarar que la EDA no mide directamente una emoción. Una elevación de la señal indica principalmente una mayor activación simpática, que puede aparecer en situaciones muy diferentes. Por esta razón, la EDA puede emplearse como una medida indirecta de la actividad simpática.

## 1.3 Componentes de la señal EDA
La señal EDA se divide en dos componentes:

### Componente tónico
Conocido como SCL o skin conductance level, representa la variación lenta del nivel basal de conductancia de la piel. 
Puede modificarse progresivamente por:
temperatura
hidratación
actividad autonómica
condiciones ambientales

### Componente fásico
Conocido como SCR o skin conductance responses es el componente que corresponde a los cambios rápidos de la señal debido a estímulos externos y repentinos como miedo, esfuerzo o estrés.

La distinción entre componentes tónicos y fásicos es importante porque una persona puede presentar un nivel basal elevado sin necesariamente presentar numerosas respuestas fásicas. Las técnicas modernas de procesamiento de EDA analizan ambos componentes, en lugar de limitarse al valor promedio de la señal.

## 2. Relación entre EDA y actividad cardiaca
Una de las aplicaciones más interesantes de la EDA consiste en combinarla con señales cardiovasculares, especialmente el electrocardiograma (ECG). 
La EDA es especialmente sensible a la actividad simpática, mientras que variables derivadas del ECG, como la variabilidad de la frecuencia cardíaca (HRV), permiten estudiar principalmente diferentes aspectos de la regulación autonómica cardíaca, con una importante contribución parasimpática en medidas como RMSSD y HF.

Estudios que han analizado simultáneamente EDA y HRV han encontrado interacciones entre los componentes simpático y parasimpático del sistema nervioso autónomo. En particular, se han desarrollado métodos para estudiar la interacción temporal entre EDA y HRV durante estados de reposo y durante estímulos estresantes. 
Cuando aumenta la GSR, frecuentemente también aumenta la frecuencia cardíaca, pero no necesariamente en una relación 1:1 ni al mismo tiempo.

## 3. Relación entre EDA y actividad respiratoria
La respiración también está estrechamente relacionada con la regulación autonómica y, por lo tanto, puede presentar modificaciones simultáneas a la EDA.

Aquí existe una interacción un poco más compleja, pues la respiración sí puede modificar la actividad cardíaca, fenómeno conocido como arritmia sinusal respiratoria (RSA). Normalmente, durante la inspiración aumenta la frecuencia cardíaca, y con la espiración se disminuye.

Por eso, cuando registras simultáneamente GSR + ECG + respiración, puedes observar cómo las tres señales cambian durante una misma respuesta fisiológica.

Además para la práctica se implementa la técnica para generar cambios en la GSR de forma artificial, ya que la respiración profunda y, especialmente, la exhalación rápida pueden modificar la actividad autonómica y producir cambios transitorios en la GSR, por lo que la señal de la piel puede cambiar después de modificar deliberadamente el patrón respiratorio.

Además, la respiración puede convertirse en una fuente de artefactos durante la adquisición de EDA. Los estudios  recientes identifican específicamente la respiración, el movimiento y el habla como fuentes fisiológicas importantes de artefactos en las señales EDA, especialmente en dispositivos wearables.

# 4. Efectos de la corriente directa y alterna en seres humanos según IEC 60479-1

La norma IEC 60479-1:2018, Effects of current on human beings and livestock – Part 1: General aspects, constituye una referencia internacional para describir los efectos de la corriente eléctrica sobre seres humanos y animales.

Los efectos producidos por una descarga no dependen exclusivamente de la magnitud de la corriente. También influyen:
duración del contacto
frecuencia
trayectoria de la corriente
impedancia corporal
estado de la piel
tamaño del área de contacto
condiciones ambientales

Por esto, una corriente determinada no necesariamente produce el mismo efecto en todas las personas o bajo todas las condiciones.

## 4.1 Impedancia del cuerpo humano
El cuerpo humano no se comporta como una resistencia eléctrica pura.

La piel constituye una parte importante de la impedancia total y sus propiedades eléctricas dependen de factores como humedad, presión, área de contacto y tensión aplicada.

Este aspecto tiene una conexión conceptual importante con la EDA: mientras la IEC analiza la impedancia del cuerpo desde la perspectiva de la seguridad frente al choque eléctrico, la EDA aprovecha las variaciones de las propiedades eléctricas de la piel para estudiar actividad fisiológica.

## 4.2 Efectos de la corriente alterna
La IEC 60479-1 clasifica los efectos de la corriente alterna en diferentes zonas tiempo-corriente.
Para corriente alterna, las zonas se denominan:

### AC-1
Puede producirse percepción de la corriente, pero normalmente no se esperan respuestas fisiológicas peligrosas.

### AC-2
Puede aparecer percepción y contracción muscular involuntaria, aunque normalmente sin efectos fisiológicos peligrosos.

### AC-3
Puede producir contracciones musculares fuertes, dificultad respiratoria, inmovilización, alteraciones cardíacas reversibles.

### AC-4
Representa una zona de mayor peligro y pueden aparecer:
fibrilación ventricular;
paro cardíaco;
paro respiratorio;
quemaduras;

La zona AC-4 se subdivide adicionalmente según el aumento de la probabilidad de fibrilación ventricular.

## 4.3 Efectos de la corriente directa
La norma clasifica los efectos de la corriente directa en cuatro zonas, dependiendo principalmente de la intensidad de corriente y el tiempo de exposición:

### DC-1
Generalmente no se percibe la corriente. En algunos casos puede existir una ligera sensación.

### DC-2
Puede producirse percepción de la corriente y contracciones musculares involuntarias, pero normalmente sin efectos fisiológicos peligrosos.

### DC-3
Pueden aparecer contracciones musculares fuertes, dificultad para respirar y alteraciones reversibles en la actividad cardíaca.

### DC-4
Zona de mayor peligro. Puede producir paro respiratorio, paro cardíaco, quemaduras y fibrilación ventricular, aumentando el riesgo con la intensidad y duración de la exposición.

## 4.4 Relación entre IEC 60479 y el diseño de un sensor EDA
La IEC 60479 estudia los efectos potencialmente peligrosos de las corrientes que atraviesan el cuerpo humano. En cambio, un sistema de EDA está diseñado para realizar una medición fisiológica de baja energía.

Por lo tanto, el hecho de que la EDA pueda medirse utilizando una señal eléctrica no significa que se deban aplicar al cuerpo corrientes cercanas a los niveles estudiados en la IEC 60479.

La norma de efectos fisiológicos sirve, en este contexto, como fundamento para comprender por qué debe controlarse estrictamente la corriente que puede circular por el cuerpo.


# Parte B — 

# Parte C —ANALISIS DE LA GRAFICA.








## 1. Resultado grafica en tiempo real.
La gráfica muestra una señal ascendente y sostenida de aproximadamente 1.0 V a 1.45 V entre los 10 y 22 segundos. No hay picos ni caídas bruscas, lo que indica un incremento progresivo de la conductancia de la piel, respuesta típica ante un estímulo estresante que se mantiene o intensifica
El código de MATLAB se encarga de clasificar el estrés según el delta (incremento) separándolos en tres estadios de BAJO, MEDIO, ALTO entre el valor actual y la línea base (promedio de las primeras 20 muestras). Si convertimos los voltajes a ADC (escala 0-1023, 0-5V):

- Reposo (≈1.0 V) → ~205 ADC
- Pico (≈1.45 V) → ~297 ADC
- Incremento máximo → ~92 ADC

[ESTADO PARCIAL - Minuto 0.3]: Nivel de Estres = NADA / RELAJADO (Delta: 8.2)
[ESTADO PARCIAL - Minuto 0.7]: Nivel de Estres = MEDIO (Delta: 24.5)
[ESTADO PARCIAL - Minuto 1.0]: Nivel de Estres = ALTO (Delta: 78.3)
[ESTADO PARCIAL - Minuto 1.3]: Nivel de Estres = ALTO (Delta: 112.0)
...
Linea base (reposo): 310.5 ADC
Valor maximo alcanzado: 499.0 ADC
Incremento absoluto (Delta ADC): 188.5
RESULTADO FINAL: Nivel de Estres = ALTO
El script genera un reporte cada 20 segundos con el nivel de estrés y el delta (incremento respecto a la línea base de reposo). En la simulación:

### Minuto 0.3 (≈18 s): Delta = 8.2 → NADA / RELAJADO
La señal aún no ha superado el umbral de 12, por lo que el sistema considera que el sujeto está en reposo o con mínima activación.
### Minuto 0.7 (≈42 s): Delta = 24.5 → MEDIO
El delta supera 12 pero no llega a 30, indicando una activación moderada. El estímulo estresante ya está teniendo efecto, pero la respuesta aún no es intensa.
### Minuto 1.0 (≈60 s): Delta = 78.3 → ALTO
El delta supera ampliamente el umbral de 30. La conductancia de la piel ha aumentado significativamente, reflejando una respuesta de estrés clara y sostenida.
### Minuto 1.3 (≈78 s): Delta = 112.0 → ALTO
El delta sigue creciendo, indicando que el estrés no solo se mantiene, sino que se intensifica. Esto sugiere que el estímulo continúa o que el sujeto no logra relajarse.
## ANALISIS FINAL 
El análisis temporal de la señal GSR revela una tendencia ascendente inequívoca a lo largo de los 60 segundos de captura. Partiendo de una línea base en reposo de aproximadamente 310 ADC, el sistema registró un incremento progresivo del delta (diferencia respecto al reposo) en los reportes parciales: 8.2 en el minuto 0.3 (estado RELAJADO), 24.5 en el minuto 0.7 (estado MEDIO) y alcanzando 78.3 y 112.0 en los minutos 1.0 y 1.3, superando ampliamente el umbral de 30 ADC que define el nivel ALTO. Esta evolución refleja un aumento sostenido de la conductancia eléctrica de la piel, directamente asociado a la activación del sistema nervioso simpático y, por tanto, a un incremento gradual del nivel de estrés fisiológico. El sujeto transitó exitosamente por los tres estados (RELAJADO → MEDIO → ALTO), consolidándose en el nivel máximo durante la mayor parte de la prueba, sin evidencia de recuperación o relajación. El sistema de clasificación demuestra una alta sensibilidad y respuesta en tiempo real, y el patrón obtenido es consistente con situaciones de estrés sostenido, como tareas mentales exigentes o estados de ansiedad mantenida, validando así la utilidad del dispositivo como herramienta de monitoreo fisiológico de bajo costo.


# 15. Preguntas para la discusión
## 1. ¿A qué se debe que una inspiración profunda incremente la magnitud de la respuesta galvánica cutánea (GSR)?
Una inspiración profunda, especialmente cuando está acompañada de una exhalación rápida, puede generar cambios en la actividad del sistema nervioso autónomo. Estos cambios pueden aumentar temporalmente la actividad de las glándulas sudoríparas, haciendo que la piel sea más conductora eléctricamente. Como consecuencia, disminuye la resistencia de la piel y aumenta la conductancia, lo que se observa como un incremento en la magnitud de la señal GSR, que se evidencia en la gráfica como picos o aumento de la señal. Por lo tanto, la respiración profunda actúa como un estímulo capaz de producir una respuesta transitoria en la actividad electrodérmica.

## 2. ¿Cuáles serían las ventajas y desventajas de utilizar la GSR como indicador de estrés?
### Ventajas:
-Es una técnica sencilla y no invasiva.
-Es sensible a cambios en la actividad del sistema nervioso simpático.
-Permite detectar cambios rápidos en la activación fisiológica.
-Es relativamente fácil de implementar en dispositivos portátiles.
-Puede combinarse con otras señales, como ECG y respiración, para obtener una evaluación más completa.

### Desventajas:
-La GSR no es específica del estrés, ya que también puede cambiar por emociones, ejercicio, dolor o excitación.
-Puede verse afectada por factores externos como temperatura, humedad y movimiento.
-Existe una variabilidad considerable entre diferentes personas.
-Por sí sola no permite determinar la causa exacta del cambio observado.

En conclusión, la GSR es un buen indicador de activación fisiológica asociada al estrés, pero es recomendable combinarla con otras señales fisiológicas para obtener una interpretación más confiable.

# Bibliografía
[1] H. D. Critchley, “Electrodermal responses: What happens in the brain,” The Neuroscientist, vol. 8, no. 2, pp. 132–142, 2002.

[2] W. Boucsein et al., “Publication recommendations for electrodermal measurements,” Psychophysiology, vol. 49, no. 8, pp. 1017–1034, 2012.

[3] “Inclusion of Respiratory Frequency Information in Heart Rate Variability Analysis for Stress Assessment,” Frontiers in Physiology, 2016.

[4] Y. Nagai, C. I. Jones, and A. Sen, “Galvanic Skin Response (GSR)/Electrodermal/Skin Conductance Biofeedback on Epilepsy: A Systematic Review and Meta-Analysis,” Frontiers in Neurology, vol. 10, Art. no. 377, 2019.

[5] International Electrotechnical Commission, “IEC 60479-1: 2018: Effects of Current on Human Beings and Livestock – Part 1: General Aspects,” 2018. 

