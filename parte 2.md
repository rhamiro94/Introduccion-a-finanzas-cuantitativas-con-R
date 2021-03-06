
##   Introducción-a-finanzas-cuantitativas-con-R segunda parte

En nuestro script anterior nos introducimos al mundo de los activos de renta variable a traves del analisis de indices y de Exchange Traded Funds.
En este segunda parte haremos un recorrido similar pero centrando nuestro estudio en el cierre ajustado de los precios de indices y activos seleccionados.
Esto nos permitira trabajar con mayor prolijidad en la generación de gráficos que reúnan de manera mas ordenada la información requerida.
##  Análisis Global
Comenzamos instalando y llamando a las librerías que necesitaremos para comenzar a trabajar.
Al utilizar un indicador de rentabilidad utilizaremos las librerias fportfolio y tseries

```{r echo=FALSE}
install.packages("fPortfolio")
install.packages("tseries")
library(fPortfolio)
library(tseries)
```

Antes de obtener la información definimos el rango temporal que utilizaremos.En nuestro caso los útlimos seis meses.
```{r echo=FALSE}
fechaInicial  <- Sys.Date() - 180  # fecha del sistema menos 180 días 

fechaFinal <- Sys.Date() # Fecha del sistema cómo fecha final 
```


Continuamos definiendO por separado cada indice de nuestro analisis global
```{r echo=FALSE}

SP500<- get.hist.quote(instrument = "^GSPC", 
                        start=fechaInicial, 
                        end=fechaFinal, quote = "AdjClose")


NASDAQ<- get.hist.quote(instrument = "^IXIC", 
                       start=fechaInicial, 
                       end=fechaFinal, quote = "AdjClose")

Volatilidad<- get.hist.quote(instrument = "^VIX", 
                        start=fechaInicial, 
                        end=fechaFinal, quote = "AdjClose")


Oro <- get.hist.quote(instrument = "GLD", 
                             start=fechaInicial, 
                             end=fechaFinal, quote = "AdjClose")
Petroleo <- get.hist.quote(instrument = "OIL", 
                      start=fechaInicial, 
                      end=fechaFinal, quote = "AdjClose")



Bitcoin <- get.hist.quote(instrument = "GBTC", 
                           start=fechaInicial, 
                           end=fechaFinal, quote = "AdjClose")


global <- merge(SP500,NASDAQ,Volatilidad,Oro,Petroleo,Bitcoin,  all = FALSE) 


Renombramos los indices y activos
names(global)<-c("SP500", "NASDAQ", "Volatilidad", "Oro", "Petroleo", "Bitcoin")
plot(global, main=" ", col="darkgreen", xlab="Fecha")
title(main="Evolución global")

```
De esa manera obtenemos de manera organizada la evolución del cierre ajustado de cada activo durante los últimos seis meses

## Análisis de ETFS

Para nuestros etfs utilizaremos también el cierre ajustado de cada activo para su visualización.
Continuamos definiendo los Etfs con sus respectivos "tickers". Reduciermos a seis los etfs seleccionados.

'XLP'-Consumo básico 
'XLK'-Tecnología 
'XLB'-Materiales basicos
'XLRE'-Real State
'XLF'-Financiero
'XLY'- Consumo discrecional


Generamos la informacón sobre nuestro etfs de manera individual

```
etfs = c('XLU', 'XLP', 'XLK', 'XTL','XLB','XLRE','XLF','XLY','XLE','XLV')

```{r echo=FALSE}

Consumo basico<- get.hist.quote(instrument = "XLP", 
                        start=fechaInicial, 
                        end=fechaFinal, quote = "AdjClose")


Tecnología <- get.hist.quote(instrument = "XLK", 
                       start=fechaInicial, 
                       end=fechaFinal, quote = "AdjClose")

Materiales basicos<- get.hist.quote(instrument = "XLB", 
                        start=fechaInicial, 
                        end=fechaFinal, quote = "AdjClose")


Real State <- get.hist.quote(instrument = "XLRE", 
                             start=fechaInicial, 
                             end=fechaFinal, quote = "AdjClose")

Financiero <- get.hist.quote(instrument = "XLF", 
                      start=fechaInicial, 
                      end=fechaFinal, quote = "AdjClose")


Consumo discrecional <- get.hist.quote(instrument = "XLY", 
                           start=fechaInicial, 
                           end=fechaFinal, quote = "AdjClose")


etfs <- merge(Consumo basico,Tecnología,Materiales basicos,Real State,Financiero,Consumo discrecional,  all = FALSE) 


names(etfs)<-c("Consumo basico", "Tecnología", "Materiales basicos", "Real State", "Financiero", "Consumo discrecional")

plot(global, main=" ", col="darkgreen", xlab="Fecha")
title(main="Evolución Etfs")



```


De esta manera obtenemos de manera más prolija que en la entrega anterior los documentos que conformarán nuestra hoja de ruta en nuestro recorrido por los distintos activos mencionados.
Para finalizaar debemos mencionar que si bien entendemos las potenciales limitaciones de este segundo script consideramos que permite mejorar algunas incomidades de la primer entrega.
En la tercer parte agregaremos un análisis con el rendimiento ajustado de estos activos e índices.

© 2021 GitHub, Inc.
