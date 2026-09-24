# CALCULO AMBULATORIO DEL ÍNDICE PLETISMOGRAFICO QUIRÚRGICO (SPI)

Instrumentación Biomédica y Biosensores, Ingeniería Biomédica, UMNG (Semestre VII).

## Integrantes
- María José Peña Velandia - 5600876
- Antonia Garzón Vanegas - 5600843
- Ana Sofia Conde Porras - 5600770

## I. INTRODUCCIÓN
Esta práctica se realizó con el objetivo de evaluar en tiempo real el equilibrio entre la nociocepción la cual es la respuesta del sistema nervioso a estímulos potencialmente dañinos y la analgesia, este es el efecto de los fármacos que bloquean dicha respuesta; se han buscado índices cuantitativos y objetivos para evaluar ambas variables, uno de los índices es el Índice Pletismográfico Quirúrgico (SPI), el cual se obtiene a través de la técnica fotopletismográfica (PPG).  
El PPG permite detectar los cambios en el volumen sanguíneo periférico asociados a cada latido cardíaco, y el SPI se encarga de relacionar la amplitud y la frecuencia de los pulsos cardíacos, con esto tenemos como resultado un valor numérico entre 0 y 100, donde valores más altos indican una mayor respuesta nocioceptiva (estrés quirúrgico), o hasta cambios de temperatura.  
La presente práctica busca el desarrollo de un sistema de medición continua del SPI utilizando un sensor óptico de reflectancia y así poder realizar la detección de picos por medio de MATLAB, para la adquisición y cálculo del SPI. Todo lo anterior se realiza con el motivo de cumplir los siguientes objetivos:

### Objetivo general
Desarrollar un sistema de medición continua del índice pletismográfico quirúrgico (SPI) en condiciones ambulatorias. 

### Objetivos específicos
- Reconocer las características fundamentales de la onda de pulso a partir de las cuales se obtiene el SPI.
- Construir un sistema que calcule el SPI en tiempo real y bajo condiciones ambulatorias.
- Validar el funcionamiento dinámico del sistema desarrollado mediante la aplicación de un estímulo térmico en este caso el frío localizado en la región cervical (cuello) para inducir una respuesta nociceptiva y de activación simpática por medio de una botella de agua fría. 

## II. MARCO TEÓRICO
### a) PPG para la medición del volumen sanguíneo periférico.
La fotopletismografía es una técnica no invasiva que permite detectar cambios en el volumen sanguíneo en los tejidos periféricos, como los dedos, a través de la absorción de luz por parte de la hemoglobina. Un sensor de PPG consta de una fuente de luz como un LED y un fotodetector; la luz emitida atraviesa el tejido, una parte de esa luz es absorbida por la sangre, mientras que el resto llega al fotodetector. La cantidad de luz absorbida varía respecto al volumen sanguíneo arterial generando una señal pulsátil que permite comprender la actividad cardiovascular y del sistema nervioso autónomo.  
El PPG indica la vasoconstricción simpática: ante una activación del sistema nervioso simpático como lo es el dolor o un estado de alerta, los vasos sanguíneos periféricos se contraen, disminuyendo la amplitud de la onda de pulso y alterando la frecuencia cardíaca. Por lo anterior, esta señal es usada para estimar el equilibrio nocicepción-analgesia y el SPI.

### b) Índice Pletismográfico Quirúrgico (SPI).
El SPI es un parámetro cuantitativo que evalúa el equilibrio entre nocicepción y analgesia, especialmente utilizado durante la anestesia general, este se calcula combinando dos componentes de la onda de pulso:  
- Amplitud normalizada de la onda de pulso (PPGA / NP): Representa la relación entre la amplitud de la onda pulsátil y la componente continua de la señal PP, esta disminuye cuando ocurre vasoconstricción simpática ante un estado de dolor o alerta.  
- Intervalo entre latidos normalizado (HBI / IBI): Indica los cambios en la frecuencia cardíaca. Durante la activación simpática, el intervalo entre latidos tiende a disminuir (lo que indica un aumento de la frecuencia cardíaca).
La fórmula matemática para el cálculo del SPI según lo investigado corresponde a:

