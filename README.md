# RESUMEN COMPLETO DE MONGODB: CONCEPTOS BÁSICOS

## Introducción a MongoDB

MongoDB es una base de datos NoSQL orientado a documentos, que ofrece una alternativa flexible y escalable a las tradicionales bases de datos relacionales. Su diseño está específicamente optimizado para manejar grandes volúmenes de datos tanto estructurados como no estructurados, lo que lo hace adecuado para aplicaciones modernas que requieren alta escalabilidad y rendimiento.

MongoDB tiene un modelo de datos basado en documentos, que almacena información en formato BSON (Binary JSON). Ya que BSON permite una representación eficiente de datos en binario mientras mantiene la flexibilidad y legibilidad de JSON. Esta es una combinación ideal para un desarrollo ágil donde los requisitos de datos pueden evolucionar rápidamente como puede ser una aplicacion web, un sistema de análisis de datos o microservicios.

### Características principales y diferenciadoras

1. **Modelo basado en documentos**: A diferencia de las bases de datos relacionales que tienen una estructura rígida definida de antemano, MongoDB te permite que cada documento tenga una estructura independiente. Los documentos tienen una estructura flexible que se adapta a cualquier uso y tambien jerarquica, estas dos características facilitan mucho el almacenamiento de datos complejos en un solo documento.

2. **NoSQL (No Relacional):** MongoDB no usa tablas como las bases de datos tradicionales, permite guardar datos con diferentes formas y estructuras, esto es útil para proyectos que cambian o crecen rápido.

3. **Escalabilidad horizontal:** Se puede dividir la base de datos en varios servidores (sharding), lo que ayuda a manejar grandes cantidades de información sin perder rendimiento sobre todo en aplicaciones donde el volumen de datos crece rápidamente.

4. **Alta disponibilidad mediante replicación:** MongoDB puede copiar automáticamente los datos en otros servidores. Si alguno falla otro puede seguir funcionando sin perder información.

5. **Consultas flexibles:** Permite hacer búsquedas avanzadas usando filtros y operadores. También se pueden hacer análisis de datos con funciones de agregación.

6. **Soporte de transacciones:** Se pueden realizar operaciones en varios documentos al mismo tiempo como si fuera una sola, asegurando que los datos siempre estén correctos.

7. **Agregaciones y MapReduce:** Tiene herramientas que permiten transformar y analizar grandes volúmenes de datos, algo parecido a lo que se hace con SQL en bases de datos relacionales.

8. **Compatibilidad con múltiples lenguajes:** Se puede usar MongoDB desde muchos lenguajes de programación como Python, Java o JavaScript, lo que lo hace versatil y práctico para distintos tipos de proyectos.


### Comparación detallada con bases de datos relacionales

| **Aspecto**              | **MongoDB**                                      | **Bases de Datos Relacionales**               |
|--------------------------|--------------------------------------------------|-----------------------------------------------|
| Modelo de datos          | Documentos (BSON/JSON) con estructura flexible  | Tablas con filas y columnas de estructura fija |
| Esquema                  | Dinámico, puede variar entre documentos         | Rígido, definido antes de insertar datos      |
| Escalabilidad            | Horizontal (añadiendo más servidores)           | Vertical (mejorando el servidor existente)    |
| Lenguaje de consulta     | Operadores en JSON                              | SQL estandar                                  |
| Transacciones            | Soporte para transacciones multi-documento      | Soporte completo para transacciones ACID      |
| Relaciones entre datos   | Embebidas o por referencia                      | Claves foráneas y operaciones JOIN            |
| Rendimiento              | Optimizado para lectura/escritura rápida        | Optimizado para consistencia e integridad     |

MongoDB es una base de datos flexible que no requiere una estructura fija, lo que permite modificar la forma de los datos según sea necesario. Esto lo hace especialmente útil en aplicaciones donde la información cambia con frecuencia o tiene formatos variados, ya que se adapta fácilmente a esos entornos dinámicos.

---

## Estructura de datos en MongoDB

### Definición de Documentos

MongoDB utiliza una estructura de datos que se basa en documentos que son pares de `campo-valor`, estos documentos se guardan en formato **BSON** que es un formato similiar al JSON y esto optimiza el rendimiento de MongoDB. Aparte los documentos tienen amplios tipos de datos que se pueden guardar y utilizar.

