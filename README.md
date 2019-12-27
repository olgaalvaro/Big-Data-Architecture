# BIG DATA ARCHITECTURE
Big Data Architecture Repository

## Detalles de Práctica en Arquitectura
Recomendador de Airbnb por Idealista.com para determinar las mejores zonas en Madrid para alquilar, comprar y/o compartir viviendas.

### Definición de la estrategia del DAaaS
Estadística semanal por lotes de los pisos en mejor estado para alquilar, comprar o compartir de Airbnb en Madrid.

### Arquitectura del DAaaS
Crawler con scrapy (Colaboratory) que lee de la web de Idealista https://www.idealista.com/ para obtener en un fichero 'datatotal_idealistatoscrape.csv', las viviendas de Madrid en mejor estado con la información referente a su dirección, precio, si tiene o no garaje, detalles (nº de habitaciones, m2, planta y si es interior o exterior) y una breve descripción para así, enriquecer el dataset de Airbnb.

Utilizaremos Hadoop para el almacenamiento de los datos, tanto del dataset de Airbnb como del fichero 'datatotal_idealistatoscrape.csv' obtenido en la operativa anterior.
Realizaremos el análisis de los datos junto con la preparación y limpiez de ellos, para su extracción ¿?.  

Los ficheros los meto en Hadoop y los junto con el dataset de Airbnb (no se como) y hago un mapreduce para obtener el top 50 en un archivo que luego enviare por correo.

Todo irá a un cluster de Hadoop en mi PC + Docker.

### Operating Model
Haré todo a mano, y sacaré el reporte final con un copyToLocal de hdfs y eso lo envio como un email a mano.

### Diagrama
https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/bdarquitecture.png

https://drive.google.com/open?id=18s0uLHJ2PrXpvivcAvhPk01FmX_kgTkO

### Enunciado
Diseñar, especificar y desplegar un datalake para el procesamiento de datos provenientes de fuentes de datos no estructurados extraídos mediante técnicas de scraping/crawling de sitios de dominio público.



### Parte 1

Utilizar una herramienta de diagramado como Google Draw o DIA para diseñar y especificar el flujo de datos y herramientas utilizadas

### Parte 2 

Crawler con scrapy URL http://quotes.toscrape.com

https://drive.google.com/open?id=1-zVe1fDJnStt0J40_73lUqD7XSC9-zTZ
