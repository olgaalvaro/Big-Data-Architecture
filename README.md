# BIG DATA ARCHITECTURE
Big Data Architecture Repository

## Detalles de Práctica en Arquitectura
Recomendador de Airbnb por Idealista.com para determinar las mejores zonas en Madrid para alquilar, comprar y/o compartir viviendas.

### Definición de la estrategia del DAaaS
Estadística semanal por lotes de los pisos en mejor estado para alquilar, comprar o compartir de Airbnb en Madrid utilizando herramientas de la Nube para facilitar el manejo de los datos.
Limpieza de datos (Talend Data Preparation)

### Arquitectura del DAaaS
Arquitectura CLOUD basada en Scraping API + Google Coud Storage + HIVE + Dataproc

Se contemplan dos técnicas para el proceso de obtención de datos:

1. Crawler con scrapy (Colaboratory) que lee de la web de Idealista https://www.idealista.com/ para obtener, mediante la técnica css selectors, un fichero 'datatotal_idealistatoscrape.csv', con las viviendas de Madrid en mejor estado con la información referente a su dirección, precio, si tiene o no garaje, detalles (nº de habitaciones, m2, planta y si es interior o exterior) y una breve descripción para así, enriquecer el dataset de Airbnb.

2. Scraping a la API Idealista previo acceso https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/accesoapidealista.PNG para igualmente extraer mediante la técnica css selectors, un fichero 'data_apidealistapipe.csv', una muestra de viviendas en Madrid con la información referente a su dirección, distrito, tipo de propiedad, precio, tamaño en m2, nº habitaciones, nº baños, si es exterior, longitud, latitud, url y el precio por área.

Enumeramos los pasos a seguir:

- Insertar el dataset de Airbnb en HIVE.
- Para el proceso de extracción de datos mediante Scraping a una API aplicaré una Cloud Function.
- Almacenar tanto el dataset de Airbnb como el resultado del scraping a una API en un segmento de Google Cloud Storage llamado bdarchitecture_segidealista.  (enlace a gsutil gs://bdarchitecture_segidealista)
- Join entre ambos ficheros restando la longitud y latitud para obtener las mejores viviendas de airbnb más cercanas a las mejores zonas según el idealista, con el TOP de mejores viviendas de airbnb según el importe por noche disponible en Google Cloud Storage.

### Operating Model
Haré todo a mano, y sacaré el reporte final con un copyToLocal de hdfs y eso lo envio como un email a mano.

Existe un operador que dispara el Cloud Function cada mañana con Google Home, con un mensaje de OK. Y esto disparará el Cloud Function y guardará el resultado en un directorio del Segmento llamado input_yelp.

En el segmento siempre existirá un directorio llamado input_airbnb.

Seguire el estandar de levantar el cluster solamente cuando quiera regenerar el TOP, es decir cada mañana levantaré el cluster  a mano, enviaré las tablas de:

- crear tabla de airbnb
- crear tabla de idealista
- load data inpath de gc://xxxx:input_idealista into table idealista
- load data inpath de gc://xxxx:input_airbnb into table airbnb
- SELET JOIN INTO DIRECTORY 'gs//output/results'

Una vez concluidas dichas tareas, apagaré el cluster.
Web con un link directo a Google Storage Segment Object.

#### Desarrollo

- Cloud function
- Query de HIVE
- Pantallazo/video (parte 3 en adelante)


### Diagrama
Diagrama del flujo de datos y herramientas utilizadas

https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/bdarquitecture.png

https://drive.google.com/open?id=18s0uLHJ2PrXpvivcAvhPk01FmX_kgTkO


### Parte 2 

1. Crawler con scrapy URL https://www.idealista.com/alquiler-viviendas/madrid-madrid/con-buen-estado/
https://colab.research.google.com/drive/1mKBOby1MsfzKEfmQ3sHiVK_lJmO8A7yr

2. Scrapy a la API de Idealista URL https://api.idealista.com/3.5/es/search
https://colab.research.google.com/drive/1Nb-uhKZy3ebiOJKar_mYJXPkDcmZoV6z

En este apartado, el resultado de los ficheros (csv separados por pipes) se copian en el segmento 'bdarchitecture_segidealista' de Google Cloud Storage. Pero también se contempla la creación de un Cloud Function para reproducir este apartado tanto para el fichero resultante de este punto como del dataset de Airbnb.

### Enunciado
Diseñar, especificar y desplegar un datalake para el procesamiento de datos provenientes de fuentes de datos no estructurados extraídos mediante técnicas de scraping/crawling de sitios de dominio público.
xx
