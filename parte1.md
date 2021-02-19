# Introducción-a-finanzas-cuantitativas-con-R

Uno de los mas importantes desafíos  para quienes buscan introducirse en el análisis bursatil es  saber por donde empezar o mejor dicho en que activos centrarse.
Las preguntas ¿Qué acciones tengo que mirar? o ¿qué sector me conviene operar? pueden resumir esta situación.
En este sentido la visualización y manipulación de la información disponible es clave para alcanzar los mejores resultados posibles.
Podemos resumir nuestra propuesta en dos secciones: La primera consiste en la presentación resumida de la "situación golbal del mercado". Para lograrlo presentaremos la evolución de los índices,activos y commodities más relevantes: SP500,Nasdaq, Crude Oil, Gold, Vix,BTC combinadas en una sola de manera de favorecer visualemente el análisis comparativo de cada activo. Especfícamente volcaremos la información en un docuemento con formato pdf.  
La segunda consiste en analizar los principales sectores del mercado obervando la evolución de los Exchange traded Funds ETFs vinculados a ellos. Además de la información referida a las variables de apertura, cierre, máximo y mínimmo de los precios incorporaremos un importante indicador del análisis técnico:el MACD.  
En este sentido generaremos un documento que permita contener la información relacionada al volumen y al comportamiento de los activos junto con la evolución del MACD, brindando así mayor información al analista.  

## Análisis Global
Comenzamos instalando y llamando a las librerías que necesitaremos para comenzar a trabajar.

```{r echo=FALSE}
install.packages("quantomod")
install.packages("tidyverse")
library(quantmod)
library(tidyverse)
```

Continuamos definiendo un vector con los índices y activos mencionados anteriormente y que utiliaremos como indicadores del estado general del mercado. Como nuestro procedimiento consiste en descargar esta información desde un servidor externo, en este caso yahoo Finance, es necesario conocer la denominación de cada uno. Esta denominación es general y consiste en el "ticker" del activo.  

Antes de obtener la información definimos el rango temporal que utilizaremos.En nuestro caso los útlimos seis meses.
```{r echo=FALSE}
fechaInicial  <- Sys.Date() - 180  # fecha del sistema menos 180 días 

fechaFinal <- Sys.Date() # Fecha del sistema cómo fecha final 
```


Continuamos definiendo un vector con los índices y activos mencionados anteriormente y que utiliaremos como indicadores del estado general del mercado. Como nuestro procedimiento consiste en descargar esta información desde un servidor externo, en este caso yahoo Finance, es necesario conocer la denominación de cada uno. Esta denominación es general y consiste en el "ticker" del activo.  

```{r echo=FALSE}

global = c('^GSPC', '^IXIC','^VIX','GLD', 'OIL','GBTC')
g_simbolos <- list (getSymbols(global,from=fechaInicial, to=fechaFinal, source = "yahoo"))

```

Procedemos a armar nuestro documento pdf que condensará la información gráfica en una sola carilla.

```{r echo=FALSE}
pdf(file = "Panorama Global.pdf")
par(mfrow = c( 3, 2))
chart_Series(GSPC,name="SP500")
chart_Series(IXIC,name="NASDAQ")
chart_Series(VIX,name="Volatilidad")
chart_Series(GLD,name="Oro")
chart_Series(OIL,name="Petróleo")
chart_Series(GBTC,name="Bitcoin")

dev.off()
```

De esa manera obtenemos nuestro primer docuemento en formato pdf en donde resumimos la evolucion de OHLC de los activos escogidos que componenen nuestro panorama.


## Análisis de ETFS
Continuamos definiendo los Etfs con sus respectivos "tickers"
'XLU'-Servicios públicos 
'XLP'-Consumo básico 
'XLK'-Tecnología, 
'XTL'-Telecomunicación,
'XLB'-Materiales basicos
'XLRE'-Real State
'XLF'-Financiero
'XLY'- Consumo discrecional
'XLE'-Energía
'XLV'-Cuidado de Salud
'XLI'- Industria

Generamos nuestro vector de etfs. Al reunir 12 símbolos no parece conveniente en este caso agruparlos en un documento de una carilla. 
En lugr de ello, generaremos un documento más extenso respecto a sus carillas pero con mayor información para el análisis técnico ya que incorporaremos el indicador MACD.

```
etfs = c('XLU', 'XLP', 'XLK', 'XTL','XLB','XLRE','XLF','XLY','XLE','XLV')
simbolos <- list (getSymbols(etfs,from=fechaInicial, to=fechaFinal, source = "yahoo"))
pdf(file = "Prueba_etfs.pdf")
par(mfrow = c( 6, 2))
chartSeries(XLU,name="Servicios_Publicos",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLP,name="Consumo Basico",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLK,name="Tecnologia",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XTL,name="Telecoumunicaciones",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLB,name="Materiales básicos",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLRE,name="Real State",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLF,name="Financiero",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLY,name="Consumo discrecional",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLE,name="Energia",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLV,name="Cuidado de la Salud",type="candlesticks",TA='addMACD()',theme =("white"))
chartSeries(XLI,name="Industria",type="candlesticks",TA='addMACD()',theme =("white"))


dev.off()
```


De esta manera obtenemos los documentos que conformarán nuestra hoja de ruta en nuestro recorrido por los distintos activos mencionados.
Para finalizaar debemos mencionar que si bien entendemos las potenciales limitaciones de este primer script consideramos que los resultados obtenidos pueden ser útiles para usuarios de R interesados en finanzas cuanitativas, al mismo tiempo que entendemos es un poryecto sobre el cual introduciremos mejoras para el beneficio de todxs.  

