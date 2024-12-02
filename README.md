# Análisis de Cobertura Móvil por Tecnología y Proveedor

Este proyecto analiza la cobertura móvil por tecnología (2G, 3G, 4G, 5G), departamento y municipio en Colombia, procesando datos en tiempo real utilizando **Apache Spark** y **Kafka**. Toda la configuración y ejecución se realiza en una máquina virtual configurada con **Hadoop**, **Spark** y **Kafka**, a la cual se accede mediante **SSH** utilizando PuTTY.

---

## **Objetivo**
- Identificar brechas de conectividad móvil.
- Analizar la distribución de cobertura tecnológica por región y proveedor.
- Utilizar herramientas de Big Data para procesar y visualizar datos en tiempo real.

---

## **Tecnologías Utilizadas**
- **Apache Spark**: Procesamiento distribuido y análisis en tiempo real.
- **Apache Kafka**: Sistema de mensajería para flujo de datos.
- **Python**: Scripts para productor Kafka y consumidor Spark Streaming.
- **Hadoop**: Almacenamiento distribuido de datos.

---

## **Requisitos**
### **1. Máquina Virtual**
- Configurada previamente con **Hadoop**, **Spark** y **Kafka**.
- Acceso por SSH mediante **PuTTY**.
- Usuario y contraseña predefinidos:
  - **Usuario**: `vboxuser`
  - **Contraseña**: `bigdata`

### **2. Software en la Máquina Virtual**
- **Java 8+**
- **Apache Spark 3.5.3**
- **Apache Kafka**
- **Python 3.8+** con librerías:
  - `pyspark`
  - `kafka-python`

---

## **Pasos para la Configuración**

### **1. Acceso a la Máquina Virtual**
1. Abre **PuTTY** e ingresa la IP de tu máquina virtual.
   - Ejemplo: `192.168.1.13`
2. Inicia sesión:
   - **Usuario**: `vboxuser`
   - **Contraseña**: `bigdata`

---

### **2. Configuración de Apache Spark**
1. Descarga e instala Apache Spark:
   ```bash
   wget https://dlcdn.apache.org/spark/spark-3.5.3/spark-3.5.3-bin-hadoop3.tgz
   tar xvf spark-3.5.3-bin-hadoop3.tgz
   sudo mv spark-3.5.3-bin-hadoop3 /opt/spark

