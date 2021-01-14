---
title: Sesión 1. Introducción a Stata
date: 2021-01-14
categories: [Evolución y Teoría del Estado]
tags: [Stata]     # TAG names should always be lowercase
mermaid: true
image: /assets/img/posts/sesion1/stata.png
---

¡Vamos allá!

En esta primera sesión vamos a ver una introducción a Stata, ¿qué es?, ¿cómo se usa?, etc.

# ¿Qué es Stata?

**Stata** es un programa estadístico multiplataforma (es decir, que puede usarse en distintos sistemas operativos: windows, linux, macOS...) creado en 1985. Esto quiere decir que es más moderno que, por ejemplo SAS, SPSS y otros programas que quizás conozcáis. Es intuitivo, rápido y fácil de usar, aunque para operaciones más avanzadas (como el Big Data) puede llegar a ser algo limitado. Además, **no es gratuito**, frente a otras opciones como R o Python, este es, sin lugar a dudas, su mayor punto débil.

Para vosotros esto de momento no es un problema, la Universidad os facilita ordenadores con Stata instalado y listo para usarse. Sin embargo, aquellos que queráis seguir trabajando con datos en el futuro quizás pensáis en adquirir una licencia.

Sin embargo, tiene varios puntos a su favor, es **fácil de usar** y desde la versión 11 tiene una GUI (*Grafical User Interface*) que puede ser de utilidad. No obstante, ya os adelanto que en este curso aprenderemos a usar Stata escribiendo código (o más bien, comandos) y no usando la interfaz gráfica de ventanas.
Pero incluso utilizando comandos, Stata es más sencillo de usar que otras opciones ya que no es un lenguaje de programación como tal. Con unos cuantos comandos puedes hacer muchas cosas y no tienes que preocuparte de saber qué es un vector, o qué es una función. A nivel básico Stata es mucho más accesible que los lenguajes de programación que mencionaba.

# ¿Para qué sirve Stata?

Stata es muy completo, se pueden hacer desde los cálculos más sencillos como sumar y restar:

<pre class="sh_Stata">
// Suma con Stata:
 display 2+4
</pre>

<pre class="sh_Stata">
// Salida:
6
</pre>

hasta grandes cálculos como modelos de regresión líneal o gran cantidad de gráficos:

