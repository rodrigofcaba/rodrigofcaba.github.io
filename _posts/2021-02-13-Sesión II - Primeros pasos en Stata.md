---
title: Sesión 2. Primeros pasos con Stata
date: 2021-02-13
categories: [Evolución y Teoría del Estado]
tags: [stata]     # TAG names should always be lowercase
mermaid: true
image: /assets/img/posts/sesion1/stata.png
---

Bienvenidos un día más. En la sesión anterior vimos cómo funciona Stata desde la base. Cómo se relaciona con el sistema operativo y cómo cargar una base de datos desde el directorio de trabajo. En esta sesión vamos a empezar a trabajar los datos que vimos la semana pasada, los datos de Quality of Government.

Para trabajar con datos, el tratamiento de estos suelen seguir un patrón claro:
1. Primero recopilamos los datos,
2. Segundo, limpiamos los datos,
3. Finalmente, interpretamos los datos
   
Lo primero que querríamos hacer para trabajar de forma cómoda, por tanto, es filtrar la base de datos para quedarnos unicamente con las variables en que estamos interesados. Obviamente, **esto es opcional**, si el ordenador con el que trabajamos tiene memoria suficiente (este suele ser el caso hoy en día), podemos trabajar con toda la base de datos original. En caso de que decidamos filtrar, nuestro objetivo será crear una base de datos **nueva**. Será parcial, es decir, será una parte de la original. Y como será una nueva base de datos, tendremos que guardar un nuevo archivo con extensión .dta. Stata contiene dos comandos que permiten filtrar las bases de datos: `keep` y `drop`.

Como su propio nombre indica, `keep`[¹] se queda con las variables que le indiquemos y deshecha las demás. Por ejemplo, si sólo quisieramos trabajar con var1 y var2, el comando sería el siguiente:

[¹]: Más información: [keep](https://www.stata.com/manuals/dsave.pdf)

<pre class="sh_Stata">
// La nueva base de datos sólo contendrá las variables var1 y var2:

keep var1 var2

// Si queremos guardar la nueva base de datos, utilizamos el comando save:

save mi_nueva_base_de_datos, replace //La opción replace hace que, en caso de existir, el archivo se sobreescriba.
// También fíjate en que el nombre del archivo no tiene espacios. Si los tiene, necesitas encerrarlo entre comillas. Ej:

save "mi nueva base de datos"
</pre>

La otra opción es eliminar aquellas variables con las que no queremos trabajar. Este es un proceso más largo en general, pues necesitamos primero saber todas las variables que existen en nuestra base de datos. Pero se puede hacer. Imagina que quiero quitar una variable en concreto, var3, la sintaxis sería la siguiente:

<pre class="sh_Stata">
// La nueva base de datos NO contendrá la siguiente variable:

drop var3

save "mi nueva base de datos"

</pre>

Probablemente te estarás preguntando si no hay una manera más fina de filtrar. En efecto, la hay y es lo que se suele hacer. Esto consiste en filtrar partes de las variables (aunque podríais querer quitar variables enteras). Lo más habitual es que, por ejemplo, sólo quieras trabajar con ciertos valores de una variable. Por ejemplo, sólo con los datos para una región del mundo. Esto también se puede hacer con los comandos `keep` y `drop`. Para ello, simplemente añadimos una condicion con la palabra clave **if**:

<pre class="sh_Stata">

// Esto mantiene todas las variables pero sólo mantiene las observaciones que cumplen la condición.
// Es decir, sólo si los valores de var1 son value1 esas observaciones sobreviven, si no, se eliminan.

keep if var1 == value1 //Fíjate en que cuando comparamos (preguntamos), el operador son dos iguales (==).
</pre>

Bien, ahora que tenemos sólo las variables y observaciones que queremos, veamos los dos comandos que probablmente más veces se usan durante una sesión de Stata. Estos son los que nos permiten hacer **tablas de frecuencias**. Estas tablas muestran (si no se pasa ninguna opción al comando) cuántas veces aparece cada valor de una variable en nuestra base de datos.

Hagamos una tabla con, por ejemplo, las frecuencias de la variable *gol_est*, que mide el tipo de sistema electoral del Estado. Esta variable es categórica y toma tres valores: mayoritario, proporcional o una mezcla.

<pre class="sh_Stata">

<ins>tab</ins>ulate gol_est //el comando tabulate puede acortarse, por eso he subrayado hasta dónde se podría acortar.
</pre>

El resultado de este comando sería el siguiente:

