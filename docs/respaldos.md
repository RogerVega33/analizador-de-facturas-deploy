# Respaldar y restaurar base de datos PostgreSQL

Ejecuta los comandos desde la carpeta que contiene `compose.yaml` y `.env`.

## Crear un respaldo

En Linux o macOS:

```bash
mkdir -p backups
docker compose exec -T db sh -c 'pg_dump -U "$POSTGRES_USER" -d "$POSTGRES_DB" -Fc' > "backups/analizador-$(date +%Y%m%d-%H%M%S).dump"
```

En PowerShell:

```powershell
New-Item -ItemType Directory -Force backups | Out-Null
docker compose exec -T db sh -c 'pg_dump -U "$POSTGRES_USER" -d "$POSTGRES_DB" -Fc' > "backups/analizador-$(Get-Date -Format yyyyMMdd-HHmmss).dump"
```

Comprueba que el archivo resultante exista y no esté vacío. Guarda una copia fuera del equipo que ejecuta Docker.

## Restaurar un respaldo

La restauración reemplaza el contenido actual de la base. Conserva una copia antes de continuar.

```bash
docker compose stop frontend api
docker compose exec -T db sh -c 'pg_restore --clean --if-exists --no-owner -U "$POSTGRES_USER" -d "$POSTGRES_DB"' < backups/ARCHIVO.dump
docker compose up -d --wait
```

En PowerShell, la redirección de archivos binarios puede depender de la versión. Si produce un respaldo alterado, ejecuta los comandos desde WSL o utiliza una herramienta de PostgreSQL instalada localmente.
