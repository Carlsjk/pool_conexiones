# Proyecto: Sistema de Gestión con PostgreSQL y Python

Este proyecto es una implementación de un sistema basado en Python que utiliza PostgreSQL como base de datos. Incluye funcionalidades para manejar conexiones, registrar eventos, y realizar operaciones CRUD utilizando el patrón DAO (Data Access Object).

---

## Estructura del Proyecto

📁 proyecto ├── conexion.py # Clase para gestionar conexiones a la base de datos ├── persona.py # Clase que representa la entidad Persona ├── persona_dao.py # Clase DAO para las operaciones CRUD ├── logger_base.py # Configuración del sistema de logging ├── capa_datos.log # Archivo donde se registran los logs └── README.md # Documentación del proyecto

yaml
Copiar
Editar

---

## Requisitos Previos

1. **Python 3.8 o superior**.
2. **PostgreSQL** instalado y configurado.
3. Una base de datos PostgreSQL con la siguiente tabla:

```sql
CREATE TABLE persona (
    id_persona SERIAL PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
Instalar el módulo psycopg2 para manejar la conexión a PostgreSQL:
bash
Copiar
Editar
pip install psycopg2
Clases y Funcionalidades
1. Clase Conexion (Gestión de Conexiones)
Archivo: conexion.py

Esta clase se encarga de gestionar las conexiones a la base de datos mediante un pool (conjunto de conexiones reutilizables).

Principales Métodos:
obtenerPool(): Crea un pool de conexiones si aún no existe.
obtenerConexion(): Obtiene una conexión desde el pool.
liberarConexion(conexion): Retorna una conexión al pool.
cerrarConexiones(): Cierra todas las conexiones del pool.
Ejemplo de uso:

python
Copiar
Editar
from conexion import Conexion

conexion1 = Conexion.obtenerConexion()
Conexion.liberarConexion(conexion1)
2. Clase Persona (Entidad)
Archivo: persona.py

Esta clase representa la entidad Persona y modela los datos que interactúan con la base de datos.

Atributos:
id_persona
nombre
apellido
email
Métodos:
Métodos de acceso (getter) y modificación (setter) para cada atributo.
Sobrecarga del método __str__ para facilitar la impresión.
Ejemplo de creación de una instancia:

python
Copiar
Editar
from persona import Persona

persona1 = Persona(1, 'Juan', 'Perez', 'jperez@mail.com')
print(persona1)
3. Clase PersonaDAO (Operaciones CRUD)
Archivo: persona_dao.py

Esta clase implementa el patrón DAO (Data Access Object) para realizar operaciones CRUD en la tabla persona.

Principales Métodos:
seleccionar(): Recupera todos los registros.
insertar(persona): Inserta un nuevo registro.
actualizar(persona): Actualiza un registro existente.
eliminar(persona): Elimina un registro.
Ejemplo de uso:

python
Copiar
Editar
from persona_dao import PersonaDAO
from persona import Persona

# Insertar una nueva persona
nueva_persona = Persona(nombre='Pedro', apellido='Gomez', email='pgomez@mail.com')
PersonaDAO.insertar(nueva_persona)

# Obtener todas las personas
personas = PersonaDAO.seleccionar()
for persona in personas:
    print(persona)
4. Logger (Registro de Eventos)
Archivo: logger_base.py

El sistema de logging permite registrar eventos en un archivo (capa_datos.log) y en la consola. Está configurado para mostrar los niveles de log: DEBUG, INFO, WARNING, ERROR, y CRITICAL.

Configuración:
python
Copiar
Editar
import logging as log

log.basicConfig(
    level=log.DEBUG,
    format='%(asctime)s: %(levelname)s [%(filename)s:%(lineno)s] %(message)s',
    datefmt='%I:%M:%S %p',
    handlers=[
        log.FileHandler('capa_datos.log'),
        log.StreamHandler()
    ]
)
Ejemplo de uso:

python
Copiar
Editar
from logger_base import log

log.debug('Mensaje de depuración')
log.info('Mensaje de información')
Ejecución del Proyecto
Configura los parámetros de conexión en la clase Conexion (conexion.py):

Nombre de la base de datos.
Usuario y contraseña.
Puerto y host.
Ejecuta los bloques de prueba incluidos en los archivos:

persona_dao.py
persona.py
conexion.py
Verifica los resultados en la consola y en el archivo capa_datos.log.

Ejemplo Completo
El siguiente ejemplo realiza las operaciones CRUD completas:

python
Copiar
Editar
from persona_dao import PersonaDAO
from persona import Persona

# Insertar
persona1 = Persona(nombre='Carlos', apellido='Martinez', email='cmartinez@mail.com')
PersonaDAO.insertar(persona1)

# Seleccionar
personas = PersonaDAO.seleccionar()
for persona in personas:
    print(persona)

# Actualizar
persona2 = Persona(1, 'Carlos', 'Gomez', 'cgomez@mail.com')
PersonaDAO.actualizar(persona2)

# Eliminar
persona3 = Persona(id_persona=2)
PersonaDAO.eliminar(persona3)
Mejoras Futuras
Manejo de excepciones más específicas.
Implementación de pruebas unitarias para validar la funcionalidad.
Configuración dinámica de los parámetros de conexión mediante un archivo .env.

### Clase CursorDelPool
La clase CursorDelPool facilita la gestión de conexiones a la base de datos utilizando el patrón de contexto (with). Esta clase asegura:

- Obtención automática de una conexión y cursor: Al entrar en el contexto.
- Commit o rollback automático: Dependiendo de si se produce una excepción dentro del bloque.
- Liberación de la conexión: Cuando el bloque finaliza.

#### Ejemplo de uso

```python
from conexion import Conexion
from cursor_del_pool import CursorDelPool

if __name__ == '__main__':
    with CursorDelPool() as cursor:
        cursor.execute('SELECT * FROM persona')
        print(cursor.fetchall())

---
### Modificaciones en la Clase PersonaDAO
Se han actualizado los métodos de la clase PersonaDAO para utilizar el CursorDelPool con bloques `with`. Esto mejora la gestión de conexiones y asegura que los recursos se liberen correctamente, incluso en caso de excepciones.

#### Métodos disponibles

**seleccionar(cls)**  
Recupera todos los registros de la tabla persona.

```python
personas = PersonaDAO.seleccionar()
for persona in personas:
    print(persona)

