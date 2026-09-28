# Analizador de Facturas Ecuador

Aplicación web para importar, organizar y analizar facturas electrónicas en formato XML emitidas por el SRI de Ecuador.

Las facturas se almacenan en una base de datos PostgreSQL junto con su XML original, evitando registros duplicados y conservando el historial de clasificación. Desde la aplicación puedes consultar tus comprobantes, administrar reglas de clasificación de gastos por proveedor, procesar automáticamente los gastos y establecer objetivos anuales de deducción.

Su propósito es facilitar el seguimiento de gastos deducibles y ayudarte a medir el avance hacia tus metas tributarias durante el año.

# Instalación con Docker

Este paquete instala la aplicación Analizador de Facturas usando imágenes públicas de Docker Hub.

Incluye el frontend web, el API y una base de datos PostgreSQL persistente. Es compatible con equipos `amd64` y `arm64` que puedan ejecutar Docker Compose, incluidos servidores, computadores personales y numerosos dispositivos NAS.

## Requisitos

- Docker Engine con el complemento Docker Compose, o Docker Desktop.
- Conexión a Internet durante la primera instalación y las actualizaciones de versión.
- Un puerto disponible para la página web; el predeterminado es `8080`.

## Instalación

1. Descarga o copia el contenido de este repositorio a una carpeta.
2. Copia `.env.example` como `.env`.
3. Abre `.env` y sustituye `POSTGRES_PASSWORD` por una contraseña segura para tu base de datos.
4. Si el puerto `8080` está ocupado, cambia `WEB_PORT`.
5. Desde esa carpeta, ejecuta:

```bash
docker compose pull
docker compose up -d --wait
```

Abre `http://localhost:8080` en el mismo equipo o `http://IP_DEL_EQUIPO:8080` desde la red local. Sustituye `8080` si configuraste otro puerto.

La primera vez que accedas, la aplicación te pedirá crear el usuario administrador.

El API espera a que PostgreSQL esté disponible, aplica automáticamente las migraciones pendientes y después inicia el servidor. El API y PostgreSQL no publican puertos; solamente el frontend es accesible desde el equipo anfitrión.

## Comandos habituales

```bash
# Consultar el estado
docker compose ps

# Consultar los registros
docker compose logs --tail=100 frontend api db

# Detener la aplicación conservando los datos
docker compose stop

# Volver a iniciarla
docker compose up -d --wait
```

## HTTPS

Para publicar la aplicación en Internet, utiliza HTTPS mediante un proxy inverso y establece `AUTH_COOKIE_SECURE=true` en `.env`. Consulta [Configuración de un proxy inverso](docs/proxy-inverso.md).

## Actualizaciones y versiones

Consulta [Actualizar o recuperar una versión](docs/actualizacion.md). Antes de una actualización importante, crea un [respaldo de la base de datos](docs/respaldos.md).

## Persistencia y seguridad

Los datos se guardan en el volumen indicado por `POSTGRES_VOLUME`. `docker compose down` elimina los contenedores pero conserva ese volumen; `docker compose down -v` también elimina los datos y no debe utilizarse en una instalación con información que quieras conservar.

El archivo `.env` contiene la contraseña de PostgreSQL y está excluido de Git. No lo publiques ni lo compartas.

## Licencia

Copyright 2026 Roger Vega.

Los archivos de este repositorio se distribuyen bajo la [Apache License 2.0](LICENSE). Consulta también el archivo de [atribuciones](NOTICE).

Las imágenes Docker utilizadas corresponden al proyecto Analizador de Facturas y se distribuyen conforme a la licencia y los avisos de su proyecto de origen. Las dependencias de terceros conservan sus respectivas licencias.
