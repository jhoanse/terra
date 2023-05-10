---
layout: page
title: Filtrado y Máscaras
parent: Introducción a Google Earth Engine - Parte 2
nav_order: 1
---

## Script
El script completo que se usará en esta sección esta disponible [aquí]().

# Filtrado de colecciones

Las colecciones puede ser filtradas de acuerdo a localización, fechas, y propiedades. Por ejemplo, hay colecciones que vienen segmentadas en escenas ('tiles') y es necesario usar un criterio válido para filtrar las escenas que nos interesan. Este es un punto de partida para casi cualquier proceso o estudio que deseen hacer. Aqui podemos ver la diferencia entre las escenas de Landsat y Sentinel-2.

<img align="center" src="../../images/intro-gee/06_fig1.png" vspace="10" width="700"> 

Vamos a filtrar la colección de Sentinel-2 por localidad (Medellín), fecha, y porcentaje de nubes. Para filtrar por localidad podemos usar la funcion `filterBounds` la cual permite filtrar segun las coordenadas de una figura geométrica en un área de interés. Para esto vamos a dibujar un punto o polígono sobre Medellín y la vamos a renombrar "medellin" para ser usada en el siguiente ejemplo. Luego filtraremos por fecha usando la función `filterDate`, la cual permite filtrar elementos en un rango de fechas. La fecha debe ser escrita como texto y en formato YYYY-MM-DD.

```javascript
// Cargar Image Collection
var sentinel2 = ee.ImageCollection("COPERNICUS/S2_SR");

// Filtrar imágenes de colección por localidad y fecha.
var filtro1 = sentinel2.filterBounds(medellin).filterDate('2021-01-01','2021-12-31');

// Verificar cantidad de imágenes disponibles en esa localidad y fecha específicas:
print('Colección filtrada 1:',filtro1);
```