Un ejemplo de documento podría ser:

```json
{
"nombre": "Laptop",
"precio": 999.99,
"cantidad": 15,
"categoria": "Electrónica",
"especificaciones": {
  "procesador": "Intel i7",
  "ram": "16GB",
  "almacenamiento": "512GB SSD"
  }
}
```

Con este ejemplo vemos varias cosas importantes como que tiene soporte para documentos anidados (`especificaciones`) que proporciona gran flexibilidad para modelar los datos.


### Estructura
MongoDB guarda sus documentos internamente en formato BSON, que es una forma binaria del formato JSON. Esto permite que el sistema almacene y acceda a los datos de manera más rápida y eficiente. Aunque los datos se escriben y leen como JSON, MongoDB los transforma a BSON para trabajar internamente.

Entre los tipos de datos que BSON soporta se incluyen:

* **Textos** (strings)
* **Números** (enteros y decimales)
* **Booleanos** (true o false)
* **Arrays** (listas)
* **Subdocumentos** (documentos dentro de otros)
* **Nulos** (null)
* **Fechas y horas**
* **ObjectId**, que es un identificador único generado automáticamente por MongoDB.
  
### Colecciones

En MongoDB, una colección es un grupo de documentos relacionados entre sí, pero que no necesitan tener exactamente la misma estructura (comparten estructura lógica). A diferencia de las tablas en bases de datos tradicionales no se exige que todos los documentos tengan los mismos campos, por ejemplo, en una colección llamada "productos" de una tienda online podríamos tener documentos de distintos artículos como laptops, teclados o mouses, cada uno con diferentes datos.

### Bases de datos en MongoDB

Una base de datos en MongoDB es un contenedor lógico para colecciones de documentos. Cada base de datos tiene su propio conjunto de archivos en el sistema de archivos y puede contener múltiples colecciones.

**Operaciones básicas con bases de datos:**

1. **Crear/Seleccionar una base de datos**:
   En MongoDB no hace falta denifir la estructura para crear la base de datos si no que se crea automaticamente cuando se vaya a insertar el primer documento que tenga la base de datos.
   ```javascript
   use miBaseDeDatos
   ```
   Este comando selecciona `miBaseDeDatos` si existe, o la crea al insertar el primer documento.

2. **Listar bases de datos**:
   ```javascript
   show dbs
   ```
   Muestra todas las bases de datos con al menos una colección que contenga datos, y muestra lo que ocupa de espacio en el disco.

3. **Eliminar una base de datos**:
   ```javascript
   use miBaseDeDatos
   db.dropDatabase()
   ```
   Con este comando primero seleccionamos la base de datos que queremos eliminar y después elimina la base de datos actual con todas sus colecciones.

4. **Obtener estadísticas**:
   ```javascript
   db.stats()
   ```
   Proporciona información sobre el tamaño, número de colecciones y otros detalles de la base de datos actual.

### Buenas prácticas
- Usar nombres descriptivos y sin espacios para las base de datos.
- Mantener lógica coherente entre documentos.
- Monitorizar tamaño y uso con `db.stats()`.

---

## Operaciones CRUD

Las operaciones **CRUD** son las que se realizan sobre datos: Crear, Leer, Actualizar y Borrar.

### Creación de documentos (Create)

MongoDB ofrece varios métodos para insertar documentos:

1. **insertOne()**: Inserta un solo documento y si no se le proporciona una id MongoDB le crea una automaticamente.
   ```javascript
   db.productos.insertOne({
     nombre: "Tablet Avanzada",
     precio: 349.99,
     categoria: "Electrónica",
     stock: 50
   })
   ```

2. **insertMany()**: Inserta múltiples documentos en una sola operación, para grandes volúmenes de datos `insertMany()` es más eficiente que múltiples `insertOne()`.
   ```javascript
   db.productos.insertMany([
     {
       nombre: "Teclado Inalámbrico",
       precio: 59.99,
       categoria: "Accesorios",
       stock: 120
     },
     {
       nombre: "Ratón Gaming",
       precio: 45.50,
       categoria: "Accesorios",
       stock: 80,
       caracteristicas: ["RGB", "6 botones programables"]
     }
   ])
   ```

### Consultas (Read)

