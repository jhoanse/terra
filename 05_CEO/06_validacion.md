---
layout: page
title: Introducción a la validación de mapas - Matriz de confusión
parent: Collect Earth Online - Evaluación de mapas
nav_order: 6
---

# Introducción a la validación de mapas - Matriz de confusión

La validación del mapa puede realizarse comparando las clases cartográficas de los puntos representativos de la muestra con las etiquetas de referencia en esos lugares, que se recogen mediante interpretación humana y se consideran etiquetas "correctas" para esos puntos. Si los índices de concordancia entre las etiquetas del mapa y las etiquetas de referencia del intérprete son muy altos, podemos deducir que el mapa es una buena representación de las características cartografiadas.

Podemos cuantificar la precisión del mapa utilizando una matriz de confusión (matriz de error). Los datos de referencia dictan el valor real (la verdad) mientras que la izquierda muestra la predicción (o clasificación del mapa)

- Verdadero positivo y verdadero negativo significan que la clasificación clasificó correctamente las etiquetas (por ejemplo, un píxel inundado se clasificó correctamente como inundado).
- Los falsos positivos y falsos negativos significan que la clasificación no coincide con la verdad (por ejemplo, un píxel inundado se clasificó como no inundado).

<img align="center" src="../images/ceo/7A_confusionmatrix.png" vspace="10" width="400"> 

Rellenemos esta matriz de confusión con valores de ejemplo si se recogieran 100 puntos.

<img align="center" src="../images/ceo/7B_accuraciestable.png" vspace="10" width="600"> 

**Precisión del productor**

* El porcentaje de veces que una clase identificada sobre el terreno se clasifica en la misma categoría en el mapa. La precisión del productor es la precisión del mapa desde el punto de vista del cartógrafo (productor) y se calcula como el número de píxeles correctamente identificados de una clase determinada dividido por el número total de píxeles de esa clase de referencia. Es decir, para una clase determinada en los píxeles de referencia, ¿cuántos píxeles del mapa se clasificaron correctamente?
* Exactitud del productor (para inundación) = Verdadero positivo / (Verdadero positivo + Falso positivo)
* Exactitud del productor (sin inundación) = Verdadero negativo / (Verdadero negativo + Falso negativo)

**Error de omisión**  

* Los errores de omisión se refieren a los píxeles de referencia que quedaron fuera (u omitidos) de la clase correcta en el mapa clasificado. Un error de omisión se contabilizará como un error de comisión en otra clase
* Error de omisión = 100% - Precisión del productor
* Error de Omisión (Inundación) es cuando 'inundación' se clasifica como alguna 'otra' categoría.

**Precisión del usuario**

* Porcentaje de veces que una clase identificada en el mapa se clasifica en la misma categoría sobre el terreno. La precisión del usuario es la precisión desde el punto de vista de un usuario del mapa, no del creador del mapa, y se calcula como el número identificado correctamente en una determinada clase del mapa dividido por el número que se afirma que se encuentra en esa clase del mapa. La precisión del usuario nos indica esencialmente con qué frecuencia la clase que aparece en el mapa es realmente esa clase sobre el terreno.
* Precisión del usuario (para inundación) = Verdadero Positivo / (Verdadero Positivo + Falso Negativo)
* Precisión del usuario (para no inundación) = Verdadero Negativo / (Verdadero Negativo + Falso Positivo)

**Error de comisión**

* Los errores de comisión se refieren a los píxeles de clase que se clasificaron erróneamente en el mapa y son complementarios a la precisión del usuario
* Error de comisión = 100% - precisión del usuario.
* El error de comisión (inundación) se produce cuando "otro" se clasifica como "inundación".

**Precisión global**

* Precisión global = (Verdadero positivo + Verdadero negativo) / Tamaño de la muestra
* La precisión global nos indica esencialmente qué proporción de los datos de referencia se clasificó correctamente.

**Ejemplo**

Un ejemplo para el mapa que utilizamos se encuentra [aquí](https://docs.google.com/spreadsheets/d/1G_ailWx4FRtbNY__Ck38tD1qZlCRdxMg/edit?usp=sharing&ouid=117588040825190888554&rtpof=true&sd=true). Toma en cuenta que necesitamos verificar clasificación vs. referencia y crear la matriz de confusión. Después, calcular las métricas. En AREA2 es posible hacer los calculos automaticamente, una vez que proporcionemos la tabla en su formato correcto. Para más informaciones, acceder a la [documentación de AREA2](https://area2.readthedocs.io/en/latest/) y [script de estimación estratificada](https://code.earthengine.google.com/2c42b4b050967405da4d3d4187b731c4). En este taller mencionamos solamente la contaje de píxeles para verificar la exactitud, pero es recomendado hacer como vemos en el ejemplo acima, tomando en cuenta la area de cada clase. Para más detalles, consulte [Olofsson et al. (2014)](https://www.sciencedirect.com/science/article/abs/pii/S0034425714000704).
