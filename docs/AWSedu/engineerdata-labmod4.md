# Laboratorio: Athena, AWS Glue y CloudFormation

## Acceso a la Consola de Administración de AWS

1\. En la parte superior de estas instrucciones, elige **Start Lab**.

   - Se inicia la sesión del laboratorio.
   - Aparece un temporizador en la parte superior de la página que muestra el tiempo restante de la sesión.
   - **Consejo:** Para renovar la duración de la sesión en cualquier momento, vuelve a elegir **Start Lab** antes de que el temporizador llegue a 00:00.
   - Antes de continuar, espera a que el icono circular a la derecha del enlace **AWS** (esquina superior izquierda) se ponga verde.

2\. Para conectarte a la Consola de Administración de AWS, elige el enlace **AWS** en la esquina superior izquierda.

   - Se abre una nueva pestaña del navegador que te conecta a la consola.
   - **Consejo:** Si no se abre una pestaña nueva, normalmente aparece un banner o icono en la parte superior del navegador indicando que se está bloqueando la apertura de ventanas emergentes. Elige el banner o icono y luego selecciona **Permitir ventanas emergentes**.

---

## ✒️ Tarea 1: Crear y consultar una base de datos y una tabla de AWS Glue en Athena

Tu primera tarea es usar sentencias SQL para definir el esquema de una tabla que permita trabajar con los datos CSV de ejemplo.

En esta tarea harás lo siguiente:

- Especificar un bucket de S3 para los resultados de las consultas.
- Crear una base de datos de AWS Glue mediante el editor de consultas de Athena.
- Crear una tabla en la base de datos de AWS Glue e importar datos.
- Previsualizar los datos en la tabla de AWS Glue.

### Pasos

3\. En la Consola de Administración de AWS, en el cuadro de búsqueda situado a la derecha de **Services**, busca y elige **Athena** para abrir la consola de Athena.

4\. **Especifica un bucket de S3 para los resultados de las consultas.**

   Cuando usas Athena, primero debes especificar un bucket de S3 que almacene los resultados de las consultas que ejecutes. Ya se ha creado un bucket para ti. Sigue estos pasos para configurarlo y usarlo:

   - En la consola de Athena, elige **Explore the query editor**.
   - Elige la pestaña **Settings**.
   - En la sección **Query result and encryption settings**, elige **Manage**.
   - En **Location of query result**, elige **Browse S3**.
   - Elige el bucket que se creó para ti y luego selecciona **Choose**.
   - Mantén los valores predeterminados del resto de ajustes de la página y elige **Save**.
   - **Nota:** En este momento no es necesario que especifiques un **Expected bucket owner**.

5\. **Crea una base de datos de AWS Glue con el editor de consultas de Athena.**

   - Elige la pestaña **Editor**.
   - En la sección **Query 1**, introduce el siguiente comando SQL:

     ```sql
     CREATE DATABASE taxidata;
     ```

   - Elige **Run**.
   - Aparece el mensaje *Query successful* y se crea una base de datos de AWS Glue llamada `taxidata`.
   - **Consejo:** Para confirmar que la base de datos se creó en AWS Glue, abre una pestaña nueva y ve a la consola de AWS Glue. En el panel de navegación, elige **Databases**. La base de datos `taxidata` aparecerá en la lista.

6\. **Crea una tabla en la base de datos de AWS Glue e importa los datos.**

   En la consola de Athena, en la sección **Tables and views** de la izquierda, elige **Create > S3 bucket data** y configura lo siguiente:

   - **Table name:** introduce `yellow`
   - **Description:** introduce `Table for taxi data`
   - **Database configuration:** selecciona **Choose an existing database** y luego elige `taxidata` en la lista desplegable.
   - **Location of input dataset:** copia el siguiente enlace en el campo:

     ```
     s3://aws-tc-largeobjects/CUR-TF-200-ACDSCI-1/Lab2/yellow/
     ```

   - **Encryption:** mantén la configuración predeterminada (no seleccionada).
   - **Data format:** elige **CSV**.

   En la sección **Column details**, elige **Bulk add columns**. (Agregar columnas en bloque)

   - **Nota:** Esta función permite añadir rápidamente metadatos a la tabla, como los nombres de columna y las declaraciones de tipo de dato.

   En el cuadro emergente, copia y pega el siguiente texto y luego elige **Add**:

   ```
   vendor string,
   pickup timestamp,
   dropoff timestamp,
   count int,
   distance int,
   ratecode string,
   storeflag string,
   pulocid string,
   dolocid string,
   paytype string,
   fare decimal,
   extra decimal,
   mta_tax decimal,
   tip decimal,
   tolls decimal,
   surcharge decimal,
   total decimal
   ```

   Los nombres de columna y los tipos de dato se rellenan en la sección **Column details**.

   En la sección **Preview table query**, revisa el texto de la consulta de vista previa, que coincide con el siguiente:

   ```sql
   CREATE EXTERNAL TABLE IF NOT EXISTS taxidata.yellow (
       `vendor` string,
       `pickup` timestamp,
       `dropoff` timestamp,
       `count` int,
       `distance` int,
       `ratecode` string,
       `storeflag` string,
       `pulocid` string,
       `dolocid` string,
       `paytype` string,
       `fare` decimal,
       `extra` decimal,
       `mta_tax` decimal,
       `tip` decimal,
       `tolls` decimal,
       `surcharge` decimal,
       `total` decimal
     )
     ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
     WITH SERDEPROPERTIES (
       'serialization.format' = ',',
       'field.delim' = ','
     ) LOCATION 's3://aws-tc-largeobjects/CUR-TF-200-ACDSCI-1/Lab2/yellow/'
     TBLPROPERTIES ('has_encrypted_data'='false');
   ```

   Observa que los datos no están cifrados. *Serde* es un formato de serialización de datos, y la propiedad `SERDE` especifica que los datos deben estar delimitados por comas, que es el caso.

   - Elige **Create table**.
   - Aparece el mensaje *Query successful*.

   Ahora que tienes una tabla con los datos de los taxis, puedes escribir consultas para recuperar datos del origen de datos en Amazon S3.

