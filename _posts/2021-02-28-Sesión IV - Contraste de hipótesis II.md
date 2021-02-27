---
title: Sesión 4. Contraste de hipótesis II
date: 2021-02-28
categories: [Evolución y Teoría del Estado]
tags: [stata]     # TAG names should always be lowercase
mermaid: true
math: true
image: /assets/img/posts/sesión3/popper.jpg
---

Bienvenido. La semana pasada nos adentramos en el mundo del contraste de hipótesis, quizás con uno de los test estadísticos más intuitivos que existen, la prueba T (T-Test). Hoy continuaremos aprendiendo nuevos contrastes de hipótesis. Esta vez nos centraremos en las variables categóricas. Estas variables tienen un tratamiento algo especial cuando hablamos de contraste de hipótesis. Recuerda la tabla con la que terminábamos el post anterior, cuando nuestras variables son categóricas o categóricas y dummies (dicotómicas), usamos tablas de contingencia para llevar a cabo el contraste. Pero entonces ¿es suficiente con estas tablas? Simplificando la respuesta: no. La tabla nos da una idea de cómo los valores de dos variables se relacionan pero, como ocurría con el contraste de la semana anterior, no es suficiente con ver los datos, necesitamos aplicar alguna prueba estadística.

## El contraste de hipótesis con variables categóricas

El objetivo de este contraste será determinar si dos variables se encuentran relacionadas mediante alguna **medida de asociación**, preferiblemente acompañada de su correspondiente **prueba de significación**. Es decir, primero comprobamos si se observa alguna asociación y luego tratamos de ver si ésta es estadísticamente significativa. Pero, para entender todo el proceso, es necesario hablar -siquiera brevemente- de las frecuencias que aparecen en las tablas que le pedimos a Stata con el comando `tab`.

### Tablas de frecuencias y tablas de contingencia

Lo primero es distinguir entre tablas de frecuencia y tablas de contingencia. Como ya hemos mencionado previamente en este curso podemos pedir a Stata una tabla con la frecuencia con la que se repiten los valores de **una** variable (**tabla de frecuencias**) y también podemos pedirle una tabla en la que se **cruzan** las frecuencias de **dos** variables (**tabla de contingencia**) ambas se obtienen con el comando tab:

<pre class="Stata_sh">

tab gol_est // tabla de frecuencias
</pre>

![freq](/assets/img/posts/sesión4/freq.png)


<pre class="Stata_sh">

tab ht_region gol_est  // tabla de contingencia
</pre>

![conti](/assets/img/posts/sesión4/conti.png)

En la sesión anterior utilizábamos una única variable para comparar la media de dos grupos con la T-Test, a partir de ahora nos ceñiremos a las **tablas de contingencia**. Toda tabla de contingencia contiene **frecuencias**, **marginales** y, opcionalmente, **porcentajes**. Una pregunta pertinente antes de nada es saber que variable colocar en las filas y cuál en las columnas. No es en ningún caso obligatorio pero sí que se suele usar la convención de colocar en las columnas la variable independiente mientras que en las filas se coloca la variable dependiente. Así, en el siguiente ejemplo vemos el efecto de la región a la que pertenece el país (en columnas) sobre el tipo de sistema electoral del mismo (en filas):

<pre class="Stata_sh">

tab gol_est ht_region
</pre>

![tabla](/assets/img/posts/sesión4/tabla.png)

Para ver un poco más claras las partes de las que hablábamos a continuación vemos las filas y las columnas transpuestas:

![partes](/assets/img/posts/sesión4/marginales.png)

Para obtener los porcentajes se añade al comando `tab` una opción. Es posible pedir porcentajes "de fila" o "de columna". Esto es relevante porque la interpretación de los porcentajes es distinta en cada caso. Si seguimos la convención de colocar en columnas la variable independiente, obtener los porcentajes por columna nos permite interpretar el efecto que esta variable explicativa tiene sobre la independiente (será lo más común).

<pre class="Stata_sh">

tab gol_est ht_region, <ins>col</ins>umn
</pre>

![cols](/assets/img/posts/sesión4/cols.png)