![test](https://static.wixstatic.com/media/16eebe_6ee5ae49bf134080aa48d5a95e3fef98~mv2.png/v1/fill/w_856,h_623,al_c,q_90/16eebe_6ee5ae49bf134080aa48d5a95e3fef98~mv2.webp){: width:400 }

Es muy usado en campos como la medicina y las ciencias sociales. En nuestro caso, es ideal para trabajar con bases de datos con registros a nivel individual, de países, etc. Con Stata podemos hacer prácticamente cualquier análisis estadístico que se nos ocurra.

# Estructura de la interfaz gráfica

Pero vamos a lo que de verdad os interesa. ¡Abramos Stata!

>Todas las capturas y explicaciones usarán la versión de **Windows** dado que será la que usemos en clase. Aunque veréis la versión 16 y en clase probablemente usemos la 14, pero no hay prácticamente diferencias.

Lo primero que se puede decir de la ventana principal es que tiene en la parte superior (como cualquier programa) la barra de herramientas. En esta barra hay algunos botones que usaremos muy a menudo: el editor de *do-files* y el editor o visor de datos. Por lo demás, nada interesante porque, como decía más arriba, vamos a aprender a usar comandos y no ventanas.

![toolbar](/assets/img/posts/sesion1/stata1.png)

El programa se divide en distintas secciones y tiene distintas posibilidades en cuanto a la organización de estas secciones. Por defecto viene configurado de manera que aparecen las cuatro principales:

- La ventana de **revisión**
- La ventana de **resultados**
- La ventana de **comandos**
- Y la ventana de **variables**

>La apariencia de estas ventanas puede cambiarse en `preferencias/opciones gráficas`

## La ventana de Revisión

Es una sección muy útil pues nos permite ver cada comando que se ha ejecutado por parte de Stata. Así podemos volver a ejecutar algo anterior o ver si tenemos algún error (aparecerá en rojo).

## La ventana de Resultados

Es la que miraremos constantemente para ver las "salidas" del programa, es decir, los resultados de las operaciones que le pidamos a través de los distintos comandos.

## La ventana de Comandos

Como su propio nombre indica es la ventana en la que escribiremos los comandos que queramos que Stata ejecute. Sin embargo, veremos que una práctica más recomendable es usar para esto el editor de **Do-files** (que veremos enseguida).

## La ventana de variables

Y finalmente, la ventana en la que aparecen las variables que contiene nuestra base de datos. Hasta ahora no tenemos ninguna base de datos cargada, luego no tenemos ninguna variable en esta ventana. Así que, carguemos alguna.

![qog](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Quality_of_Government_Institute_logo.png/245px-Quality_of_Government_Institute_logo.png)

Lo más interesante para que podamos empezar cuanto antes a familiarizarnos con la base de datos de Quality of Goverment es que la descarguemos de [aquí](https://www.gu.se/en/quality-government/qog-data/data-downloads/standard-dataset).

Lo primero que debes saber es en qué formato descargarla, para usarla con Stata, debe ser el archivo con extensión **.dta**. A partir de ahora ya sabes cuál es la extensión de los archivos que contienen datos para usar directamente en Stata. No obstante, Stata puede leer también archivos CSV y en formato xls (Excel), como veremos más adelante.

Antes de cargar la base de datos en Stata vamos a abrir el editor de Do-files.

# ¿Qué es un Do-file?

¿Te imaginas comenzar a pedirle a Stata que haga complejísimos cálculos a través de la ventana de comandos, por ejemplo: que recodifique una variable, que calcule la media, la desviación típica y que luego haga una tabla cruzando los valores con los de otra variable y que cuando termines todas esas instrucciones sólo sirvan una vez? Sería muy tedioso tener que volver a escribir lo mismo cada vez que quieras que Stata haga algo que vas a querer volver a pedirle. Para eso existen los Do-files.

Básicamente un **do-file** es un archivo con extensión **.do** que sirve para escribir comandos con la sintaxis de Stata y guardarlo para poder ejecutarlo completo o por partes en cualquier momento.

Es por este motivo por el que usaremos el editor de do-files y no la ventana de comandos. Así, todos los comandos que escribas en clase quedarán guardados para volver a ejecuarlos cuando quieras.

Lo primero que suele escribirse en un do-file (por ser ordenados, simplemente) es una serie de datos sobre quién lo ha escrito y qué contiene el archivo, por ejemplo:

<pre class="sh_Stata">
******************************************
** Nombre: Sesión 1. Introducción a Stata
** Asignatura: Evolución y Teoría del Estado
** Autor: Rodrigo Fernández
******************************************

// Código a partir de aquí
</pre>

Es totalmente opcional y de hecho, esto no incluye ningún comando que vaya a ejecutar Stata aún. Cuando incluímos un asterisco delante del texto Stata entiende que lo que viene a continuación es un **comentario** y no un comando. Es decir, que se lo va a "saltar" y no va a ejecutar nada.

Hay varias formas de hacer comentarios en un do-file:

<pre class="sh_Stata">
** Esto es un comentario de una línea
// Esto también es un comentario de una línea

/*
Esto es un comentario que
puede ocupar varias líneas
*/
</pre>

Y a partir de aquí empezamos con los primeros comandos:

Para trabajar con Stata primero hay que decirle al programa cuál será nuestro directorio de trabajo. Por defecto siempre se coloca en `C:\users\documents`.
Para cambiarlo usamos el siguiente comando:

<pre class="sh_Stata">
cd "C:/users/tu_usuario/documents/Stata/sesion1" //Para windows
cd "Users/tu_usuario/Documents/Stata/sesion1" // Para sistemas UNIX
</pre>

Una vez situados donde queremos trabajar (hay que crear antes el directorio si no existe) podemos cargar la base de datos que la descompimiremos en esta ruta.

<pre class="sh_Stata">
// Para cargar una base de datos en formato .dta:

use nombre_de_la_bd
</pre>

Ahora, para ejecutar el do-file podemos pinchar en el siguiente botón (run):

![Stata2](/assets/img/posts/sesion1/stata2.png)

o, mejor aún, utilizar el atajo de teclado `ctrl+D` en windows o `cmd+D` en mac.

> **ATENCIÓN:** Esto ejecuta todas las líneas del do-file. Ahora no nos importa pero cuando vayamos escribiendo más código no querremos que Stata ejecute todos los comandos. Para evitarlo, selecciona con el ratón sólo lo que quieras que se ejecute y pulsa `ctrl+D`

Si te fijas, en la ventana de variables han aparecido las variables de QoG y en la ventana de resultados Stata muestra los comandos que ha ejecutado desde el do-file.

¡Ya podemos empezar a trabajar!

Pero este post se está haciendo más largo de lo necesario, para terminar con la introducción vamos a ejecutar nuestro primer comando, que sirve para ver en una tabla (como una hoja de cálculo de excel) los datos que Stata ha cargado en memoria:

<pre class="sh_Stata">
// Abre la ventana del visor de datos
browse
</pre>

En el siguiente post veremos qué información nos da esta ventana porque lo primero es entender **qué tipo de datos existen y cómo los interpreta Stata**.