El método `find()` es la herramienta principal para consultar documentos:

**Consulta básica:**
```javascript
db.productos.find()
```

**Consulta con filtro:**
```javascript
db.productos.find({ categoria: "Electrónica" })
```

**Proyecciones (seleccionar campos específicos):**
```javascript
// Incluir solo nombre y precio, excluyendo _id
db.productos.find(
  { categoria: "Electrónica" },
  { nombre: 1, precio: 1, _id: 0 }
)
```

### Actualización de documentos (Update)

MongoDB proporciona varios métodos para actualizar documentos:

1. **updateOne()**: Actualiza el primer documento que coincide con el filtro
   ```javascript
   db.productos.updateOne(
     { nombre: "Tablet Avanzada" },
     { $set: { precio: 329.99 } }
   )
   ```

2. **updateMany()**: Actualiza todos los documentos que coinciden con el filtro
   ```javascript
   db.productos.updateMany(
     { categoria: "Accesorios" },
     { $set: { envioGratis: true } }
   )
   ```

### Eliminación de documentos (Delete)

1. **deleteOne()**: Elimina el primer documento que coincide con el filtro
   ```javascript
   db.productos.deleteOne({ nombre: "Tablet Obsoleta" })
   ```

2. **deleteMany()**: Elimina todos los documentos que coinciden con el filtro
   ```javascript
   db.productos.deleteMany({ stock: 0 })
   ```

### Operadores de Comparación Comunes

- `$eq`: igual a  
- `$ne`: diferente de  
- `$gt`: mayor que  
- `$gte`: mayor o igual  
- `$lt`: menor que  
- `$lte`: menor o igual  
- `$in`: en una lista  
- `$nin`: no en una lista  

**Ejemplo:**
```javascript
db.productos.find({ "precio": { $gt: 500 } })
```

### Operadores Lógicos

* `$and`: cumple todas las condiciones
* `$or`: cumple al menos una
* `$not`: niega una condición
* `$nor`: no cumple ninguna

**Ejemplo:**
```javascript
db.productos.find({
  $or: [
    { "precio": { $lt: 100 } },
    { "categoria": "Accesorios" }
  ]
})
```

### Operadores con Arreglos

* `$all`: contiene todos los valores dados
* `$size`: tamaño exacto del arreglo
* `$elemMatch`: un elemento cumple múltiples condiciones

**Ejemplo:**
```javascript
db.productos.find({
  "etiquetas": { $all: ["nuevo", "popular"] }
})
```

---

## Índices en MongoDB

Los índices en MongoDB son estructuras especiales que permiten acceder a los datos de forma rápida, sin tener que escanear toda la colección. Facilitan encontrar lo que se busca con eficiencia. 
Por defecto, MongoDB crea un índice en el `campo _id` pero es posible crear índices personalizados para mejorar el rendimiento de consultas específicas. 

Su principal **ventaja** es que aceleran las consultas, sobre todo cuando se filtra o se ordena información, lo que mejora el rendimiento en operaciones de lectura. 
Sin embargo, tienen algunas **desventajas**, aumentan el uso de almacenamiento y generan una pequeña sobrecarga en operaciones de escritura, ya que los índices deben mantenerse actualizados cada vez que se insertan, actualizan o eliminan documentos.

### Tipos de índices

1. **Índice único**: Asegura que no existan valores duplicados en el campo indexado
   ```javascript
   db.usuarios.createIndex({ email: 1 }, { unique: true })
   ```
2. **Índice Simple**: Acelera búsquedas sobre ese campo
   ```javascript
   db.usuarios.createIndex({ email: 1 })
   ```

3. **Índice compuesto**: Acelera consultas que filtran por múltiples campos.
   ```javascript
   db.usuarios.createIndex({ nombre: 1, email: 1 })
   ```

4. **Índice de texto**: Permite búsquedas de texto completo
   ```javascript
   db.productos.createIndex({ descripcion: "text" })
   ```

5. **Índice geoespacial**: Para consultas basadas en ubicación geográfica
   ```javascript
   db.tiendas.createIndex({ ubicacion: "2dsphere" })
   ```

### Gestión de índices

**Crear un índice:**
```javascript
db.usuarios.createIndex({ nombre: 1 })
```

**Listar índices de una colección:**
```javascript
db.usuarios.getIndexes()
```