Fíjate que debajo de todos los marginales de columna ahora hay un 100 que indica que los porcentajes de cada columna suman esta cantidad. Por tanto, la interpretación sería:

> De los países de la región "Latinoamérica" (segunda columna), un 78.95% tienen sistemas proporcionales, mientras que un 21.05% tienen sistemas mixtos.

La opción para obtener los porcentajes "de fila" es `row`. Y una manera de limpiar un poco la tabla para leer sólo porcentajes es indicarle a Stata que además de mostrar los porcentajes, elimine (no muestre) las frecuencias:

<pre class="Stata_sh">

tab gol_est ht_region, col nofreq
</pre>

![perc](/assets/img/posts/sesión4/perc.png)

### ¿Cómo calcula Stata los porcentajes?

Si llamamos $f_{ij}$ a la frecuencia de la fila i y la columna j y $f_i$ al marginal de la fila i, el porcentaje de la fila i y la columna j se calcula como el cociente entre las frecuencias y marginales de cada fila multiplicado por 100, es decir:

$$ p_{ij}^H = \frac{f_{ij}}{f_i}\cdot100 $$

Para el caso de los porcentajes de columna, se usan los marginales de columna:

$$ p_{ij}^V = \frac{f_{ij}}{f_j}\cdot100 $$

También existe la posibilidad de pedirle a Stata los **porcentajes de "celda"** pero su interpretación es más complicada y no haremos hincapié aquí.

Se calculan como sigue:

$$ p_{ij}^T = \frac{f_{ij}}{f}\cdot100 $$

Y se piden en Stata así:

<pre class="Stata_sh">

tab gol_est ht_region, cell
</pre>

Finalmente, ¿cómo deben interpretarse (leerse) las tablas de contingencia con porcentajes? Siguiendo la regla de Zeisel:

> Los porcentajes han de calcularse en la dirección de la variable independiente e interpretarse en la de la dependiente. Es decir, se calcularán porcentajes sobre la columna que habrán de ser interpretados por filas.

Para leer una tabla, por tanto, aplicaremos la Regla de Zeisel. Los porcentajes estarán calculados en columnas (variable independiente) y se compararán/interpretarán según filas (variable dependiente).

Esta comparación entre los porcentajes de columna y marginales sirve como primera aproximación al concepto de **independencia** de las tablas de
contingencia Como veremos, parte de los estadísticos de significación y asociación estadística se basan en la comparación entre las **frecuencias observadas** de las casillas y las **frecuencias esperadas**.

Pues bien, estas frecuencias que aparecen por defecto con el comando `tab` se denominan **frecuencias observadas**, pues son las que se observan en la muestra con la que estamos trabajando, es decir, en los datos que estamos manejando. Sin embargo, podemos pedirle a Stata que nos muestre lo que se denominan **frecuencias esperadas**, es decir, las frecuencias teóricas que esperaríamos en el hipotético caso de que las variables que hemos enfrentado en la tabla de contingencia **no** **tuvieran** ninguna **relación** entre sí.

## Medidas de asociación y significación

Para calcular las **frecuencias esperadas** $f_{ij}^*$ Stata utiliza la siguiente fórmula:

$$ f_{ij}^* = \frac{f_if_j}{f_{..}} $$

donde $f_i$ es el total de fila, $f_j$ el total de columna y $f_{..}$ el número total de casos. Veamos un ejemplo:

Sea la siguiente tabla de contingencia:

![teor](/assets/img/posts/sesión4/tab%20teor.png)

Para calcular la tabla teórica, es decir, las **frecuencias esperadas**, usamos las fórmulas de arriba:

La frecuencia esperada en la fila 1, columna 1 ($f_{11}$), es decir, el cantidad esperada de países de américa Latina y el Caribe con sistema electoral mayoritario (si no hubiera ningún efecto de la región geográfica sobre el tipo de sistema electoral) sería:

$$ f_{11} = \frac{f_1\cdot f_1}{f_{..}} = \frac{16\cdot27}{64} = 6.75 $$

Y así sucesivamente. Finalmente obtendríamos la **tabla teórica** que -por supuesto- no vamos a calcular a mano, y en Stata puede obtenerse de la siguiente forma:

