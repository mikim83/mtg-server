# mtg-server

Servidor [Cockatrice](https://github.com/Cockatrice/Cockatrice) (Servatrice + MariaDB) con Docker Compose, para `mtg.dob-world.com`.

## Puertos

| Puerto | Uso |
|--------|-----|
| 4747/TCP | Cliente de escritorio Cockatrice |
| 4748/TCP | WebSocket (Webatrice) |

El 4747 no es HTTP: no pasa por el proxy de Cloudflare. El registro DNS debe ser un A sin proxy (nube gris) y el puerto abierto en el router.

## Puesta en marcha

```bash
cp .env.example .env                                         # poner contraseñas
cp servatrice/servatrice.ini.example servatrice/servatrice.ini
# poner en servatrice.ini la misma DB_PASSWORD que en .env
docker compose up -d
docker compose logs -f servatrice
```

`.env` y `servatrice/servatrice.ini` están en `.gitignore` (contienen la contraseña de la BD).

## Esquema de la base de datos

`db-init/servatrice.sql` debe corresponder a la versión de esquema que soporta la imagen `ghcr.io/cockatrice/servatrice`. Ahora mismo es la versión 37 (commit `073ec29c4d` de Cockatrice). Si se actualiza la imagen, hay que actualizar el esquema o migrar la BD; si no coinciden, Servatrice se cierra al arrancar.

## Usuarios

El registro está abierto: desde el cliente, *Cuenta → Registrar*. Para hacer administrador a un usuario, poner `admin=1` en la tabla `cockatrice_users`.
