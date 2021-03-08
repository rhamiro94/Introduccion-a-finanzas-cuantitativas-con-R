# Acceso- web a nuestro panorama general
En las secciones precedentes hemos establecido dos abordajes introducitorios al mercado de activos de renta variable.
El primero consistía en un análisis gráfico en torno a la evolución de global de los siguientes índices y activos: "SP500","VIX", "NASDAQ","PETRÓLEO","ORO","BITCOIN".
En esta entrega nos cocuparemos de generar un acceso web para el análisis interactivo .
Para nuestro desarrollo utilizaremos el paquete Shyni que permite desarrollo de aplicaciones web con R.
Específcamente utilizaramos  app.R que en un archivo permite ejecutar nuestro código integrando la interfaz de usuario (iu) y el servidor(server).
Comenzamos con nuestro código  

```{r echo=FALSE}
install.packages("quantmod")
install.packages("shiny")
library(shiny)
library(quantmod)
```
Una vez instalados nuestros paquetes y llamadas las librerías, continuamos definiendo nuestro rango temporal de análisis.
```{r echo=FALSE}
fechaInicial  <- Sys.Date() - 180  # fecha del sistema menos 180 días 

fechaFinal <- Sys.Date() # Fecha del sistema cómo fecha final 
```
A continuación especifcamos los objetos iu y server. Nuestra interfaz de usuario tiene un diseño orientado a que el usuario pueda visualizar en el rango temporal especificado más arriba los activos e índices que componen nuestra panorama global a través de un diagrama de velas al cual incorporamos el MACD como herramienta de análisis técnico.

```{r echo=FALSE}
ui<-fluidPage(
  titlePanel("Análisis Global  con R"),
  sidebarLayout(
    sidebarPanel("Seleccione el activo  que desea consultar",
                 selectInput('accion', 
                             label = 'Activo', 
                             choices = c("SP500"='^GSPC',
                                         "NASDAQ"='^IXIC', "ORO"='GLD',
                                         "VOLATILIDAD"='^VIX', "PETROLEO"='OIL',
                                         "BITC"='GBTC'))),
    mainPanel("Gráfico de activos principales del Mercado  Americano",
              h1('Gráficos de Precios'),
              p('A continuación se muestra la gráfica del precio de la acción seleccionada.'),
              plotOutput('grafico'))
  )
)
```

A continuación especificamos nuestro servidor.
```{r echo=FALSE}
servidor<-function(input, output) {
  output$grafico <- renderPlot({
    stockdata <- getSymbols(input$accion, src="yahoo", from = fechaInicial,
                       to = fechaFinal, auto.assign = FALSE)
    candleChart(stockdata, name=input$accion,TA='addMACD()',theme =("white"))
  })
}
```
Por último especificamos los objetos con la funicón ´´´shiny.app´´´
```{r echo=FALSE}
server<-function(input, output) {
  output$grafico <- renderPlot({
    stockdata <- getSymbols(input$accion, src="yahoo", from = fechaInicial,
                       to = fechaFinal, auto.assign = FALSE)
    candleChart(stockdata, name=input$accion,TA='addMACD()',theme =("white"))
  })
}
```
De esta manera queda constituida nuestra aplicación web para que tengamos de manera interactiva la posiblidad de explorar los activos que dan un panorama de la situación de mercado.