<pre class="Stata_sh">

tab sis_elec regiones, expected //Esto muestra tanto las observadas como las esperadas
</pre>

![teoremp](/assets/img/posts/sesión4/tab%20teoremp.png)

Así, podemos comparar los valores observados con los esperados. Pues bien, el hecho de que la tabla observada no sea idéntica a la teórica nos indica que estas variables tienen algún tipo de asociación, es decir, que la variable independiente tiene cierto efecto sobre la variable dependiente. Una primera forma de ver qué tipo de efecto consiste en obtener los **residuos**.

> Los residuos son las **diferencias existentes en cada casilla entre la tabla empírica y la tabla teórica**, es decir, entre las **frecuencias observadas** y las **frecuencias esperadas**(esperadas si no hubiera relación entre las variables) (Martín, 2008)

Por tanto los residuos se calculan de la siguiente manera:

$$ r_{ij} = f_{ij}-f_{ij}^\prime$$

Ahora bien, suele ser más interesante utilizar estos residuos **tipificados**. Estos se llaman **residuos de Pearson o estandarizados** y no son más que el residuo neto dividido por la raíz cuadrada de la frecuencia esperada:

$$ r_{ij}^S = \frac{f_{ij}-f_{ij}^\prime}{\sqrt{f_{ij}^\prime}}$$

Finalmente, una forma más refinada aún de mostrar los residuos es obtener los **residuos corregidos o ajustados** a través de la siguiente fórmula:

$$ r_{ij}^A = \frac{r_{ij}^S}{\sqrt{(1-\frac{f_i}{f})\cdot(1-\frac{f_j}{f}))}} $$

Para obtener la tabla de residuos de Pearson o los residuos ajustados en Stata es necesario instalar un paquete (.ado). Los paquetes .ado son comandos que extienden la funcionalidad de Stata. Son comandos escritos por la comunidad y pueden instalarse con el comando `ssc install`.  Para obtener los residuos ajustados y de Pearson en concreto necesitamos el comando `tabchi`:

<pre class="Stata_sh">

ssc install tab_chi
</pre>

Ahora, ya podemos pedir a Stata cualquier tipo de residuo.

<pre class="Stata_sh">

tabchi sis_elec regiones, raw pearson adjust noo noe
</pre>

![pearson](/assets/img/posts/sesión4/pearson.png)

Como ves, arriba Stata nos indica que en cada celda de la tabla se puede leer el residuo "crudo", a continuación el residuo de Pearson y finalmente el residuo ajustado. Cada uno de ellos se obtiene con una de las opciones tras la coma en el código de arriba, así que usa el que más te convenga en cada caso. Las opciones `noo` y `noe` son para pedirle a Stata que **no** muestre las frecuencias **o**bservadas ni las **e**speradas.

Como podrás haber intuido los residuos pueden tener signo, pues el valor observado puede ser mayor o menor que el esperado. Un residuo positivo indica que el valor observado es mayor que el esperado mientras que un residuo negativo indica que el valor observado es menor que el esperado. Esto ya nos da una idea de la dirección de la relación entre las variables.

>En general, si el residuo ajustado supera $1.96$ en términos absolutos (negativo o positivo) puede decirse que la diferencia entre el valor observado y el esperado en caso de no haber relación entre las variables no es debido al azar con un 95% de confianza.

Esto es muy útil, pues nos permite saber específicamente en qué casillas se da una relación estadísticamente significativa entre las variables. Sin embargo, para tener una medida que nos permita contrastar una hipótesis, aún nos queda hablar de las dos últimas líneas de esta salida.

### La Chi-cuadrado y la razón de verosimilitud

Además de la significación de cada casilla, es posible medir si la **relación global entre las dos variables es significativa**. Para esto se utilza el **estadístico chi-cuadrado o ji-cuadrado($\chi^2$)**

Este estadístico fue propuesto por Pearson y permite **contrastar la hipótesis de que los dos criterios de clasificación utilizados (las dos variables categóricas) son independientes**. Es decir, se parte de la hipótesis nula de independencia de las variables. Para el que esté interesado el estadístico se construye a partir de las frecuencias esperadas para cada casilla siguiendo la siguiente fórmula:

