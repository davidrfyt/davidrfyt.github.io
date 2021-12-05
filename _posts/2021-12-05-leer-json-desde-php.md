---
layout: single
title: Leer JSON desde PHP
excerpt: "¿Qué es un archivo JSON?

Un archivo JSON es un formato de texto sencillo usado para el intercambio de datos.

Un pequeño ejemplo de JSON sería:"
date: 2021-12-05
classes: wide
header:
  teaser: /assets/images/12_2021/json.png
  teaser_home_page: true
  icon: /assets/images/php.png
categories:
  - PHP
  - JSON
tags:
  - PHP
  - JSON
---

![](/assets/images/12_2021/json.png)

¿Qué es un archivo JSON?

Un archivo JSON es un formato de texto sencillo usado para el intercambio de datos.

Un pequeño ejemplo de JSON sería:

```json

[
    {
        "num": 1,
        "name": "Bulbasaur",
        "variations": [
            {
                "name": "Bulbasaur",
                "description": "Bulbasaur is a Grass/Poison type Pokémon introduced in Generation 1. It is known as the Seed Pokémon.",
                "image": "images/bulbasaur.jpg",
                "types": [
                    "Grass",
                    "Poison"
                ],
                "specie": "Seed Pokémon",
                "height": 0.7,
                "weight": 6.9,
                "abilities": [
                    "Overgrow",
                    "Chlorophyll"
                ],
                "stats": {
                    "total": 318,
                    "hp": 45,
                    "attack": 49,
                    "defense": 49,
                    "speedAttack": 65,
                    "speedDefense": 65,
                    "speed": 45
                },
                "evolutions": [
                    "bulbasaur",
                    "ivysaur",
                    "venusaur"
                ]
            }
        ],
        "link": "https://pokemondb.net/pokedex/bulbasaur"
    }
]

```

Pues bien a través de este pequeño codigo JSON podríamos sacar los datos con PHP y poder o bien guardarlos en una base de datos o mostrarlos en pantalla.

Ahora haremos un ejercicio para sacar los datos de un pokémon y mostrarlos en pantalla, para este ejercicio usaremos el siguiente archivo JSON https://davidrf.es/assets/json/pokemon.json que contiene datos de pokémons.

Lo primero de todo es cargar el archivo JSON para ello escribiremos lo siguiente:

```php

$dataJson = @file_get_contents("https://davidrf.es/assets/json/pokemon.json");

```

Bien, con esto tendriamos cargado en la variable $dataJson todo el contenido del archivo pokemon.json, ahora deberemos decodificar este archivo usando json_decode.

```php

$dataItems = json_decode($dataJson, true);

```

En este punto ya podríamos acceder a los datos del archivo JSON. Para acceder a dichos datos debemos de tratar el archivo como si fuera un array multidimensional.

Podriamos ver el contenido del JSON haciendo el uso de var_dump(). En este caso escribiriamos var_dump($dataItems); y nos mostraría en pantalla el contenido de dicho archivo.

Bien, siguiendo con el ejercicio para acceder al nombre del primer pokémon usariamos el siguiente código:

```php

echo $dataItems[0]['name'];

```

El 0 sería la posición en el array, por lo que 0 sería Bulbasaur, el 1 sería Ivysaur y así sucesivamente.

Para terminar este ejercicio he recorrido con un bucle todos los datos de JSON mostrando en pantalla todos los Pokémons.

![](/assets/images/12_2021/pokemons.png)

El código usado para el anterior ejemplo es este:

```php

<?php

$jsonFile = "https://davidrf.es/assets/json/pokemon.json";

$getDataFromFile = @file_get_contents($jsonFile);

$decodeData = json_decode($getDataFromFile, true);

foreach($decodeData as $dataPokemon){
    ?>
        <table border="1">
        <tr>
        <td>Nº: </td>
        <td>
            <?= $dataPokemon['num']; ?> 
        </td>
        </tr>
        <tr>
        <td>Nombre: </td>
        <td>
            <?= $dataPokemon['name']; ?>
        </td>
        </tr>
        <tr>
        <td>Foto: </td>
        <td>
            <img src="https://davidrf.es/assets/images/12_2021/pokemons/<?= $dataPokemon['variations'][0]['image']; ?>" width="200px">
        </td>
        </tr>
        <tr>
        <td>Habilidades: </td>
        <td>
            <?php
                foreach ($dataPokemon['variations'][0]['abilities'] as $dataAbilities){
                    echo " - " . $dataAbilities . "</br>";
                }
            ?>
        </td>
        </tr>
        <tr>
        <td>Evoluciones: </td>
        <td>
            <?php
                foreach ($dataPokemon['variations'][0]['evolutions'] as $dataEvolutions){
                    echo " - " . $dataEvolutions . "</br>";
                }
            ?>
        </td>
        </tr>
        </table><p>
    <?php
}

?>

```

Espero que os sirva, Saludos!