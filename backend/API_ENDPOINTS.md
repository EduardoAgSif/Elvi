# API REST - endpoints

Base URL local: `http://localhost/backend`. Todas las rutas responden JSON. Para solicitudes con cuerpo, enviar `Content-Type: application/json` y usar JSON raw. Los ejemplos de autenticacion usan el usuario de prueba indicado en el README; no uses credenciales reales en capturas.

En Android, `localhost` apunta al dispositivo y no a la computadora: usa `10.0.2.2` en el emulador Android o la IP local de la computadora en un dispositivo fisico.

## Productos: `api.php`

La aplicacion consume el catalogo con `GET` y expone las operaciones CRUD remotas en `ProductService`. Las operaciones de modificacion pueden probarse desde Postman.

| Metodo | URL | Cuerpo | Resultado correcto |
|---|---|---|---|
| GET | `/api.php` | - | `200`, `{ "status":"success", "total":N, "data":[...] }` |
| GET | `/api.php?id=1` | - | `200`, `{ "status":"success", "data":{...} }` |
| POST | `/api.php` | `{"name":"Producto de prueba","description":"Prueba","price":125.5,"stock":10}` | `201`, `{ "status":"success", "id":N, ... }` |
| PUT | `/api.php?id=ID` | `{"name":"Producto actualizado","description":"Actualizacion completa","price":135.5,"stock":8}` | `200`, estado `success` |
| PATCH | `/api.php?id=ID` | `{"stock":6}` | `200`, estado `success` |
| DELETE | `/api.php?id=ID` | - | `200`, estado `success` |

`name` y `price` son obligatorios para POST y PUT; `price` y `stock` no pueden ser negativos, y `stock` debe ser entero. PATCH acepta uno o mas de estos campos: `name`, `description`, `price`, `stock`. Usa un producto de prueba para DELETE: la eliminacion es permanente.

Errores comunes: `400` entrada o ID invalido/ausente, `404` recurso inexistente, `405` metodo no soportado y `500` error interno o de conexion a MySQL. La API devuelve `status: "error"` y un `message`.

## Autenticacion

| Metodo | URL | Cuerpo JSON | Respuestas |
|---|---|---|---|
| POST | `/signup.php` | `{"name":"Nombre de prueba","email":"prueba@example.com","password":"ClaveTemporal123"}` | `201` creado; `400` entrada invalida; `409` correo existente |
| POST | `/login.php` | `{"email":"prueba@example.com","password":"ClaveTemporal123"}` | `200` autenticado; `401` credenciales incorrectas; `400` faltan campos |

Una respuesta correcta de login incluye `user` sin la contrasena. Signup tambien devuelve el usuario creado sin la contrasena. No incluyas contrasenas en la bitacora ni en evidencia publica.

## Ejemplo de secuencia de prueba

1. Importa `backend/schema.sql`; inicia Apache y MySQL.
2. Ejecuta GET para confirmar el catalogo y copia el ID de un producto de prueba creado mediante POST.
3. Usa PUT para reemplazarlo, PATCH para cambiar solo `stock`, y GET por ID para verificar el valor.
4. Elimina solo el producto temporal con DELETE.
5. Prueba las respuestas de error con un ID inexistente; no uses IDs de productos seed para DELETE.