<img width="941" height="94" alt="image" src="https://github.com/user-attachments/assets/93738e7b-0fba-4c9b-8b41-230f81efe8bf" />
(Ecuación 1. Fórmula cálculo SPI)
Los rangos de valores para PPGA-norm y HBI-norm están escalados entre 0 y 100, mientras que las constantes 0.7 y 0.3 son coeficientes establecidos  para priorizar el componente vascular. El resultado del SPI se obtiene en una escala entre 0 y 100, donde valores más altos reflejan una mayor respuesta nociceptiva. Durante la anestesia general, los rangos esperados de este índice rondan entre 20 y 50; un rango mayor puede provocar cierto grado de conciencia, mientras que un rango menor puede simbolizar un alto riesgo para el paciente.

### c) Estimulación Térmica Localizada (Prueba de Frío en el Cuello / CPT).
La prueba de estimulación térmica consiste en la aplicación de un estímulo frío en la piel (en este caso, mediante la colocación de una botella helada en la región cervical/cuello) durante un periodo de 30 a 40 segundos. Esta maniobra activa intensamente los termorreceptores cutáneos y desencadena un reflejo simpático agudo mediado por el sistema nervioso autónomo.  
Esta activación simpática inmediata genera vasoconstricción periférica por la contracción del músculo liso vascular, eleva levemente la presión arterial y produce un incremento transitorio en la frecuencia cardíaca. Al registrar la señal PPG y calcular continuamente el SPI, la prueba simula una respuesta nociceptiva equivalente a un estímulo quirúrgico doloroso, permitiendo comprobar si el sistema detecta correctamente los aumentos en el índice SPI.

## III. METODOLOGÍA EXPERIMENTAL 
### a) Sensor y Circuito de Acondicionamiento Análogo (TCST110)
Para la adquisición de la señal fotopletismográfica (PPG), se empleó un sensor óptico basado en el optointerruptor TCST110, el cual fue adecuado físicamente para funcionar en modo de reflectancia cutánea, la verificación inicial del emisor de luz infrarroja (LED IR) se realizó mediante una cámara digital para comprobar la emisión óptica continua y asegurar la correcta polarización directa del diodo.
Como la variación en la absorción de luz por parte de los lechos capilares produce cambios de voltaje de muy baja amplitud (del orden de milivoltios) y propensos a interferencias por luz ambiental o nivel de acoplamiento, se implementó una etapa de acondicionamiento analógico.
- Acondicionamiento del Sensor TCST110: La corriente circulante por el foto transistor del TCST110 se convierte a una señal de voltaje pulsátil mediante una red de resistencias de polarización.
- Amplificación y Filtrado Analógico: Se utilizó una etapa de amplificación basada en un amplificador operacional (cuyo arreglo e integrados son visibles en el circuito) con un filtro pasabanda analógico diseñado para acoplar en AC la señal (eliminando la componente continua DC) y atenuar el ruido de alta frecuencia o la hum inducida por la red eléctrica.
- Ajuste de Ganancia y Offset: Mediante potenciómetros de precisión colocados en el circuito, se ajustó manualmente el nivel de ganancia y la referencia de tensión para maximizar el rango dinámico de la onda de pulso antes de entregarla al canal de entrada analógica del microcontrolador (Arduino UNO).
### b) Explicación del circuito 

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/693145a0-f427-4e81-9438-3b4ceead571e" />
(Fig 1. Conección del circuito sensor TCST110 - arduino uno)

### c) ¿Qué hace cada componente? 

### d) Código de adquisición y cálculo SPI 
Para el funcionamiento del sistema se distinguió la detección de los componentes fisiológicos de la onda y el cálculo dinámico del índice SPI conforme a la ecuación descrita en el marco teórico:

### 1). Detección diferencial de picos sistólicos y valles diastólicos

    d1 = value - prev_value;
    d2 = d1 - prev_d1;

    if (prev_d1 > 0 && d1 < 0) && (d2 < 0) && (t - last_peak_time > refractory)

