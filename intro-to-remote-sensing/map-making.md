---
layout: page
title: Elaboración de mapas
parent: Introducción a SIG & Sensores Remotos (Teledetección)
nav_order: 5
---

# Elaboración de mapas

Hacer y personalizar un mapa es un componente crucial del análisis GIS. Usaremos un conjunto de puntos de altura generados desde Google Earth para el área de estudio con el fin de crear un modelo de elevación digital (DEM).

Primero, generamos un conjunto de puntos (ruta) en Google Earth. Agregamos nuestro shapefile de área protegida de tierra única.

<img align="center" src="../images/intro-rs-images/ex-4.1-ge-set-points.png" vspace="10" width="600">

Agreguemos un camino sobre nuestra capa y rastreémoslo a través de toda el área de estudio:

<img align="center" src="../images/intro-rs-images/ex-4.1-ge-add-path.png" vspace="10" width="500">

<img align="center" src="../images/intro-rs-images/ex-4.1-ge-new-path.png" vspace="10" width="600">

Llamemos a la ruta **points_jamaica_pa** y usemos la opción 'Guardar lugar como' con la extensión *.kml*.

<img align="center" src="../images/intro-rs-images/ex-4.1-ge-save-place.png" vspace="10" width="600">

Ahora podemos usar cualquiera de los servicios web en línea disponibles, por ejemplo, el visualizador GPS [(https://www.gpsvisualizer.com/elevation)](https://www.gpsvisualizer.com/elevation) para convertir nuestro archivo Kml en un archivo GPX, que es un formato válido para la entrada de puntos de altura en QGIS. Usamos el botón Convertir y agregar elevación, luego descargamos el archivo.

<img align="center" src="../images/intro-rs-images/ex-4.1-convert.png" vspace="10" width="600">

Podemos renombrar el archivo descargado. Ahora vamos a QGIS. Asegurémonos de haber instalado el Importador de segmentos GPX.

<img align="center" src="../images/intro-rs-images/ex-4.1-gpx-segmenter.png" vspace="10" width="600">

E importamos el archivo GPX.

<img align="center" src="../images/intro-rs-images/ex-4.1-gpx-segmenter-importer.png" vspace="10" width="600">

Posteriormente, extraemos los vértices. Establecemos un nombre de archivo de salida como **points_jamaica_pa.shp**.

<img align="center" src="../images/intro-rs-images/ex-4.1-extract-vertices.png" vspace="10" width="600">

<img align="center" src="../images/intro-rs-images/ex-4.1-extract-vertices-tool.png" vspace="10" width="600">

Ejecutamos el proceso y obtenemos nuestro shapefile de puntos de elevación. Abramos la tabla de atributos del archivo y podremos ver el campo de elevación con la información requerida para la creación del modelo digital de elevación (DEM).

<img align="center" src="../images/intro-rs-images/ex-4.1-attribute-table.png" vspace="10" width="600">

Ahora es el momento de generar nuestro DEM. Tenemos que abrir ToolBox y buscar la función Inverse Distance Weighted (IDW).

<img align="center" src="../images/intro-rs-images/ex-4.1-idw.png" vspace="10" width="500">

Seleccionamos nuestro shapefile de puntos o vértices y el atributo para la interpolación ***‘ele’***.

<img align="center" src="../images/intro-rs-images/ex-4.1-idw-tool.png" vspace="10" width="600">

Hacemos clic en el botón más y se agrega la capa correspondiente. Establecemos la extensión desde nuestro archivo de forma **negril_pa_shapefile_NoSea**, definimos un nombre **dem_negril_pa.tif** y ejecutamos el proceso.

<img align="center" src="../images/intro-rs-images/ex-4.1-idw-tool-inputs.png" vspace="10" width="600">

Nuestro DEM finalmente se crea. Se ve de color gris, por lo que debemos definir una configuración de visualización adecuada.

<img align="center" src="../images/intro-rs-images/ex-4.1-dem.png" vspace="10" width="300">

Vamos a Propiedades → Simbología → seleccionamos la opción de pseudo-color de banda única. Luego elegimos una Nueva rampa de color:

<img align="center" src="../images/intro-rs-images/ex-4.1-dem-pseudo-colors.png" vspace="10" width="600">

A continuación, seleccionamos la opción de tipo de rampa, la categoría Topografía, y elegimos la paleta de elevación.

<img align="center" src="../images/intro-rs-images/ex-4.1-dem-pseudo-colors-palette.png" vspace="10" width="600">

Ahora trabajemos en la creación de un mapa a partir de nuestras capas. Vamos al menú Proyecto. Necesitamos establecer un ***Nuevo diseño de impresión***:

<img align="center" src="../images/intro-rs-images/ex-4.1-print-layout.png" vspace="10" width="300">

Luego, dentro del menú ***Agregar elemento***, usamos ***Agregar mapa***:

<img align="center" src="../images/intro-rs-images/ex-4.1-add-map.png" vspace="10" width="600">

Debe incluir los elementos básicos para una creación cartográfica formal: leyenda, barra de escala, flecha de norte, título. Asegúrese de proporcionar un formato y una fuente apropiados para cada elemento.

<img align="center" src="../images/intro-rs-images/ex-4.1-add-legend.png" vspace="10" width="300">

Finalmente obtuvimos nuestro mapa. Podemos usar ‘Exportar imagen’, elegir el tipo de archivo (png, jpeg, bmp, etc.) y guardarlo en nuestra carpeta de trabajo.

<img align="center" src="../images/intro-rs-images/ex-4.1-final-map.png" vspace="10" width="600">

### Desafío 4: crear una capa de sombreado

Basado en el último DEM que acabamos de crear, cree un mapa de sombreado y luego cree un mapa con todos los elementos necesarios.

*Sugerencia: Use la herramienta HillShade del menú Ráster -> Análisis/Análisis del terreno*.

<img align="center" src="../images/intro-rs-images/ex-4.1-hillshade.png" vspace="10" width="600">