7\. **Previsualiza los datos en la tabla de AWS Glue.**

   - En la sección **Data** de la izquierda, en **Database**, elige `taxidata`.
   - En la sección **Tables**, a la derecha de la tabla `yellow`, elige el icono de puntos suspensivos (tres puntos) y luego elige **Preview Table**.
   - La sección **Results** muestra los 10 primeros registros de la tabla.
   - Antes de continuar, en la parte superior del editor de consultas, cierra las consultas abiertas eligiendo el icono **X** en cada pestaña de consulta. Cuando se te pida, elige **Close query**.
   - **Nota:** Solo puedes tener 10 consultas activas en Athena en un momento dado, así que debes gestionarlas en consecuencia.

### Resumen de la Tarea 1

En esta tarea creaste una base de datos y una tabla de AWS Glue mediante el editor de consultas de Athena. Conectaste un conjunto de datos de Amazon S3 a la tabla y definiste el esquema de la tabla usando la función de añadir columnas en bloque. Después de crear la tabla, aprendiste a previsualizar los datos con la función de vista previa de tabla.

Mary ya puede usar consultas SQL más avanzadas y tipos de datos de columna personalizados para archivos CSV almacenados de forma segura en Amazon S3. Esto no era posible usando solo S3 Select.

---

## ✒️ Tarea 2: Optimizar consultas de Athena mediante buckets

Cuando trabajas con conjuntos de datos grandes repartidos en varios archivos, dos objetivos principales son optimizar el rendimiento de las consultas y minimizar el coste. El coste de Athena se basa en el uso, es decir, en la cantidad de datos escaneados, y los precios varían según la región.

Tres estrategias posibles para minimizar costes y mejorar el rendimiento son comprimir los datos, agruparlos en buckets y particionarlos.

- **Comprimir los datos:** comprime tus datos usando uno de los estándares abiertos de compresión de archivos (como Gzip o tar). La compresión da como resultado un tamaño menor del conjunto de datos cuando se almacena en Amazon S3.

- La cardinalidad de tus datos también afecta a cómo deberías optimizar tus consultas. Para más información, consulta *Cardinality (SQL Statements)*. Hay dos opciones para optimizar en función de una cardinalidad alta o baja:

  - **Agrupar los datos en buckets:** para datos con cardinalidad alta, almacena los registros en buckets distintos según un valor compartido en un campo específico. Considera esta agrupación como parte de la fase de preprocesamiento de tu canalización de datos. En este laboratorio, los datos de un único mes se agruparán por separado del conjunto de datos original, que contiene los datos de todo un año. Esta estrategia ayudará a optimizar el rendimiento.

  - **Particionar los datos:** también puedes usar particiones para mejorar el rendimiento y reducir el coste. La partición se usa con datos de cardinalidad baja, es decir, campos con pocos valores únicos o distintos.

En esta tarea experimentarás con la agrupación de datos en buckets para optimizar las consultas de Athena. Realizarás las siguientes acciones:

- Crear una tabla llamada `jan` para contener los datos agrupados.
- Comparar cuánto tarda en ejecutarse una consulta sobre los datos agrupados de enero de 2017 frente a cuánto tarda consultar el conjunto de datos completo para obtener los datos de enero de 2017.

### Pasos