- Primera derivada (d1): Representa la pendiente instantánea de la señal. Cuando la pendiente cruza de valores positivos a negativos (prev_d1 > 0 && d1 < 0), se detecta un cambio de dirección correspondiente a un máximo local.
- Segunda derivada (d2): Representa la concavidad de la curva. La condición d2 < 0 confirma que el punto detectado corresponde a una cúspide o máximo real (curvatura negativa), evitando la detección de falsos picos en mesetas o zonas planas de la onda.
- Periodo refractario (refractory = 0.4): Define un tiempo de bloqueo de 400 milisegundos posterior a la detección de un pico. Esto impide que el algoritmo identifique picos múltiples en una misma onda cardíaca debido a artefactos de alta frecuencia o al paso de la muesca dicrota.

### 2). Extracción de parámetros SPI

    HBI = peak_time - last_peak_time_spi;   % Heart Beat Interval (s)
    PPGA = value - last_valley_value;       % Amplitud de pulso pico a valle (V)

El HBI se calcula restando el tiempo del pico sistólico actual con respecto al tiempo del pico inmediatamente anterior. Por su parte, la amplitud PPGA se determina mediante la diferencia de voltaje entre el pico sistólico y el valor del valle diastólico basal previo.
- Para adaptar la escala del índice a la variabilidad dinámica del paciente, los valores de HBI y PPGA se almacenan en búferes circulares de ventana móvil con un tamaño de 10 muestras (window_size = 10). Con cada nuevo latido detectado, ambas variables se normalizan en una escala de 0 a 100 mediante la transformación Min-Max:

      PPGA_norm = (PPGA - min(PPGA_buffer)) / (max(PPGA_buffer) - min(PPGA_buffer) + eps) * 100;
      HBI_norm  = (HBI  - min(HBI_buffer))  / (max(HBI_buffer)  - min(HBI_buffer)  + eps) * 100;

La constante infinitesimal eps se añade al denominador para evitar errores por división sobre cero en caso de que los valores extremos dentro de la ventana sean iguales. Finalmente, con las variables normalizadas se evalúa la ecuación estándar del SPI:

    SPI = 100 - (0.7*PPGA_norm + 0.3*HBI_norm);

Donde la ponderación asigna un 70% al componente vascular (PPGA_norm) y un 30% al componente cardíaco (HBI_norm), garantizando que a mayor respuesta nociceptiva o vasoconstricción el valor resultante del SPI se incremente hacia 100. La visualización en tiempo real se realiza graficando la señal filtrada con una ventana móvil fija de 10 segundos (xlim([t-10 t])). A continuación se muestran los resultados obtenidos en la gráfica de tiempo real y la gráfica de resultados SPI

<img width="1600" height="850" alt="image" src="https://github.com/user-attachments/assets/bcabcaaf-4f92-435e-8561-71d10f1d189d" />
(Fig 3. Gráfica señal PPG en tiempo real)

### e) Método ejecución CPT
Para validar la respuesta dinámica del sistema frente a un estímulo nociceptivo, se llevó a cabo el protocolo de la Prueba del Frío (Cold Pressor Test - CPT) con una botella de agua congelada sobre el cuello del sujeto de estudio durante un intervalo continuo de 120 segundos divididos en tres fases:  
- Fase de Línea de Base / Reposo (0 a 40 segundos): El sujeto de prueba permaneció sentado en estado de relajación y reposo absoluto, manteniendo la mano con el sensor TCST110 totalmente inmóvil para evitar artefactos de movimiento. En esta etapa se registraron las ondas fotopletismográficas basales para establecer la amplitud y la frecuencia cardíaca de referencia del individuo.  
- Fase de Estimulación Térmica Aguda (40 a 80 segundos): En el segundo 40, se aplicó un estímulo frío localizado en la región cervical (cuello) mediante la colocación de una botella con agua helada durante un periodo de 35 a 40 segundos. Esta maniobra activa intensamente los termorreceptores cutáneos cervicales, desencadenando una descarga simpática refleja que induce vasoconstricción periférica y permite verificar el incremento en el valor del SPI en la consola de MATLAB.  
- Fase de Recuperación (80 a 120 segundos): A los 80 segundos se retiró la botella fría del cuello del sujeto, continuando la captura de datos durante 40 segundos adicionales para registrar la vasodilatación compensatoria post-estímulo y la estabilización paulatina de la señal PPG hacia los valores de la línea de base.

## IV. ANÁLISIS Y RESULTADOS 

