---
layout: post
title: Creando bots para telegram con Harbour.
---

Este es un intento de crear un primer post acerca de la creación de un bot para telegram con Harbour. Próximamente estaré completando la información.

![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.

En este post se entiende que ud conoce telegram y ha interactuado con algún bot en esta plataforma, tiene conocimientos de programación preferiblemente en Harbour aunque si es desarrollador en otro lenguaje, este documento podría ser una referencia en su desarrollo de bots.

| <img src="/home/riztan/tmp/tgm_photo_2020-09-28_12-56-30.jpg" style="zoom: 50%;" /> | Para poder interactuar con el API de telegram, debemos obtener un token o credencial que le permita a telegram reconocer nuestro bot, por lo tanto debemos registrar el bot y para eso utilizamos al bot principal al cual podemos buscar a través de la opción de búsqueda en telegram y escribiendo "botFather", lo agregan al contacto y seleccionan "Iniciar". La interacción con botFather no la vamos a desarrollar acá, es bástante intuitiva y existe suficiente material en el internet respecto a cómo registrar un bot en telegram. Entonces, resumimos que se debe solicitar el registro de un nuevo bot, indicarán el nombre y luego obtendrán el TOKEN de su bot. Ese dato deben guardarlo pues es el identificador que utilizarán en la interacción del bot con telegram. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

Para poder interactuar con el API de telegram, debemos obtener un token o credencial que le permita a telegram reconocer nuestro bot, por lo tanto debemos registrar el bot y para eso utilizamos al bot principal al cual podemos buscar a través de la opción de búsqueda en telegram y escribiendo "botFather", lo agregan al contacto y seleccionan "Iniciar". La interacción con botFather no la vamos a desarrollar acá, es bástante intuitiva y existe suficiente material en el internet respecto a cómo registrar un bot en telegram. Entonces, resumimos que se debe solicitar el registro de un nuevo bot, indicarán el nombre y luego obtendrán el TOKEN de su bot. Ese dato deben guardarlo pues es el identificador que utilizarán en la interacción del bot con telegram.

Ya tenemos bot, por lo tanto tenemos nuestro token. Continuamos, en telegram hay dos formas de obtener la información de actualizaciones (updates en telegram), estas actualizaciones o entradas "sin revisar", estos pueden ser: mensajes, notas de voz o video, audios, videos, encuestas entre otros. Entonces para mayor comodidad en lo sucesivo yo les diré: "entradas".

Entonces, un bot al igual que nosotros debe obtener de telegram esas entradas por revisar; hay dos formas de obtener esa información:

1. Accediendo al api de telegram y solicitar directamente la entrega de estas entradas, para ello telegram nos ofrece el método [getUpdates](https://core.telegram.org/bots/api#getupdates) 

   Usando getUpdates, simplemente 

2. Otorgando a telegram los datos a los que deberá enviar estas entradas cuando son recibidas, en el api esto lo vemos en el método [webHook](https://core.telegram.org/bots/api#setwebhook).