8\. **Crea una tabla para los datos de enero de 2017.**

   - En el editor de consultas de Athena, elige la pestaña **Editor**.
   - Para abrir una nueva pestaña de consulta, elige el icono más (+) en el lado derecho de la sección de consultas.
   - Copia y pega el siguiente texto en la nueva pestaña de consulta y luego elige **Run**:

     ```sql
     CREATE EXTERNAL TABLE IF NOT EXISTS jan (
       `vendor` string,
       `pickup` timestamp,
       `dropoff` timestamp,
       `count` int,
       `distance` int,
       `ratecode` string,
       `storeflag` string,
       `pulocid` string,
       `dolocid` string,
       `paytype` string,
       `fare` decimal,
       `extra` decimal,
       `mta_tax` decimal,
       `tip` decimal,
       `tolls` decimal,
       `surcharge` decimal,
       `total` decimal
       )
       ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
       WITH SERDEPROPERTIES (
       'serialization.format' = ',',
       'field.delim' = ','
     ) LOCATION 's3://aws-tc-largeobjects/CUR-TF-200-ACDSCI-1/Lab2/January2017/'
       TBLPROPERTIES ('has_encrypted_data'='false');
     ```

   - Aparece el mensaje *Query successful*. Athena creó una nueva tabla llamada `jan`, que contiene los datos de taxis únicamente del mes de enero de 2017. La tabla aparece en la sección **Tables** de la izquierda.

   **Análisis:** En este paso creaste una tabla para almacenar datos solo de enero de 2017 usando Serde. Todas las carreras de este conjunto de datos tienen marcas de tiempo de recogida, y casi todas son únicas para cada registro (lo que significa que los registros tienen cardinalidad alta), pero también tienen *January* en la columna del mes, así que puedes usar la agrupación en buckets. Los datos de enero están almacenados en un archivo de Amazon S3 distinto del archivo con el conjunto de datos completo. En los siguientes pasos investigarás si agrupar estos registros en un archivo separado mejora el rendimiento frente a consultar los registros de enero dentro del conjunto de datos completo.

9\. **Ejecuta la siguiente consulta sobre la tabla `yellow`**, que contiene los datos de todo el año. Los datos no están divididos en buckets mensuales.

   ```sql
   SELECT count (count) AS "Number of trips" ,
          sum (total) AS "Total fares" ,
          pickup AS "Trip date"
   FROM yellow WHERE pickup

   between TIMESTAMP '2017-01-01 00:00:00'
      and TIMESTAMP '2017-02-01 00:00:01'
   GROUP BY pickup;
   ```

   Aparece el mensaje *Query successful*. Anota el tiempo de ejecución y la cantidad de datos escaneados por la consulta.

   Ahora vas a compararlo ejecutando una consulta sobre la tabla `jan`.

10\. **Ejecuta la siguiente consulta sobre la tabla `jan`.**

   ```sql
   SELECT count (count) AS "Number of trips" ,
          sum (total) AS "Total fares" ,
          pickup AS "Trip date"
   FROM jan
   GROUP BY pickup;
   ```

   Anota el tiempo de ejecución y la cantidad de datos escaneados. Como puedes ver, se escanean muchos menos datos cuando la consulta se realiza sobre la tabla que contiene únicamente los datos de enero.

**Análisis:** Poner los datos en buckets separados funciona cuando tienes datos con un alto grado de cardinalidad. La cardinalidad se refiere al número de registros distintos en una base de datos. En este ejemplo, el campo `pickup` tiene un alto grado de cardinalidad porque cada carrera tiene una fecha y hora específicas.

### Resumen de la Tarea 2

En esta tarea creaste una tabla para los datos de enero de 2017. Aprendiste a optimizar consultas mediante diversas estrategias, como dividir los datos en múltiples buckets y particionar los datos.

Para lograrlo, primero ejecutaste una consulta sobre datos que no estaban divididos en buckets y la usaste como referencia. Después ejecutaste una consulta sobre los datos de enero de 2017, que estaban divididos en un bucket separado, y comparaste los resultados con la referencia.

---

## ✒️ Tarea 3: Optimizar consultas de Athena mediante particiones

Si te interesa consultar un campo con cardinalidad baja, es decir, un campo con pocos valores únicos, deberías particionar los datos en lugar de usar buckets distintos. En algunos casos, tus datos estarán particionados por otro proceso. Para encontrar el enfoque más eficiente, vas a intentar particionar los datos usando el campo `paytype` en una consulta específica de Athena.

El campo `paytype` almacena el tipo de pago mediante los siguientes códigos:

- 1 = Tarjeta de crédito
- 2 = Efectivo
- 3 = Sin cargo
- 4 = Disputa
- 5 = Desconocido
- 6 = Viaje anulado

