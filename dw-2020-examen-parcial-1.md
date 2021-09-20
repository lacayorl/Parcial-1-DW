dw-2020-parcial-1
================
Tepi
9/3/2020

# Examen parcial

Indicaciones generales:

-   Usted tiene el período de la clase para resolver el examen parcial.

-   La entrega del parcial, al igual que las tareas, es por medio de su
    cuenta de github, pegando el link en el portal de MiU.

-   Pueden hacer uso del material del curso e internet (stackoverflow,
    etc.). Sin embargo, si encontramos algún indicio de copia, se
    anulará el exámen para los estudiantes involucrados. Por lo tanto,
    aconsejamos no compartir las agregaciones que generen.

## Sección I: Preguntas teóricas.

-   Existen 10 preguntas directas en este Rmarkdown, de las cuales usted
    deberá responder 5. Las 5 a responder estarán determinadas por un
    muestreo aleatorio basado en su número de carné.

-   Ingrese su número de carné en `set.seed()` y corra el chunk de R
    para determinar cuáles preguntas debe responder.

``` r
set.seed(20190019) 
v<- 1:10
preguntas <-sort(sample(v, size = 6, replace = FALSE ))

paste0("Mis preguntas a resolver son: ",paste0(preguntas,collapse = ", "))
```

    ## [1] "Mis preguntas a resolver son: 1, 2, 3, 7, 9, 10"

### Preguntas a resolver: 1, 2, 3, 7, 9, 10

### Listado de preguntas teóricas

1.  Para las siguientes sentencias de `base R`, liste su contraparte de
    `dplyr`:

    -   `str()`
    -   `df[,c("a","b")]`
    -   `names(df)[4] <- "new_name"` donde la posición 4 corresponde a
        la variable `old_name`
    -   `df[df$variable == "valor",]`

2.  Al momento de filtrar en SQL, ¿cuál keyword cumple las mismas
    funciones que el keyword `OR` para filtrar uno o más elementos una
    misma columna?

3.  ¿Por qué en R utilizamos funciones de la familia apply
    (lapply,vapply) en lugar de utilizar ciclos?

4.  ¿Cuál es la diferencia entre utilizar `==` y `=` en R?

5.  ¿Cuál es la forma correcta de cargar un archivo de texto donde el
    delimitador es `:`?

6.  ¿Qué es un vector y en qué se diferencia en una lista en R?

7.  ¿Qué pasa si quiero agregar una nueva categoría a un factor que no
    se encuentra en los niveles existentes?

8.  Si en un dataframe, a una variable de tipo `factor` le agrego un
    nuevo elemento que *no se encuentra en los niveles existentes*,
    ¿cuál sería el resultado esperado y por qué?

    -   El nuevo elemento
    -   `NA`

9.  En SQL, ¿para qué utilizamos el keyword `HAVING`?

10. Si quiero obtener como resultado las filas de la tabla A que no se
    encuentran en la tabla B, ¿cómo debería de completar la siguiente
    sentencia de SQL?

    -   SELECT \* FROM A \_\_\_\_\_\_\_ B ON A.KEY = B.KEY WHERE
        \_\_\_\_\_\_\_\_\_\_ = \_\_\_\_\_\_\_\_\_\_

Extra: ¿Cuántos posibles exámenes de 5 preguntas se pueden realizar
utilizando como banco las diez acá presentadas? (responder con código de
R.)

### Respuestas a las preguntas

1.  Las funciones del paquete dplyr como alternativa para las de R base
    son:

    -   `glimpse()`
    -   `select(df, A, B)`
    -   `rename(df, old_name = new_name)`
    -   `filter(df, variable = "valor")`

2.  Al momento de filtrar en mysql uno o más elementos de una columna es
    la keyword `in`

3.  Con la familia apply no es necesario usar ciclos porque las
    funciones dentro de esta familia trabajan por default sobre cada
    elemento de la estructura.

4.  Me da un NA en la categoría que no existe, pero puedo utiliar la
    función `levels()` para agregarla

5.  La keyword `HAVING` se utiliza para incluir condiciones con
    funciones de mysql como `MAX` `MIN` `SUM`, ya que `WHERE` no las
    puede usar.

