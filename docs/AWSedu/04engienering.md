## 01📌 Organizaciones basadas en datos (Data-Driven Organizations)
 
> **Fuente:** AWS Academy Data Engineering — Módulo 1: *Data-Driven Organizations*
> **Resumen elaborado a partir de las diapositivas y notas del instructor/alumno.**
 
## Objetivos del módulo
 
Al finalizar este módulo se debería ser capaz de:
 
- Comparar y contrastar cómo se aplican la analítica de datos y la inteligencia artificial / machine learning (IA/ML) a la toma de decisiones basada en datos.
- Nombrar y describir cada capa de un *pipeline* de datos.
- Enumerar las acciones que se realizan sobre los datos a medida que atraviesan un pipeline.
- Describir las responsabilidades del *data engineer* y del *data scientist* en el procesamiento de datos a través de un pipeline.
- Definir tres estrategias modernas de datos que influyen en cómo se construye la infraestructura de datos.
---
 
## 1. Decisiones basadas en datos
 
Las organizaciones, igual que las personas en su vida diaria (elegir restaurante, comparar precios, etc.), han pasado de tomar decisiones de forma subjetiva a apoyarse en datos y ciencia de datos. Esto se debe a la explosión de datos procedentes de webs, apps móviles y dispositivos inteligentes (IoT), junto con un abaratamiento del cómputo y del almacenamiento en la nube.
 
### Analítica de datos vs. IA/ML
 
| | **Analítica de datos** | **IA/ML** |
|---|---|---|
| **Qué es** | Análisis sistemático de grandes volúmenes de datos (*big data*) para encontrar patrones y tendencias | Modelos matemáticos que hacen predicciones a partir de datos, a una escala difícil o imposible para humanos |
| **Cómo funciona** | Usa lógica de programación (reglas *if/then*) para responder preguntas | Aprende a partir de ejemplos en los datos para responder preguntas |
| **Cuándo es adecuada** | Datos estructurados con un número limitado de variables | Datos no estructurados y variables complejas |
 
**Ejemplo — identificar fotos de perros:**
- *Enfoque analítico*: definir reglas (features) que determinen si una imagen es un perro.
- *Enfoque IA/ML*: mostrar al modelo muchas imágenes etiquetadas como "perro" para que aprenda a identificar nuevas imágenes no etiquetadas.
**Ejemplo de negocio — gestión de relaciones con clientes (CRM):**
- *Analítica*: segmentar clientes por ingresos totales para dar mejor atención a quienes más gastan.
- *IA/ML*: analizar el *churn* (abandono de clientes) para descubrir factores que influyen en la retención.
> En la práctica, muchas organizaciones combinan ambos enfoques como parte de una estrategia unificada de decisión.
 
### Tipos de insights (de menor a mayor valor, y de menor a mayor dificultad)
 
1. **Descriptivo** — ¿Qué pasó?
2. **Diagnóstico** — ¿Por qué pasó?
3. **Predictivo** — ¿Qué pasará?
4. **Prescriptivo** — ¿Cómo hacemos que algo pase o evitamos que pase?
El volumen de datos, el cómputo/almacenamiento necesario y la complejidad de la ciencia de datos aumentan a medida que se avanza de descriptivo a prescriptivo.
 
### Más datos + menos barreras = más decisiones basadas en datos
 
El aumento del volumen de datos disponibles, junto con la reducción de barreras técnicas (gracias a servicios cloud *pay-as-you-go* y arquitecturas cada vez más accesibles), amplía la oportunidad de tomar decisiones basadas en datos.
 
⚠️ **Pero más datos no siempre implica más valor.** Acumular datos sin criterio conlleva:
 
- Mayor coste de almacenamiento
- Más datos no estructurados
- Mayores riesgos de seguridad
- Consultas más lentas
### El valor de los datos decae con el tiempo
 
| Tiempo de análisis | Valor | Tipo de uso |
|---|---|---|
| Casi en tiempo real | Más valioso | Preventivo / predictivo |
| En segundos | Alto | Accionable |
| En minutos/horas | Medio | Reactivo |
| En días/meses | Menos valioso | Histórico |
 
### Trade-offs de las decisiones basadas en datos
 
| Factor | Preguntas clave |
|---|---|
| **Coste** | ¿Cuánto invertir para ir más rápido o predecir con más precisión? ¿Qué mejora incremental justifica un coste adicional? |
| **Velocidad** | ¿Con qué rapidez se necesita la respuesta? ¿Se puede sacrificar precisión por velocidad? |
| **Precisión** | ¿Qué nivel de precisión requiere la predicción? ¿Compensa esperar una respuesta mejor frente a responder más rápido? |
 
### 📌 Puntos clave
 
- Las organizaciones basadas en datos usan ciencia de datos para tomar decisiones informadas.
- La analítica de datos se basa en lógica de programación; es adecuada para datos estructurados con pocas variables.
- La IA/ML aprende de ejemplos; es adecuada para datos no estructurados y variables complejas.
- El aumento de datos disponibles junto con la caída de costes tecnológicos incrementa la oportunidad de tomar decisiones basadas en datos.
---
 
