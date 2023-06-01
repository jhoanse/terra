---
layout: page
title: Mapeo de Inundaciones
parent: Google Earth Engine Avanzado - Parte 2
nav_order: 1
---

## Script
El script completo que se usará en esta sección esta disponible [aquí]().

# Mapeo de Inundaciones

Mapear areas inundadas puede ser logrado con imágenes multiespectrales, sin presencia de nubes y si los cuerpos de agua son directamente observados (no cubiertos por vegetación). Estas son limitaciones para las imágenes espectrales, pero no para las imágenes de radar de apertura sintética (SAR), ya que esta señal de radar es capaz de ir y volver a través de nubes y vegetación no tan densa para detectar cuerpos de agua o zonas inundadas. En GEE se encuentra la colección de [Sentinel-1](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S1_GRD) que desde 2014 provee datos a una frecuencia de 5.4 GHz (C-Band), a tres resoluciones de 10, 25, o 40 m por pixel, diferentes polarizaciones (HH, VV, VH, HV), y entrega una imagen cada 16 días, aproximadamente.

<img align="center" src="../../images/gee-avanzado/03_fig1.png" vspace="10" width="800">

```javascript

```
