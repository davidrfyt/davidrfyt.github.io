---
layout: single
title: Efecto de nieve para tu web
excerpt: ""
date: 2021-12-20
classes: wide
header:
  teaser: /assets/images/12_2021/json.png
  teaser_home_page: true
  icon: /assets/images/js.png
categories:
  - JAVASCRIPT
tags:
  - JAVASCRIPT
---

![](/assets/images/12_2021/snowflake.png)

En épocas de navidad los webmasters solemos poner algún efecto navideño en nuestros sitios web.

Para ello podemos usar jQuery y JavaScript para adornar nuestro sitio web con copos de nieve, en este post te enseñaré como poner los copos de nieve en tu sitio web.

Merodeando por GitHub encontré un código muy fácil de poner, el código es del usuario loktar00 en GitHub.

Para empezar incluiremos jQuery en el header de nuestro sitio web.

```javascript

<script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>

```

Seguidamente en el body agregaremos lo siguiente:

```javascript

<script src='https://davidrf.es/assets/js/snowfall.jquery.min.js'></script>
<script type='text/javascript'>     
$(document).ready(function(){
	$(document).snowfall({shadow : true, round : true,  minSize: 5, maxSize:8});
});
</script>

```

ya con esto empezará a caer nieve desde la parte de arriba de nuestro sitio web. 

Feliz Navidad ;)