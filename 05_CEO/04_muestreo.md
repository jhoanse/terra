---
layout: page
title: Introducción al Muestreo Aleatorio Estratificado
parent: Collect Earth Online - Evaluación de mapas
nav_order: 4
---

# Introducción al Muestreo Aleatorio Estratificado

Existen varias formas de muestrear una zona para conseguir una muestra representativa del paisaje y de las variaciones que existen en él. Los datos de muestreo recogidos en CEO pueden utilizarse para diversos fines, incluida la validación de mapas.

<img align="center" src="../images/ceo/4D_systematicsampling.png" vspace="10" width="250"> 

**Muestreo sistemático**: las observaciones se colocan a intervalos iguales según una estrategia.

<img align="center" src="../images/ceo/4E_randomsampling.png" vspace="10" width="250"> 

**Muestreo aleatorio simple**: observaciones colocadas aleatoriamente independiente de las clases.

<img align="center" src="../images/ceo/4F_stratifiedsampling.png" vspace="10" width="250"> 

**Muestreo aleatorio estratificado**: Utilizando un mapa para informar el diseño, un número mínimo de observaciones colocadas aleatoriamente en cada clases.

El muestreo aleatorio estratificado tiene dos ventajas fundamentales:

* Actualiza las áreas basadas en el mapa para aumentar la precisión (reduce la incertidumbre)
* Ayuda a aumentar la probabilidad de tener parcelas en clases poco frecuentes



**Ejemplo 1 de muestreo Aleatorio estratificado:**

Utilizaremos el muestreo aleatorio estratificado para realizar un análisis de validación de mapas. Puede utilizar diferentes herramientas, como Google Earth Engine, para generar las ubicaciones de los puntos de muestreo.

Utilizaremos el mapa que desarrollamos en una sesion impartida (T5) para ilustrar este proceso. Recuerde que se trata de una imagen de diferencias MNDWI de dos fechas con umbrales de clasificación, en la que se destacan las regiones de ganancia de agua en azul y pérdida de agua en rosa. Este producto fue generado con una diferencia de imágenes de 2015-2016 y 2022-2023.

<img align="center" src="../images/ceo/4G_ChangeMapForSampling.png" vspace="10" width="600"> 

Hemos calculado previamente los recuentos aproximados de píxeles de las clases de mapas utilizando Google Earth Engine.

| Valor mapa | Clase     | Conteo de píxeles | 
|------------|-----------|-------------|
| 2020       | 2020 | 20595572899 |
| 2021       | 2021  | 17196112.61 |
| 2022       | 2022   | 2133022.105 | 
| 2023       | 2023   | 2133022.105 | 

Si hubiéramos utilizado un muestreo sistemático o aleatorio, sólo obtendríamos un punto de ganáncia o pérdida de agua en menos de 1 de cada 100 puntos recogidos. Eso es súper ineficiente para analizar las clases de interés. Con el muestreo aleatorio estratificado puedo poner un valor mínimo al número de puntos de cada clase cartográfica, o estrato.

El muestreo aleatorio estratificado se realizó en GEE utilizando este [script](https://code.earthengine.google.com/bdf75ed7bfde4024268dee12e3842225), que exporta un CSV de las ubicaciones de las muestras listo para ser importado en CEO.

Definimos precisiones de usuário deseadas como 80% para cada clase y un error estimado para la precisión general de 2%. Eso generó un tamaño de muestreo de 400 puntos. Como las áreas de las clases de interés son muy pequeñas, la asignación proporcional resultó en cero para esas clases. Pero podemos definir una asignación de 200 puntos para la clase "No cambio", 100 puntos para la clase "Ganancia" y 100 puntos para clase "Perdida".

*Descargue ese archivo CSV [aquí](https://drive.google.com/file/d/1S3gsy77cTqVqhypm4URjlNAp813DHQi1/view?usp=sharing).*

Las 400 muestras incluidas se distribuyen dentro de los estratos cartográficos asignados.
<img align="center" src="../images/ceo/4H_samplesonmap.png" vspace="10" width="600">



 **Muestreo Aleatorio estratificado de alertas Terra-i para el periodo 2020-2023:**
 
Utilizaremos el muestreo aleatorio estratificado para realizar un análisis de validación de alertas de pérdida de cobertura Terra-i. Puede utilizar diferentes herramientas, como Google Earth Engine, para generar las ubicaciones de los puntos de muestreo.

Se emplea las alertas Terra-i para los años 2020-2021-2022-2023.

Se ha calculado previamente los recuentos  de píxeles de las clases/año.

| Valor mapa | Clase     | Conteo de píxeles | 
|------------|-----------|-------------|
| 2020       | 2020 | 20595572899 |
| 2021       | 2021  | 17196112.61 |
| 2022       | 2022   | 2133022.105 | 
| 2023       | 2023   | 2133022.105 | 

Si se hubiera empleado un muestreo sistemático o aleatorio, sólo obtendríamos un punto de ganáncia o pérdida de agua en menos de 1 de cada 100 puntos recogidos. Eso es súper ineficiente para analizar las clases de interés. Con el muestreo aleatorio estratificado puedo poner un valor mínimo al número de puntos de cada clase cartográfica, o estrato.

El muestreo aleatorio estratificado se realizó en GoogleEarthEngine utilizando este script [script](https://code.earthengine.google.com/bdf75ed7bfde4024268dee12e3842225), que exporta un CSV de las ubicaciones de las muestras listo para ser importado en CEO.

Definimos precisiones de usuário deseadas como 84% para cada clase y un error estimado para la precisión general de 5%. Eso generó un tamaño de muestreo de xxx puntos. Cuando las áreas de las clases de interés son muy pequeñas, la asignación proporcional resulta en cero para esas clases. Pero podemos definir una asignación mayor a 30.

Descargue ese archivo CSV [aquí](https://drive.google.com/file/d/1S3gsy77cTqVqhypm4URjlNAp813DHQi1/view?usp=sharing).

Las xx muestras incluidas se distribuyen dentro de los estratos cartográficos asignados.

A continuación se presenta el paso a paso para realizar el muestreo estartificado aleatorio empleando Google Earth Engine:

* Subir la información de pérdida de vegetación Terra-i al Asset de GEE

<img align="center" src="../images/ceo/crs_img25.png" vspace="10" width="600">

* Abrir link de AREA2: https://code.earthengine.google.com/3400a6f1f9197ad92a273c194b71e4f7
* Ejectuar script de AREA2: Presionar "RUN"
* Cambiar ruta de la imagen del área de estudio (Ruta del raster de périda de vegetación de Terra-i: users/ingperezescobar/HN/alertas_terrai)
* Cargar imagen: 'Load image'

<img align="center" src="../images/ceo/crs_img26.png" vspace="10" width="600">

* Seleccionar el método de muestreo: "Target SE of overall accuracy"
* Especificar la exactitud de usuario por clase: 0.84 para todas
* Especificar el error estándar de la exactitud global: 0.05
* Calcular tamaño de muestra: "Calculate sample size"
* Introducir el tamaño de muestra sugerido: 11, 6, 4, 24 (Obeservar el que aparece en GEE)
* Crear muestreo >> Adicionar al mapa >> exportar muestras

<img align="center" src="../images/ceo/crs_img27.png" vspace="10" width="600">

<img align="center" src="../images/ceo/crs_img28.png" vspace="10" width="600">