Como el conjunto de valores posibles es limitado, `paytype` es una columna excelente para crear particiones. En esta tarea usarás la función `CREATE TABLE AS` para particionar los datos. También especificarás un formato de almacenamiento columnar.

Puedes almacenar datos en Athena con los formatos Apache Parquet u Optimized Row Columnar (ORC). Los formatos de almacenamiento columnar comprimen los datos, lo que reducirá aún más los costes de tus consultas.

En esta tarea experimentarás con el uso de particiones en tus consultas. Completarás los siguientes pasos:

- Crear una tabla basada en el formato Apache Parquet que use el campo `paytype` como partición.
- Comparar el tiempo que tarda una consulta sobre la base de datos `yellow` para los registros con `paytype` 1 (tarjeta de crédito) frente al tiempo que tarda consultar la tabla particionada creada con el formato Apache Parquet.

### Pasos

11\. Para empezar, crearás una partición ejecutando una consulta que seleccione los datos que quieres usar para la partición e indique un formato de almacenamiento.

   Para crear una nueva tabla llamada `taxidata.creditcard` particionada para `paytype = 1` (transacciones con tarjeta de crédito), ejecuta la siguiente consulta en una nueva pestaña de consulta:

   ```sql
   CREATE TABLE taxidata.creditcard
   WITH (
     format = 'PARQUET'
     )AS
     SELECT * from "yellow"
     WHERE paytype = '1';
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 251 ms
   - Run time: 28.572 sec
   - Data scanned: 9.32 GB

   Ahora vas a comparar el rendimiento de ejecutar consultas sobre los datos no particionados de la tabla `yellow` y los datos particionados de la tabla `creditcard`.

12\. Para consultar los datos no particionados de la tabla `yellow`, ejecuta la siguiente consulta en una nueva pestaña:

   ```sql
   SELECT sum (total), paytype FROM yellow
     WHERE paytype = '1' GROUP BY paytype;
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 215 ms
   - Run time: 8.081 sec
   - Data scanned: 9.32 GB

13\. Para consultar los datos particionados de la tabla `creditcard`, ejecuta la siguiente consulta en una nueva pestaña:

   ```sql
   SELECT sum (total), paytype FROM creditcard
     WHERE paytype = '1' GROUP BY paytype;
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 204 ms
   - Run time: 2.259 sec
   - Data scanned: 73.22 MB

**Análisis:** Observa que los resultados de cada consulta son los mismos, pero la consulta sobre los datos particionados tardó bastante menos porque escaneó menos datos. Recuerda que se te cobra por la cantidad de datos escaneados en cada consulta. Así que, como la última consulta escanea menos datos gracias al uso de particiones, tu coste se reduce.

### Resumen de la Tarea 3

En esta tarea particionaste el conjunto de datos donde el campo `paytype` contenía un valor específico. Después ejecutaste consultas sobre las versiones particionada y no particionada de los datos y comparaste los resultados.

---

## ✒️ Tarea 4: Usar vistas de Athena

Puede que hayas notado que Athena puede crear vistas. Crear y usar vistas con los datos ayuda a simplificar el análisis, porque puedes ocultar parte de la complejidad de las consultas a los usuarios.

Athena solo admite ejecutar una sentencia SQL a la vez, pero puedes usar vistas para combinar datos de varias tablas. También puedes usar vistas para optimizar el rendimiento de las consultas, experimentando con distintas formas de recuperar los datos y guardando después la mejor consulta como una vista.

En esta tarea harás lo siguiente:

- Crear una vista para calcular el valor total en dólares de las tarifas de taxi pagadas con tarjeta de crédito.
- Crear una vista para calcular el valor total en dólares de las tarifas pagadas en efectivo.
- Recuperar todos los registros de cada una de estas vistas.
- Crear una nueva vista que combine (join) los datos de estas dos vistas.
- Previsualizar los resultados del join.

### Pasos

14\. Para crear una vista con el valor total en dólares de las tarifas pagadas con tarjeta de crédito, ejecuta la siguiente consulta:

   ```sql
   CREATE VIEW cctrips AS
     SELECT "sum"("fare") "CreditCardFares"
     FROM yellow
     WHERE ("paytype"='1');
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 55 ms
   - Run time: 408 ms
   - Data scanned: -

15\. Para crear una vista con el valor total en dólares de las tarifas pagadas en efectivo, ejecuta la siguiente consulta:

   ```sql
   CREATE VIEW cashtrips AS
     SELECT "sum"("fare") "CashFares"
     FROM yellow
     WHERE ("paytype"='2');
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 68 ms
   - Run time: 378 ms
   - Data scanned: -

   Observa que la sección **Views** de la izquierda ahora muestra las dos vistas que creaste: `cctrips` y `cashtrips`.

16\. Para seleccionar todos los registros de la vista `cctrips`, ejecuta la siguiente consulta:

   ```sql
   Select * from cctrips;
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 158 ms
   - Run time: 9.071 sec
   - Data scanned: 9.32 GB

