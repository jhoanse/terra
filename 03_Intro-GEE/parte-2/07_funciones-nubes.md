---
layout: page
title: Máscara de Nubes y Funciones Avanzadas
parent: Introducción a Google Earth Engine - Parte 2
nav_order: 2
---

## Script
El script completo que se usará en esta sección esta disponible [aquí]().

# Enmascaramiento de nubes y funciones avanzadas

Hasta ahora hemos aprendido como acceder a colecciones, conocer su propiedades, filtrarlas, y visualizarlas. Pero, para poder sacar más provecho a los datos geoespaciales hay que aprender procesarlos, y para esto hay que aprender a escribir nuestras propias funciones. En este segmento vamos a usar un grupo de imágenes Sentinel-2 sobre nuestra área de interés, y vamos a aplicar una serie de funciones para preparar nuestras imágenes antes de aplicar una máscara de nubes. Finalmente, realizaremos un mosaico compuesto final para observar nuestra área de interés casi limpia de nubes.

Podemos usar la parte inicial del código usado en la sección anterior. Cargaremos la coleccion de Sentinel-2 y filtramos sobre la ciudad de Medellín.

```javascript
// Cargar Image Collection
var sentinel2 = ee.ImageCollection("COPERNICUS/S2_SR");

// Filtrar imágenes de colección por localidad, fecha y cantidad de nubes.
var filtro = sentinel2.filterBounds(medellin).filterDate('2021-01-01','2021-12-31');
              .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',30));

// Verificar cantidad de imagénes:
print('Colección filtrada:',filtro);

// Extraer lista de IDs:
print('Lista de IDS:', filtro.aggregate_array('system:index'));

// Sellecionar una imagen cualquiera:
var image = ee.Image("COPERNICUS/S2_SR/20210118T152639_20210118T153102_T18NVM");
```

Luego de seleccionar una imágen cualquiera de la lista, vamos a proceder a escalarla para poder obtener unidades de reflectancia correctas. Esto se puede verificar en la documentación de cada colección. No todas las colecciones tienen las mismas escalas o unidades, o incluso las bandas dentro de una misma colección pueden tener diferentes valores de escala. Esto hay que tenerlo presente y ser cuidadoso si vamos a usar imágenes sátelitales para análisis más avanzados.

<img align="center" src="../../images/intro-gee/07_fig1.png" vspace="10" width="700"> 

Para Sentinel-2 el factor de escala en todas las bandas multiespectrales es de 0.0001. Vamos a usar la siguiente función para escalar las bandas multiespectrales. Sin embargo si solo hacemos esto, la función regresará una imágen con únicamente esas bandas. Por eso vamos a incluir otra línea de código para extraer las bandas QA que vamos a necesitar más adelante para enmascarar nubes.

```javascript
/// Función para re-escalar reflectancia de imagenes Sentinel-2:
var escalar_s2 = function(image){
  var opticalBands = image.select('B.').divide(10000); // o multiply(0.0001)
  var qaBands = image.select('QA.*');
  return image.addBands(opticalBands, null, true)
              .addBands(qaBands, null, true);
};
```

La función la podemos aplicar a un solo elemento o un ee.Image de la siguiente forma:

```javascript
// Aplicar función para escalar reflectancia de imagen S2:
var img_scaled = escalar_s2(image);

// Visualizar imagen:
var params = {min:0,max:0.2, bands:['B4','B3','B2']};
Map.addLayer(img_scaled,params,'Imagen S2');
```

La imagen fue escalada, y ahora el rango para visualizar la imágen RGB es 0 a 0.2, apróximadamente.

<img align="center" src="../../images/intro-gee/07_fig2.png" vspace="10" width="600"> 
