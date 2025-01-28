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


