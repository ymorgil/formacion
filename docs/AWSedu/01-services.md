# 🧰 Servicios AWS tratados en los cursos de AWS Academy

Recopilación de los servicios de AWS que aparecen en las clases del [dashboard de educador](00-educator-classes.md) (Learner Lab, Cloud Foundations, Data Engineering y Cloud Data Pipeline Builder), con una descripción breve de para qué se usa cada uno.

| Servicio | Categoría | Para qué se usa |
|---|---|---|
| **Amazon EC2** (Elastic Compute Cloud) | Cómputo | Servidores virtuales (instancias) escalables bajo demanda; base de las prácticas de Cloud Foundations y del Learner Lab. |
| **Amazon S3** (Simple Storage Service) | Almacenamiento | Almacenamiento de objetos duradero y escalable; origen/destino habitual de los pipelines de datos. |
| **Amazon VPC** (Virtual Private Cloud) | Redes | Red virtual privada y aislada donde se despliegan los recursos: subredes, tablas de rutas, grupos de seguridad. |
| **AWS IAM** (Identity and Access Management) | Seguridad | Gestión de usuarios, roles y permisos de acceso a los servicios de la cuenta AWS. |
| **Amazon RDS** (Relational Database Service) | Bases de datos | Bases de datos relacionales gestionadas (MySQL, PostgreSQL, etc.) sin administrar el servidor manualmente. |
| **AWS Lambda** | Cómputo sin servidor | Ejecución de código bajo demanda sin aprovisionar servidores; se usa para transformar o mover datos en los pipelines. |
| **Amazon CloudWatch** | Monitorización | Métricas, registros (logs) y alarmas para supervisar el estado de los recursos (por ejemplo, instancias EC2). |
| **AWS Glue** | Datos / ETL | Servicio de integración de datos: catalogación (Data Catalog) y trabajos ETL para preparar los datos. |
| **Amazon Redshift** | Datos / Analítica | Almacén de datos (data warehouse) para consultas analíticas a gran escala. |
| **Amazon Kinesis** | Datos / Streaming | Ingesta y procesamiento de datos en tiempo real (streaming). |
| **Amazon Athena** | Datos / Analítica | Consultas SQL directamente sobre datos almacenados en S3, sin necesidad de cargarlos en una base de datos. |
| **Amazon QuickSight** | Analítica / BI | Visualización de datos y creación de paneles (dashboards) de inteligencia de negocio. |
| **AWS Step Functions** | Orquestación | Coordinación de flujos de trabajo entre distintos servicios (por ejemplo, varios pasos de un pipeline de datos). |
| **Amazon EMR** (Elastic MapReduce) | Datos / Big Data | Procesamiento distribuido de grandes volúmenes de datos con Hadoop, Spark y frameworks similares. |
| **Amazon CloudFront** | Redes / Contenido | Red de distribución de contenido (CDN) para servir contenido con baja latencia. |
| **Amazon Route 53** | Redes / DNS | Servicio de DNS gestionado para resolución de nombres de dominio. |

`[captura pendiente: diagrama de arquitectura de referencia de un pipeline de datos con estos servicios]`
