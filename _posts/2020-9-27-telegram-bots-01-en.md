---
layout: post
title: Creating telegram bots with Harbour (Part 1)
---

In this post it is understood that you know telegram and has interacted with some bot on this platform, has knowledge of programming (preferably in Harbour) although if you work with another language, this document could be a reference in their development of bots.

This document is also based on the use of a library called hbtelegram that can be obtained at: https://github.com/riztan/hbtelegram. A class that provides access to the telegram API for bots.

![_config.yml]({{site.baseurl}}/images/tlgrm_botfather_01.png)

In order to interact with the telegram API, we must obtain a token or credential that allows telegram to recognize our bot, therefore we must register the bot and for that we use the main bot which we can search through the telegram search option and typing "botFather", add it to the contact and select "Start". The interaction with botFather is not going to be developed here, it is quite intuitive and there is enough material on the Internet about how to register a bot in telegram. So, we will summarize that you must request the registration of a new bot, indicate the name and then get the TOKEN of your bot. This data must be saved because it is the identifier used in the interaction of the bot with telegram.

Example of code to instantiate a TelegramBot type object with hbtelegram:

```c
  procedure Main()
   local oBot
   local cToken := "MI_TOKEN_AQUI"
   oBot := TlgrmBot():New( cToken, "Nombre de mi Bot" )
   oBot:end()  // Finalizamos el bot
```

Well, we already have a token, so we have our bot. We continue; in telegram there are two ways to obtain the information of updates (updates in telegram), these updates or "unchecked" entries, these can be: messages, voice or video notes, audios, videos, surveys among others. So for convenience we will call these "entries" in the following.

So, a bot like us must telegraph these unreviewed entries; there are two ways to obtain that information:

1. By accessing the api of telegram and directly requesting the delivery of these tickets, for this telegram offers us the method [getUpdates](https://core.telegram.org/bots/api#getupdates)  

2. Giving to telegram the data to which you must send these entries when they are received, in the api this we see in the method [setWebhook](https://core.telegram.org/bots/api#setwebhook).

At the moment hbtelegram offers support only for getUpdates, we hope to incorporate soon the via webhook.

getUpdates. We will get all the pending updates simply after invoking this method. Complementing the previous example:

```c
procedure Main()
   local oBot, oUpdates
   local cToken := "MI_TOKEN_AQUI"
   oBot := TlgrmBot():New( cToken, "Nombre de mi Bot" )
   oUpdates := oBot:getUpdates()
   oBot:end()  // Finalizamos el bot
```
As you can see, it's very simple to get a cursor type object that contains the bot's updates. Now we need to know the content of these entries, remember that we can have: messages, voice or video notes, audios, videos, documents, etc. Then, it is possible that we do not have anything pending to review or many entries, how do we visualize them?

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

Quick explanation of the example. We instantiate an "oBot" object, and request the "oUpdates" entries, then we start a path that instances in "oUpdate" the current entry "oUpdate:Current()". For each entry, it shows us through the terminal the content (JSON) of the current entry "oUpdate:cTlgrmJSON".  

Now, let's complement the example even more by showing some more instructions.

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

If we send some messages to our bot, when we execute this code, we will see in our terminal something like:

![_config.yml]({{site.baseurl}}/images/tlgrm_terminal_01.png)

If we look closely at the results obtained, we can then relate the JSON content received and the way it has been processed by the class. If the entry is a message, then we can deduce that we have in our entry "oUpdate" a "from" object that contains the sender's data. oUpdate:from, oUpdate:from:id, etc. We can know in more detail how a message type entry is made in the documentation that offers telegram here: [https://core.telegram.org/bots/api#message](https://core.telegram.org/bots/api#message) You can also see the structure for each of the entries (updates) that we can receive. 

To be continued...