**Eliminar un índice:**
```javascript
db.usuarios.dropIndex({ nombre: 1 })
```

### Estrategias para un buen diseño de índices

**Cuándo crear índices:**

* Cuando haces consultas frecuentes sobre uno o varios campos.
* Si usas `sort` en un campo de forma regular.
* En consultas complejas con múltiples condiciones (AND, OR, rangos).

**Buenas prácticas al crear índices:**

* No indexar todos los campos, solo los que se usan en consultas comunes.
* Usar `explain()` para analizar cómo se usan los índices.
* Crear índices compuestos si filtras por varios campos al mismo tiempo.
* Evitar duplicar índices que ya están cubiertos por otros compuestos.

---

## Agregaciones en MongoDB

La agregación en MongoDB es un método para procesar y analizar datos de forma avanzada. Permite hacer cosas como sumas, promedios, agrupaciones, filtros y transformaciones, directamente dentro de la base de datos. Es útil cuando necesitas obtener informes o análisis complejos sin tener que mover los datos a tu aplicación. Todo se hace dentro de MongoDB, lo que ahorra tiempo y recursos.


MongoDB usa el **Aggregation Framework**, que se basa en un sistema de **etapas encadenadas** llamado **pipeline** (tubería).
Cada etapa:
- **Recibe** documentos.
- **Los modifica o analiza**.
- **Pasa el resultado** a la siguiente etapa.

### Ejemplo de sintaxis

```javascript
db.coleccion.aggregate([
  { etapa1 },
  { etapa2 },
  ...
])
```


### Etapas comunes

1. **$match**: Filtra documentos (similar a `find()`)
   ```javascript
   db.productos.aggregate([
     { $match: { "precio": { $gt: 500 } } }
   ])
   ```

2. **$group**: Agrupa documentos por campos y calcula agregaciones
   ```javascript
   db.productos.aggregate([
     { $group: {
         _id: "$categoria",precioPromedio: { $avg: "$precio" }
     } }
   ])
   ```

3. **$sort**: Ordena los documentos
   ```javascript
   db.productos.aggregate([
     { $sort: { "precio": -1 } }
   ])
   ```

4. **$project**: Remodela los documentos (similar a proyección en `find()`)
   ```javascript
   db.productos.aggregate([
     { $project:
       {"nombre": 1,
        "precio": 1,
        "precioConIVA": { $multiply: ["$precio", 1.21] }
     } }
   ])

   ```

5. **$unwind**: Descompone arrays en documentos individuales
   ```javascript
   db.productos.aggregate([
     { $unwind: "$etiquetas" }
   ])

   ```

6. **$limit**: Limita los resultados
   ```javascript
   {
     $lookup: {
       from: "inventario",
       localField: "item",
       foreignField: "sku",
       as: "detalles_inventario"
     }
   }
   ```


#### Ejemplo combinando varias etapas
```javascript
  db.productos.aggregate([
    { $match: { "precio": { $gt: 100 } } },
    { $group: { _id: "$categoria", precioPromedio: { $avg: "$precio" } } },
    { $sort: { precioPromedio: -1 } }
  ])
```


### Operadores de expresión en agregaciones

MongoDB proporciona un rico conjunto de operadores para usar en las etapas de agregación:

**Operadores Matemáticos:**

- **`$sum`**: Suma de valores.  
- **`$avg`**: Promedio de valores.  
- **`$min` / `$max`**: Mínimo / Máximo valor.  
- **`$multiply`**: Multiplica dos o más valores.  
- **`$divide`**: Divide un valor por otro.  

**Operadores de Comparación:**

- **`$eq`**: Igualdad.  
- **`$ne`**: Desigualdad.  
- **`$gt` / `$gte`**: Mayor / Mayor o igual.  
- **`$lt` / `$lte`**: Menor / Menor o igual.  

**Operadores de Arreglos:**

- **`$push`**: Inserta valores en un arreglo.  
- **`$addToSet`**: Inserta valores únicos en un arreglo.  
- **`$size`**: Devuelve el tamaño de un arreglo.  


Ejemplo de uso de operadores:
```javascript
db.productos.aggregate([
  { $group: {
    _id: "$categoria",
    totalProductos: { $sum: 1 }
  } }
])
```

---

## Replicación y alta disponibilidad

