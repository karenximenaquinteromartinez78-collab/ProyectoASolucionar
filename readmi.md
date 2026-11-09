ACTIVIDAD 1. MODELADO DE DATOS, NORMALIZACIÓN Y PRAGMA

1. ¿Qué son las directivas PRAGMA y para qué sirven?

Las directivas PRAGMA son instrucciones especiales de SQLite que permiten configurar y consultar el funcionamiento de la base de datos. En este proyecto se utilizan para mejorar la seguridad, el rendimiento y el manejo de la información.

- "foreign_keys = ON": activa la verificación de las claves foráneas.
- "journal_mode = WAL": permite que las lecturas y escrituras funcionen de una manera más eficiente.
- "synchronous = NORMAL": ofrece un equilibrio entre rendimiento y seguridad al guardar los cambios.

2. Diagrama Entidad-Relación (ER)

En el proyecto existen dos tablas principales:

- Usuarios: almacena la información de los entrenadores. Su clave primaria es "id".
- Capturas: almacena los Pokémon capturados por cada entrenador. Su clave primaria es "id" y tiene una clave foránea llamada "usuario_id".

La relación entre las tablas es de uno a muchos (1:N), porque un usuario puede tener muchos Pokémon capturados, pero cada captura pertenece a un solo usuario.

Representación:

USUARIOS (id) 1 ───────── N CAPTURAS (usuario_id)

3. ¿Por qué guardamos la imagen del Pokémon como TEXT en lugar de BLOB?

Porque guardamos la dirección o URL de la imagen y no el archivo completo. Esto permite ocupar menos espacio en la base de datos y facilita la carga de las imágenes desde Internet.

4. ¿Cómo ayuda JSON a guardar nuevos atributos sin utilizar ALTER TABLE?

El campo "datos_json" permite almacenar información flexible del Pokémon, como sus habilidades, movimientos y características. Así podemos agregar nuevos atributos sin tener que modificar la estructura de la tabla cada vez que necesitemos guardar información diferente.

ACTIVIDAD 2. AUTENTICACIÓN POR TOKEN Y ARQUITECTURA CLIENTE-SERVIDOR

1. ¿Qué es la arquitectura Cliente-Servidor?

Es una forma de organizar una aplicación donde el cliente, en este caso la aplicación web, envía solicitudes al servidor. El servidor procesa la información, consulta la base de datos SQLite y devuelve una respuesta al cliente.

2. ¿Qué es un token de autenticación?

Es una identificación digital que recibe el usuario cuando inicia sesión correctamente. Este token permite reconocer al entrenador y verificar que tiene permiso para acceder a sus datos.

3. ¿Cómo funciona el middleware autenticarToken?

El middleware verifica que el usuario tenga un token válido antes de permitirle acceder a las funciones protegidas. De esta manera, cada entrenador solamente puede consultar, modificar o eliminar sus propios Pokémon capturados.

4. ¿Qué es la autorización y el aislamiento de datos?

La autorización permite controlar qué acciones puede realizar cada usuario. El aislamiento de datos evita que un entrenador pueda acceder a la información privada de otro entrenador.

Evidencia práctica pendiente: el taller solicita registrar dos entrenadores y comprobar sus tokens. En nuestro proyecto apareció un error al intentar registrarse, por lo que todavía no podemos afirmar que esta parte esté completada.

ACTIVIDAD 3. OPERACIONES CRUD, JSON_EXTRACT Y PAGINACIÓN

1. ¿Qué significa CRUD?

CRUD representa las cuatro operaciones principales que se realizan en una base de datos:

- Create: crear o insertar información.
- Read: consultar información.
- Update: actualizar información.
- Delete: eliminar información.

2. ¿Para qué sirve SELECT + json_extract()?

Permite consultar información almacenada en la base de datos y extraer datos específicos de un documento JSON. En este proyecto se utiliza para obtener características del Pokémon, como su tipo principal y sus puntos de vida (HP).

3. ¿Qué es la paginación y para qué sirven LIMIT y OFFSET?

La paginación permite mostrar los registros por grupos o páginas, en lugar de mostrarlos todos al mismo tiempo.

- "LIMIT 4": muestra máximo cuatro registros.
- "OFFSET 0": comienza desde el primer registro.
- "OFFSET 4": comienza después de los primeros cuatro registros.

