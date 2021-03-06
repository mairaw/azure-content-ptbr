<a id="what-are-service-bus-queues"></a>
## O que são as filas do Barramento de Serviço?

Filas do Barramento de Serviço dão suporte a um modelo de comunicação de **sistema de mensagens agenciado**. Ao usar filas, os componentes de um aplicativo distribuído não se comunicam diretamente uns com os outros, mas trocam mensagens por meio de uma fila, que atua como um intermediário (agente). Um produtor de mensagem (remetente) transmite uma mensagem para a fila e, em seguida, continua o processamento. De forma assíncrona, um consumidor de mensagem (receptor) recebe a mensagem da fila e a processa. O produtor não precisa esperar por uma resposta do consumidor a fim de continuar a processar e enviar mais mensagens. As filas oferecem entrega de mensagem do tipo **FIFO (primeiro a entrar, primeiro a sair)** para um ou mais consumidores concorrentes. Ou seja, as mensagens são normalmente recebidas e processadas pelos receptores na ordem em que foram adicionadas à fila, sendo que cada mensagem é recebida e processada por apenas um consumidor de mensagem.

![Conceitos de fila](./media/service-bus-java-how-to-create-queue/sb-queues-08.png)

Filas do Barramento de Serviço são uma tecnologia de uso geral que pode ser usada para uma grande variedade de cenários:

-   Comunicação entre as funções Web e de trabalho em um aplicativo multicamada do Azure.
-   Comunicação entre aplicativos locais e aplicativos hospedados pelo Azure em uma solução híbrida.
-   Comunicação entre os componentes de um aplicativo distribuído executado localmente em diferentes organizações ou departamentos de uma organização.

O uso de filas permite que você escale horizontalmente seus aplicativos com mais facilidade e concede mais resiliência à sua arquitetura.

## Criar um namespace de serviço

Para começar a usar filas do Barramento de Serviço no Azure, primeiro crie um namespace de serviço. Um namespace fornece um contêiner de escopo para endereçar recursos do barramento de serviço dentro de seu aplicativo.

Para criar um namespace de serviço:

1.  Faça logon no [portal clássico do Azure][].

2.  No painel de navegação esquerdo do portal, clique em **Barramento de Serviço**.

3.  No painel inferior do portal, clique em **Criar**. ![](./media/service-bus-java-how-to-create-queue/sb-queues-03.png)

4.  No diálogo **Adicionar um novo namespace**, digite um nome de namespace. O sistema imediatamente verifica para ver se o nome está disponível. ![](./media/service-bus-java-how-to-create-queue/sb-queues-04.png)

5.  Depois de verificar se o nome do namespace está disponível, escolha o país ou a região em que o namespace deve ser hospedado (certifique-se de usar o mesmo país/região em que você está implantando seus recursos de computação).

	IMPORTANTE: selecione a **mesma região** que você pretende escolher para implantar seu aplicativo. Isso lhe dará o melhor desempenho.

6. 	Deixe os outros campos na caixa de diálogo com seus valores padrão (**Mensagens** e **Camada padrão**), em seguida, clique na marca de seleção. Agora, o sistema cria o seu namespace e o habilita. Talvez você precise aguardar vários minutos, enquanto o sistema provisiona recursos para sua conta.

	![](./media/service-bus-java-how-to-create-queue/getting-started-multi-tier-27.png)

O namespace que você criou demora algum tempo para ser ativado, e então aparece no portal do Azure. Aguarde até que o status do namespace seja **Ativo** para continuar.

## Obter as credenciais de gerenciamento padrão do namespace

A fim de executar operações de gerenciamento, como criar uma fila no novo namespace, obtenha as credenciais de gerenciamento para o namespace. Você pode obter essas credenciais no portal.

###Para obter as credenciais de gerenciamento do portal

1.  No painel de navegação esquerdo, clique no nó **Barramento de Serviço** para exibir a lista de namespaces disponíveis: ![](./media/service-bus-java-how-to-create-queue/sb-queues-13.png)

2.  Na lista mostrada, clique no namespace que acabou de criar.

3.  Clique em **Configurar** para exibir as políticas de acesso compartilhado para seu namespace. ![](./media/service-bus-java-how-to-create-queue/sb-queues-14.png)

4.  Anote a chave primária ou copie-a na área de transferência.

  [portal clássico do Azure]: http://manage.windowsazure.com

  [34]: ./media/service-bus-java-how-to-create-queue/VSProperties.png

<!---HONumber=AcomDC_1203_2015-->