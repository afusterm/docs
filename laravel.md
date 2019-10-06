# Laravel

## Composer

[Composer](https://getcomposer.org) es un gestor de dependencias para PHP. Genera dos ficheros:

- *composer.json*: requisitos y las versiones mínimas o máximas.
- *composer.lock*: lo que sea ha instalado.

Si existe *composer.lock* el comando `composer install` instalará las versiones indicadas en dicho fichero. Para actualizar las versiones de *composer.lock* habrá que ejecutar el comando `composer update`.


## Instalación de Laravel

- `composer global require "laravel/installer"`: instala laravel y sus dependencias en el directorio de usuario.
- `laravel new [nombre]`: crea un proyecto laravel.
- `php artisan`: muestra todos los comandos de artisan.


## Proyecto laravel

### Lanzar el servidor de desarrollo

`php artisan serv`


## Base de datos

- *config/database.php*: aquí se encuentran las distintas posibles conexiones de bases de datos con sus parámetros.
- *./.env*: se definen las variables utilizadas en *database.php*.
- `php artisan migrate`: ejecuta las migraciones.
- `php artisan make:auth`: crea enlaces en la web para el registro de un usuario y su autenticación.


### Operaciones con tinker

- Crear un usuario: usar la factoría definida en *UserFactory.php* de la siguiente manera:

```
$factory(App\User::class)->create()
```

- Buscar un usuario por id:

```
$id = 1
App\User::find($id)
```

- Buscar un usuario por email:

```
$user = App\User::where('email', 'wolff.maryse@example.com')->first()
```

- Cambiar la contraseña de un usuario:

```
$user = App\User::where('email', 'wolff.maryse@example.com')->first()
$user->password = Hash::make('miclave')
$user->save()
```

### Pasos para crear una tabla

1 - Crear una migración. Ej:

```
php artisan make:migration create_markets_table --create=markets
```

2 - La migración crea la clase de migración en *database/migrations* con el formato año_mes_dia_hora_<nombre_migracion>. Esta clase contiene los métodos `up()` y `down()`. El método `up()` se editará para añadir las columnas de la tabla. Ej:

```
public function up()
    {
        Schema::create('markets', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('description');
            $table->timestamps();
        });
    }
```

3 - Ejecutar la migración: `php artisan migrate`.

4 - Crear el modelo con `php artisan make:model <Modelo>`. El nombre del modelo será el nombre de la tabla creada anteriormente en singular. Esto creará una clase en la raíz del directorio *app*. 

5 - Opcionalmente se puede definir en la clase la variable protegida *fillable* que indicará a Eloquent qué valores se escribirán en la tabla. Esta variable es un array de cadenas con los nombres de las columnas. Ej:

```
class Market extends Model
{
    protected $fillable = ['name', 'description'];

    protected $hidden = ['created_at', 'updated_at'];
}
``` 

Y de la misma forma se puede sobreescribir la variable `$hidden` para indicar las columnas que no se devolverán.

6 - Crear el controlador: `php artisan make:controller <nombre_controlador>`. Ej: `php artisan make:controller MarketController`. Esto creará el controlador en el directorio *app/Http/Controllers*

7 - Añadir el controlador al fichero de rutas *routes/web.php* de la forma:

```
Route::get('/markets', 'MarketController@index');
```

Esta ruta es para el método GET de HTTP.

En el ejemplo se indica el método `index` de `MarketController`, por lo tanto hay que definir el método `index` en la clase `MarketController`.


### Definir claves ajenas

1 - Crear la columna que relaciona la tabla con otra tabla:

```
public function up()
{
    Schema::create('stocks', function (Blueprint $table) {
		  ...
        $table->integer('market_id');
        ....
    });
}
```

2 - Esta columna de tipo `int` tiene que ser definida como `unsigned`: `$table->integer('market_id')->unsigned();`.

3 - Indicar que dicha columna referencia a otra tabla mediante la función `references` para indicar a qué columna de la otra tabla referencia y la función `on` para indicar el nombre de la otra tabla.

```
public function up()
{
    Schema::create('stocks', function (Blueprint $table) {
		  ...
        $table->integer('market_id')->unsigned();
        $table->foreign('market_id')->references('id')->on('markets');
        ....
    });
}
```

### Definir columnas únicas

Para indicar que el valor de una columna no se puede repetir se utiliza la función `unique` en la definición de la tabla: `$table->string('acronym')->unique();`.


## Trabajar con fechas

Para trabajar con fechas se utiliza la bibilioteca [Carbon](https://carbon.nesbot.com).


### Tests

#### Crear tests

`php artisan make:test <nombre_nuevo_fichero_test>`

Ej: `php artisan make:test MarketTest`

Con este comando crearía un test en el directorio *tests/Feature* (tests de integración). Para crearlo en el directorio *tests/Unit* (unitarios) se le pasa el parámetro `--unit`. Ej:

`php artisan make:test MarketTest --unit` 


#### Configuración de tests

La configuración de los tests se encuentra en *./phpunit.xml*. En la sección *php* se pueden configurar las variables de entorno que utilizarán los tests. Por ejemplo se puede configurar la base de datos para las pruebas:

```
<php>
        <env name="APP_ENV" value="testing"/>
        <env name="BCRYPT_ROUNDS" value="4"/>
        <env name="CACHE_DRIVER" value="array"/>
        <env name="SESSION_DRIVER" value="array"/>
        <env name="QUEUE_DRIVER" value="sync"/>
        <env name="MAIL_DRIVER" value="array"/>
        <env name="DB_CONNECTION" value="sqlite" />
        <env name="DB_DATABASE" value="database/larastock.sqlite" />
    </php>
```

#### Preparación de tests

Se puede hacer que se ejecute código antes de ejecutar todos los tests y después mediante los métodos `setUp` y `tearDown`:

```
public function setUp()
{
    parent::setUp();
    Artisan::call('migrate');
}

public function tearDown()
{
    Artisan::call('migrate:reset');
    parent::tearDown();
}
```

Para automatizar la migración en la base de datos de pruebas y su borrado tras ejecutar los tests se puede hacer uso de los *traits*:

```
class MarketsTest extends TestCase
{
    use DatabaseMigrations;
    use DatabaseTransactions;

    public function testCreateMarket()
    {
        Market::create(['name' => 'name',
            'description' => 'description']);
        $markets = Market::getAllMarkets();

        $this->assertCount(1, $markets);
    }
}
```


### Rutas

- Para añadir un parámetro a una ruta se indica poniendo el parámetro entre llaves. Ej:

`Route::get('/markets/{status}', 'MarketController@index');`

Si además se quiere indicar que el parámetro es opcional se hará añadiendo el símbolo *?*. Ej:

`Route::get('/markets/{status?}', 'MarketController@index');`

- Para ver las rutas:

`php artisan route:list`


## Traits

Como PHP no tiene multiherencia para heredar funcionalidad se utilizan los traits. Un [trait](https://www.php.net/manual/es/language.oop5.traits.php) se puede definir así:

```
trait ValidationTrait
{
    public $errors;

    public function validate($data)
    {
        $v = Validator::make($data, $this->rules);
        if ($v->fails()) {
            $this->errors = $v->errors();
            return false;
        }

        return true;
    }
}
```

Y a la hora de utilizarlo:

```
class Market extends Model
{
    use ValidationTrait;
    
    ...

```

## Recuperar valores de la entrada cuando falla la validación

En un formulario cuando el usuario envía los datos y se produce un error en la validación, éstos se borran al volver a cargar la página. Para evitar esto en el atributo `value` de los controles se hace lo siguiente:

```
<input type="text" name="description" id="market-description" value="{{old('description')}}" required />
```

Se llama a la función `old` pasándole el nombre del campo. Para el caso de una casilla se utiliza de la siguiente forma:

```
<input type="checkbox" name="active" id="market-active" {{old('active') ? 'checked' : ''}} value="1" />Activo
```

## Evitar ataques CSRF

Para evitar ataques [CSRF](https://es.wikipedia.org/wiki/Cross-site_request_forgery) hay que llamar a la función `csrf_field()` dentro del formulario:

```
<form class="form-horizontal" action="{{route('markets.edit', [$market->id])}}" method="post">
    {{ csrf_field() }}
    
    <div class="form-group {{ $errors->has('name') ? 'has-error' : '' }}">
        <label for="name" class="col-md-4 control-label">Nombre</label>
        <div class="col-md-6">
            <input type="text" name="name" id="market-name" value="{{old('name', $market->name)}}" required />
        </div>
    </div>
    
    .....
```

## Enviar peticiones PUT desde el navegador

Algunos navegadores no pueden realizar operaciones PUT. Para conseguir esto hay que llamar a la función `method_field('PUT')` de la siguiente forma:

```
<form class="form-horizontal" action="{{route('markets.edit', [$market->id])}}" method="post">
    {{ csrf_field() }}
    {{ method_field('PUT') }}
    
    <div class="form-group {{ $errors->has('name') ? 'has-error' : '' }}">
        <label for="name" class="col-md-4 control-label">Nombre</label>
        <div class="col-md-6">
            <input type="text" name="name" id="market-name" value="{{old('name', $market->name)}}" required />
        </div>
    </div>
```


## Añadir estilos

1 -  Abrir el fichero *resources/assets/saas/app.sccs* y añadir las clases css que se necesiten.

2 - En el terminal ir al directorio del proyecto y ejecutar `npm install`.

3 - Ejecutar `npm run dev`para integrar las nuevas clases css *public/css/app.css*. Para que se compilen conforme se hacen cambios ejecutar `npm run watch`.


## Eloquent model generator

[Eloquent model generator](https://github.com/krlove/eloquent-model-generator) Genera un modelo de una tabla con más código del que genera `php artisan make:model`. Se instala así:

`composer require krlove/eloquent-model-generator --dev`

Para generar el modelo: `php artisan krlove:generate:model NombreDelModelo`


## Seeders

Para poblar la base de datos se utilizan los seeders. Cuando se ejecuta un seeder se llama al método `run` de una clase que extiende de `Seeder`. Para crear un seeder:

`php artisan make:seed NombreSeeder`

Esto crea una clase:

```
class IbexSeeder extends NombreSeeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        //
    }
}
```

Para ejecutar un seeder:

`php artisan db:seed --class=NombreSeeder`


## Rutas

Las rutas se encuentran definidas en el fichero *routes/web.php*. Se pueden indicar con los verbos `get`, `post`, etc. O bien se pueden especificar con el método `resources`. Al llamar a este método automáticamente se establecen las rutas para cada verbo. Las rutas se pueden listar de la siguiente forma:

`php artisan route:list`

Al método `Route::resource` se le pasa como primer argumento el nombre del recurso, como segundo argumento el nombre del controlador pero sin indicar ningún método como pasaba con los métodos de los verbos anteriores.

`Route::resource('stock_historicals', 'StockHistoricalController');`

Opcionalmente también se puede especificar un tercer parámetro para indicar que se desean todos los métodos excepto los indicados, o bien solo los métodos indicados.

`Route::resource('stock_historicals', 'StockHistoricalController', ['only' => ['index', 'create']]);`

`Route::resource('stock_historicals', 'StockHistoricalController', ['except' => ['index', 'create']]);`


## Herramientas

- [Eloquent model generator](https://github.com/krlove/eloquent-model-generator).
- [Guzzle](https://github.com/guzzle/guzzle) es una bibilioteca para hacer peticiones REST.
- [unique_with Validator Rule For Laravel](https://github.com/felixkiss/uniquewith-validator).
- [Debugbar](https://github.com/barryvdh/laravel-debugbar).
- [LavaCharts](http://lavacharts.com/): biblioteca para dibujar gráficas. Instalar: `composer require khill/lavacharts`.


## Blade

- Para llamar a código php en las vistas se utilizan {{ codigo_php }}. Ej:

```
<div class="card-header">{{ $title or 'Stocks'}}</div>
```

- Para pintar el código html que devuelve php se utiliza {!! !!} para indicar que interprete el código html:

```
{!! $stock_chart !!}
```


## Comandos

Para crear comandos se utiliza el comando:

`php artisan make:command NombreComando`

Esto crea una clase en *app/Console/Commands* llamada *NombreComando*. El código a ejecutar se escribe en el método `handle()`.

Este comando se añadirá al array `$commands` de la clase *Kernel.php* en *app/Console*. Y en el método `schedule` de `Kernel` se ejecutará el comando.

Para ver la lista de comandos: `php artisan`


## Notificaciones

- Para crear una notificación `php artisan make:notification NombreNotificacion`