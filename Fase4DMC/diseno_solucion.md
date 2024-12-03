# Diseño de la Solución y Arquitectura

## Diagrama de Arquitectura
Diagrama de Arquitectura
![Diagrama de Arquitectura](img/Diagrama.png)

## Descripción

1. **Entorno Virtual**: Máquina virtual con Hadoop, Spark y Kafka.
2. **Carga de Datos**: Los datos fueron cargados y procesados en HDFS.
3. **Procesamiento**: 
   - Spark para el análisis de datos en lote.
   - Spark Streaming y Kafka para simulación de datos en tiempo real.
4. **Visualización**: Salida en consola y Spark Web UI.
