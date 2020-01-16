# BIG DATA ARCHITECTURE
Big Data Architecture Repository

## Detalles de Práctica en Arquitectura
Recomendador de Airbnb por Idealista.com para determinar las mejores zonas en Madrid para alquilar, comprar y/o compartir viviendas.

### Definición de la estrategia del DAaaS
Informe diario de los pisos en mejor estado para alquilar, comprar o compartir de Airbnb en Madrid utilizando herramientas de la Nube para facilitar el manejo de los datos.
Limpieza de datos (Talend Data Preparation)

### Arquitectura del DAaaS

*Arquitectura CLOUD basada en Scraping API + Google Coud Storage + HIVE + Dataproc*

Se contemplan dos técnicas para el proceso de obtención de datos en fichero csv:

1. Crawler con scrapy (Colaboratory) que lee de la web de Idealista https://www.idealista.com/ para obtener, mediante la técnica css selectors, un fichero 'datatotal_idealistatoscrape.csv', con las viviendas de Madrid en mejor estado con la información referente a su dirección, precio, si tiene o no garaje, detalles (nº de habitaciones, m2, planta y si es interior o exterior) y una breve descripción para así, enriquecer el dataset de Airbnb.

2. Scraping a la API Idealista previo acceso https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/accesoapidealista.PNG para igualmente extraer mediante la técnica css selectors, un fichero 'data_apidealistapipe.csv', una muestra de viviendas en Madrid con la información referente a su dirección, distrito, tipo de propiedad, precio, tamaño en m2, nº habitaciones, nº baños, si es exterior, longitud, latitud, url y el precio por área.

Enumeramos los pasos a seguir:

- Insertar el dataset de Airbnb en HIVE mediante jobs o tareas.
- Para el proceso de extracción de datos mediante las técnicas utilizadas en el apartado 2 aplicaré una Cloud Function.
- Almacenar tanto el dataset de Airbnb como el resultado del scraping a una API en un segmento de Google Cloud Storage llamado bdarchitecture_segidealista.  (enlace a gsutil gs://bdarchitecture_segidealista).
- Crear las tablas en HIVE, con los datos del dataset y los datos del fichero resultante de la parte nº 2.
- Join entre ambos ficheros restando la longitud y latitud para obtener las mejores viviendas de airbnb más cercanas a las mejores zonas según el idealista, con el TOP de mejores viviendas de airbnb según un rango de importes por noche disponible en Google Cloud Storage para cubrir las necesidades de todos los clientes según sus posibilidades económicas.
- Almacenar el resultado del TOP de las mejores viviendas en Google Cloud Storage para por ejemplo su notificación por correo y con la posibilidad de guardar un histórico de los TOPs por si se precisa un análisis más completo.
- Desarrollar una web que muestre el resultado del TOP almacenado en Google Cloud Platform-Storage

### Operating Model

Existe un operador que dispara el Cloud Function cada mañana desde Google Home, con un mensaje "Google, adelante con el scrapy a la  API del idealista.com". Y esto disparará el Cloud Function y guardará el fichero resultante en un directorio del Segmento llamado input/idealista con formato csv.

En el segmento **bdarchitecture_segidealista** siempre existirá un directorio llamado input_airbnb con el dataset de Airbnb de la muestra reducida y completa.

Operativa diaria de creación del clúster para a continuación, ejecutar  las siguientes tareas:

- crear tabla de airbnb 
- crear tabla de idealista
- LOAD DATA INPATH  'gs://bdarchitecture_segidealista/input/idealista/ficheroparte2.csv' INTO TABLE idealista 
- LOAD DATA INPATH  'gs://bdarchitecture_segidealista/input_airbnb/airbnb-listings.csv' INTO TABLE airbnb;
- SELET JOIN INTO DIRECTORY 'gs//output/results'

Una vez alcanzado el objetivo de obtener el informe con el TOP de las mejores viviendas, se recomienda eliminar el clúster. 
Web con un link directo en Google Storage Segment Object para la descarga del resultado o notificación por correo adjuntando dicha información.


#### Desarrollo

- Insertar el dataset de Airbnb en HIVE.

  a) Crear tabla de airbnb con la estructura del dataset de Airbnb en HIVE -- (job o tarea: **create_table_airbnb**)
  
  b) Volcar el dataset de Airbnb en la tabla del punto anterior con el comando -- (job o tarea: **volcar_dataset_airbnb_hive**)
  
     LOAD DATA INPATH  'gs://bdarchitecture_segidealista/input_airbnb/airbnb-listings-reducida.csv' INTO TABLE airbnb;

   https://colab.research.google.com/drive/175qx9RyULP-_exgftzRnbmr5SazrNdQG
   
- Cloud functions para almacenar el resultado de los ficheros obtenidos en el apartado 2 en el segmento referenciado de Google Gloud Storage.
  
  https://colab.research.google.com/drive/145Od49s4Dx85yRQhUoAdAzXj9C5H5x6-

- Query de HIVE


### Diagrama
Diagrama del flujo de datos y herramientas utilizadas

https://github.com/olgaalvaro/Big-Data-Architecture/blob/master/bdarquitecture.png

https://drive.google.com/open?id=18s0uLHJ2PrXpvivcAvhPk01FmX_kgTkO


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
