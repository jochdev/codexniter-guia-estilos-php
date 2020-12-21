<h1 align="center">Guía de Estilo de PHP para Codeigniter v3.xx</h1>
Autor: jochdev

Proyecto: Guia estilos codexniter

<h1>Tabla de Contenidos</h1>


## Introducción
El propósito de esta pequeña guía de estilo es proporcionar estandarización y
orden en el código que desarrollamos como empresa de software, utilizando el
framework de Codeigniter.

## Estilo de Código PHP
En general, el único requerimiento que adoptamos, es que las funciones se
enfoquen más en tabulación que en espaciado para el orden. Por ejemplo, no
utilizamos corchetes de apertura bajo las deficiones de funciones:

```
<?php

class Controller extends CI_Controller
{

    public function index()
    {
        if (!$somevalue)
        {
            echo 'Some text';
        }
        else
        {
            echo 'Some other text';
        }
    }

}
```

La forma anterior es demasiado espaciado para nosotros. Lo óptimo es hacerlo de
esta manera mientras se configura el editor o el IDE para que muestre un poco
más de espacio entre líneas de código.

```
<?php

class Controller extends CI_Controller {

    public function index() {
        if (!$somevalue) {
            echo 'Some text';
        } else {
            echo 'Some other text';
        }
    }

}
```

El código queda más comprimido, y con los espacios en el editor o IDE, será
sencillo de leer y examinar.

## Convenciones de Desarrollo
Para seguir estas convenciones, vamos a imaginar que tenemos una aplicación
que permite a usuarios guardar notas. Entonces, nuestra app imaginaria tendrá
dos recursos principales: usuarios y notas.

### Nomenclatura de Base de Datos
Las tablas en la base de datos deberán nombrarse en español, con minúsculas y
en plural, con un nombre representativo del recurso. Los campos de base de datos
también deben nombrarse en español y con minúsuclas utilizando un prefijo para cada campo
relacionado con el nombre de la tabla. Para separar espacios seutiliza el guión bajo.
**usamos prefijos para nuestras tablas, y tambien
para los campos de esas tablas de 4 letras.**

Para nuestra app imaginaria, entonces, nuestra estructura de datos sería más
o menos así.

| Nombre Tabla: | tbl_users         |
| ------------- | ------------- |
| user_id            | int(11)       |
| user_name          | varchar(255)  |
| user_email         | varchar(255)  |
| user_password      | varchar(255)  |
| user_created_at*   | timestamp(12) |
| user_updated_at*   | timestamp(12) |

| Nombre Tabla: | notes         |
| ------------- | ------------- |
| note_id            | int(11)       |
| note_user_id       | int(11)       |
| note_content       | text          |
| note_created_at*   | timestamp(12) |
| note_updated_at*   | timestamp(12) |

>*NOTA: Todas nuestras tablas tienen, por obligación, un campo de created_at y
uno de updated_at, cuyos valores deben ser guardados directamente desde el
modelo utilizando la función `time()` en PHP. No guardamos fechas formateadas
en la base de datos. Éstas se formatean en la aplicación.

### Nomenclatura de Controladores
Nombramos nuestros controladores de forma que correspondan con el recurso. El
nombre del controlador siempre es en inglés y en plural, con mayúsculas en la
primera letra. Si requiere espacio, se utiliza un guión bajo. Entonces, la
estructura en COdeigniter para nuestra app imaginaria:

- controllers
    + Users
    + Notes

Además, utilizamos convenciones estándar para los métodos básicos de nuestros
controladores. Tenemos una plantilla disponible que hace el trabajo fácil, pero
nunca está de más repasar. Los métodos son los siguientes:

| Nombre Método | URI                      | Función                  |
| ------------- | ------------------------ | ------------------------ |
| index()       | /{recurso}               | Lista todos los recursos |
| show($id)     | /{recurso}/show/{id}     | Lista un recurso         |
| new()         | /{recurso}/new           | Formulario para nuevo    |
| create()      | /{recurso}/create        | Crea el recurso          |
| edit($id)     | /{recurso}/edit/{id}     | Formulario para editar   |
| update($id)   | /{recurso}/update/{id}   | Actualiza un recurso     |
| destroy($id)  | /{recurso}/destroy/{id}  | Elimina un recurso       |

Métodos que realizan operaciones específicas de un determinado recurso, deben
colocarse más abajo. Los nombres de éstos métodos son siempre en inglés, y
pueden tener espaciado con guión bajo.

### Contenido del Controlador
**El controlador contiene el procesamiento de los datos, tanto de los que obtiene
del modelo como de los que va a envíar al modelo**. Esto es muy importante. En
este enfoque, los controladores tenderán a ser largos, mientras que los modelos
muy pequeños. Sin embargo, creemos que este es el propósito de un MVC: es el
controlador el que debe manejar la lógica de nuestra aplicación.

