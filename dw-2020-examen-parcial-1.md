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
gg1_plotly <- plotly::ggplotly(gg1)
gg1_plotly
```

<div id="htmlwidget-72ea22016d065a167ab3" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-72ea22016d065a167ab3">{"x":{"data":[{"orientation":"v","width":[0.900000000000091,0.900000000000091,0.900000000000091],"base":[23450.25,22869.16,5154.75],"x":[2019,2018,2020],"y":[5410.39,3529.45,496.4],"text":["Year: 2019<br />Utilidad: 5410.39<br />Cliente: 044118d4","Year: 2018<br />Utilidad: 3529.45<br />Cliente: 044118d4","Year: 2020<br />Utilidad:  496.40<br />Cliente: 044118d4"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"044118d4","legendgroup":"044118d4","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.900000000000091,0.900000000000091,0.900000000000091],"base":[14329,14191.02,3136.44],"x":[2019,2018,2020],"y":[9121.25,8678.14,2018.31],"text":["Year: 2019<br />Utilidad: 9121.25<br />Cliente: a17a7558","Year: 2018<br />Utilidad: 8678.14<br />Cliente: a17a7558","Year: 2020<br />Utilidad: 2018.31<br />Cliente: a17a7558"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(196,154,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"a17a7558","legendgroup":"a17a7558","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.900000000000091,0.900000000000091,0.900000000000091],"base":[14191.02,14329,3136.44],"x":[2018,2019,2020],"y":[0,0,0],"text":["Year: 2018<br />Utilidad:    0.00<br />Cliente: bf1e94e9","Year: 2019<br />Utilidad:    0.00<br />Cliente: bf1e94e9","Year: 2020<br />Utilidad:    0.00<br />Cliente: bf1e94e9"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(83,180,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"bf1e94e9","legendgroup":"bf1e94e9","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.900000000000091,0.900000000000091,0.900000000000091],"base":[7245.98,8353.69,2243.92],"x":[2018,2019,2020],"y":[6945.04,5975.31,892.52],"text":["Year: 2018<br />Utilidad: 6945.04<br />Cliente: c53868a0","Year: 2019<br />Utilidad: 5975.31<br />Cliente: c53868a0","Year: 2020<br />Utilidad:  892.52<br />Cliente: c53868a0"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,192,148,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"c53868a0","legendgroup":"c53868a0","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.900000000000091,"base":8061.08,"x":[2019],"y":[292.610000000001],"text":"Year: 2019<br />Utilidad:  292.61<br />Cliente: f2aab44e","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,182,235,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"f2aab44e","legendgroup":"f2aab44e","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.900000000000091,0.900000000000091],"base":[6646.51,1466.05],"x":[2019,2020],"y":[1414.57,777.87],"text":["Year: 2019<br />Utilidad: 1414.57<br />Cliente: f676043b","Year: 2020<br />Utilidad:  777.87<br />Cliente: f676043b"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(165,138,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"f676043b","legendgroup":"f676043b","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.900000000000091,0.900000000000091,0.900000000000091],"base":[0,0,0],"x":[2018,2019,2020],"y":[7245.98,6646.51,1466.05],"text":["Year: 2018<br />Utilidad: 7245.98<br />Cliente: ff122c3f","Year: 2019<br />Utilidad: 6646.51<br />Cliente: ff122c3f","Year: 2020<br />Utilidad: 1466.05<br />Cliente: ff122c3f"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(251,97,215,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"ff122c3f","legendgroup":"ff122c3f","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":26.2283105022831,"r":7.30593607305936,"b":40.1826484018265,"l":54.7945205479452},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[2017.405,2020.595],"tickmode":"array","ticktext":["2018","2019","2020"],"tickvals":[2018,2019,2020],"categoryorder":"array","categoryarray":["2018","2019","2020"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Year","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1443.032,30303.672],"tickmode":"array","ticktext":["0","10000","20000","30000"],"tickvals":[0,10000,20000,30000],"categoryorder":"array","categoryarray":["0","10000","20000","30000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Utilidad","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"Cliente","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"34b4728559d9":{"x":{},"y":{},"fill":{},"type":"bar"}},"cur_data":"34b4728559d9","visdat":{"34b4728559d9":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

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
gg2_plotly <- plotly::ggplotly(gg2)
gg2_plotly
```

