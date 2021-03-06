---
layout: post
title: Creando bots para telegram con Harbour (Parte 1).
---

En este post se da por entendido que ud conoce telegram y ha interactuado con algún bot en esta plataforma, tiene conocimientos de programación (preferiblemente en Harbour) aunque si es trabaja con otro lenguaje, este documento podría ser una referencia en su desarrollo de bots. 

Este documento igualmente está basado en el uso de una librería de nombre hbtelegram que se puede obtener en: https://github.com/riztan/hbtelegram. Una clase que facilita el acceso al API de telegram para bots.

![_config.yml]({{site.baseurl}}/images/tlgrm_botfather_01.png)

Para poder interactuar con el API de telegram, debemos obtener un token o credencial que le permita a telegram reconocer nuestro bot, por lo tanto debemos registrar el bot y para eso utilizamos al bot principal al cual podemos buscar a través de la opción de búsqueda en telegram y escribiendo "botFather", lo agregan al contacto y seleccionan "Iniciar". La interacción con botFather no la vamos a desarrollar acá, es bástante intuitiva y existe suficiente material en el internet respecto a cómo registrar un bot en telegram. Entonces, resumimos que se debe solicitar el registro de un nuevo bot, indicarán el nombre y luego obtendrán el TOKEN de su bot. Ese dato deben guardarlo pues es el identificador que utilizarán en la interacción del bot con telegram.

Ejemplo de código para instanciar un objeto de tipo TelegramBot con hbtelegram:
```c
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
```c
procedure Main()
   local oBot, oUpdates
   local cToken := "MI_TOKEN_AQUI"
   oBot := TlgrmBot():New( cToken, "Nombre de mi Bot" )
   oUpdates := oBot:getUpdates()
   oBot:end()  // Finalizamos el bot
```
Como se puede observar, es muy simple obtener un objeto de tipo cursor que contiene las entradas (updates) del bot. Ahora necesitamos conocer el contenido de estas entradas, recordemos que podemos tener: mensajes, notas de voz o video, audios, videos, documentos, etc. Entonces, es posible que no tengamos nada pendiente por revisar o muchas entradas, ¿cómo las visualizamos? continuamos con el ejemplo, veamos:

```c
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

Explicación rápida del ejemplo.Instanciamos un objeto "oBot", y solicitamos las entradas "oUpdates", luego iniciamos un recorrido que instancia en "oUpdate" la entrada actual "oUpdate:Current()". Por cada entrada, nos muestra por la terminal el contenido (JSON) de la entrada actual "oUpdate:cTlgrmJSON".  

Ahora, vamos a complementar aún más el ejemplo mostrando algunas instrucciones más.

```c
   procedure Main()
   local oBot, oUpdates
   local cToken := "MI_TOKEN_AQUI"
   oBot := TlgrmBot():New( cToken, "Nombre de mi Bot" )
   oUpdates := oBot:getUpdates()
   While !oUpdates:Eof()

      oUpdate := oUpdates:Current()
      ? "Update: "
      tracelog oUpdate:getJSON() 
      ? "update_id: "      + oUpdate:update_id
      ? "type: "           + oUpdate:type
      oFrom := oUpdate:getFrom()
      ? "from_id: "        + oFrom:id 
      ? "from_firstname: " + oFrom:first_name
      Do Case
      Case oUpdate:type = "message"
         ? "chat: "   + oUpdate:message:chat:id
         ? "type: "   + oUpdate:message:chat:type
         if oUpdate:message:isDef("text")
            ? "text: "   + oUpdate:message:text
         endif
      Case oUpdate:type = "callback_query"
      EndCase

      if oUpdate:isdef("text")
         if len( oUpdate:tokens ) > 0 
            ? "tokens: ", hb_valtoexp( oUpdate:tokens )
         endif

         if left( oUpdate:text,1 ) = "/"
            oBot:Reply()
         endif
      endif

      oUpdates:Skip()
   EndDo   
```

Si enviamos algunos mensajes a nuestro bot, al ejecutar este código, veremos en nuestra terminal algo como:

![_config.yml]({{site.baseurl}}/images/tlgrm_terminal_01.png)

Si observamos bien los resultados obtenidos, podemos relacionar entonces el contenido JSON recibido y la forma como ha sido procesado por la clase. Si la entrada es un mensaje, entonces podemos deducir que tenemos en nuestra entrada "oUpdate" a su vez un objeto "from" que contiene los datos del remitente.  oUpdate:from,  oUpdate:from:id, etc. Podemos conocer con más detalle como está conformada una entrada de tipo mensaje en la documentación que ofrece telegram acá: [https://core.telegram.org/bots/api#message](https://core.telegram.org/bots/api#message) igualmente se puede ver la estructura para cada una de las entradas (updates) que podemos recibir. 

Continuará...