La replicación en MongoDB crea copias de los datos en varios servidores para asegurar que siempre estén disponibles, incluso si uno falla. Esto se hace con un `Replica Set`, donde un servidor principal controla los datos y los secundarios mantienen copias actualizadas para respaldar al principal si es necesario.

### Replicación en MongoDB

La replicación es el proceso de sincronizar datos en varios servidores para que, si uno falla, los datos sigan disponibles en otros. En MongoDB, esto se logra con los **`Replica Sets`**.
Un Replica Set es un grupo de instancias de MongoDB que comparten los mismos datos y consta de:

- **Nodo primario (Primary):** Recibe todas las operaciones de escritura.  
- **Nodos secundarios (Secondary):** Tienen una copia de los datos del primario.  
- **Nodo árbitro (Arbiter) (opcional):** No almacena datos, pero ayuda a decidir qué nodo será primario en caso de una falla.

#### Ventajas de la replicación

- **Alta disponibilidad:** Si el primario falla, un secundario puede reemplazarlo para mantener la base de datos activa.  
- **Escalabilidad de lectura:** Las lecturas pueden distribuirse entre nodos secundarios para mejorar el rendimiento.  
- **Tolerancia a fallos:** La base de datos sigue funcionando aunque uno de los servidores deje de estar disponible.

#### Componentes del Replica Set

- **Nodo primario:** Único nodo que acepta escrituras y replica los cambios a los secundarios.  
- **Nodo secundario:** Replica datos del primario y puede atender lecturas. Se promueve a primario si este falla.  
- **Árbitro:** Participa en elecciones de nodos para elegir un nuevo primario, pero no guarda datos ni afecta el rendimiento.


### Configuración básica del Replica Set
Para configurar un Replica Set, se deben iniciar varias instancias de MongoDB, preferiblemente en diferentes servidores, y configurarlas para que trabajen juntas como un Replica Set.

1. **Iniciar instancias en modo réplica**:

```bash
mongod --replSet rs0 --port 27017 --dbpath /data/db1
mongod --replSet rs0 --port 27018 --dbpath /data/db2
mongod --replSet rs0 --port 27019 --dbpath /data/db3
```

2. **Iniciar Replica Set** desde mongo shell:

```js
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "localhost:27017" },
    { _id: 1, host: "localhost:27018" },
    { _id: 2, host: "localhost:27019" }
  ]
})
```

3. **Ver estado**:

```js
rs.status()
```

### Elecciones automáticas

Los secundarios votan para elegir un nuevo nodo primario, se hace por seleccion y necesita de una mayoría (mas de la mitad de los nodos), se hace una elección por las siguientes causas:
- El nodo primario falla o se desconecta.
- El nodo primario es degradado manualmente.
- Se añade un nuevo nodo al Replica Set

Si no se puede alcanzar una mayoría el Replica Set entra en un estado de `sin primario` hasta que se restaure el quorum.

### Escalabilidad de lectura

Por defecto las lecturas se hacen desde el primario, pero puedes permitir lectura desde secundarios:

```javascript
db.getMongo().setReadPref("secondaryPreferred")
```

Esto es útil donde se prioriza la disponibilidad y la velocidad sobre la consistencia.

### Ventajas y Desventajas de la replicacion:

**Ventajas:**
- Alta disponibilidad.
- Mejora de rendimiento de lectura.
- Recuperación ante fallos.

**Desventajas:**
- Carga extra en escrituras.
- Lecturas en secundarios pueden estar desactualizadas.

### Consistencia:

* **Fuerte**: leyendo solo del primario.
* **Eventual**: leyendo de secundarios, pueden estar los datos desactualizados.

---

## Sharding (Fragmentación) en MongoDB

### Conceptos fundamentales

El sharding en MongoDB es una forma de dividir una base de datos muy grande en partes más pequeñas para que puedan almacenarse y procesarse en varios servidores al mismo tiempo. En lugar de tener todos los datos en un solo lugar, se reparten en secciones llamadas shards.