<div id="htmlwidget-6703db2ab6d8f6b3aa5c" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-6703db2ab6d8f6b3aa5c">{"x":{"data":[{"orientation":"v","width":0.423000000000002,"base":0,"x":[-33.56],"y":[1],"text":"count: 1<br />Venta:  -33.56<br />Territorio: 0320288f","type":"bar","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"0320288f","legendgroup":"0320288f","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-62.93],"y":[1],"text":"count: 1<br />Venta:  -62.93<br />Territorio: 0bbe6418","type":"bar","marker":{"autocolorscale":false,"color":"rgba(230,134,19,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"0bbe6418","legendgroup":"0bbe6418","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-28.3],"y":[1],"text":"count: 1<br />Venta:  -28.30<br />Territorio: 1d407777","type":"bar","marker":{"autocolorscale":false,"color":"rgba(205,150,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"1d407777","legendgroup":"1d407777","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-51.66],"y":[1],"text":"count: 1<br />Venta:  -51.66<br />Territorio: 2e4c5a7c","type":"bar","marker":{"autocolorscale":false,"color":"rgba(171,163,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"2e4c5a7c","legendgroup":"2e4c5a7c","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-8.72],"y":[1],"text":"count: 1<br />Venta:   -8.72<br />Territorio: 3cae948b","type":"bar","marker":{"autocolorscale":false,"color":"rgba(124,174,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"3cae948b","legendgroup":"3cae948b","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-12.5],"y":[1],"text":"count: 1<br />Venta:  -12.50<br />Territorio: 67696f68","type":"bar","marker":{"autocolorscale":false,"color":"rgba(12,183,2,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"67696f68","legendgroup":"67696f68","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.423000000000002,0.423000000000002],"base":[0,0],"x":[-133.73,-20.39],"y":[1,1],"text":"count: 1<br />Venta:      NA<br />Territorio: 67e9cc18","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,190,103,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"67e9cc18","legendgroup":"67e9cc18","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-26.55],"y":[1],"text":"count: 1<br />Venta:  -26.55<br />Territorio: 6837187c","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,193,154,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"6837187c","legendgroup":"6837187c","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-117.6],"y":[1],"text":"count: 1<br />Venta: -117.60<br />Territorio: 7b674f31","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,191,196,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"7b674f31","legendgroup":"7b674f31","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":1,"x":[-31.78],"y":[1],"text":"count: 1<br />Venta:  -31.78<br />Territorio: 9d9f2da6","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,184,231,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"9d9f2da6","legendgroup":"9d9f2da6","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-9.97],"y":[1],"text":"count: 1<br />Venta:   -9.97<br />Territorio: bc8e06ed","type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,169,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"bc8e06ed","legendgroup":"bc8e06ed","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.423000000000002,0.423000000000002],"base":[0,0],"x":[-56.7,-8.25],"y":[1,1],"text":"count: 1<br />Venta:      NA<br />Territorio: bf1e94e9","type":"bar","marker":{"autocolorscale":false,"color":"rgba(132,148,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"bf1e94e9","legendgroup":"bf1e94e9","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-87.2],"y":[1],"text":"count: 1<br />Venta:  -87.20<br />Territorio: d43e8f6a","type":"bar","marker":{"autocolorscale":false,"color":"rgba(199,124,255,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"d43e8f6a","legendgroup":"d43e8f6a","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":0.423000000000002,"base":0,"x":[-31.78],"y":[1],"text":"count: 1<br />Venta:  -31.78<br />Territorio: d7254672","type":"bar","marker":{"autocolorscale":false,"color":"rgba(237,104,237,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"d7254672","legendgroup":"d7254672","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.423000000000002,0.423000000000002,0.423000000000002,0.423],"base":[0,0,0,0],"x":[-27.73,-25.18,-21.66,-6.18],"y":[1,1,1,1],"text":"count: 1<br />Venta:      NA<br />Territorio: f7dfc635","type":"bar","marker":{"autocolorscale":false,"color":"rgba(255,97,204,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"f7dfc635","legendgroup":"f7dfc635","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"orientation":"v","width":[0.423000000000002,0.423000000000002,0.423000000000002],"base":[0,0,0],"x":[-71.91,-64.14,-22.21],"y":[1,1,1],"text":"count: 1<br />Venta:      NA<br />Territorio: fed6647d","type":"bar","marker":{"autocolorscale":false,"color":"rgba(255,104,161,1)","line":{"width":1.88976377952756,"color":"transparent"}},"name":"fed6647d","legendgroup":"fed6647d","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":26.2283105022831,"r":7.30593607305936,"b":40.1826484018265,"l":43.1050228310502},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-140.34015,0.430150000000001],"tickmode":"array","ticktext":["-100","-50","0"],"tickvals":[-100,-50,0],"categoryorder":"array","categoryarray":["-100","-50","0"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Venta","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.1,2.1],"tickmode":"array","ticktext":["0.0","0.5","1.0","1.5","2.0"],"tickvals":[0,0.5,1,1.5,2],"categoryorder":"array","categoryarray":["0.0","0.5","1.0","1.5","2.0"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"count","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"Territorio","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"34b422516476":{"x":{},"fill":{},"type":"bar"}},"cur_data":"34b422516476","visdat":{"34b422516476":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