## 2. El pipeline de datos: infraestructura para decisiones basadas en datos
 
Un **pipeline de datos** es la infraestructura que convierte datos en insights para apoyar decisiones. En su forma más simple:
 
```
Recolectar datos → Almacenar y procesar datos → Construir algo útil con los datos
```
 
### Diseñar la infraestructura "hacia atrás" (*work backwards*)
 
1. **¿Qué decisión se quiere tomar?**
2. **¿Qué datos se necesitan para respaldarla?**
3. Sopesar los trade-offs de **coste, velocidad y precisión**.
No es necesario (ni recomendable) construir una única infraestructura que sirva para todos los casos de uso: la nube facilita levantar infraestructuras distintas para cada necesidad de negocio.
 
### Capas de la infraestructura del pipeline
 
```
Fuentes de datos → Ingesta → Almacenamiento ⇄ Procesamiento → Análisis y visualización → Predicciones y decisiones
```
 
| Capa | Función |
|---|---|
| **Fuentes de datos** | Origen de los datos (sistemas internos, sensores, apps, etc.) |
| **Ingesta** | Mecanismos para traer los datos al pipeline |
| **Almacenamiento** | Dónde y cómo se guardan los datos |
| **Procesamiento** | Transformación y preparación de los datos |
| **Análisis y visualización** | Herramientas para extraer insights |
| **Predicciones y decisiones** | Resultado final que apoya la toma de decisiones |
 
### Acciones sobre los datos: *data wrangling*
 
A medida que los datos avanzan por el pipeline, se les aplican tareas de **data wrangling**:
 
- **Descubrir** (discover) — entender qué contienen los datos y si encajan con el objetivo.
- **Limpiar** (clean) — corregir inconsistencias, fusionar fuentes, resolver datos mal mantenidos.
- **Normalizar** (normalize) — preparar los datos para que sean viables para el análisis.
- **Enriquecer** (enrich) — añadir metadatos, calcular valores adicionales, rellenar huecos.
Además, los datos casi siempre se **transforman** (por ejemplo, cambiar de formato o sustituir valores nulos). Un proceso clásico que combina estas tareas es **ETL** (*extract, transform, load*): extraer datos de una fuente, transformarlos y cargarlos en el destino donde se analizarán.
 
### Procesamiento iterativo
 
El proceso de extraer insights del pipeline es casi siempre **iterativo**:
 
1. Se parte de una hipótesis y se experimenta.
2. Si el resultado no es el esperado, se refina el modelo y se reprocesan los datos.
3. Si se necesita más detalle, se incorporan fuentes de datos adicionales y se vuelve a procesar.
La iteración puede darse dentro de un segmento del pipeline o a lo largo de todo el pipeline, y a menudo incluye varias vueltas de almacenamiento y procesamiento con distintos niveles de refinamiento.
 
### 📌 Puntos clave
 
- El pipeline de datos aporta la infraestructura para la toma de decisiones basada en datos.
- Al diseñar un pipeline, hay que partir del problema de negocio y trabajar hacia atrás hasta los datos.
- El pipeline incluye capas de ingesta, almacenamiento, procesamiento, y análisis/visualización.
- El *data wrangling* describe cómo se actúa sobre los datos en su recorrido: descubrimiento, limpieza, normalización, transformación y enriquecimiento.
- El procesamiento es iterativo: se evalúan y mejoran los resultados progresivamente.
---
 
## 3. El rol del ingeniero de datos en organizaciones basadas en datos
 
Para construir el pipeline correcto hay que responder muchas preguntas sobre la naturaleza de los datos y la intención de su procesamiento. Estas preguntas se agrupan en dos perfiles:
 
### Preguntas típicas del *data engineer* (centrado en la infraestructura)
 
- ¿Dispone la organización de los datos que cubren la necesidad? ¿Dónde están almacenados y en qué formato?
- ¿Habrá que combinar datos de múltiples fuentes?
- ¿Cuál es el tipo y la calidad de los datos? ¿Cuál será la fuente de la verdad?
- ¿Cuáles son los requisitos de seguridad? ¿Quién necesita acceso y en qué estado?
- ¿Qué mecanismos se necesitan para transferir los datos hasta el pipeline?
- ¿Cuánto volumen de datos hay y con qué frecuencia se actualiza o procesa?
- ¿Qué importancia tiene la velocidad cuando se solicitan los datos?
### Preguntas típicas del *data scientist* (centrado en el análisis)
 
- ¿Qué pueden decirme los datos?
- ¿Cómo evaluaré los resultados?
- ¿Qué tipo de visualización necesito?
- ¿Con qué formatos y herramientas están familiarizados los analistas?
- ¿Necesito un framework de *big data*?
- ¿Qué tipo de modelos de IA/ML encajan?
- ¿Cuál es la forma más sencilla de implementar IA/ML?
### 📌 Puntos clave
 