17\. Para seleccionar todos los registros de la vista `cashtrips`, ejecuta la siguiente consulta:

   ```sql
   Select * from cashtrips;
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 212 ms
   - Run time: 5.157 sec
   - Data scanned: 9.32 GB

   Ahora aprenderás a crear una vista que combine datos de dos vistas diferentes. Usarás esta nueva vista para averiguar los ingresos totales de los pagos con tarjeta de crédito en comparación con los pagos en efectivo para dos proveedores.

18\. Para crear una nueva vista que combine los datos, ejecuta la siguiente consulta:

   ```sql
   CREATE VIEW comparepay AS
   WITH
     cc AS
       (SELECT sum(fare) AS cctotal,
           vendor
       FROM yellow
       WHERE paytype = '1'
       GROUP BY paytype, vendor),
       cs AS
       (SELECT sum(fare) AS cashtotal,
               vendor, paytype
       FROM yellow
       WHERE paytype = '2'
       GROUP BY paytype, vendor)

   SELECT cc.cctotal, cs.cashtotal
   FROM cc
   JOIN cs
   ON cc.vendor = cs.vendor;
   ```

   Los valores de tiempo de ejecución y datos escaneados son similares a los siguientes:

   - Time in queue: 54 ms
   - Run time: 435 ms
   - Data scanned: -

19\. **Previsualiza los resultados del join.**

   - En la sección **Views** de la izquierda, a la derecha de la vista `comparepay`, elige el icono de puntos suspensivos y luego elige **Preview View**.
   - Si se te pregunta por abrir la consulta, elige **Open query**.
   - Los resultados se muestran y son similares a los siguientes:

     ```
     #   cctotal     cashtotal
     1   584502884   250849783
     2   460097126   199181978
     ```

### Resumen de la Tarea 4

En esta tarea aprendiste a crear vistas en Athena. Creaste dos vistas para calcular los ingresos totales de las tarifas de taxi pagadas con tarjeta de crédito y en efectivo. También aprendiste a usar una vista para combinar datos de otras dos vistas. Las habilidades que aprendiste en esta tarea te ayudarán a simplificar el análisis de datos con Athena.

---

## ✒️ Tarea 5: Crear consultas con nombre (named queries) en Athena mediante CloudFormation

El equipo de ciencia de datos quiere compartir las consultas que creó con el conjunto de datos de taxis. Al equipo le gustaría compartirlas con otros departamentos, pero esos departamentos usan otras cuentas de AWS. Los demás departamentos tienen menos experiencia con AWS, así que el equipo de ciencia de datos quiere simplificar el proceso de uso de Athena para ellos.

> **Nota:** En este laboratorio te centrarás en crear una plantilla de CloudFormation para crear una consulta con nombre en Athena. La plantilla no creará la base de datos de AWS Glue ni las tablas y datos que contiene.

Quieres simplificar el proceso de despliegue de consultas. Tras investigar un poco, determinas que la mejor solución es crear una plantilla de CloudFormation para cada consulta. La plantilla se puede compartir con otros departamentos y desplegarse cuando sea necesario.

Primero quieres experimentar construyendo una consulta de ejemplo y probándola con el usuario de IAM de Mary. Si lo consigues, compartirás la plantilla con otros departamentos.

### Pasos

20\. **Revisa y ejecuta la consulta de ejemplo.**

   Mary te proporciona la siguiente consulta de ejemplo:

   ```sql
   SELECT distance, paytype, fare, tip, tolls, surcharge, total FROM yellow WHERE total >= 100.0 ORDER BY total DESC
   ```

   Esta consulta usa la tabla `yellow` de la base de datos `taxidata`. La consulta realiza las siguientes acciones:

   - Selecciona todos los registros de la tabla `yellow` donde el cargo total es mayor o igual a 100,00 $.
   - Muestra los registros en orden descendente según el campo `total` (de modo que la carrera más cara aparece primero en los resultados).
   - Muestra los siguientes campos de los registros resultantes:
     - `distance`
     - `paytype`
     - `fare`
     - `tip`
     - `tolls`
     - `surcharge`
     - `total`

   Ejecuta esta consulta de ejemplo en el editor de consultas de Athena.

   Los resultados son similares a los de la imagen: captura de pantalla de la consola con los resultados en orden descendente por `total`.

   La consulta funciona según lo previsto. Ahora céntrate en crear la plantilla de CloudFormation para la consulta, de modo que pueda reutilizarse y compartirse.

21\. **Ve al IDE.**

   Para abrir el IDE, copia el valor de **LabIDEURL** del panel situado a la izquierda de estas instrucciones y pégalo en una nueva pestaña del navegador. Cuando se te solicite, introduce el valor de **LabIDEPassword** como contraseña.

22\. **Crea una nueva plantilla de CloudFormation.**

   - En el IDE, elige **File > New File**.
   - Guarda el archivo vacío como `athenaquery.cf.yml`, pero déjalo abierto.
   - Copia y pega el siguiente código en el archivo y guárdalo:

     ```yaml
     AWSTemplateFormatVersion: 2010-09-09
     Resources:
       AthenaNamedQuery:
         Type: AWS::Athena::NamedQuery
         Properties:
           Database: "taxidata"
           Description: "A query that selects all fares over $100.00 (US)"
           Name: "FaresOver100DollarsUS"
           QueryString: >
                         SELECT distance, paytype, fare, tip, tolls, surcharge, total
                         FROM yellow
                         WHERE total >= 100.0
                         ORDER BY total DESC
     Outputs:
       AthenaNamedQuery:
         Value: !Ref AthenaNamedQuery
     ```

   - Examina el comando para ver qué se está creando. El comando crea una plantilla de CloudFormation llamada `athenaquery.cf.yml`. Esta plantilla creará una pila (stack) de CloudFormation que hace lo siguiente:
     - Crea una consulta de Athena.
     - Usa una base de datos preexistente en Athena llamada `taxidata`. (**Nota:** Para que otro usuario pueda usar esta plantilla, la base de datos `taxidata` debe existir ya en su cuenta de AWS.)
     - Nombra la consulta `FaresOver100DollarsUS`.
     - Usa la consulta de ejemplo que probaste anteriormente en esta tarea.
     - Muestra los detalles de la consulta en la pestaña **Outputs** de la pila una vez que esta se haya creado.

23\. **Para validar la plantilla de CloudFormation**, ejecuta el siguiente comando en la terminal del IDE:

   ```bash
   aws cloudformation validate-template --template-body file://athenaquery.cf.yml
   ```

   > **Nota:** Si recibes un error que dice *YAML not well-formed*, comprueba el valor del nombre de la consulta y también las tabulaciones y el espaciado de cada línea. Los documentos YAML requieren un espaciado exacto, y el analizador dará errores si el espaciado no coincide.

   Si la plantilla se valida, se muestra la siguiente salida:

   ```json
   {
       "Parameters": []
   }
   ```

   > **Importante:** No pases al siguiente paso hasta que la plantilla esté validada.

   Ahora usarás la plantilla para crear una pila de CloudFormation. Una pila implementa y gestiona el grupo de recursos descritos en una plantilla. Con una pila puedes gestionar el estado y las dependencias de esos recursos de forma conjunta. Piensa en una plantilla de CloudFormation como un plano. La pila es entonces la instancia real de la plantilla que se registra en AWS y que crea realmente los recursos.

24\. **Para crear la pila**, ejecuta el siguiente comando:

   ```bash
   aws cloudformation create-stack --stack-name athenaquery  --template-body file://athenaquery.cf.yml
   ```

   Si la pila se valida, se muestra en la salida el nombre de recurso de Amazon (ARN) de CloudFormation, similar a lo siguiente:

   ```json
   {
       "StackId": "arn:aws:cloudformation:us-east-1:338778555682:stack/athenaquery/2d8cec90-5c42-11ec-8fbf-12034b0079a5"
   }
   ```

   El comando `create-stack` de CloudFormation crea la pila y la despliega. Si la validación pasa y nada provoca una reversión (rollback) en la creación de la pila, continúa con el siguiente paso.

   > **Consejo:** Para comprobar el progreso de la creación de la pila, ve a la consola de CloudFormation. En el panel de navegación, elige **Stacks** y busca el estado de la pila `athenaquery`.

25\. **Confirma el recurso de consulta con nombre que creó la pila de CloudFormation.**

   - Para verificar que se creó la consulta con nombre, ejecuta el siguiente comando en la terminal del IDE:

     ```bash
     aws athena list-named-queries
     ```

     La salida es similar a la siguiente:

     ```json
     {
         "NamedQueryIds": [
           "644f6c10-bf57-48a2-a0ec-a3de179db511"
         ]
     }
     ```

     El ID de la consulta con nombre es un identificador único en AWS para la consulta que creaste mediante la pila de CloudFormation.

   - Copia el ID de la consulta en un editor de texto.

   - Para recuperar los detalles de la consulta con nombre, incluida la sentencia SQL asociada, ejecuta el siguiente comando. Sustituye `<QUERY-ID>` por el ID que guardaste en el editor de texto.

     ```bash
     aws athena get-named-query --named-query-id <QUERY-ID>
     ```

     La salida es similar a la siguiente:

     ```json
     {
     "NamedQuery": {
         "Name": "FaresOver100DollarsUS",
         "Description": "A query that selects all fares over $100.00 (US)",
         "Database": "taxidata",
         "QueryString": "SELECT distance, paytype, fare, tip, tolls, surcharge, total FROM yellow WHERE total >= 100.0 ORDER BY total DESC",
         "NamedQueryId": "644f6c10-bf57-48a2-a0ec-a3de179db511",
         "WorkGroup": "primary"
     }
     }
     ```

   - Para guardar el ID de la consulta con nombre como una variable de bash, ejecuta el siguiente comando. Sustituye `<QUERY-ID>` por el ID que copiaste antes. Esto facilitará usar el ID en comandos de pasos posteriores:

     ```bash
     NQ=<QUERY-ID>
     ```

   - Confirma que el ID de la consulta con nombre se guardó como la variable de bash `NQ`. Ejecuta el siguiente comando:

     ```bash
     echo $NQ
     ```

     El resultado es similar al siguiente:

     ```
     644f6c10-bf57-48a2-a0ec-a3de179db511
     ```

### Resumen de la Tarea 5

En esta tarea aprendiste a integrar una consulta con nombre de Athena en una plantilla de CloudFormation. También aprendiste a validar y desplegar una plantilla para crear una consulta con nombre en una pila de CloudFormation.

Muchas empresas usan varias cuentas de AWS para mantener entornos separados de desarrollo, pruebas y producción. Aislar estos entornos ayuda a garantizar que los equipos sigan las mejores prácticas. Crear consultas en una cuenta de desarrollo y luego probarlas en una cuenta controlada con datos de producción ayuda a asegurar que la consulta está diseñada como se pretendía y genera la información necesaria. Tras validar la consulta, puedes usar las mejores prácticas de DevOps con CloudFormation para llevarla rápidamente a producción, de modo que las partes interesadas del negocio puedan reutilizarla sin tener que construirla desde cero.

---

## ✒️ Tarea 6: Revisar la política de IAM para el acceso a Athena y AWS Glue

Ahora que has creado la consulta con nombre de Athena mediante CloudFormation, revisa la política de IAM de la consulta para asegurarte de que otras personas puedan usarla en producción.

> **Nota:** La política de IAM para la consulta se creó por ti; no tienes la posibilidad de crear políticas de IAM en el entorno del laboratorio.

### Pasos

26\. **Revisa la política `Policy-For-Data-Scientists` en IAM.**

   - En el cuadro de búsqueda a la derecha de **Services**, busca y elige **IAM** para abrir la consola de IAM.
   - En el panel de navegación, elige **Users**.
   - Observa que `mary` es uno de los usuarios de IAM que aparecen en la lista. Este usuario forma parte del grupo de IAM `DataScienceGroup`.
   - Elige el enlace del nombre de usuario `mary`.
   - En la pestaña **Permissions**, elige el enlace de la política `Policy-For-Data-Scientists`.
   - Se abre la página de detalles de `Policy-For-Data-Scientists`. Revisa los permisos asociados a esta política. Observa que los permisos proporcionan acceso limitado únicamente a los servicios Athena, CloudFormation, AWS Glue y Amazon S3.

### Resumen de la Tarea 6

En esta tarea revisaste la política de IAM del grupo `DataScienceGroup`.

La política contiene permisos de acceso limitado a Amazon S3, AWS Glue y Athena. La política está asociada al grupo de IAM al que pertenece el usuario `mary`. La política le permite listar consultas con nombre. La política también incluye los siguientes permisos:

- **Para Amazon S3:** acceder a buckets, listar el contenido de los buckets y leer y escribir objetos, pero no crear un bucket.
- **Para AWS Glue:** crear bases de datos y tablas dentro de las bases de datos.
- **Para CloudShell:** usar CloudShell, por ejemplo para emitir comandos de la AWS CLI.
- **Para CloudFormation:** crear pilas y validar plantillas.
- **Para Athena:** crear un catálogo de datos, crear y obtener consultas con nombre e iniciar consultas.

La política podría usarse como política de ejemplo para usuarios que pretendan crear y usar consultas y vistas de Athena con un conjunto de datos cargándolo en una base de datos de AWS Glue. Como con todos los servicios de AWS, los usuarios de IAM deben tener aplicados los permisos adecuados para poder realizar acciones.

---

## ✒️ Tarea 7: Confirmar que Mary puede acceder y usar la consulta con nombre

Ahora que has revisado la política de IAM, la usarás para probar el acceso de otro usuario a la consulta con nombre y su capacidad de usarla en la AWS CLI.

### Pasos

27\. **Recupera las credenciales del usuario de IAM `mary`.**

   - En el cuadro de búsqueda junto a **Services**, busca y elige **CloudFormation**.
   - En el panel de navegación, elige **Stacks**.
   - Elige el enlace de la pila que creó el entorno del laboratorio. El nombre de la pila incluye una cadena aleatoria de letras y números, y debería ser la pila con la fecha de creación más antigua.
   - En la página de detalles de la pila, elige la pestaña **Outputs**.

     > **Nota:** Cuando creas una plantilla de CloudFormation, puedes elegir mostrar información sobre los recursos que creará la plantilla. La plantilla de CloudFormation que creó los recursos de tu entorno de laboratorio mostró la clave de acceso y la clave de acceso secreta del usuario `mary`.

   - Copia el valor de **MarysAccessKey** al portapapeles.
   - Vuelve a la terminal del IDE.
   - Para crear una variable para la clave de acceso, ejecuta el siguiente comando. Sustituye `<ACCESS-KEY>` por el valor de tu portapapeles.

     ```bash
     AK=<ACCESS-KEY>
     ```

   - Vuelve a la consola de CloudFormation y copia el valor de **MarysSecretAccessKey** al portapapeles.
   - Vuelve a la terminal del IDE.
   - Para crear una variable para la clave de acceso secreta, ejecuta el siguiente comando. Sustituye `<SECRET-ACCESS-KEY>` por el valor de tu portapapeles.

     ```bash
     SAK=<SECRET-ACCESS-KEY>
     ```

   Para probar si el usuario `mary` puede ejecutar el comando `get-named-query`, puedes pasar las credenciales del usuario como variables de bash (`AK` y `SAK`) junto con el comando. También usarás una variable de bash (`NQ`) para incluir el ID de la consulta con nombre. La API intentará entonces ejecutar ese comando como el usuario especificado.

28\. **Para probar si Mary puede usar la consulta con nombre**, ejecuta el siguiente comando:

   ```bash
   AWS_ACCESS_KEY_ID=$AK AWS_SECRET_ACCESS_KEY=$SAK aws athena get-named-query --named-query-id $NQ
   ```

   La salida es similar a la siguiente y se parece a la salida que se mostró cuando ejecutaste el comando anteriormente:

   ```json
   {
       "NamedQuery": {
           "Name": "FaresOver100DollarsUS",
           "Description": "A query that selects all fares over $100.00 (US)",
           "Database": "taxidata",
           "QueryString": "SELECT distance, paytype, fare, tip, tolls, surcharge, total FROM yellow WHERE total >= 100.0 ORDER BY total DESC",
           "NamedQueryId": "644f6c10-bf57-48a2-a0ec-a3de179db511",
           "WorkGroup": "primary"
       }
   }
   ```

   Este resultado confirma que Mary tiene acceso a la consulta con nombre que creaste y desplegaste mediante CloudFormation.

### Resumen de la Tarea 7

¡Enhorabuena! Aprendiste a crear una consulta con nombre de Athena mediante CloudFormation y a desplegarla para los usuarios con una política de IAM segura. Como la consulta está en una plantilla de CloudFormation, puedes reutilizar la plantilla para crear y desplegar la consulta en cualquier cuenta de AWS y cambiar los parámetros según desees.

---

## Novedades del equipo

El equipo está contento con lo que has aprendido y demostrado usando Athena, AWS Glue y CloudFormation.

---

## Enviar tu trabajo

29\. Para registrar tu progreso, elige **Submit** en la parte superior de estas instrucciones.
30\. Cuando se te pregunte, elige **Yes**.
31\. Después de un par de minutos aparece el panel de calificaciones y te muestra cuántos puntos has obtenido en cada tarea. Si los resultados no se muestran tras un par de minutos, elige **Grades** en la parte superior de estas instrucciones.

   > **Importante:** Algunas de las comprobaciones que realiza el proceso de envío en este laboratorio solo te darán crédito si han pasado al menos 5 minutos desde que completaste la acción. Si no recibes crédito la primera vez que envías, puede que tengas que esperar un par de minutos y enviar de nuevo para recibir crédito por esos elementos.

   > **Consejo:** Puedes enviar tu trabajo varias veces. Después de modificar tu trabajo, elige **Submit** de nuevo. Se registra tu último envío.

32\. Para ver comentarios detallados sobre tu trabajo, elige **Submission Report**.

---

## Laboratorio completado

¡Enhorabuena! Has completado el laboratorio.

33\. En la parte superior de esta página, elige **End Lab** y luego elige **Yes** para confirmar que quieres finalizar el laboratorio.
34\. Un panel de mensajes indica que el laboratorio se está terminando.
35\. Para cerrar el panel, elige **Close** en la esquina superior derecha.