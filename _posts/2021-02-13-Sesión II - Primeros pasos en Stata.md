---
title: Sesión 2. Primeros pasos con Stata
date: 2021-02-13
categories: [Evolución y Teoría del Estado]
tags: [stata]     # TAG names should always be lowercase
mermaid: true
image: /assets/img/posts/sesion1/stata.png
---

Bienvenidos un día más. En la sesión anterior vimos cómo funciona Stata desde la base. Cómo se relaciona con el sistema operativo y cómo cargar una base de datos desde el directorio de trabajo. En esta sesión vamos a empezar a trabajar los datos que vimos la semana pasada, los datos de [Quality of Government](https://www.gu.se/en/quality-government/qog-data).

Cuando se trabaja con datos, el tratamiento de estos suele seguir un patrón claro:
1. Primero recopilamos los datos,
2. Segundo, limpiamos los datos,
3. Finalmente, interpretamos los datos

Lo primero que querríamos hacer para trabajar de forma cómoda, por tanto, es filtrar la base de datos para quedarnos unicamente con las variables en que estamos interesados. Obviamente, **esto es opcional**, si el ordenador con el que trabajamos tiene memoria suficiente (este suele ser el caso hoy en día), podemos trabajar con toda la base de datos original. En caso de que decidamos filtrar, nuestro objetivo será crear una base de datos **nueva**. Será parcial, es decir, será una parte de la original. Y como será una nueva base de datos, tendremos que guardar un nuevo archivo con extensión .dta. Stata contiene dos comandos que permiten filtrar las bases de datos: `keep` y `drop`.

## Limpiando los datos

Como su propio nombre indica, `keep`[¹] se queda con las variables que le indiquemos y deshecha las demás. Por ejemplo, si sólo quisieramos trabajar con var1 y var2, el comando sería el siguiente:


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

En ambos casos utilizamos el comando `save` para guardar la nueva base de datos. Si no se especifica ninguna extensión, Stata guarda por defecto el archivo en formato .dta, es decir: `mi_nueva_base_de_datos.dta`.

Probablemente te estarás preguntando si no hay una manera más fina de filtrar. En efecto, la hay y es lo que se suele hacer. Esto consiste en filtrar partes de las variables (aunque podríais querer quitar variables enteras). Lo más habitual es que, por ejemplo, sólo quieras trabajar con ciertos valores de una variable. Por ejemplo, sólo con los datos para una región del mundo. Esto también se puede hacer con los comandos `keep` y `drop`. Para ello, simplemente añadimos una condicion con la palabra clave **if**:

<pre class="sh_Stata">

// Esto mantiene todas las variables pero sólo mantiene las observaciones que cumplen la condición.
// Es decir, sólo si los valores de var1 son value1 esas observaciones sobreviven, si no, se eliminan.

keep if var1 == value1 //Fíjate en que cuando comparamos (preguntamos), el operador son dos iguales (==).
</pre>

## Interpretando los datos

Bien, ahora que tenemos sólo las variables y observaciones que queremos, veamos los dos comandos que probablmente más veces se usan durante una sesión de Stata. Estos son los que nos permiten hacer **tablas de frecuencias**. Estas tablas muestran (si no se pasa ninguna opción al comando) cuántas veces aparece cada valor de una variable en nuestra base de datos.

### ¿Qué es una variable y qué tipos existen?

Antes de pedir a Stata nuestra primera tabla, recordemos brevemente qué es una variable y qué tipos de variables existen. En la [sesión anterior](https://rodrigofcaba.github.io/posts/Introducci%C3%B3n-a-Stata/) vimos qué tipos de datos y cómo los clasifica por colores Stata. Pues bien, una variable es:

>“cualquier cualidad o característica de un objeto (o evento) que contenga, al menos, dos atributos (categorías o valores), en los que pueda clasificarse un objeto o evento determinado” (D’Ancona 1996: 126). 

En general podemos distinguir dos tipos de variables:

- Las que toman **VALORES** numéricos (edad, altura, nivel de ingresos).
- Las que adoptan **CATEGORÍAS** (sexo, estado civil, voto).

> La **medición** de la variable consiste en el proceso de **asignar valores o categorías** a las distintas características que conforman el objeto de estudio (Pinta, 2020).

### Clasificación de variables

::: mermaid
graph TD
A[Variables] --> B{Según su nivel de medición};
A --> C{Según la escala de medición};
A --> D{Según su función en la investigación};
B --> E[Categóricas]
E --> I[Nominales]
E --> J[Ordinales]
B --> F[Numéricas]
F --> K[De intervalo]
F --> L[De razón o proporción]
C --> G[Continuas]
C --> H[Discretas]
D --> M[Independientes, explicativas o predictoras]
D --> N[Dependientes o explicadas]
D --> O[De control]
:::

Sería muy prolijo entrar aquí en detalles de cada una de ellas, las comentaremos en clase y a lo largo de los ejemplos que veamos en Stata.

Hagamos una tabla con, por ejemplo, las frecuencias de la variable *gol_est*, que mide el tipo de sistema electoral del Estado. Esta variable es categórica y toma tres valores: mayoritario, proporcional o una mezcla.

<pre class="sh_Stata">

<ins>tab</ins>ulate gol_est //el comando tabulate (y cualquera) puede acortarse, por eso he subrayado hasta dónde se podría acortar.

tab gol_est //Hace lo mismo.
</pre>

El resultado de este comando sería el siguiente:

Como véis, aparece la frecuencia de cada uno de los valores, es decir, cuántas veces la variable toma cada valor. Por ejemplo, de los 192 países en la muestra, X tienen un sistema presidencialista. Al lado de la frecuencia obtenemos la frecuencia relativa (en porcentaje) y la frecuencia acumulada.

En la tabla aparece el nombre de la variable que es bastante incomprensible. Una buena práctica cuando estamos limpiando los datos consiste en **etiquetar**. En Stata se pueden etiquetar tanto variables como valores (categorías) de las variables. Para ello usamos el comando `label`



Sin embargo, quizás para vuestro estudio no os interese, pongamos por caso, tener la categoría *mixed*. Quizás queréis comparar sólo aquellos países presidencialistas con aquellos con sistemas parlamentarios. Para eso, hay que recodificar la variable. Es decir, cambiar cómo están codificados sus valores. Para eso usamos el comando `recode`. Tambien usaremos para esto `replace`, veremos la diferencia más adelante.

<pre class="sh_Stata">
// Recodifica la variable gol_est mandando la categoría mixed a perdidos:

recode gol_est 3 = . 
</pre>
