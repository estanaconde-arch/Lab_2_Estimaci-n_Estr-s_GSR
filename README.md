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
- 
### b) Explicación del circuito

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/693145a0-f427-4e81-9438-3b4ceead571e" />
(Fig 1. Conección del circuito sensor TCST110 - arduino uno)

### c) ¿Qué hace cada componente?

### d) Código de adquisición y cálculo SPI

### e) Método ejecución CPT

## IV. ANÁLISIS Y RESULTADOS 

<img width="1600" height="850" alt="image" src="https://github.com/user-attachments/assets/bcabcaaf-4f92-435e-8561-71d10f1d189d" />
(Fig 3. Gráfica señal PPG en tiempo real)

<img width="1600" height="765" alt="image" src="https://github.com/user-attachments/assets/d6f88da4-c650-40d6-a92c-643e01e47400" />
(Fig 4. Onda PPG antes y después de CPT)

## V. CONCLUSIONES

## VI. PREGUNTAS PARA LA DISCUSIÓN

## VII. REFERENCIAS BIBLIOGRÁFICAS 


