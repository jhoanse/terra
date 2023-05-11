---
layout: page
title: Productos Ambientales
parent: Introducción a Google Earth Engine - Parte 2
nav_order: 3
---

## Script
El script completo que se usará en esta sección esta disponible [aquí]().

# Productos ambientales y climáticos

Vamos a explorar diferentes productos ambientales y climáticos disponibles en GEE. Estos productos vienen en diferentes resoluciones temporales y espaciales, por tal razón el uso de cierto producto va a depender de modo, tiempo, lugar que queremos observar o monitorear. Adicionalmente se realizarán ejemplos aplicados para no solo visualizar estos productos, sino también sobre extraer información básica en áreas específicas, y sobre cómo exportar los datos.

Primero vamos a precargar algunas variables. Vamos a usar el polígono de Colombia para recortar las imágenes; cargaremos el paquete de paletas para ayudarnos a visualizar los productos; y vamos a predefinir el rango de fechas de interés para ser consistente a través de todas las colecciones:

```javascript
// Poligono de Colombia
var colombia = ee.FeatureCollection("USDOS/LSIB/2017").filter(ee.Filter.eq('COUNTRY_NA','Colombia'));

// Cargar paquete de paletas
// https://github.com/gee-community/ee-palettes
var repo = require('users/gena/packages:palettes');

// Fechas de interés:
var fechaIni = '2023-03-01';
var fechaFin = '2023-03-30';
```

## Precipitación

Vamos a usar la colección ["UCSB-CHG/CHIRPS/DAILY"](https://developers.google.com/earth-engine/datasets/catalog/UCSB-CHG_CHIRPS_DAILY), la cual es un producto de precipitación de CHIRPS con más de 30 años de datos globales. Esta colección tiene datos diarios a una resolución espacial de 5.5 km / píxel.

Esta colección la vamos a filtrar, vamos usar `sum()` para obtener la precipitación mensual, y recortaremos usando el polígono de Colombia. Es importante leer la documentación de cada colección para conocer cuántas bandas tiene, en qué unidades está, y qué factor de escala maneja. Esta colección está en mm/día y no requiere aplicar ningun factor de escala.

```javascript
// Cargar colección CHIRPS de precipitación diaria:
var chirps = ee.ImageCollection("UCSB-CHG/CHIRPS/DAILY");

// Filtrar colección, sumar precipitaciones del mes, y recortar:
var prec = chirps
            .filterDate(fechaIni,fechaFin)
            .filterBounds(colombia)
            .sum()
            .clip(colombia);

// Cargar paleta de color y visualizar capa:
var precPaleta = repo.colorbrewer.YlGnBu[9];
Map.addLayer(prec,{min:0, max:750, palette:precPaleta},'Precipitacion');
```

En solo unas líneas de código podemos visualizar nuestra imágen. En color más pálido los valores más bajos de precipitación (0 mm/mes) y en azul oscuro los valores más altos (750 mm/mes):

<img align="center" src="../../images/intro-gee/08_fig1.png" vspace="10" width="500">

## Temperatura

Vamos a usar la colección ["MODIS/061/MOD11A1"](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD11A1), producto de MODIS Terra


