# Chuck2005

Al ejercicio inicial le agregue un div Jumbotron y dentro del mismo un elemento h1 que va ser donde se va a ingresar el chiste recibido desde el servidor de ICNDB.
Agregue una verificación del estado de la petición por si esta saca algún error. Hasta ahora en las pruebas que he realizado no ha pasado (se puede simular cambiando la url)
Para el ejemplo use el CDN de bootstrap por facilidad:
```sh
<!DOCTYPE html>
<html class="no-js">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title></title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css" crossorigin="anonymous">
    </head>
    <body>
        <div class="jumbotron">
                <h1 class="display-3"></h1>
              </div>
    </body>
    <script>
    var regEx = /^[2][0-9][0-9]$/
    //la variable regex se usa para verificar si el valor de estado http devuelto por la peticion es distinto a 2XX
    var xmlhttp = new XMLHttpRequest();
    xmlhttp.open('GET', 'http://api.icndb.com/jokes/random/', true);
    xmlhttp.onreadystatechange = function(){
        if (this.readyState == 4 && this.status == 200){//si la respuesta es 200 y el estado es 4 entonces se muestra el chiste. La validación fue con el ejemplo de la w3schools
            var textoChiste = JSON.parse(this.response).value.joke;
            console.log('chiste recibido: ' + textoChiste);
            var h1s = document.getElementsByTagName('h1');
            h1s[0].innerHTML = textoChiste;
        }
        if (this.readyState == 4 && !regEx.test(this.status))//extrañamante aunque la url no exista el status es 0
            console.log('se ha presentado un error. Este es el estado de la petición ' + this.status)
    }
    xmlhttp.send();
    </script>
</html>
```
# Chuck 2006
Le agregue tambien un Jumbotron y dos h1 para probar como funciona. jQuery en este caso toma todos los elementos h1 y les aplica el texto que recibe desde el servicio web.
Trate de capturar un posible error de url pero el error que se retorna es poco diciente y no hay mucha documentacion al respecto (extraño en jQuery), por lo que al final solo dice que ha habido un error pero no exactamente qué error.
Con jQuery la sintaxis es tal vez un poco más abstracta pero definitivamente más corta:

```sh
<!DOCTYPE html>
<html class="no-js">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title></title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css" crossorigin="anonymous">
    </head>
    <body><div class="jumbotron">
        <h1 class="display-3"></h1>
        <h1 class="display-3"></h1>
      </div>
    </body>
    <script>
        $.get("http://api.icndb2.com/jokes/random", (response) =>  {
        var textoChiste = response.value.joke;
        $('h1').text(textoChiste);
        }
        )
        .fail(function(data){
            console.log("Se ha presentado un error con el llamado " + data.statusText);
        })
    </script>
</html>
```

# Chuck 2014
Se agrega el estilo de boostrap para los ul.
Antes de ECMA 6 la funcion con flecha (traduccion suelta) se hubiese escrito así:
```sh
/*$.icndb.getRandomJokes({ 
        number: 10, 
        success: function(response) {
        response.forEach(element => { $("ul").append('<li class="list-group-item">' + element.joke + '</li>'); });
        }});*/
```

Dentro del código probé con ambas notaciones (comentada la parte que esta en pre ECMA6):

```sh
<!DOCTYPE html>
<html class="no-js"> 
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title></title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
        <script src="http://code.icndb.com/jquery.icndb.min.js"></script>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css" crossorigin="anonymous">
    </head>
    <body>
        <ul class="list-group"></ul>        
    </body>
    <script>
        /*$.icndb.getRandomJoke((response) => { var textoChiste = response.joke; $('h1').text(textoChiste);});
        $('h1').text(textoChiste);*/
        $.icndb.getRandomJokes({ 
        number: 10, 
        success: (response) => {
        response.forEach(element => { $("ul").append('<li class="list-group-item">' + element.joke + '</li>'); });
        }});
        /*$.icndb.getRandomJokes({ 
        number: 10, 
        success: function(response) {
        response.forEach(element => { $("ul").append('<li class="list-group-item">' + element.joke + '</li>'); });
        }});*/

    </script>
</html>
```

De acuerdo a la documentación del plugin al llamar randonJokes no se deben producir errores por lo que no pude implementar alguna verificacón adicional.


# Chuck 2016

Para este caso usé el CDN de ICNDB para importar los WebComponents e ingresarlos en una tabla con estilo Skeleton (sin CDN en este caso):
```sh
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="import" href="https://raw.githubusercontent.com/erikringsmuth/chuck-norris-joke/master/chuck-norris-joke.html">
    <link rel="stylesheet" href="skeleton.css">
</head>
<body>
        <table class="u-full-width">
                <thead>
                  <tr>
                    <th>Chiste</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td><chuck-norris-joke></chuck-norris-joke></td>
                  </tr>
                </tbody>
              </table>
            
</body>
</html>
```