<img width="1600" height="765" alt="image" src="https://github.com/user-attachments/assets/d6f88da4-c650-40d6-a92c-643e01e47400" />
(Fig 4. Onda PPG antes y después de CPT)

## V. CONCLUSIONES

## VI. PREGUNTAS PARA LA DISCUSIÓN

### a) : ¿Cómo se relacionan las variaciones del volumen sanguíneo periférico con el balance autonómico?

Las variaciones en el volumen sanguíneo periférico están reguladas directamente por el sistema nervioso autónomo. Ante un estímulo de estrés o dolor, como la botella fría aplicada en el cuello, se desencadena una respuesta simpática reflexiva que libera noradrenalina y causa la contracción del músculo liso en los vasos sanguíneos periféricos (vasoconstricción). En la señal fotopletismográfica, esta vasoconstricción se evidencia directamente como una disminución acentuada en la amplitud de la onda de pulso (PPGA), tal como se observa en los gráficos a partir del segundo 40, donde la onda se vuelve más pequeña. Simultáneamente, el tono simpático acelera el ritmo cardíaco, reduciendo el tiempo transcurrido entre cada latido (HBI). Como el algoritmo del SPI le otorga un 70% de importancia a la reducción de la amplitud y un 30% a la aceleración de los latidos, la caída conjunta de ambas variables genera un incremento inmediato en el valor numérico del SPI en MATLAB, demostrando de forma clara el cambio del balance autonómico hacia un estado de estrés.

### b) ¿Cómo se compara el SPI con otros índices comúnmente empleados en cirugía, como el índice nocicepción-analgesia (ANI) y el índice de perfusión?

En la práctica clínica existen distintos índices para evaluar la respuesta autonómica: el SPI combina tanto la amplitud del pulso sanguíneo como el intervalo entre latidos, priorizando la vasoconstricción periférica para detectar respuestas rápidas al estrés; el ANI evalúa de manera aislada la variabilidad de la frecuencia cardíaca en altas frecuencias para medir exclusivamente el tono parasimpático o vagal; y el Índice de Perfusión (PI) mide únicamente la relación entre la sangre pulsátil y no pulsátil para evaluar el flujo vascular local sin considerar el tiempo entre latidos. En nuestra prueba ambulatoria, el sistema mostró dos limitaciones principales visibles en el registro gráfico: en primer lugar, demostró una alta susceptibilidad a los artefactos por movimiento, donde pequeños desplazamientos del dedo sobre el sensor TCST110 al retirar el objeto frío generaron picos de voltaje falsos que alteraron temporalmente el cálculo; en segundo lugar, al finalizar el estímulo frío ocurrió una vasodilatación reactiva compensatoria que aumentó la amplitud de la onda, lo que puede causar que el algoritmo interprete erróneamente un estado de relajación profunda cuando en realidad es solo una recuperación térmica de la piel.


## VII. REFERENCIAS BIBLIOGRÁFICAS 
[1] Universidad Militar Nueva Granada, Guía de Laboratorio No. 2: Cálculo Ambulatorio del Índice Pletismográfico Quirúrgico (SPI), Programa de Ingeniería Biomédica, Bogotá, Colombia, 2026.
[2] J. S. P. Ahonen, M. Uutela, e I. Korhonen, "Surgical Pleth Index (SPI) for monitoring nociception/analgesia balance during general anesthesia," Acta Anaesthesiologica Scandinavica, vol. 51, no. 7, pp. 815-822, 2007.
[3] M. Huiku et al., "Assessment of surgical stress level using photo-plethysmographic pulse wave amplitude and heart beat interval," Acta Anaesthesiologica Scandinavica, vol. 51, no. 9, pp. 1182-1192, 2007.
[4] R. Cowen, M. Stasiowski, H. Laycock, y C. M. Lopinto, "Assessing pain objectively: the Analgesia Nociception Index (ANI) and Surgical Pleth Index (SPI)," BJA Education, vol. 15, no. 3, pp. 117-123, 2015.
[5] Vishay Semiconductors, TCST1103, TCST1202, TCST1300 Transmissive Optical Sensor with Phototransistor Output, Datasheet Rev. 1.9, Document Number: 83763, 2019.
