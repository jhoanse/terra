---
layout: page
title: API Earth Engine
parent: Introducción a Google Earth Engine - Parte 1
nav_order: 4
---
## Script
El script completo que se usará en esta sección esta disponible [aquí](https://code.earthengine.google.com/5c0c1eaa9d927e9e20ce2d66119d4b84).

# API de Earth Engine (Server Side)

La API de Earth Engine es amplia y proporciona objetos y métodos para hacer de todo, desde operaciones matemáticas sencillas hasta algoritmos avanzados para el procesamiento de imágenes. En el Editor de código, puede cambiar a la pestaña `Docs` para ver las funciones de la API agrupadas por tipos de objetos (o en la [documentación de Earth Engine](https://developers.google.com/earth-engine/apidocs)). Las funciones de la API tienen el prefijo `ee` (por Earth Engine).

<img align="center" src="../../images/intro-gee/fig15.png" vspace="10" width="300"> 

Los conceptos fundamentales de Earth Engine con respecto a la teledetección incluyen:

- Imagen (`ee.Image`): El tipo de dato raster fundamental en Earth Engine. Imagen con una pila de bandas georreferenciadas. Cada banda tiene la suya Máscara, Proyección, Resolución, y una lista de propiedades que inclyen fecha, cuadro delimitador, etc.

    <img align="center" src="../../images/intro-gee/fig8.png" vspace="10" width="150">

- Colección de Imágenes (`ee.ImageCollection`): Una colección de imágenes

    <img align="center" src="../../images/intro-gee/fig9.png" vspace="10" width="150">

- Geometría (`ee.Geometry`): El tipo de dato vectorial fundamental en Earth Engine. Línea / Punto / Polígono / etc.

    <img align="center" src="../../images/intro-gee/fig10.png" vspace="10" width="150">

- Feature (`ee.Feture`): Una geometria con propriedades. Línea / Punto / Polígono / etc,
Lista de Propiedades

    <img align="center" src="../../images/intro-gee/fig11.png" vspace="10" width="150">

- Colección de Features (`ee.FeatureCollection`): Una colección de Features (geometrias con propriedades)

    <img align="center" src="../../images/intro-gee/fig12.png" vspace="10" width="150">

- Reductores (`ee.Reducer`): Objeto utilizado para agregaciones y cálculos numéricos (para bandas, séries temporales, features...)

    <img align="center" src="../../images/intro-gee/fig13.png" vspace="10" width="150">
    <img align="center" src="../../images/intro-gee/fig14.png" vspace="10" width="150">

Para más informaciones acceder al [sitio EE de objetos y métodos](https://developers.google.com/earth-engine/guides/objects_methods_overview).



Sin embargo es muy recomendable aprender a usar la API. Suponga que desea sumar dos números, representados por las variables a y b , como se muestra a continuación. Cree un nuevo script e ingrese lo siguiente:

```javascript
var a = 1;
var b = 2;
```

Anteriormente, aprendiste cómo almacenar números en variables, pero no cómo hacer ningún cálculo. Esto se debe a que cuando utiliza Earth Engine, no realiza sumas mediante operadores de JavaScript. Por ejemplo, no escribirías `var c = a + b` para sumar los dos números. En su lugar, la API de Earth Engine le brinda funciones para hacer esto, y es importante que use las funciones de la API siempre que pueda. Puede parecer incómodo al principio, pero usar las funciones, como lo describiremos a continuación, lo ayudará a evitar tiempos de espera y crear código eficiente.

Mirando la pestaña `Docs`, encontrará un grupo de métodos que se pueden llamar en un `ee.Number`. Expanda para ver las diversas funciones disponibles para trabajar con números. Verá la función `ee.Number` que crea un objeto de número de Earth Engine a partir de un valor. En la lista de funciones, hay una función de suma (`add`) para sumar dos números. Eso es lo que usas para sumar `a` y `b`.

<img align="center" src="../../images/intro-gee/fig24.png" vspace="10" width="300"> 

Para sumar `a` y `b`, primero creamos un objeto `ee.Number` a partir de la variable a con `ee.Number(a)`. Y luego podemos usar la llamada `add(b)` para agregarle el valor de `b`. El siguiente código muestra la sintaxis e imprime el resultado que, por supuesto, es el valor 3.

```javascript
var resultado = ee.Number(a).add(b);
print(resultado);
```

Es posible que ya te hayas dado cuenta de que cuando aprendes a programar en Earth Engine, no necesitas aprender JavaScript o Python en profundidad, sino que son formas de acceder a la API de Earth Engine. Esta API es la misma ya sea que se llame desde JavaScript o Python.

Aquí hay otro ejemplo para llevar este punto a casa. Supongamos que está trabajando en una tarea que requiere que cree una lista de años desde 1980 hasta 2020 con un intervalo de cinco años. Si se enfrenta a esta tarea, el primer paso es cambiar a la pestaña `Docs` y abrir el módulo `ee.List`. Navegue a través de las funciones y vea si hay alguna función que pueda ayudar. Notará una función `ee.List.sequence`. Al hacer clic en él, aparecerá la documentación de la función.

<img align="center" src="../../images/intro-gee/fig25.png" vspace="10" width="500"> 

La función `ee.List.sequence` puede generar una secuencia de números desde un valor inicial dado hasta el valor final. También tiene un paso de parámetro opcional para indicar el incremento entre cada número. Podemos crear una `ee.List` de números que representen los años desde 1980 hasta 2020, contando de 5 en 5, llamando a esta función predefinida con los siguientes valores: `start` = 1980, `end` = 2020 y `step` = 5.

```javascript
var listaAnos = ee.List.sequence(1980, 2020, 5);
print(listaAnos);
```

El resultado impreso en el `Console` mostrará que la variable yearList contiene la lista de años con el intervalo correcto.

<img align="center" src="../../images/intro-gee/fig26.png" vspace="10" width="500"> 

Ahora vamos mirar un ejemplo de redución (`ee.Reducer`). Como vimos, `ee.Reducer` es el objeto utilizado para agregaciones y cálculos. Creamos una lista con números de 1 a 5 y queremos calcular la média destos números. Para eso, utilizamos la función `reduce()` para listas y elegimos el `ee.Reducer` (mire en el `Docs` el redutor `ee.Reducer.mean()`). Tenga en cuenta que podemos utilizar la misma función `ee.List.sequence` para crear la lista pero sin la necesidad de definir el `step` ya que `step` tiene el valor 1 como padrón.

```javascript
var listaNumeros = ee.List.sequence(1, 5);

var media = listaNumeros.reduce(ee.Reducer.mean());
print(media);
```

Acaba de realizar una tarea de programación moderadamente compleja con la ayuda de Earth Engine API.

### Desafío 1

Suponga que tiene las siguientes dos variables de cadena definidas en el código a continuación. Usa la API de Earth Engine para crear una nueva variable de cadena llamada `resultado` combinando estes dos `Strings`. Imprímelo en el `Console`. El valor impreso debe decir "Sentinel2A".

```javascript
var mision = ee.String('Sentinel');
var satelite = ee.String('2A');
```

*Sugerencia*: use la función `cat` del módulo `ee.String` para "concatenar" (unir) los dos `strings`. Encontrará más información sobre todas las funciones disponibles en la pestaña `Docs` del Editor de códigos.

<img align="center" src="../../images/intro-gee/fig27.png" vspace="10" width="400"> 

### Desafío 2

Crear un diccionario llamado `misInformaciones` utilizando el objeto `ee.Dictionary` con sus informaciones personales: `nombre`, `edad`, y una lista de sus 3 películas preferidas `peliculas`. Imprímelo en el `Console`.

### Desafío 3

Obtener el valor guardado en el `edad` y guardarlo en una variable con un nuevo nombre (ejemplo: `miEdad`). Imprímelo en el `Console`.

*Sugerencia*: utilice la función `get()` del objeto `ee.Dictionary`.

Observe que el valor impreso es del tipo `Computed Object`. Eso significa que no es un objeto EE aún. Si tuviera que realizar una operación matemática con este número, necesitaría "lanzarlo" (cast) al objeto `ee.Number`.

### Desafío 4

Obtener el segundo valor de la lista abajo y guardarlo en una variable.
Dividir ese número por 2. Imprimir el resultado en el `Console`.

*Sugerencia*: busque informaciones para la función `get()` de `ee.List`. Tenga en cuenta que la indexación comienza en 0.

```javascript
var miLista = ee.List([1, 2, 5, 4]);
```

### Desafío 5

Multiplicar el segundo valor de `miLista` por 5 y guardarlo en una variable (ejemplo: `resultado`). Imprímelo en el `Console`.

*Sugerencia*: buscar por la función `multiply()` de `ee.Number`. Es necesario convertir (cast) el segundo valor de `miLista` a un `ee.Number` primero, antes de que se puedan usar las funciones numéricas. Tenga en cuenta que nos es necesário convertir (cast) el número 5 adentro de `multiply` - Earth Engine ya converte ese número para `ee.Number` una vez que estamos utilizando la función del API.

### Desafío 6

Multiplica cada número en `miLista` por 3. Guarda el resultado en una nueva variable llamada `nuevaLista` e imprímelo.

*Sugerencia*: complete la función `triplicar` a continuación. Use `map()` para aplicar la función a cada elemento de la lista. No te olvides del casting.

```javascript
function triplicar(numero) {
  return ; // completar esa función.
}
```

### Desafío 7

Calcula la suma de todos los Números en `nuevaLista`. Guardar y imprimir ese valor como `suma`.

### Código completo

Script "`3 API Earth Engine`" del repositorio y la carpeta `T2` o link directo:
[https://code.earthengine.google.com/f68358987ee1233c62f459e6ca9d0244](https://code.earthengine.google.com/f68358987ee1233c62f459e6ca9d0244).
