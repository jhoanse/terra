---
layout: page
title: Indices
parent: Google Earth Engine Avanzado - Parte 1
nav_order: 1
---

## Script
El script completo que se usará en esta sección esta disponible [aquí]().

# Índices y operaciones matématicas

En sesiones anteriores vimos como extraer información de productos disponibles en GEE. Uno de ellos fue el producto de MODIS Terra ["MODIS/061/MOD13Q1"](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD13Q1) que entrega indices de vegetación (NDVI y EVI) cada 16 días a 250m / pixel.
Sin embargo, podemos usar cualquier colección con datos multiespectrales para calculor nuestros propios índices, ya sea para obtener mejor resolución espacial o temporal, o información que no esté disponible en GEE.

Para los siguientes ejemplos usaremos la colección de [Landsat-8 L2](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC08_C02_T1_L2#description), para lo cual vamos a precargar algunas funciones primero:

```javascript
// Funciones precargadas:

// Función enmascarar nubes Landsat-8:
function maskL8clouds(image) {
  var qa = image.select('QA_PIXEL'); //Select the QA band

  // Bit 3 es nubes.
  var cloudBitMask = 1 << 3;  
  var mask = qa.bitwiseAnd(cloudBitMask).eq(0);

  return image.updateMask(mask);
}

// Función para aplicar factores de escala Landsat-8:
function applyScaleFactors(image) {
  var opticalBands = image.select('SR_B.').multiply(0.0000275).add(-0.2);
  var thermalBands = image.select('ST_B.*').multiply(0.00341802).add(149.0);
  return image.addBands(opticalBands, null, true)
              .addBands(thermalBands, null, true);
}
```

Adicionalmente, vamos a preparar una colección de imágenes, filtrando, aplicando factores de escala y limpiando nubes:

```javascript
// Polígono de Colombia
var colombia = ee.FeatureCollection("USDOS/LSIB/2017").filter(ee.Filter.eq('COUNTRY_NA','Colombia'));

// Cargar imagenes Landsat-8 y pre-procesar:
var coleccion = ee.ImageCollection("LANDSAT/LC08/C02/T1_L2")
                .filterBounds(colombia)
                .filterDate('2022-01-01','2022-12-31')
                .filter(ee.Filter.lt('CLOUD_COVER',30))
                .map(maskL8clouds)
                .map(applyScaleFactors);

// Generar mosaico compuesto y recortar:
var compuesto = coleccion.median().clip(colombia);

// Visualizar mapa:
Map.addLayer(compuesto,{bands:['SR_B4','SR_B3','SR_B2'],min:0,max:0.2},'Compuesto');
```

Vamos a obtener un mosaico como el siguiente, con algunos vacios de información en la zona Andina de Colombia, pero con buena calidad en sur, este y norte.

<img align="center" src="../../images/gee-avanzado/01_fig1.png" vspace="10" width="500">

## NDVI - Normalized Difference Vegetation Index:

El índice de diferencia de vegetación normalizada (NDVI) tiene un rango de valores entre -1 a +1. Típicamente, cuando hay valores negativos existe alta probabilidad que se trate de un cuerpo de agua. Por otro lado, si los valores son cercanos a +1 es probable que se trate de vegetación muy densa. Cuando el NDVI es cercano a cero es probable que se trate de un área urbana.

Para calcular el NDVI usamos la siguiente fórmula:

$$\frac{(NIR-Rojo)}{(NIR+Rojo)}$$

La relación entre las bandas NIR (infrarrojo cercano o Near-Infrared) y Rojo van a proporcionar un índice de vegetación.

<img align="center" src="../../images/gee-avanzado/01_fig2.jpg" vspace="10" width="500">

```javascript
// Calcular NDVI:
// NDVI = (NIR - RED) / (NIR + RED)
var NIR = compuesto.select('SR_B5');
var Red = compuesto.select('SR_B4');
var ndvi = NIR.subtract(Red).divide(NIR.add(Red));

// Paleta de verdes:
var palette = [
    'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901',
    '66A000', '529400', '3E8601', '207401', '056201', '004C00', '023B01',
    '012E01', '011D01', '011301'];

// Visualizar:
Map.addLayer(ndvi, {'palette': palette}, "NDVI");
```

Otra alternativa para calcular indices normalizados es usar la función `normalizedDifference`. Para este ejemplo, nuestro NDVI podría ser calculado como `var ndvi = composite.normalizedDifference(['SR_B4', 'SR_B3'])`.

<img align="center" src="../../images/gee-avanzado/01_fig3.png" vspace="10" width="500">

## EVI - Enhanced Vegetation Index:

El índice de vegetación aumentada es similar al NDVI, pero es usado para cuantificar la "verdura" de la vegetación. Este índice puede corregir algunas condiciones atmosféricas, ruido de fondo, y es más sensible en áreas de vegetación densa. 

Para calcular el EVI usamos la siguiente fórmula:

$$2.5 * \frac{NIR-Rojo}{NIR + 6 * Rojo - 7.5 * Azul + 1}$$

Esta formula es un poco distinta a los típicas diferencias normalizadas, y puede ser aplicada en GEE usando la función `expression` que permite crear expresiones matemáticas y aplicarlas sobre una imágen.

```javascript
// Calcular EVI usando expresiones:
// EVI = 2.5 * [(NIR-RED) / (NIR + 6*RED - 7.5*BLUE)+1]
var evi = compuesto.expression(
    '2.5 * ((NIR - RED) / (NIR + 6 * RED - 7.5 * BLUE + 1))', {
      'NIR': compuesto.select('SR_B5'),
      'RED': compuesto.select('SR_B4'),
      'BLUE': compuesto.select('SR_B2')
});

// Visualizar EVI:
Map.addLayer(evi, {'min': -1, 'max': 1, 'palette': ['FF0000', '00FF00']}, 'EVI');
```

<img align="center" src="../../images/gee-avanzado/01_fig4.png" vspace="10" width="500">

## NDWI - Normalized Difference Water Index:

Este índice permite detectar cuerpos de agua usando las bandas Verde y NIR. Las propiedades opticas del agua permiten mayor penetración de la longitud de onda verde, en comparación con el infrarrojo, el cual es absorbido inmediatamente en superficie. Sin embargo, los cuerpos de agua pueden contener diversos elementos disueltos que pueden alterar la eficacia de detección por el índice, por ejemplo alto contenido de algas, material suspendido, o materia orgánica disuelta. El umbral para detección de agua está alrededor de 0.3, donde valores más altos que este indican presencia de agua. 

La fórmula para calcular el NDWI es:

$$\frac{(Verde-NIR)}{(Verde+NIR)}$$

En GEE vamos a crear una función donde se incluya el cálculo del NDWI y otras variables para limpiar la máscara y visualizar solo píxeles de agua usando un umbral definido, pero que puede ser modificado. Adicionalmente, aplicaremos el NDWI sobre un área de interés más pequeña, que en este ejemplo será en el noroccidente de Colombia.

```javascript
// NDWI Función
function calcularNdwi(Image){
  var getNdwi = Image.normalizedDifference(['SR_B3', 'SR_B5']);
  getNdwi = getNdwi.select(['nd'],['ndwi']);
  var ndwiUmbral = getNdwi.gt(0.1);
  var ndwiMask = ndwi_thr.updateMask(ndwiUmbral);
  return ndwiMask;
}

// Aplicar NDWI y crear mosaico:
var ndwi = coleccion.map(calcularNdwi).mosaic().clip(aoi);

// Visualizar:
Map.addLayer(ndwi,{palette:'#0439ff'},'Agua');
}
```

<img align="center" src="../../images/gee-avanzado/01_fig5.png" vspace="10" width="500">
