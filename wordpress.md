# Wordpress


## Hacer una copia de Wordpress en local

1 - Instalar el plugin WP Migrate DB.

2 - En la página de administración (/wp-admin) ir a *Herramientas -> Migrate DB*.

3 - En el apartado *Find* indicar en el primer cuadro de texto de la columna *Replace* la url del servidor local. Ej: *http://localhost*. Y en el cuadro de texto que hay justo debajo poner el directorio donde queremos que se exporte. Este directorio debe existir. Ej: */home/desarrollo/miweb*.

4 - Opcionalmente se puede guardar el perfil si se va a exportar más veces marcando la casilla *Save migration profile*.

5 - Pulsar el botón *Export*. Comenzará la exportación de los datos y se descargar un archivo comprimido.

6 - Descomprimir el archivo comprimido del código wordpress del sitio en el directorio local.

7 - Arrancar Apache y MySQL.

8 - Ir a phpAdmin y crear la base de datos. Esta base de datos se puede llamar de cualquier forma.

9 - Ir a la pestaña *Importar* y pulsar el botón *Seleccionar archivo* en *Archivo a importar*.

10 - Ir al directorio local del sitio wordpress y editar el archivo *wp-config.php*. Modificar las variables *DB_NAME*, *DB_USER*, *DB_PASSWORD* y *DB_HOST*.

11 - Ir a la página de administración */wp-config* -> *Ajustes* -> *Enlaces permanentes* -> *Numérico* y pulsar el botón *Guardar cambios*.


## Mostrar errores en Wordpress

Para ver los errores en las páginas hay que editar el archivo *wp-config.php* y modificar la variable *WP_DEBUG*:

```php
define('WP_DEBUG', true);
```

Más información sobre depuración en Wordpress [aquí](https://codex.wordpress.org/Debugging_in_WordPress).