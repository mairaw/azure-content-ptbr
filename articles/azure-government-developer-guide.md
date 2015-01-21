﻿<properties title="Azure Government Developers Guide" pageTitle="Guia de Desenvolvedores do Azure Government" description="Isso fornece uma comparação de recursos e orientação sobre o desenvolvimento de aplicativos para o Azure Government" metaKeywords="Azure Government AzureGov GovCloud" services="" solutions="" documentationCenter="" authors="John Harvey" videoId="" scriptId="" manager="required" />

<tags ms.service="multiple" ms.devlang="na" ms.topic="article" ms.tgt_pltfrm="na" ms.workload="azure-government" ms.date="12/6/2014" ms.author="jharve" />


<!--This is a basic template that shows you how to use mark down to create a topic that includes a TOC, sections with subheadings, links to other azure.microsoft.com topics, links to other sites, bold text, italic text, numbered and bulleted lists, code snippets, and images. For fancier markdown, find a published topic and copy the markdown or HTML you want. For more details about using markdown, see http://sharepoint/sites/azurecontentguidance/wiki/Pages/Content%20Guidance%20Wiki%20Home.aspx.-->

<!--Properties section (above): this is required in all topics. Please fill it out! See http://sharepoint/sites/azurecontentguidance/wiki/Pages/Markdown%20tagging%20-%20add%20a%20properties%20section%20to%20your%20markdown%20file%20to%20specify%20metadata%20values.aspx for details. -->

<!-- Tags section (above): this is required in all topics. Please fill it out! See http://sharepoint/sites/azurecontentguidance/wiki/Pages/Markdown%20tagging%20-%20add%20a%20tags%20section%20to%20your%20markdown%20file%20to%20specify%20metadata%20for%20reporting.aspx for details. -->

<!--The next line, with one pound sign at the beginning, is the page title--> 
#  Guia do Desenvolvedor do Microsoft Azure Government 

<p> O Microsoft Azure Government é uma instância isolada fisicamente e da rede do Microsoft Azure.  Este guia de desenvolvedores fornecerá detalhes sobre as diferenças que os desenvolvedores de aplicativos e administradores precisam interagir e trabalhar com essas regiões separadas do Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## Neste tópico