- Los roles de *data scientist* y *data engineer* trabajan ambos con el pipeline; algunas tareas pueden solaparse.
- El *data engineering* se centra en la infraestructura por la que pasan los datos; el *data scientist* se centra en trabajar con esos datos.
- Para construir el pipeline adecuado hay que preguntarse por los resultados deseados y los datos disponibles, e iterar a medida que se aprende más.
---
 
## 4. Estrategias modernas de datos
 
AWS recomienda abordar la infraestructura de datos siguiendo tres estrategias complementarias: **modernizar, unificar e innovar.**
 
### 🔧 Modernizar — aumentar la agilidad y reducir la carga operativa no diferenciada
 
- Migrar de infraestructura *on-premises* a servicios en la nube.
- Migrar a herramientas y almacenes de datos específicos (*purpose-built*) en lugar de una base de datos única para todo.
- Construir pipelines débilmente acoplados (*loosely coupled*) para poder escalar/modificar cada componente de forma independiente.
### 🔗 Unificar — crear una única fuente de la verdad
 
- Romper los silos de datos.
- Democratizar el acceso (los usuarios acceden directamente en lugar de depender de un equipo central como IT).
- Dotar a los usuarios de herramientas para visualizar sus propios insights.
- Usar un *data lake* y permitir consultas directas sobre los datos.
- Facilitar el gobierno y el movimiento de datos entre el *data lake* y los almacenes específicos.
> Este cambio suele requerir modificaciones culturales importantes sobre quién es "propietario" de los datos y qué gobernanza se necesita.
 
### 💡 Innovar — usar IA y ML para descubrir insights más rápido
 
- Pasar de una toma de decisiones reactiva a una proactiva.
- Incorporar IA/ML en la toma de decisiones para aprovechar grandes volúmenes de datos no estructurados.
- Aprovechar servicios cloud con funcionalidades de IA/ML que democratizan quién puede usar ML (por ejemplo, interfaces visuales sin código o integración de ML mediante SQL).
> *"Ser una organización basada en datos significa tratar culturalmente los datos como un activo estratégico y luego construir las capacidades para ponerlo en uso, no solo para grandes decisiones sino también para la acción diaria en primera línea."*
> — Ishit Vachhrajani, AWS Enterprise Strategist
 
### 📌 Puntos clave
 
- Las organizaciones que quieren ser data-driven deben modernizar, unificar e innovar su infraestructura de datos.
- **Modernizar**: moverse a infraestructuras cloud y servicios específicos para reducir el esfuerzo administrativo/operativo.
- **Unificar**: crear una única fuente de la verdad y hacer los datos accesibles en toda la organización.
- **Innovar**: buscar nuevas formas de extraer valor de los datos aplicando IA y ML.
---
 
## 5. Caso práctico de ejemplo
 
**Situación:** Un minorista tradicional con poca presencia online invierte en marketing e infraestructura IT para aumentar las ventas online, pero los resultados trimestrales quedan muy por debajo de lo esperado.
 
**Pregunta:** ¿Cuál sería el primer paso apropiado para abordar esto de forma basada en datos?
 
**Opciones evaluadas:**
 
| Opción | Acción propuesta | ¿Cuándo sería apropiada? |
|---|---|---|
| A | Entrenar un modelo de ML sobre comentarios de clientes | Si se sospecha que la causa es insatisfacción del cliente |
| B | Test A/B con una nueva interfaz de compra simplificada | Si se sospecha que la dificultad está en el proceso de compra |
| **C ✅** | **Usar una herramienta de BI para analizar los datos de ventas de los dos últimos trimestres y generar una hipótesis** | Es el paso correcto como *punto de partida*, antes de tener una hipótesis concreta |
| D | Segmentar clientes por ingresos y personalizar ofertas | Si se sospecha que el problema está en cómo compran los distintos segmentos |
 
**Conclusión:** El primer paso correcto es partir del problema de negocio (ventas bajas), analizar los datos disponibles para formular una hipótesis, y a partir de ahí construir el pipeline adecuado para recopilar y procesar datos de forma iterativa (opción **C**). Las demás opciones son válidas, pero solo *después* de haber formulado una hipótesis concreta.
 
---
 
## Resumen general del módulo
 
- Las organizaciones basadas en datos usan **analítica de datos** e **IA/ML** para tomar decisiones informadas; ambas ayudan con predicciones, pero la IA/ML aprende de los datos en lugar de aplicar reglas programadas.
- Un **pipeline de datos** tiene cuatro capas principales: **ingesta, almacenamiento, procesamiento, y análisis/visualización**.
- Los datos se someten a **data wrangling** (descubrir, limpiar, normalizar, transformar, enriquecer) en su recorrido por el pipeline.
- El **data engineer** se centra en la infraestructura; el **data scientist** se centra en el análisis de los datos dentro de esa infraestructura.
- Construir una infraestructura de datos moderna implica **modernizar, unificar e innovar**.
 
