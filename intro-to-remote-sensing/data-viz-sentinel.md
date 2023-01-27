---
layout: page
title: Data Visualization with Sentinel-2
parent: Introduction to Remote Sensing
nav_order: 4
---

# Visualización de datos con Sentinel-2

Los datos de Landsat son más útiles por su largo alcance temporal. La recopilación de datos para las misiones Landsat comenzó en 1972, aunque la mayoría de los investigadores utilizan datos de Landsat 5 y posteriores, que comenzaron en 1984. Sin embargo, la resolución temporal y espacial de los productos de datos Landsat se encuentra más en el rango moderado. Aunque el rango de recopilación de datos no es tan largo, los datos de Sentinel-2 tienen una resolución espacial y temporal más alta y también están disponibles gratuitamente. Esta lección examinará las diferencias entre los datos de Landsat y Sentinel-2 y cómo se compara el análisis de vegetación con los productos de datos de Sentinel-2 con los productos de Landsat 8.

## Objetivos

1. Comprender los factores a considerar al seleccionar datos de sensores remotos para usar en un proyecto de investigación.
2. Tenga en cuenta las diferencias entre los productos de datos Landsat y Sentinel-2.
3. Calcule el NDVI usando los productos de datos Sentinel-2.

## Selección de datos

Existen muchos tipos diferentes de fuentes de datos y productos de teledetección. Seleccionar los productos correctos puede ser un desafío, por lo que es muy importante comprender qué aspectos considerar en el proceso de selección. Considere los siguientes puntos en el contexto de su pregunta de investigación al seleccionar el tipo correcto de datos de teledetección.

1. **Resolución espacial**. Considere el tamaño de la característica que le interesa estudiar. Si está intentando clasificar la cubierta terrestre de un país, no es necesaria una alta resolución espacial e incluso podría dificultar el proceso de clasificación. Si la resolución espacial es tan alta que los objetos individuales, como los árboles, están formados por varios píxeles, es probable que cada píxel se vea diferente. En este ejemplo, cuando define la clase de bosque para su análisis de cobertura terrestre, debe tener en cuenta las diferencias en el sombreado, el color o la edad de los píxeles que forman los árboles en lugar de tener coherencia entre píxeles de mayor tamaño. Por otro lado, si está inspeccionando la cobertura de cultivos en fincas pequeñas, puede ser necesaria una resolución más alta.
2. **Resolución temporal y extensión**. Considere la frecuencia con la que necesita nuevos datos. Para un proyecto de monitorización diaria sería necesario un producto con alta resolución temporal. Para un estudio estacional, donde todas las imágenes pueden promediarse en una imagen para cada estación, podría ser aceptable una resolución temporal más baja. La extensión temporal, o qué tan atrás van los datos, importa dependiendo de la duración de su estudio. Para los análisis históricos, serían apropiados conjuntos de datos con grandes extensiones temporales.
3. **Resolución espectral**. Muchos sensores solo miden energía y almacenan los datos en un número limitado de bandas. Los satélites hiperespectrales recopilan datos con una mayor cantidad de detalles espectrales y, por lo tanto, pueden discernir una mayor cantidad de atributos en una región. Sin embargo, estos conjuntos de datos pueden ser más difíciles de procesar y se usan con menos frecuencia, por lo que podría ser difícil encontrar e implementar una metodología para responder a su pregunta de investigación.
4. **Disponibilidad de datos y productos asociados**. La cantidad de datos disponibles, su facilidad de acceso y el nivel de preprocesamiento dependen del proveedor de datos. Algunos conjuntos de datos, como Landsat y Sentinel-2, están disponibles gratuitamente y puede adquirir imágenes con varios niveles de preprocesamiento. El acceso a otros conjuntos de datos puede costar dinero o someterse a un procesamiento previo limitado. Además, algunos proveedores generarán productos utilizando datos de sensores que podrían ser útiles, como rásteres que analizan los niveles de clorofila-a o la productividad primaria bruta.
5. **Aplicabilidad**. Considere el tema de su proyecto de investigación. Diferentes tipos de datos son más adecuados para diferentes tipos de análisis. La detección de cambios, por ejemplo, utiliza con mayor frecuencia imágenes ópticas, mientras que las mediciones de la altura del dosel se calculan mejor utilizando datos LIDAR. Familiarizarse con la literatura sobre su tema para ver qué tipos de datos se usan comúnmente es un buen punto de partida.

No asuma que el producto de datos con la resolución espacial, temporal y espectral más alta es siempre la mejor opción. Con un aumento en la resolución viene un aumento en la cantidad de datos que deben procesarse. La potencia computacional requerida para analizar este tipo de datos y la capacidad de almacenamiento para contener esta cantidad de datos pueden actuar como factores limitantes importantes: asegúrese de sopesar los costos y beneficios de cada tipo de datos y seleccione el que mejor se adapte a su proyecto.

## Análisis de vegetación: Parte 2

Volvamos a la pregunta de investigación propuesta en la sección Análisis de vegetación: [Parte 1](data-viz-landsat.md) de Visualización de datos con Landsat: **¿Cómo difiere la cobertura vegetal entre áreas protegidas y no protegidas?** Para Para responder completamente a esta pregunta, realizaremos el mismo análisis de NDVI que hicimos en la Parte 1, pero esta vez usando datos de Sentinel-2.

