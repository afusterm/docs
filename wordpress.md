# Wordpress


## Meter código php en una página

1 - Para poder programar en php una página de WordPress primero hay que crear un tema. Para eso hay que ir al directorio *wp-content/themes* y crear un directorio con el nombre del tema que se desee.

2 - Dentro del nuevo directorio crear el fichero *style.css* que tendrá el siguiente formato:

```
/*
Theme Name:     Nombre del tema
Theme URI:      la web donde se encuentra el tema. Puede estar en blanco.
Description:    La descripción que se quiera dar.
Author:         Nombre del autor.
Author URI:     
Template:       Nombre del tema del que se quiera heredar.
Version:        Versión. Ej: 0.1
*/

@import url("../tema_del_que_se_hereda/style.css");

```

3 - Ir a la web de administración. Sección *Appearance* -> *Themes* y seleccionar el nuevo tema.

4 - Ir a la sección *Pages* y crear una nueva página.

5 - Una vez estemos en la pantalla de creación de la página en la parte derecha, en la sección *Page Attributes* seleccionar *Default Template* del desplegable *Template*.

6 - Crear en el directorio del tema un fichero que se llamará *page-nombre-de-la-pagina-separada-por-guiones.php*. Por ejemplo, si la página se llama *Solicite presupuesto* el fichero php se llamará *page-solicite-presupuesto.php*.

7 - La página puede tener el siguente formato:

```
<?php get_header(); ?>

<div id="primary" class="site-content">
    <div id="content" role="main">
	    <?php while ( have_posts() ) : the_post(); ?>
	
	        <article id="post-<?php the_ID(); ?>" <?php post_class(); ?>>
	
	            <header class="entry-header">
	            <h1 class="entry-title"><?php the_title(); ?></h1>
	            </header>
	
	            <div class="entry-content">
	                <?php the_content(); ?>
	
	                <form action="<?php the_permalink() ?>" method="post">
	                    ...
	                </form>
	            </div>
	        </article>
	    <?php endwhile; ?>
    </div>
</div>
<?php get_footer(); ?>

```


## Hacer una copia de Wordpress en local

1 - Instalar el plugin WP Migrate DB.

2 - En la página de administración (/wp-admin) ir a *Herramientas -> Migrate DB*.

3 - En el apartado *Find* indicar en el primer cuadro de texto de la columna *Replace* la url del servidor local. Ej: *http://localhost*. Y en el cuadro de texto que hay justo debajo poner el directorio donde queremos que se exporte. Este directorio debe existir. Ej: */home/desarrollo/miweb*.

4 - Opcionalmente se puede guardar el perfil si se va a exportar más veces marcando la casilla *Save migration profile*.

5 - Pulsar el botón *Export*. Comenzará la exportación de los datos y se descargará un archivo comprimido.

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