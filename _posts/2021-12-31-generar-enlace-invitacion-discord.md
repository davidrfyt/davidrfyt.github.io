---
layout: single
title: Cómo generar enlace de invitación a Discord automáticamente
excerpt: "En este artículo te voy a enseñar como crear un enlace de invitación automáticamente para que tus usuarios se unan a tu servidor de Discord. Esto es útil para poner el enlace en tu página web y claro deberás tener permisos en ese servidor de Discord.

Para empezar nos iremos a nuestro servidor de Discord y le daremos a Ajustes del servidor."
date: 2021-12-31
classes: wide
header:
  teaser: /assets/images/12_2021/discord.jpg
  teaser_home_page: true
  icon: /assets/images/php.png
categories:
  - PHP
tags:
  - PHP
---

![](/assets/images/12_2021/discord.jpg)

En este artículo te voy a enseñar como crear un enlace de invitación automáticamente para que tus usuarios se unan a tu servidor de Discord. Esto es útil para poner el enlace en tu página web y claro deberás tener permisos en ese servidor de Discord.

Para empezar nos iremos a nuestro servidor de Discord y le daremos a Ajustes del servidor.

![](/assets/images/12_2021/discord_1.png)

Seguidamente, nos iremos hacia Widget en el menú de la izquierda, habilitaremos widget del servidor como en la siguiente imagen.

![](/assets/images/12_2021/discord_2.png)

La id del servidor la necesitaremos para más adelante, apuntarla si queréis para no volver abrir las opciones del servidor luego.

Ahora paremos al código PHP

```php
<?php

$configServer = [
  'serverID' => 000000000000000000 // ID de tu Discord.
];

$apiDiscord = "https://discordapp.com/api/servers/".$configServer['serverID']."/embed.json";

$getDataFromApi = @file_get_contents($apiDiscord);

$json = json_decode($getDataFromApi);

if ($json->instant_invite != ""){
  header("Location: " . $json->instant_invite);
} else {
  header("Content-Type: text/json");
  echo json_encode(array('message' => 'Error al obtener el link de invitación.'));
}

?>
```

Bien, en este código PHP necesitaremos cambiar el serverID por el id de tu Discord que lo podrás encontrar en el apartado Widget de los ajustes de tu servidor.

Listo, con esto al acceder al archivo PHP nos redireccionará a nuestro Discord. Feliz año nuevo ;)