Esto permite organizar mejor la información y evitar cargar demasiados datos al mismo tiempo.

4. ¿Para qué sirve UPDATE?

UPDATE permite modificar información que ya existe en la base de datos. En el proyecto se utiliza cuando marcamos un Pokémon como favorito o cambiamos su apodo y sus notas de entrenamiento.

5. ¿Para qué sirve DELETE?

DELETE permite eliminar registros de la base de datos. En el proyecto se utiliza cuando presionamos el botón Liberar para quitar un Pokémon del equipo de un entrenador.

6. ¿Por qué se utilizan parámetros vinculados (?) en las consultas SQL?

Porque permiten enviar los datos por separado de la instrucción SQL. Esto ayuda a evitar ataques de inyección SQL y hace que las consultas sean más seguras.

ACTIVIDAD 4. MODO SIN CONEXIÓN Y TRANSACCIONES ACID

1. ¿Qué es el modo sin conexión (Offline Queue)?

Es una función que permite utilizar algunas características de la aplicación cuando no hay conexión a Internet. Las operaciones realizadas se guardan temporalmente en una cola local para sincronizarlas cuando regrese la conexión.

2. ¿Qué sucede cuando capturamos Pokémon sin conexión?

Las capturas quedan almacenadas temporalmente en el dispositivo y aparecen como operaciones pendientes. Cuando vuelve la conexión, se pueden enviar al servidor para guardarlas en la base de datos.

3. ¿Qué sucede cuando presionamos Sincronizar Equipo Ahora?

La aplicación envía las capturas pendientes al servidor, donde se procesan y se guardan en SQLite mediante una transacción.

4. ¿Qué significa BEGIN TRANSACTION y COMMIT?

- "BEGIN TRANSACTION": inicia una transacción.
- "COMMIT": confirma los cambios y guarda las operaciones realizadas.

5. ¿Qué ocurriría si falla el registro número 2 durante la sincronización?

Gracias a la sentencia ROLLBACK, se pueden deshacer los cambios realizados durante la transacción. Así se evita que se guarde solamente una parte de la información.

6. ¿Qué propiedad de ACID representa este funcionamiento?

Representa la atomicidad, porque garantiza que una transacción se complete totalmente o que sus cambios se deshagan si ocurre un error.

ACTIVIDAD 5. CONSULTAS DE AGREGACIÓN E INDEXACIÓN

1. ¿Qué es AVG(peso_kg)?

Es una función de agregación que calcula el peso promedio de los Pokémon almacenados en la base de datos.

2. ¿Qué hace MAX(CAST(json_extract(...) AS INTEGER))?

Extrae los puntos de vida (HP) desde el documento JSON, los convierte a números enteros y obtiene el valor máximo.

3. ¿Para qué sirve GROUP BY tipo?

Permite agrupar los Pokémon según su tipo y conocer cuántos registros existen de cada uno.

4. ¿Qué es un índice B-Tree?

Es una estructura que permite organizar y localizar información de manera más eficiente dentro de una base de datos.

5. ¿Qué significa SEARCH capturas USING INDEX?

Significa que SQLite utiliza un índice para encontrar los registros que necesita, sin tener que recorrer toda la tabla.

6. ¿Qué significa SCAN capturas?

Significa que SQLite realiza un recorrido completo de la tabla para encontrar los registros que cumplen la condición de búsqueda.

7. ¿Cuál es la diferencia entre buscar con índice y sin índice?

Con índice, la búsqueda puede ser más rápida porque SQLite utiliza una estructura organizada para localizar los datos. Sin índice, debe revisar los registros de la tabla hasta encontrar los que necesita.

8. ¿Por qué no debemos crear índices en absolutamente todas las columnas?

Porque los índices ocupan espacio adicional y pueden hacer más lentas las operaciones de insertar, actualizar y eliminar información. Por eso, es mejor crear índices solamente en las columnas que se utilizan frecuentemente para realizar búsquedas.

9. Conclusión de la actividad 5

Los índices B-Tree ayudan a mejorar el rendimiento de las consultas en SQLite. Al comparar los resultados de EXPLAIN QUERY PLAN, se observa que con índice se utiliza SEARCH, mientras que sin índice se realiza un SCAN de toda la tabla. Esto demuestra la importancia de utilizar índices en las columnas adecuadas.