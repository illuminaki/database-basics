# Repositorio Teórico: Introducción a Bases de Datos

Bienvenido/a a este repositorio pensado para dar tus primeros pasos en el mundo de las **bases de datos**. Aquí encontrarás teoría, ejemplos prácticos y ejercicios que te ayudarán a dominar los conceptos esenciales antes de escribir código SQL o trabajar con un motor de base de datos real.

## ¿A quién va dirigido?

Coders nuevos o personas en transición al desarrollo de software que necesitan una base sólida sobre bases de datos.

## ¿Qué aprenderás?

1. Conceptos fundamentales: qué es una base de datos y por qué la necesitas.
2. Diferencias entre bases de datos **relacionales** y **NoSQL**.
3. Principios de diseño y normalización de datos.
4. Sintaxis básica de **SQL** y operaciones CRUD.
5. Propiedades **ACID** y transacciones.
6. Cuándo y por qué elegir NoSQL.

## Estructura del repositorio

| Archivo | Descripción |
|---------|-------------|
| `docker-compose.yml` | Configuración de contenedores para PostgreSQL y pgAdmin. |
| `.env` | Variables de entorno para la configuración de PostgreSQL y pgAdmin. |
| `README.md` | Visión general y guía de uso del repositorio. |
| `teoria.md` | Contenido teórico completo con ejemplos y diagramas. |
| `ejercicios.md` | Ejercicios de reforzamiento y auto-evaluación. |

## Cómo usar este repositorio

1. **Lectura secuencial**: sigue el orden de las secciones en `teoria.md`.
2. **Práctica activa**: a medida que avances, intenta responder a los ejemplos y ejecutar las consultas en tu propio gestor de base de datos.
3. **Auto-evaluación**: completa los retos y preguntas en `ejercicios.md`.
4. **Discusión**: compara tus respuestas o comparte dudas con tu grupo de estudio.

> "La mejor manera de aprender es haciendo" — ¡manos a la obra!

---

## Entorno Docker (PostgreSQL + pgAdmin)

Este repositorio incluye un `docker-compose.yml` que levanta:

* **PostgreSQL 15** (`db`)
* **pgAdmin 4** (`pgadmin`) en `http://localhost:5050`

### 1. Comandos básicos

| Acción | Docker Compose v2 (`docker compose`) | Docker Compose v1 (`docker-compose`) |
|--------|--------------------------------------|--------------------------------------|
| Descargar imágenes | `docker compose pull` | `docker-compose pull` |
| Levantar contenedores **en segundo plano** | `docker compose up -d` | `docker-compose up -d` |
| Levantar contenedores **en primer plano** | `docker compose up` | `docker-compose up` |
| Ver logs en vivo | `docker compose logs -f` | `docker-compose logs -f` |
| Detener y eliminar contenedores | `docker compose down` | `docker-compose down` |

> **Nota sobre `-d`**: la opción `-d` (detached) inicia los contenedores en segundo plano y devuelve de inmediato el control a la consola. Sin `-d`, la consola se queda mostrando los logs en tiempo real hasta que presiones `Ctrl+C`.

### 2. Variables de entorno

Modifica el archivo `.env` para cambiar usuario, contraseña o puertos antes de ejecutar los comandos anteriores.

### 3. Conectar desde pgAdmin

1. Inicia sesión con las credenciales de `.env` (`PGADMIN_DEFAULT_EMAIL`, `PGADMIN_DEFAULT_PASSWORD`).
2. Crea un nuevo servidor con:
   * **Host**: `db`
   * **Puerto**: `5432`
   * **Usuario/Contraseña**: según `.env`
   * **Base de datos**: `demo_db`

¡Listo! Ya puedes practicar las consultas SQL del repositorio en un entorno real.

