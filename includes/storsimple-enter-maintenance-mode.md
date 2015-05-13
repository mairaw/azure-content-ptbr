<properties
   pageTitle="Entrar no modo de manutenção"
   description="Explica como colocar o dispositivo StorSimple no modo de manutenção."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="adinah"
   editor="tysonn" /> <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/21/2015"
   ms.author="v-sharos" />

### Para entrar no modo de Manutenção

1. No menu do console serial, escolha a opção 1, **Efetuar login com acesso total**.

2. Digite a senha. A senha padrão é **Senha1**.

3. No prompt de comando, digite

    **Enter-HcsMaintenanceMode**

4. Você verá uma mensagem de aviso informando que o modo de Manutenção interromperá todas as solicitações de E/S e a conexão com o Portal de Gerenciamento de servidor, e você será solicitado para confirmação. Digite **Y** para entrar no modo de Manutenção.

    Ambos os controladores serão reiniciados. Quando a reinicialização estiver concluída, outra mensagem será exibida indicando que o dispositivo está em modo de Manutenção.

<!--HONumber=52-->