# Diseño de la Solución y Arquitectura

## Flujo de Trabajo Propuesto
1. **Recolección de Datos**: Uso de Kafka para simular la generación de datos en tiempo real.
2. **Procesamiento**: Spark Streaming para procesar y analizar datos en tiempo real.
3. **Almacenamiento**: Hadoop para almacenar los datos simulados.
4. **Visualización**: Consola de Spark para observar los resultados en tiempo real.

## Arquitectura Propuesta
La solución utiliza una arquitectura distribuida:
- **Kafka**: Generación y transmisión de datos en tiempo real.
- **Spark**: Procesamiento y análisis de datos.
- **Hadoop**: Almacenamiento de datos procesados.

![Diagrama de Arquitectura](../img/arquitectura_cobertura_movil.png)
