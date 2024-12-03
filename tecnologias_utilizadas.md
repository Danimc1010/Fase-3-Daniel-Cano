# Proyecto de Cobertura Móvil por Tecnología, Departamento y Municipio

## Descripción de la Solución

Este proyecto tiene como objetivo analizar y visualizar la cobertura móvil por diferentes tecnologías (2G, 3G, 4G, 5G) en Colombia, desglosado por departamento y municipio. El análisis se realiza utilizando herramientas de procesamiento de datos como Apache Spark y Kafka, en combinación con un flujo de trabajo de Spark Streaming para procesar datos en tiempo real.

### Herramientas y Tecnologías Utilizadas

- **Apache Spark**: Para el procesamiento y análisis de grandes volúmenes de datos.
- **Kafka**: Para la ingesta y transmisión de datos en tiempo real.
- **Python (PySpark)**: Para la programación y manipulación de los datos.
- **Hadoop**: Para gestionar y almacenar grandes volúmenes de datos distribuidos.
- **Machine Learning**: Opcional para futuras mejoras en el análisis y predicción de tendencias de cobertura.

## Requisitos para la Ejecución

### Requisitos del Sistema

- **Máquina Virtual con Ubuntu/Debian** (con IP local)
- **Hadoop y Spark Instalados** en la máquina virtual
- **Kafka** para la transmisión de datos en tiempo real
- **Python 3.x** y **Pip** instalados

### Instalación

Siga estos pasos para configurar el entorno de trabajo:

1. **Instalar Apache Spark**:
   - Descargue y descomprima Spark en la máquina virtual.
   - Configure las variables de entorno necesarias en `.bashrc`.

2. **Iniciar Spark**:
   - Inicie el maestro de Spark y el trabajador de Spark usando los comandos:
     ```bash
     start-master.sh
     start-slave.sh spark://bigdata:7077
     ```

3. **Instalar Kafka**:
   - Cree un tema llamado `sensor_data` para almacenar los datos de los sensores utilizando Kafka:
     ```bash
     /opt/Kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic sensor_data
     ```

4. **Instalar PIP y PySpark**:
   - Instale PIP y PySpark en su máquina virtual:
     ```bash
     sudo apt install -y python3-pip
     sudo pip install pyspark
     ```

## Instrucciones para la Ejecución

1. **Ejecutar el Productor de Kafka**:
   - El productor de Kafka genera datos simulados de sensores y los envía al tema `sensor_data`.
   - Ejecutar el script de productor:
     ```bash
     python3 kafka_producer.py
     ```

2. **Ejecutar el Consumidor de Spark Streaming**:
   - El consumidor de Spark Streaming lee los datos de Kafka, los procesa en tiempo real y calcula estadísticas como la temperatura y humedad promedio.
   - Ejecutar el script de consumidor:
     ```bash
     spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.3 spark_streaming_consumer.py
     ```

3. **Monitorización**:
   - Monitorear la ejecución de Spark a través de la interfaz web de Spark Context:
     ```
     http://<IP_MaquinaVirtual>:4040
     ```

## Código

### Productor de Kafka (`kafka_producer.py`)

```python
import time
import json
import random
from kafka import KafkaProducer

def generate_sensor_data():
    return {
        "sensor_id": random.randint(1, 10),
        "temperature": round(random.uniform(20, 30), 2),
        "humidity": round(random.uniform(30, 70), 2),
        "timestamp": int(time.time())
    }

producer = KafkaProducer(bootstrap_servers=['localhost:9092'],
                         value_serializer=lambda x: json.dumps(x).encode('utf-8'))

while True:
    sensor_data = generate_sensor_data()
    producer.send('sensor_data', value=sensor_data)
    print(f"Sent: {sensor_data}")
    time.sleep(1)
from pyspark.sql import SparkSession
from pyspark.sql.functions import from_json, col, window
from pyspark.sql.types import StructType, StructField, IntegerType, FloatType, TimestampType
import logging

# Configura el nivel de log a WARN para reducir los mensajes INFO
spark = SparkSession.builder \
    .appName("KafkaSparkStreaming") \
    .getOrCreate()
spark.sparkContext.setLogLevel("WARN")

# Definir el esquema de los datos de entrada
schema = StructType([
    StructField("sensor_id", IntegerType()),
    StructField("temperature", FloatType()),
    StructField("humidity", FloatType()),
    StructField("timestamp", TimestampType())
])

# Configurar el lector de streaming para leer desde Kafka
df = spark \
    .readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "sensor_data") \
    .load()

# Parsear los datos JSON
parsed_df = df.select(from_json(col("value").cast("string"), schema).alias("data")).select("data.*")

# Calcular estadísticas por ventana de tiempo
windowed_stats = parsed_df \
    .groupBy(window(col("timestamp"), "1 minute"), "sensor_id") \
    .agg({"temperature": "avg", "humidity": "avg"})

# Escribir los resultados en la consola
query = windowed_stats \
    .writeStream \
    .outputMode("complete") \
    .format("console") \
    .start()
query.awaitTermination()