$$ \chi^2=\sum^m_{j=1}\sum^n_{i=1} \frac{(f_{ij}-f_{ij}^\prime)}{f_{ij}^\prime} $$

Por tanto, simplemente es la suma de los residuos tipificados al cuadrado (veremos esto con más detalle cuando hablemos del modelo de regresión lineal simle).

Lo más importante es recordar que este estadístico vale cero cuando las variables son completamente **independientes**. Asimismo, es mayor cuanto mayor sea la discrepancia entre las $f_o$ y las $f_o$ (o cuanto mayor sea la relación entre las variables).

> ATENCIÓN: Sin embargo, este estadístico no suele utilizarse como indicador de la fuerza de la asociación porque no está estandarizado entre 0 y 1. Veremos más abajo qué estadísticos usar para este propósito.

El estadístico $\chi^2$ sigue una distribución conocida según sus **Grados de Libertad** que se pueden obtener como sigue:

$$ gl=(k-1)*(r-1) $$

donde $k$ es el número de filas en la tabla y $j$ es el número de columnas. La representación gráfica de este estadístico es la siguiente:

![chicuadrado](https://upload.wikimedia.org/wikipedia/commons/thumb/2/21/Chi-square_distributionPDF.png/1280px-Chi-square_distributionPDF.png){: width="400"}

Al no ser este un curso de estadística, no entraremos en más detalles sobre el estadístico y sus grados de libertad. Para calcular el valor del estadístico debe usarse la fórmula de arriba. Sin embargo, Stata calcula por nosotros este valor como podemos leer en la última salida del apartado anterior. En ese caso, estamos ante una $\chi^2$ con 1 grado de libertad (entre paréntesis) que tiene un valor de $6.1715$. Pero el valor en el que debemos fijarnos para el contraste de hipótesis es el que está al lado, de nuevo, el famoso p-value ($Pr=0.013$).

> Si el p-value se encuentra por debajo de 0.05, es decir, el valor de $\alpha$, diremos que hay evidencia suficiente en contra de la hipótesis nula de independencia y, por tanto, podemos concluir que las variables enfrentadas están relacionadas y que esta asociación es estadísticamente significativa con un nivel de confianza del 95%

Por tanto, en nuestro ejemplo, la región y el tipo de sistema electoral están asociados y esta relación es estadísticamente significativa con una confianza del 95%.

La segunda fila de la salida que estamos analizando indica la razón de verosimilitud que se asa en el cociente de las diferencias entre las frecuencias observadas y esperadas en lugar de en su diferencia, como podemos ver en la siguiente fórmula:

$$ L^2 = 2\sum^m_{j=1}\sum^n_{i=1}f_{ij}\ln{\frac{f_{ij}}{f_{ij^*}}}$$

Simplemente se trata de otra alternativa para testar la hipótesis nula de independencia que es especialmente **útil cuando los valores de las frecuencias esperadas son muy pequeñas (menores que 5)**. En tales casos, los valores de la razón de verosimilitud y de la $\chi^2$ difieren (y el primero es más preciso), pero tienden a converger cuando los valores de las frecuencias esperadas aumentan.


### Un resumen de las medidas de asociación y significación

| Variables cruzadas          | Número de categorías  | Medida de asociación | Indicación de dirección |
|-----------------------------|-----------------------|----------------------|-------------------------|
| Nominal X Nominal           | Solo (2x2)            | Phi                  | Sí                      |
| Nominal X Nominal           | Mayor de (2x2)        | V de Cramer          | No                      |
| Nominal X Ordinal           | Al menos (2x3)        | V de Cramer          | No                      |
| Ordinal X Ordinal           | Cuadrada (ej. 3x3)    | Tau-b de Kendall     | Sí                      |
| Ordinal X Ordinal           | Rectangular (ej. 3x4) | Tau-c de Kendall     | Sí                      |
| De intervalo X De intervalo |                       | R de Pearson         | Sí                      |

Fuente: Adaptado de [Data Art](http://www.dataart.ca/labs/lab-13/)