Cada shard guarda una porción distinta de los datos, y MongoDB se encarga de organizar dónde se guarda cada cosa y cómo se responde a las consultas, utilizando una clave específica para distribuir la información de forma equilibrada. Esto permite que el sistema funcione más rápido y pueda crecer sin perder rendimiento. 
Es fundamental para:
- **Escalabilidad horizontal:** Permite dividir los datos entre varios servidores, lo que ayuda a manejar grandes volúmenes de información y mejora el rendimiento.
- **Carga equilibrada:** Distribuye el trabajo entre distintos shards, evitando que un solo servidor tenga que hacer todo.
- **Alta disponibilidad:** Los shards pueden estar replicados, por lo que si un servidor falla, los datos siguen estando accesibles.

**Componentes clave:**
1. **Shards**: Cada fragmento que contiene un subconjunto de los datos
2. **mongos**: Router de consultas que dirige las operaciones al shard correcto
3. **Config Servers**: Almacenan metadatos sobre la distribución de datos

### Proceso de sharding

El proceso de sharding comienza cuando lo habilitas en una coleccion, para esto se define una **clave de fragmentación** (shard key), que es el campo que MongoDB usa para repartir los datos de una colección en rangos basados en esa clave.


#### Shard Key (clave de fragmentación)

Es el campo que decide cómo se distribuyen los documentos. Elegirlo bien es clave para que el sistema sea eficiente.

**Buena shard key debe:**
- Tener muchos valores distintos, para que se distribuyan de manera uniforme (alta cardinalidad).
- Repartir los datos de forma equilibrada.
- Usarse en las consultas más frecuentes.

**Ejemplo:**
Si tienes una colección de usuarios, usar `"usuario_id"` como clave puede funcionar bien porque tiene valores únicos y distribuye los datos de manera uniforme.

### Configuración del Sharding

#### 1. Activar sharding en una base de datos

```javascript
sh.enableSharding("mi_base_de_datos")
```

#### 2. Aplicar sharding a una colección

```javascript
sh.shardCollection("mi_base_de_datos.usuarios", { "usuario_id": 1 })
```
Esto hace que los documentos se repartan usando el campo `"usuario_id"`.

#### 3. Ver estado del clúster

```javascript
sh.status()
```
Muestra cómo están distribuidos los datos entre los shards.

### Tipos de Sharding

- **Sharding por Rango:** Divide los documentos según el rango de valores de la clave. Por ejemplo: IDs del 1 al 1000 en un shard, del 1001 al 2000 en otro.
- **Sharding por Hash:** Aplica una función hash a la clave, para repartir los datos más uniformemente, es útil si los valores están muy concentrados.

### Balanceo de Fragmentos

MongoDB usa un **balanceador automático** que mueve datos entre shards cuando alguno se llena más que otros. Este proceso se hace en segundo plano, sin interrumpir el acceso a la base de datos.

### Ventajas y Desventajas del Sharding

- Ventajas:
  * Escala horizontalmente, repartiendo los datos en varios servidores.
  * Evita sobrecargas en un solo servidor.
  * Alta disponibilidad si se combina con réplicas.

- Desventajas:
  * Configuración y gestión más complejas.
  * Consultas ineficientes si no usan bien la clave de fragmentación.
  * Mal diseño de la shard key puede desbalancear el sistema.

### Cuándo Usar o Evitar Sharding

**Usar cuando:**
- La base de datos es muy grande.
- Hay muchas lecturas y escrituras.
- Algunas partes del sistema usan más recursos que otras.

**Evitar cuando:**

- La base de datos es pequeña.
- Las consultas son simples y no necesitan repartirse.
- El tráfico no justifica la complejidad.

### Conclusión

El sharding es útil para repartir datos entre varios servidores y mejorar el rendimiento en bases de datos grandes. Pero requiere una planificación cuidadosa, sobre todo al elegir la shard key. Bien aplicado, permite escalar MongoDB sin perder rendimiento ni disponibilidad.

---

## Seguridad en MongoDB

La seguridad en MongoDB es clave para proteger los datos frente a accesos no autorizados y amenazas. Para ello, ofrece mecanismos como autenticación, permisos por roles, cifrado, auditoría y medidas de red. Todo esto ayuda a mantener los datos seguros y controlados.

### Autenticación

La autenticación permite verificar quién intenta acceder a la base de datos. MongoDB ofrece varios métodos para asegurar que solo los usuarios permitidos puedan entrar.

#### Tipos de autenticación:

**1. SCRAM (el más común)**
Es el método por defecto y usa contraseñas encriptadas.
Pasos básicos:

