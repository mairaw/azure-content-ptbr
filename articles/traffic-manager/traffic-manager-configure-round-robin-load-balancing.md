<properties
   pageTitle="Configurar o balanceamento de carga round robin do Gerenciador de Tráfego"
   description="Este artigo o ajudará a configurar o balanceamento de carga round robin para seus pontos de extremidade do Gerenciador de Tráfego."
   services="traffic-manager"
   documentationCenter=""
   authors="cherylmc"
   manager="adinah"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/27/2015"
   ms.author="cherylmc" />

# Configurar o balanceamento de carga round robin

Um padrão comum de balanceamento de carga é fornecer um conjunto de pontos de extremidade idênticos, que incluem serviços de nuvem e sites, e enviar tráfego a cada um deles em um estilo round-robin. As etapas a seguir descrevem como configurar o Gerenciador de Tráfego para executar esse tipo de balanceamento de carga. Para obter mais informações sobre os diferentes métodos de balanceamento de carga, consulte [Sobre os métodos de balanceamento de carga do Gerenciador de Tráfego](traffic-manager-load-balancing-methods.md).

>[AZURE.NOTE] Os sites do Azure já fornecem funcionalidade de balanceamento de carga round robin para sites em um data center (também conhecido como região). O Gerenciador de Tráfego permite que você especifique o balanceamento de carga round-robin para sites em data centers diferentes.

## Balancear a carga de tráfego igualmente (round robin) em um conjunto de pontos de extremidade:

1. No Portal de Gerenciamento, no painel esquerdo, clique no ícone do **Gerenciador de Tráfego** para abrir o painel do Gerenciador de Tráfego. Se você ainda não tiver criado seu perfil do Gerenciador de Tráfego, consulte [Gerenciar perfis do Gerenciador de Tráfego](traffic-manager-manage-profiles.md) para obter as etapas para criar um perfil básico do Gerenciador de Tráfego.
2. No Portal de Gerenciamento, no painel do Gerenciador de Tráfego, localize o perfil do Gerenciador de Tráfego que contém as configurações que você deseja modificar e, em seguida, clique na seta à direita do nome do perfil. Isso abrirá a página de configurações do perfil.
3. Na página de seu perfil, clique em **Pontos de extremidade** na parte superior da página e verifique se os pontos de extremidade de serviço que você deseja incluir em sua configuração estão presentes. Para obter as etapas para adicionar ou remover pontos de extremidade, consulte [Gerenciar pontos de extremidade no Gerenciador de Tráfego](traffic-manager-endpoints.md).
4. Na página de seu perfil, clique em **Configurar** na parte superior para abrir a página de configuração.
5. Para **Configurações de método de balanceamento de carga**, verifique se o método de balanceamento de carga é **Round Robin**. Se não for, clique em **Round Robin** na lista suspensa.
6. Verifique se as **Configurações de Monitoramento** estão definidas corretamente. O monitoramento garante que não seja enviado tráfego aos pontos de extremidade que estão offline. Para monitorar os pontos de extremidade, você deve especificar um caminho e um nome de arquivo. Observe que uma barra "/" é uma entrada válida para o caminho relativo e implica que o arquivo está no diretório raiz (padrão). Para obter mais informações sobre o monitoramento, consulte [Sobre o monitoramento do Gerenciador de Tráfego](../traffic-manager-about-monitoring.md).
7. Depois de concluir as alterações de configuração, clique em **Salvar** na parte inferior da página.
8. Teste as alterações em sua configuração. Para obter mais informações, consulte [Testar as configurações do Gerenciador de Tráfego](traffic-manager-testing-settings.md).
9. Depois que seu perfil do Gerenciador de Tráfego estiver configurado e funcionando, edite o registro de DNS em seu servidor DNS autoritativo para apontar o nome de domínio de sua empresa para o nome de domínio do Gerenciador de Tráfego. Para obter mais informações sobre como fazer isso, consulte [Apontar um domínio de Internet de empresa para um domínio do Gerenciador de Tráfego](traffic-manager-point-internet-domain.md).

## Consulte também

[Sobre os métodos de balanceamento de carga do Gerenciador de Tráfego](traffic-manager-load-balancing-methods.md)

[Tarefas de configuração do Gerenciador de Tráfego](https://msdn.microsoft.com/library/azure/hh744830.aspx)

[Visão geral do Gerenciador de Tráfego](../traffic-manmager-overview.md)

[Serviços de Nuvem](http://go.microsoft.com/fwlink/?LinkId=314074)

[Sites](http://go.microsoft.com/fwlink/p/?LinkId=393327)

[Operações no Gerenciador de Tráfego (referência de API REST)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Cmdlets do Gerenciador de Tráfego do Azure](http://go.microsoft.com/fwlink/p/?LinkId=400769)

<!--HONumber=49--> 