6.  El query para obtener las filas de una tabla que no estan en otra
    tabla es:

    -   SELECT \* FROM A LEFT OUTER JOIN B ON A.KEY = B.KEY WHERE
        A.Column &lt;&gt; B.Column

EXTRA

``` r
library(gtools)
```

    ## Warning: package 'gtools' was built under R version 4.1.1

``` r
Examenes <- combinations(10,5)
```

Se puede obtener 252 examenes diferentes.

## Sección II Preguntas prácticas.

-   Conteste las siguientes preguntas utilizando sus conocimientos de R.
    Adjunte el código que utilizó para llegar a sus conclusiones en un
    chunk del markdown.

A. De los clientes que están en más de un país,¿cuál cree que es el más
rentable y por qué?

B. Estrategia de negocio ha decidido que ya no operará en aquellos
territorios cuyas pérdidas sean “considerables”. Bajo su criterio,
¿cuáles son estos territorios y por qué ya no debemos operar ahí?

``` r
parcial_anonimo <- readRDS("~/GitHub/Parcial 1 DW Roberto Lacayo/parcial_anonimo.rds")
```

### Respuestas a las preguntas prácticas

## A

``` r
###resuelva acá
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
parcial_anonimo$Year <- year(parcial_anonimo$DATE)
Clientes_rentables <- parcial_anonimo %>% 
  group_by(Cliente, Year) %>%
  summarise(Paises = n_distinct(Pais),
            Unidades = sum(`Unidades plaza`),
            Utilidad = sum(Venta)) %>% 
  filter(Paises >= 2)
```

    ## `summarise()` has grouped output by 'Cliente'. You can override using the `.groups` argument.

``` r
Clientes_rentables <- arrange(Clientes_rentables,-Utilidad)
Clientes_rentables
```

    ## # A tibble: 18 x 5
    ## # Groups:   Cliente [7]
    ##    Cliente   Year Paises Unidades Utilidad
    ##    <chr>    <dbl>  <int>    <dbl>    <dbl>
    ##  1 a17a7558  2019      2     1106    9121.
    ##  2 a17a7558  2018      2      949    8678.
    ##  3 ff122c3f  2018      2      571    7246.
    ##  4 c53868a0  2018      2      816    6945.
    ##  5 ff122c3f  2019      2      658    6647.
    ##  6 c53868a0  2019      2      785    5975.
    ##  7 044118d4  2019      2      652    5410.
    ##  8 044118d4  2018      2      425    3529.
    ##  9 a17a7558  2020      2      219    2018.
    ## 10 ff122c3f  2020      2      134    1466.
    ## 11 f676043b  2019      2      110    1415.
    ## 12 c53868a0  2020      2       89     893.
    ## 13 f676043b  2020      2       58     778.
    ## 14 044118d4  2020      2       57     496.
    ## 15 f2aab44e  2019      2       30     293.
    ## 16 bf1e94e9  2018      2        0       0 
    ## 17 bf1e94e9  2019      2        0       0 
    ## 18 bf1e94e9  2020      2        0       0

Como podemos notar estos 7 cleintes son los que están en más de 1 país,
pero solo 6 se encuentran activos, nuestros clientes más rentables
serian el top 3 que es conformado por `a17a7558` `ff122c3f` `c53868a0` a
lo largo de los 3 años y si analizamos las unidades que vendemos y su
rendimiento veremos que tambien estos son los mayor retorno otorgan por
unidad, aunque existe una gran cantidad de unidades que devuelven, lo
que genera perdida para nosotros.

``` r
library(ggplot2)
gg1 <- ggplot(Clientes_rentables,aes(x = Year, y = Utilidad, fill = Cliente))+
  geom_bar(stat = 'identity')
gg1
```

![](dw-2020-examen-parcial-1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## B

``` r
###resuelva acá

Territorio <- parcial_anonimo %>% 
  group_by(Territorio, Cliente) %>%
  summarise(Venta = sum(Venta)) %>% 
  filter(Venta <= -1)
```

    ## `summarise()` has grouped output by 'Territorio'. You can override using the `.groups` argument.

Podemos Notar que estos 15 territorios representan perdidas para la
empresa, por lo que deberiamos dejarlos.

``` r
library(ggplot2)
gg2 <- ggplot(Territorio,aes(Venta, fill = Territorio))+
  geom_bar()
gg2
```

![](dw-2020-examen-parcial-1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->
