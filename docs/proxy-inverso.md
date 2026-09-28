# Configuración de un proxy inverso

Un proxy inverso proporciona un dominio y HTTPS delante del puerto publicado por el frontend. Puede utilizarse Nginx, Caddy, Traefik, el proxy de un NAS o un servicio equivalente.

## Configuración de la aplicación

En `.env`:

```dotenv
WEB_BIND_ADDRESS=127.0.0.1
WEB_PORT=8080
AUTH_COOKIE_SECURE=true
```

Usa `127.0.0.1` cuando el proxy se ejecute en el mismo equipo. Si el proxy está en otro equipo, publica el puerto en una interfaz accesible y limita el acceso mediante el cortafuegos.

Después de modificar `.env`, recrea los contenedores:

```bash
docker compose up -d --force-recreate --wait
```

## Ejemplo con Nginx

```nginx
server {
    listen 443 ssl;
    server_name facturas.ejemplo.com;

    ssl_certificate /ruta/al/certificado.pem;
    ssl_certificate_key /ruta/a/la/clave.pem;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_pass http://127.0.0.1:8080;
    }
}
```

El certificado, la renovación automática y la configuración del cortafuegos dependen del proxy o proveedor utilizado. No expongas directamente la base de datos ni el API a Internet.