Primero, tenga en cuenta las diferencias entre los datos de Landsat 9 y Sentinel-2 dentro del marco de selección de datos descrito en la sección anterior:

|   Características   |                                                    Landsat                                                   |                                    Sentinel-2                                   |
|:-------------------:|:------------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------:|
| Resolución espacial | 30 metros                                                                                                    | 10 metros                                                                       |
| Resolución temporal | 16 días (o 8 días en conjunto)                                                                               | 5 días                                                                          |
| Temporalidad        | 1984 - presente                                                                                              | 2015 - presente                                                                 |
| Disponibilidad      | Disponible de forma gratuita; muchos niveles de procesamiento diferentes y productos científicos disponibles | Disponible de forma gratuita; 3 niveles de procesamiento diferentes disponibles |
| Aplicabilidad       | Para aplicaciones de datos opticos                                                                           | Optical data applications                                                       |

Las principales diferencias entre los dos satélites radican en la resolución espacial y temporal y la extensión temporal. Examinemos si estos factores marcan una diferencia al evaluar nuestra pregunta de investigación sobre vegetación.

### Ejercicio 3.1 Calcular NDVI usando Sentinel-2

Este ejercicio proporciona un proceso paso a paso para crear y mostrar una nueva imagen de una sola banda de valores NDVI para Negril usando productos de datos Sentinel-2.

1. Abra QGIS.
2. En "Proyectos recientes", seleccione "Intro-detección remota" y ábralo.
3. Haga clic en "Capa > Agregar capa > Agregar capa ráster..." y haga clic en "..." junto al campo "Conjunto(s) de datos ráster" para buscar y seleccionar un archivo.
4. Navegue para seleccionar el archivo “s2-sr-negril-cloudfree.tif”. Haga clic en el botón "Abrir" y luego haga clic en "Agregar" para agregar la capa al lienzo.
5. Usaremos la fórmula NDVI para crear una nueva imagen de una sola banda que muestre los valores NDVI para la región. Seleccione "Procesamiento > Caja de herramientas" para abrir la caja de herramientas de QGIS.
6. Escriba "Calculadora ráster" en el cuadro de búsqueda o navegue hasta "Análisis ráster > Calculadora ráster" y haga doble clic para abrir la herramienta.
7. En la calculadora, puede escribir su propia expresión o usar una expresión predefinida. En el campo “Expresión”, copie y pegue el siguiente texto: (NIR - Red) / (NIR + Red)
8. Ahora, resalte la primera instancia donde dice "NIR" y haga doble clic en "s2-sr-negril-cloudfree@8" para reemplazarlo con la banda de imagen real. Repita para la otra instancia de "NIR".
9. A continuación, resalte la primera instancia donde dice "Rojo" y haga doble clic en "s2-sr-negril-cloudfree@4" para reemplazarlo con la banda de la imagen real. Repita para la otra instancia de "Rojo".

<img align="center" src="../images/intro-rs-images/ex-3.1-ndvi-expression.png" vspace="10" width="600">

10. Desplácese hacia abajo hasta el campo "Capa(s) de referencia". Haga clic en "..." y marque "s2-sr-negril-cloudfree-ndvi". Haga clic en Aceptar."
11. Haga clic en "... > Guardar en archivo..." junto al campo "Salida". Nombra el archivo "s2-sr-ndvi-negril". y haga clic en "Guardar".
12. Haga clic en "Ejecutar". Una vez que el proceso haya terminado de ejecutarse, haga clic en "Cerrar" para cerrar la ventana. Guarde el proyecto.

<img align="center" src="../images/intro-rs-images/ex-3.1-ndvi-canvas.png" vspace="10" width="600">

13. Personaliza la capa usando la misma rampa de color que elegiste en el Ejercicio 2.5. Guarde el proyecto.

<img align="center" src="../images/intro-rs-images/ex-3.1-ndvi-colored-canvas.png" vspace="10" width="600">

Active y desactive las capas Sentinel-2 y Landsat 8 NDVI. ¿Cómo difieren estos resultados? ¿Qué conjunto de datos sería mejor usar para responder la pregunta de investigación (si es que hace alguna diferencia)? No existe una manera perfecta de determinar el mejor conjunto de datos para usar en un proyecto determinado; a veces se necesita un poco de experimentación para encontrar el mejor ajuste.

### Desafío 3: cuantificar el NDVI derivado de Sentinel-2

Use QGIS para encontrar los valores medios y medianos de NDVI dentro de las áreas protegidas dentro de Negril y en las áreas no protegidas de Negril. Compare los valores para ver si hay una diferencia en los niveles de vegetación entre áreas protegidas y no protegidas. ¿Estos valores difieren de los valores calculados en el Desafío 1?

* *Sugerencia 1: deberá agregar negril-pa-shapefile.zip y **non_protectedArea_savanna.shp** como dos capas diferentes en el proyecto.*
* *Sugerencia 2: La herramienta Estadísticas zonales será útil en este ejercicio.*