---
layout: page
title: Series de Tiempo
parent: Google Earth Engine Avanzado - Parte 2
nav_order: 2
---

## Script
El script completo que se usará en esta sección esta disponible [aquí]().

# Series de Tiempo & Ojetos UI

Las series de tiempo se pueden obtener de colecciones de imágenes o features, sobre pixeles individuales o regiones, en modo de gráficos o tablas. En los siguientes ejemplos se mostrará como procesar y obtener datos mensuales de variables ambientales. Adicionalmente, es necesario familiarizarse con los objetos UI de GEE, en especial `ui.Chart`, que nos permitrá hacer gráficas dentro de la interfaz de GEE. Documentación adicional sobre las gráficas pueden encontrarse [aquí](https://developers.google.com/earth-engine/guides/charts_overview).

Inicialmente cargaremos algunas variables que serán usados a lo largo del script:

```javascript
// Precargar variables:

// Poligono de Colombia
var colombia = ee.FeatureCollection("USDOS/LSIB/2017").filter(ee.Filter.eq('COUNTRY_NA','Colombia'));

// PNN
var pnn = ee.FeatureCollection('users/lsandoval-sig/PNN_Colombia_2023');

// Fechas de interés:
var fechaIni = '2022-01-01';
var fechaFin = '2022-12-30';

// Crear lista númerica de meses:
var months = ee.List.sequence(1, 12);

// Función para obtener promedios mensuales
function datosMes(coleccion, mes, reducer){ 
  //Extraer año y para introducirlo automaticamente en la imagen saliente
  var fecha = coleccion.first().get('system:time_start');
  var yr = ee.Date(fecha).get('year');
  // Aplicar reductor a grupo de imagenes mes por mes.
  var calculo = coleccion
                .filter(ee.Filter.calendarRange(mes, mes, 'month'))
                .reduce(reducer)
                .set('month', mes)
                .set("system:time_start", ee.Date.fromYMD(yr, mes, 1).millis());
  return calculo;
}
```

## Serie de tiempo por pixel y localidades:

Para este ejercicio usaremos la colección de ["MODIS/061/MOD11A1"](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD11A1). Vamos a procesar esta colección para observar cómo varía la temperatura a lo largo del año 2022 en la zona urbana y en zona de bosque en la ciudad de Bucaramanga. Estas dos localidades serán variables de tipo `ee.Features` con una propiedad llamada "sitio", respectivamente.

```javascript
var ciudad = /* color: #999900 */ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([-73.12524872642457, 7.120345156458741]),
            {
              "sitio": "ciudad",
              "system:index": "0"
            })]),
    bosque = /* color: #009999 */ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([-73.0902941785425, 7.120259987689842]),
            {
              "sitio": "bosque",
              "system:index": "0"
            })]),
```

Luego, es necesario tener las dos localidades en un solo `ee.FeatureCollection`, y se pre-procesará la colección de temperatura:

```javascript
// Unir puntos en una colección, que se usará para tomar datos de temperatura
var puntos = ciudad.merge(bosque);

// Cargar producto de Modis Terra MOD11A1.061 y seleccionar banda de temperatura
var mod11a1 = ee.ImageCollection("MODIS/061/MOD11A1").select('LST_Day_1km');

// Filtrar colección, sumar precipitaciones del mes, y recortar:
var temp = mod11a1
            .filterDate(fechaIni,fechaFin)
            .filterBounds(colombia);

// Aplicar factor de escala, convertir a Celsius, obtener promedio, y recortar figura:
var tempEscala = temp
                  .map(function(x){return x.multiply(0.02).subtract(273.15)
                    .copyProperties(x,['system:time_start','system:time_end']); // Importante copiar las fechas
                  });
```

Ahora, tenemos una colección pre-procesada con datos diarios. En este momento podemos realizar una gráfica con datos diarios. Adicionalmente, damos un poco de formato y estilo:

```javascript
// Elaborar gráfico con datos diarios de temperatura en dos puntos.
var tempChart = ui.Chart.image.seriesByRegion({
  imageCollection: tempEscala,
  regions: puntos,
  reducer: ee.Reducer.mean(),
  scale: 1000,
  seriesProperty: 'sitio'
});

// Configurar gráfica
tempChart.setOptions({
  title: 'Temperatura diaria sobre dos sitios en 2022',
  vAxis: {
    title: 'Temperatura (Celsius)',
    viewWindow: {min: 15, max: 45}
  },
  lineWidth: 1,
  pointSize: 4,
  interpolateNulls: true,
  series: {
    0: {color: 'red'},
    1: {color: 'green'},
  }
});

// Imprimir gráfico en consola
print(tempChart);
```

Esta gráfica debe ser imprimida para poder visualizarla. 

<img align="center" src="../../images/gee-avanzado/04_fig1.png" vspace="10" width="500">

La flecha en la esquina superior derecha nos permite abrir la gráfica en una ventana independiente y habilitará opciones para descargar los datos en formato CSV, SVG, y PNG.

<img align="center" src="../../images/gee-avanzado/04_fig2.png" vspace="10" width="500">
