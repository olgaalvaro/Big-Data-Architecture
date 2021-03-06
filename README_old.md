# BIG DATA ARCHITECTURE
Big Data Architecture Repository

## Detalles de Práctica en Arquitectura
Recomendador de Airbnb por Idealista.com para determinar las mejores zonas en Madrid para alquilar, comprar y/o compartir viviendas.

### Definición de la estrategia del DAaaS
Estadística semanal por lotes de los pisos en mejor estado para alquilar, comprar o compartir de Airbnb en Madrid.

### Arquitectura del DAaaS
Se contemplan dos técnicas para el proceso de obtención de datos:

1. Crawler con scrapy (Colaboratory) que lee de la web de Idealista https://www.idealista.com/ para obtener, mediante la técnica css selectors, un fichero 'datatotal_idealistatoscrape.csv', con las viviendas de Madrid en mejor estado con la información referente a su dirección, precio, si tiene o no garaje, detalles (nº de habitaciones, m2, planta y si es interior o exterior) y una breve descripción para así, enriquecer el dataset de Airbnb.

2. Scraping a la API Idealista previo acceso https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/accesoapidealista.PNG para igualmente extraer mediante la técnica css selectors, un fichero 'data_apidealistapipe.csv', una muestra de viviendas en Madrid con la información referente a su dirección, distrito, tipo de propiedad, precio, tamaño en m2, nº habitaciones, nº baños, si es exterior, longitud, latitud, url y el precio por área.


En ambos casos, utilizaremos Hadoop para el almacenamiento de los datos, tanto del dataset de Airbnb como de los ficheros obtenidos en la operativa anterior.
Realizaremos el análisis de los datos junto con la preparación y limpieza de ellos, para su extracción ¿?.  

Los ficheros los meto en Hadoop y los junto con el dataset de Airbnb (no se como) y hago un mapreduce para obtener el top 50 en un archivo que luego enviare por correo.

Todo irá a un cluster de Hadoop en mi PC + Docker.

### Operating Model
Haré todo a mano, y sacaré el reporte final con un copyToLocal de hdfs y eso lo envio como un email a mano.

### Diagrama
Diagrama del flujo de datos y herramientas utilizadas

https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/bdarquitecture.png

https://drive.google.com/open?id=18s0uLHJ2PrXpvivcAvhPk01FmX_kgTkO


### Parte 2 

1. Crawler con scrapy URL https://www.idealista.com/alquiler-viviendas/madrid-madrid/con-buen-estado/
https://drive.google.com/open?id=1tvhfnKcaWR1MTB1SU_TT84YjDDoeKtsW

2. Scrapy a la API de Idealista URL https://api.idealista.com/3.5/es/search
https://colab.research.google.com/drive/1RpLVXAOt3IDpv3lNgt4A4kvFrtsZ3jUp


### Enunciado
Diseñar, especificar y desplegar un datalake para el procesamiento de datos provenientes de fuentes de datos no estructurados extraídos mediante técnicas de scraping/crawling de sitios de dominio público.