+ [Visão geral](#Overview)
+ [Diretrizes para Desenvolvedores](#Guidance)
+ [Recursos disponíveis atualmente no Microsoft Azure Government](#Features)
+ [Mapeamento de ponto de extremidade](#Endpoint)
+ [Próximas etapas](#next)


## <a name="Overview"></a>Visão geral

O Microsoft Azure Government é uma instância separada do serviço Microsoft Azure para tratar dass necessidades de segurança e conformidade das agências federais, estaduais e municipais dos EUA e seus provedores de soluções. O Azure Government oferece isolamento físico e de rede de implantações não-governamentais e fornece triagem do pessoal dos EUA. 

A Microsoft fornece várias ferramentas para criar e implantar aplicativos em nuvem no Microsoft global service do Microsoft Azure ("Serviço Global") e nos serviços Microsoft Azure Government.

Ao criar e implantar aplicativos para os serviços do Azure Government, em vez de serviços globais, os desenvolvedores precisam saber as principais diferenças entre os dois serviços.  Especificamente sobre instalar e configurar o ambiente de programação, configurar pontos de extremidade, gravar aplicativos e implantá-lo como serviços do Azure Government.

As informações contidas neste documento resumem essas diferenças e complementam as informações disponíveis sobre o site do [Azure Government](http://www.azure.com/gov "Azure Government") e a [biblioteca técnica do Microsoft Azure](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") no MSDN. Informações oficiais podem também estar disponíveis em muitos outros locais, como o [Microsoft Azure Trust Center](http://azure.microsoft.com/pt-br/support/trust-center/ "Microsoft Azure Trust Center"), [Centro de documentação do Azure](http://azure.microsoft.com/pt-br/documentation/) e em [Azure Blogs](http://azure.microsoft.com/blog/ "Azure Blogs"). 

Este conteúdo destina-se a parceiros e desenvolvedores que estão implantando o Microsoft Azure Government.



## <a name="Guidance"></a>Diretrizes para Desenvolvedores
Porque a maioria do conteúdo técnico que está disponível no momento assume que os aplicativos estão sendo desenvolvidos para o Serviço Global em vez de para o governo do Microsoft Azure, é importante garantir que os desenvolvedores estejam cientes das principais diferenças dos aplicativos desenvolvidos para serem hospedado no Azure Government.

- Em primeiro lugar, há diferenças de recursos e de serviços, isso significa que alguns recursos que estão em regiões específicas do serviço Global não estão disponíveis no Azure Government.

- Em segundo lugar, os recursos que são oferecidos no Azure Government, têm configuração difente do Global Service.  Portanto, você deve examinar o código de exemplo, as configurações e as etapas para garantir que esteja criando e executando dentro do ambiente de serviços de nuvem do Azure Government.


## <a name="Features"></a> Recursos disponíveis atualmente no Microsoft Azure Government
O Azure Government atualmente tem os seguintes serviços disponíveis nas regiões do governo de IOWA e da VIRGÍNIA nos E.U.A.:

- Máquinas Virtuais
- Serviços de Nuvem
- Armazenamento
- Active Directory
- Agendador
- Rede Virtual
- Banco de dados SQL

Outros serviços estão disponíveis e mais serviços serão adicionados continuamente.  Para obter a lista mais atual dos serviços, consulte a [página regiões](http://azure.microsoft.com/pt-br/regions/#services) que destacará todas as regiões disponíveis e seus serviços.  

Atualmente, o Governo de Iowa e da Virgínia nos E.U.A. são os centros de dados de suporte para o Azure Government.  Consulte acima a página de regiões de centros de dados existentes e serviços disponíveis.

## <a name="Endpoint"></a>Mapeamento de ponto de extremidade

Use a tabela a seguir para guiá-lo ao mapear pontos de extremidade públicos do Microsoft Azure e do Banco de dados SQL para os pontos de extremidade específicos do Azure Government.

<table>
<tr style='font-weight:bold'><td>
Tipo de serviço</td><td>	Público do Azure</td><td>	Azure Government
</td></tr><tr><td>
Página inicial do Azure Government</td><td>	windowsazure.com	</td><td>microsoftazure.us
</td></tr><tr><td>
Portal de Gerenciamento</td><td>	manage.windowsazure.com</td><td>	manage.windowsazure.us
</td></tr><tr><td>
Geral</td><td>	*.windows.net	</td><td>*.usgovcloudapi.net
</td></tr><tr><td>
Núcleo	</td><td>*.core.windows.net	</td><td>*.core.usgovcloudapi.net
</td></tr><tr><td>
Computação	</td><td>*.cloudapp.net	</td><td>*.usgovcloudapp.net
</td></tr><tr><td>
Armazenamento de Blob</td><td>	*.blob.core.windows.net</td><td>	*.blob.core.usgovcloudapi.net
</td></tr><tr><td>
Armazenamento de Fila	</td><td>*.queue.core.windows.net</td><td>	*.queue.core.usgovcloudapi.net
</td></tr><tr><td>
Armazenamento de tabela</td><td>	*.table.core.windows.net	</td><td>*.table.core.usgovcloudapi.net
</td></tr><tr><td>
Banco de dados SQL</td><td>	*.database.windows.net	</td><td>*.database.usgovcloudapi.net
</td></tr><tr><td>
Gerenciamento de serviços</td><td>	management.core.windows.net</td><td>	management.core.usgovcloudapi.net
</td></tr></table> 




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next"></a>Próximas etapas

Se você estiver interessado em saber mais sobre o Azure Government e como sua organização pode se qualificar para acessar, vá para <A href="http://azure.com/gov"> http://www.azure.com/gov</a>

<!--Anchors-->
[Visão geral]: ##Overview
[Subtítulo 2]: #subheading-2
[Subtítulo 3]: #subheading-3
[Próximas etapas]: #next-steps


<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link de 1 para outro tópico de documentação azure.microsoft.com]: ../virtual-machines-windows-tutorial/
[Link de 2 para outro tópico de documentação azure.microsoft.com]: ../web-sites-custom-domain-name/
[Link de 3 para outro tópico de documentação azure.microsoft.com]: ../storage-whatis-account/

<!--HONumber=35.2-->