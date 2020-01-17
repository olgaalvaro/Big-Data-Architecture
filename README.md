# BIG DATA ARCHITECTURE
Big Data Architecture Repository

## Detalles de Práctica en Arquitectura

Recomendador de Airbnb por Idealista.com para determinar las mejores zonas en Madrid para alquilar, comprar y/o compartir viviendas.

### Definición de la estrategia del DAaaS

Estadística semanal por lotes de los pisos en mejor estado para alquilar, comprar o compartir de Airbnb en Madrid utilizando herramientas de la Nube para facilitar el manejo de los datos.
Limpieza de datos (Talend Data Preparation)

### Arquitectura del DAaaS

Arquitectura CLOUD basada en Scraping API + (Talend) + Google Coud Storage + HIVE + Dataproc

Se contemplan dos técnicas para el proceso de obtención de datos:

1. Crawler con scrapy (Colaboratory) que lee de la web de Idealista https://www.idealista.com/ para obtener, mediante la técnica css selectors, un fichero 'datatotal_idealistatoscrape.csv', con las viviendas de Madrid en mejor estado con la información referente a su dirección, precio, si tiene o no garaje, detalles (nº de habitaciones, m2, planta y si es interior o exterior) y una breve descripción para así, enriquecer el dataset de Airbnb.

2. Scraping a la API Idealista previo acceso https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/accesoapidealista.PNG para igualmente extraer mediante la técnica css selectors, un fichero 'data_apidealistapipe.csv', una muestra de viviendas en Madrid con la información referente a su dirección, distrito, tipo de propiedad, precio, tamaño en m2, nº habitaciones, nº baños, si es exterior, longitud, latitud, url y el precio por área.

Enumeramos los pasos a seguir:

- Insertar el dataset de Airbnb en HIVE mediante jobs o tareas.
- Para el proceso de extracción de datos mediante las técnicas utilizadas en el apartado 2 (crawler Scraping a una API aplicaré una Cloud Function.
- Almacenar tanto el dataset de Airbnb como el resultado del scraping a una API en un segmento de Google Cloud Storage llamado bdarchitecture_segidealista.  (enlace a gsutil gs://bdarchitecture_segidealista)
- Join entre ambos ficheros restando la longitud y latitud para obtener las mejores viviendas de airbnb más cercanas a las mejores zonas según el idealista, con el TOP de mejores viviendas de airbnb según el importe por noche disponible en Google Cloud Storage.

### Operating Model

Existe un operador que dispara el Cloud Function cada mañana con Google Home, con un mensaje de Success!. Y esto disparará el Cloud Function y guardará el fichero resultante en un directorio del Segmento llamado input_idealista.

En el segmento **bdarchitecture_segidealista** siempre existirá un directorio llamado input_airbnb.

Seguire el estandar de levantar el cluster solamente cuando quiera regenerar el TOP por las vías indicadas en el apartado 2, es decir cada mañana levantaré el cluster, para ejecutar las siguientes tareas o jobs:

- crear tabla de airbnb 
- crear tabla de idealista
- load data inpath de gc://xxxx:input_idealista into table idealista 
- LOAD DATA INPATH  'gs://nombredelsegmeto/input_airbnb/airbnb-listings.csv' INTO TABLE airbnb;
- SELET JOIN INTO DIRECTORY 'gs//output/results'

Una vez concluidas dichas tareas, eliminaré el cluster.
Web con un link directo a Google Storage Segment Object.

### Desarrollo

- Insertar el dataset de Airbnb en HIVE.

  a) Crear tabla de airbnb con la estructura del dataset de Airbnb en HIVE -- (job o tarea: **create_table_airbnb**)
  
  b) Volcar el dataset de Airbnb en la tabla del punto anterior con el comando -- (job o tarea: **volcar_dataset_airbnb_hive**)
  
     LOAD DATA INPATH  'gs://bdarchitecture_segidealista/input_airbnb/airbnb-listings-reducida.csv' INTO TABLE airbnb;

   https://colab.research.google.com/drive/175qx9RyULP-_exgftzRnbmr5SazrNdQG
   
- Cloud functions para almacenar el resultado de los ficheros obtenidos en el apartado 2 en el segmento referenciado de Google Gloud Storage.
  
  https://colab.research.google.com/drive/145Od49s4Dx85yRQhUoAdAzXj9C5H5x6-

- Query de HIVE


### Diagrama

Diagrama del flujo de datos y herramientas utilizadas:

https://drive.google.com/file/d/18s0uLHJ2PrXpvivcAvhPk01FmX_kgTkO


### Parte 2 

1. Crawler con scrapy URL https://www.idealista.com/alquiler-viviendas/madrid-madrid/con-buen-estado/
https://colab.research.google.com/drive/1mKBOby1MsfzKEfmQ3sHiVK_lJmO8A7yr

2. Scrapy a la API de Idealista URL https://api.idealista.com/3.5/es/search  
https://colab.research.google.com/drive/1Nb-uhKZy3ebiOJKar_mYJXPkDcmZoV6z

En este apartado, el resultado de los ficheros (csv separados por pipes) se copian en el segmento 'bdarchitecture_segidealista' de Google Cloud Storage. Pero también se contempla la creación de un Cloud Function para reproducir este apartado para el fichero resultante de este punto.

### Parte 3

Configuración de un clúster con al menos 3 contenedores a través del proveedor Google Cloud Platform-Dataproc.

1. Crear el clúster por distintas vías en Dataproc (Google Cloud Platform)
2. Crear regla de cortafuegos
3. Verificar el acceso al clúster y hdfs a traves de la URL://IPexternanodomaestro y puertos correspondientes

https://colab.research.google.com/drive/1erqopPNbPUoj3CioTtuH3TVR8HfuO7E9


### Parte 4

1. Subir los archivos extraídos durante la parte 2 al cluster de Hadoop e insertarlos en el HDFS.
2. Realizar la tarea de procesamiento de datos sobre los datos extraídos utilizando WordCount.

https://colab.research.google.com/drive/12Pyx_qLslp3eQpcterEVwCiW467Lrlbe


### Enunciado
Diseñar, especificar y desplegar un datalake para el procesamiento de datos provenientes de fuentes de datos no estructurados extraídos mediante técnicas de scraping/crawling de sitios de dominio público.
xx