Además, los controladores no sólo procesan los datos y controlan la lógica de
la aplicación, sino que también llaman a las vistas. Veremos más de eso en la
sección nomenclatura de vistas.

### Nomenclatura de Modelos
Nombramos nuestros modelos de la misma forma del recurso al cual están
representando, pero en **singular**. Así, en nuestra estructura de directorio:

- controllers
    + Users
    + Notes
- models
    + User
    + Note

Aquí es cuando el uso del inglés se hace conveniente, porque en la gran mayoría
del los casos, el singular se logra sólo quitando una s. En español la situación
es más compleja debido a la existencia de género en las palabras.

También tenemos métodos establecidos para los modelos, utilizando el famoso
CRUD:

| Nombre Método                 | Función                                  |
| ----------------------------- | ---------------------------------------- |
| create($table, $id)           | Crea una entrada                         |
| read($table, $data)           | Obtiene una, varias o todas las entradas |
| update($table, $data, $where) | Actualiza una entrada                    |
| delete($table, $where)        | Elimina una entrada                      |

Métodos que realizan acciones más específicas, como joins u otros, pueden o
agregarse a los métodos ya establecidos, o agregarse al final.

### Contenido del Modelo
El modelo sólo realiza consultas a la base de datos y devuelve los resultados de
esas consultas al controlador. Nada más. **No se ordenan datos en el modelo, ni
tampoco se obtienen inputs de POST y GET.** Consulta y resultado, nada más.

### Nomenclatura de Vistas
Las vistas también se corresponden al recurso que éstas muestran o con el cual
se relacionan. Se colocan en una carpeta con el nombre del recurso, y luego una
vista para cada método que necesita una, de esta forma:

- controllers
    + Users
    + Notes
- models
    + User
    + Note
- views
    + users
        * index.php
        * show.php
        * create.php
        * edit.php
    + notes
        * index.php
        * show.php
        * create.php
        * edit.php

### Llamando las Vistas
Las vistas se obtienen desde el controlador. Generalmente, como toda aplicación
web, existe un layout de front-end que debe ser considerado, generalmente un
Navbar, un Footer, quizás un Sidebar y el área de contenido principal.

Las vistas de layout se guardan en la carpeta vistas, en una subcarpeta llamada
layouts, de esta forma:

- controllers
    + Users
    + Notes
- models
    + User
    + Note
- views
    + users
        * index.php
        * show.php
        * create.php
        * edit.php
    + notes
        * index.php
        * show.php
        * create.php
        * edit.php
    + layouts
        * main.php
        * navbar.php
        * footer.php

En main.php va el código html correspondiente al header, desde donde se llama el
CSS, y luego un fragmento de php que llama a una vista con la variable content.
Luego de eso, viene el área de los scripts y el cierre de las etiquetas body y
html.

```
<html>
    <head>
        <title>Hola!</title>
    </head>
    <link src="style.css" rel="stylesheet">

<body>

    <?php $this->load->view($content)?>

</body>

```

Entonces, en el controlador, si quisieramos llamar la vista users/show.php, se
llama de esta forma:

```
<?php

public function index() {
    $data['content'] = 'users/show';
    $this->load->view('layouts/main', $data);
}
```

La ventaja de este método es que ofrece mejor control a la hora de llamar vistas,
además de pasar las variables a todas las subvistas que se puedan crear, en vez
de hacerlo vista por vista.

Luego, en la misma vista llamada, se llama al navbar y el footer si se desea.

## Workflow

### Modelación de Datos
Navicat.

### Control de Versión (Git & Bitbucket)
Todo nuestro código está en Github. Utilizamos como interfaz gráfica de Git,
aunque también utilizamos la consola o el plugin para Sublime Text 3 o Visual Studio Code. Como sea,
utilizamos Git y Github.

Siempre tenemos una rama master y una development para todos nuestros proyectos.
Todos los commit push van a development. La rama master está bloqueada para
intervención directa.
Luego de eso, cuando todos los cambios de development están listos, se manda a
producción haciendo un merge de la rama development a la master, sin borrar la
rama development. Bitbucket Pipelines automaticamente publicará los cambios en
el servidor de producción de la aplicación.

## Buenas Prácticas

### Formularios
Implementación de Token.

## Nuestro "Super" Codeigniter
El objetivo es crear nuestra propia versión de lanzamiento utilizando Codeigniter agregando algunas
características y funcionalidades últiles, como migraciones, middlewares,
sietema de login y auto-theming, además de contar con librerías muy importantes.
