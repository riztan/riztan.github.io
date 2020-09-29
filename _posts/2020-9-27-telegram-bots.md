---
layout: post
title: Creando bots para telegram con Harbour.
---

En este post se entiende que ud conoce telegram y ha interactuado con algún bot en esta plataforma, tiene conocimientos de programación (preferiblemente en Harbour) aunque si es trabaja con otro lenguaje, este documento podría ser una referencia en su desarrollo de bots. 

Este documento igualmente está basado en el uso de una librería de nombre hbtelegram que se puede obtener en: https://github.com/riztan/hbtelegram. Una clase que facilita el acceso al API de telegram para bots.

![_config.yml]({{site.baseurl}}/images/tlgrm_botfather_01.png)

Para poder interactuar con el API de telegram, debemos obtener un token o credencial que le permita a telegram reconocer nuestro bot, por lo tanto debemos registrar el bot y para eso utilizamos al bot principal al cual podemos buscar a través de la opción de búsqueda en telegram y escribiendo "botFather", lo agregan al contacto y seleccionan "Iniciar". La interacción con botFather no la vamos a desarrollar acá, es bástante intuitiva y existe suficiente material en el internet respecto a cómo registrar un bot en telegram. Entonces, resumimos que se debe solicitar el registro de un nuevo bot, indicarán el nombre y luego obtendrán el TOKEN de su bot. Ese dato deben guardarlo pues es el identificador que utilizarán en la interacción del bot con telegram.

Para poder interactuar con el API de telegram, debemos obtener un token o credencial que le permita a telegram reconocer nuestro bot, por lo tanto debemos registrar el bot y para eso utilizamos a botFather de telegram, podemos buscar a botFather a través de la opción de búsqueda en telegram y escribiendo "botFather", lo seleccionamos de los resultados de la búsqueda y luego seleccionamos "Iniciar". La interacción con botFather no la vamos a desarrollar acá, es bástante intuitiva y existe suficiente material en el internet respecto a cómo registrar un bot en telegram. Entonces, resumimos que se debe solicitar el registro de un nuevo bot, indicarán el nombre y luego obtendrán el TOKEN de su bot. Ese dato deben guardarlo pues ese token es el identificador que se utilizará en la interacción del bot con telegram.

Ejemplo de código para instanciar un objeto de tipo TelegramBot con hbtelegram:

```xbase
procedure Main()
   local oBot
   local cToken := "MI_TOKEN_AQUI"
   oBot := TlgrmBot():New( cToken, "Nombre de mi Bot" )
   oBot:end()  // Finalizamos el bot
```

Bien, ya tenemos token, por lo tanto tenemos nuestro bot. Continuamos; en telegram hay dos formas de obtener la información de actualizaciones (updates en telegram), estas actualizaciones o entradas "sin revisar", estos pueden ser: mensajes, notas de voz o video, audios, videos, encuestas entre otros. Entonces para mayor comodidad en lo sucesivo llamaremos a esto: "entradas".

Entonces, un bot al igual que nosotros debe obtener de telegram esas entradas por revisar; hay dos formas de obtener esa información:

1. Accediendo al api de telegram y solicitar directamente la entrega de estas entradas, para ello telegram nos ofrece el método [getUpdates](https://core.telegram.org/bots/api#getupdates)  

2. Otorgando a telegram los datos a los que deberá enviar estas entradas cuando son recibidas, en el api esto lo vemos en el método [setWebhook](https://core.telegram.org/bots/api#setwebhook).

De momento en hbtelegram se ofrece el soporte solo para getUpdates, esperamos incorporar pronto la vía por webhook.

getUpdates. Obtendremos todas las actualizaciónes pendientes simplemente tras invocar este método. Complementando el ejemplo anterior:

```xbase
procedure Main()
   local oBot, oUpdates
   local cToken := "MI_TOKEN_AQUI"
   oBot := TlgrmBot():New( cToken, "Nombre de mi Bot" )
   oUpdates := oBot:getUpdates()
   oBot:end()  // Finalizamos el bot
```

Como se puede observar, es muy simple obtener un objeto de tipo cursor que contiene las entradas (updates) del bot. Ahora necesitamos conocer el contenido de estas entradas, recordemos que podemos tener: mensajes, notas de voz o video, audios, videos, documentos, etc. Entonces, es posible que no tengamos nada pendiente por revisar o muchas entradas, ¿cómo las visualizamos? continuamos con el ejemplo, veamos:

```xBase
procedure Main()
   local oBot, oUpdates
   local cToken := "MI_TOKEN_AQUI"
   oBot := TlgrmBot():New( cToken, "Nombre de mi Bot" )
   oUpdates := oBot:getUpdates()
   While !oUpdates:Eof()
      oUpdate := oUpdate:Current()
      if hb_isNIL(oUpdate)
         oUpdates:Skip()
         loop
      endif
      ? oUpdate:cTlgrmJSON
   EndDo
   oBot:end()  // Finalizamos el bot
```



Continuará...

