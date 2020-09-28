---
layout: post
title: Creando bots para telegram con Harbour.
---

En este post se entiende que ud conoce telegram y ha interactuado con algún bot en esta plataforma, tiene conocimientos de programación preferiblemente en Harbour aunque si es desarrollador en otro lenguaje, este documento podría ser una referencia en su desarrollo de bots. 

![_config.yml]({{site.baseurl}}/images/tlgrm_botfather_01.png)

Para poder interactuar con el API de telegram, debemos obtener un token o credencial que le permita a telegram reconocer nuestro bot, por lo tanto debemos registrar el bot y para eso utilizamos al bot principal al cual podemos buscar a través de la opción de búsqueda en telegram y escribiendo "botFather", lo agregan al contacto y seleccionan "Iniciar". La interacción con botFather no la vamos a desarrollar acá, es bástante intuitiva y existe suficiente material en el internet respecto a cómo registrar un bot en telegram. Entonces, resumimos que se debe solicitar el registro de un nuevo bot, indicarán el nombre y luego obtendrán el TOKEN de su bot. Ese dato deben guardarlo pues es el identificador que utilizarán en la interacción del bot con telegram.

Para poder interactuar con el API de telegram, debemos obtener un token o credencial que le permita a telegram reconocer nuestro bot, por lo tanto debemos registrar el bot y para eso utilizamos al bot principal al cual podemos buscar a través de la opción de búsqueda en telegram y escribiendo "botFather", lo agregan al contacto y seleccionan "Iniciar". La interacción con botFather no la vamos a desarrollar acá, es bástante intuitiva y existe suficiente material en el internet respecto a cómo registrar un bot en telegram. Entonces, resumimos que se debe solicitar el registro de un nuevo bot, indicarán el nombre y luego obtendrán el TOKEN de su bot. Ese dato deben guardarlo pues es el identificador que utilizarán en la interacción del bot con telegram.

Ya tenemos bot, por lo tanto tenemos nuestro token. Continuamos, en telegram hay dos formas de obtener la información de actualizaciones (updates en telegram), estas actualizaciones o entradas "sin revisar", estos pueden ser: mensajes, notas de voz o video, audios, videos, encuestas entre otros. Entonces para mayor comodidad en lo sucesivo yo les diré: "entradas".

Entonces, un bot al igual que nosotros debe obtener de telegram esas entradas por revisar; hay dos formas de obtener esa información:

1. Accediendo al api de telegram y solicitar directamente la entrega de estas entradas, para ello telegram nos ofrece el método [getUpdates](https://core.telegram.org/bots/api#getupdates) 

   Usando getUpdates, simplemente 

2. Otorgando a telegram los datos a los que deberá enviar estas entradas cuando son recibidas, en el api esto lo vemos en el método [webHook](https://core.telegram.org/bots/api#setwebhook).