- Crear un usuario administrador con permisos.
- Activar la autenticación en el archivo `mongod.conf` o usando `--auth`.
- Conectarse como usuario autenticado con el comando `mongo`.

**2. LDAP**
Permite conectar MongoDB con un directorio central de usuarios, útil en empresas.

**3. Kerberos**
Protocolo seguro usado también en entornos corporativos.

**4. Certificados X.509**
Usa certificados digitales para autenticar usuarios y servidores, ideal cuando se necesita alta seguridad.

### Autorización

Una vez que el usuario ha iniciado sesión, la **autorización** define lo que puede o no puede hacer. MongoDB lo gestiona mediante **roles**.

#### Tipos de roles:

**1. Predefinidos**
- `userAdmin`: crear/gestionar usuarios.
- `dbAdmin`: tareas administrativas.
- `read`: solo lectura.
- `readWrite`: lectura y escritura.

**2. Roles por base de datos**
Puedes asignar diferentes permisos según la base de datos.

**Ejemplo:**
```javascript
use mi_base_de_datos
  db.createUser({
    user: "usuario1",
    pwd: "password123",
    roles: [ { role: "readWrite", db: "mi_base_de_datos" } ]
})
```

**3. Roles personalizados**
Permiten definir permisos más detallados, como permitir solo ciertas acciones en una colección específica.

**Ejemplo:**
```javascript
use admin
  db.createRole({
    role: "analista",
    privileges: [
      { resource: { db: "mi_base_de_datos", collection: "" }, actions: [ "find", "listColle"],roles: []
    }]
  })
```

### Cifrado de Datos

MongoDB protege los datos tanto cuando viajan por la red como cuando están almacenados.

#### 1. Cifrado en tránsito (TLS/SSL)

Cifra los datos mientras se transfieren entre clientes y servidores.
Requiere certificados y configuración en `mongod.conf`.

#### 2. Cifrado en reposo

Cifra los archivos en disco. Solo disponible en la versión Enterprise.
Se activa en el archivo de configuración con una clave de cifrado.

#### Seguridad en la Red

Aparte del cifrado, es importante limitar quién puede conectarse a MongoDB.

1. **Firewalls:** Restringen el acceso a los puertos de MongoDB (por defecto, el 27017).
Se puede limitar el acceso por IPs en `mongod.conf`.

2. **Desactivar consola remota:** Evita que alguien acceda desde fuera a la consola de administración.

3. **Red privada (VPC):** En la nube, es mejor alojar MongoDB en una red privada y usar VPN para acceder, evitando exposición pública.

### Auditoría

La versión Enterprise de MongoDB permite registrar todas las acciones de los usuarios, lo que ayuda a detectar accesos indebidos y cumplir con normativas como GDPR o HIPAA.

**Ejemplos de eventos auditados:**

- Lecturas/escrituras.
- Creación/eliminación de bases de datos.
- Cambios en usuarios o roles.

Se configura en el archivo con `auditLog` y se guarda en un archivo `.json`.

### Buenas Prácticas de Seguridad

1. Siempre activar autenticación y permisos.
2. Usar TLS/SSL para cifrar conexiones.
3. Proteger la red con firewalls o VPC.
4. Dar a los usuarios solo los permisos necesarios.
5. Revisar periódicamente los registros de auditoría.

### Conclusión

Proteger una base de datos MongoDB implica asegurar el acceso, cifrar los datos y monitorear las actividades. Si se aplican estas medidas, se puede mantener un sistema robusto y seguro ante amenazas internas y externas.

---

# Conclusión General

MongoDB es una base de datos NoSQL poderosa y flexible, diseñada para manejar grandes volúmenes de datos estructurados y no estructurados con eficiencia. Su modelo basado en documentos, escalabilidad horizontal y soporte para operaciones avanzadas como agregaciones y transacciones multi-documento lo hacen ideal para aplicaciones modernas que requieren alto rendimiento y adaptabilidad. Además, características como la replicación, el sharding y robustas medidas de seguridad garantizan disponibilidad, distribución equilibrada de cargas y protección de datos. MongoDB combina la simplicidad de JSON con la potencia de un sistema distribuido, ofreciendo una solución versátil para desarrolladores y empresas en entornos dinámicos y exigentes.
