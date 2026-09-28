# Actualizar o recuperar una versión

## Instalar la versión más reciente

Con `IMAGE_TAG=latest` en `.env`, ejecuta desde la carpeta del proyecto:

```bash
docker compose pull
docker compose up -d --force-recreate --wait
```

Docker descarga las imágenes nuevas y reemplaza los contenedores. El volumen de PostgreSQL se conserva y el API aplica las migraciones pendientes antes de iniciar.

## Usar una versión concreta

Las publicaciones estables pueden identificarse con versiones como `1.0.0`. Cada compilación también se publica con el SHA completo del commit.

Cambia `IMAGE_TAG` en `.env`, por ejemplo:

```dotenv
IMAGE_TAG=1.0.0
```

Después ejecuta nuevamente los comandos de actualización. Fijar una versión evita que una recreación posterior instale cambios inesperados.

## Recuperar una versión anterior

1. Crea un respaldo de la base de datos.
2. Cambia `IMAGE_TAG` por la versión o SHA anterior.
3. Ejecuta `docker compose pull` y `docker compose up -d --force-recreate --wait`.

Una versión antigua de la aplicación puede no ser compatible con migraciones de base de datos más recientes. Si la actualización modificó el esquema, restaura también el respaldo creado antes de actualizar.
