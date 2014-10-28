<properties pageTitle="How create and publish a product in Azure API Management" metaKeywords="" description="Learn how to create and publish products in Azure API Management." metaCanonical="" services="" documentationCenter="API Management" title="How create and publish a product in Azure API Management" authors="sdanie" solutions="" manager="" editor="" />

<tags ms.service="api-management" ms.workload="mobile" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="01/01/1900" ms.author="sdanie"></tags>

# Como criar e publicar um produto no Gerenciamento de API do Azure

No Gerenciamento de API do Azure (visualização), um produto contém uma ou mais APIs, bem como uma cota de uso e os termos de uso. Uma vez publicado o produto, os desenvolvedores podem assinar o produto e começar a usar as APIs dele. Este tópico fornece um guia para criar um produto, adicionar uma API e publicá-la para os desenvolvedores.

## Neste tópico

-   [Criar um produto][Criar um produto]
-   [Adicionar APIs a um produto][Adicionar APIs a um produto]
-   [Adicionar informações descritivas a um produto][Adicionar informações descritivas a um produto]
-   [Publicar um produto][Publicar um produto]
-   [Tornar um produto visível para os desenvolvedores][Tornar um produto visível para os desenvolvedores]
-   [Ver os assinantes de um produto][Ver os assinantes de um produto]
-   [Próximas etapas][Próximas etapas]

## <a name="create-product"> </a>Criar um produto

Operações são adicionadas e configuradas em uma API no console de gerenciamento. Para acessar o console de gerenciamento, clique em **Console de Gerenciamento** no Portal do Azure para acessar o serviço de Gerenciamento de API.

> Se você ainda não criou uma instância de serviço de Gerenciamento de API, consulte [Criar uma instância de serviço de Gerenciamento de API][Criar uma instância de serviço de Gerenciamento de API] no tutorial [Introdução ao Gerenciamento de API do Azure][Introdução ao Gerenciamento de API do Azure].

![Console de Gerenciamento de API][Console de Gerenciamento de API]

Clique em **Produtos** no menu à esquerda para exibir a página **Produtos** e clique em **Adicionar produto**.

![Produtos][Produtos]

![Novo produto][Novo produto]

Insira um nome descritivo para o produto no campo **Nome** e uma descrição do produto no campo **Descrição**.

Marque **Requerer aprovação de assinatura** se desejar que um administrador revise e aceite ou rejeite as tentativas de assinatura para o produto. Se a caixa não estiver marcada, as tentativas de assinatura serão aprovadas automaticamente. Para obter mais informações sobre assinaturas, consulte [Ver os assinantes de um produto][Ver os assinantes de um produto].

![Produtos][1]

> Por padrão, novos produtos não são publicados e ficam visíveis somente para o grupo **Administradores**.

Para configurar um produto, clique o nome do produto na guia **Produtos**.

## <a name="add-apis"> </a>Adicionar APIs a um produto

A página **Produtos** contém quatro links para configuração: **Resumo**, **Configurações**, **Visibilidade** e **Desenvolvedores**. A guia **Resumo** é onde você pode adicionar APIs e publicar ou cancelar a publicação de um produto.

![Resumo][Resumo]

Antes de publicar o produto, você precisa adicionar uma ou mais APIs. Para fazer isso, clique em **Adicionar API ao produto**.

![Adicionar APIs][Adicionar APIs]

Selecione as APIs desejadas e clique em **Salvar**.

## <a name="add-description"> </a>Adicionar informações descritivas a um produto

A guia **Configurações** permite que você forneça informações detalhadas sobre o produto, como sua finalidade, as APIs a que fornece acesso, entre outras informações úteis. O conteúdo é voltado aos desenvolvedores que chamarão a API e pode ser escrito em texto sem formatação ou em marcação HTML.

![Configurações do produto][Configurações do produto]

Selecione **Requerer aprovação de assinatura** se desejar aprovar manualmente todas as solicitações de assinatura do produto. Por padrão, todas as assinaturas de produto são aprovadas automaticamente.

Outra opção é preencher o campo **Termos de uso** descrevendo os termos de uso do produto, que os assinantes deverão aceitar para usar o produto.

## <a name="publish-product"> </a>Publicar um produto

Antes que as APIs de um produto possam ser chamadas, o produto precisa ser publicado. Na guia **Resumo** do produto, clique em **Publicar** e depois clique em **Sim, publicar** para confirmar. Para fazer com que um produto publicado volte a ser provado, clique em **Cancelar publicação**.

![Publicar produto][Publicar produto]

## <a name="make-visible"> </a>Tornar um produto visível para os desenvolvedores

A guia **Visibilidade** permite que você selecione quais funções deseja que possam ver o produto no portal do desenvolvedor e assinar o produto.

![Visibilidade do produto][Visibilidade do produto]

Para habilitar e desabilitar a visibilidade de um produto para os desenvolvedores de um grupo, marque ou desmarque a caixa de seleção ao lado do grupo e clique em **Salvar**.

> Para obter mais informações, consulte [Como criar e usar grupos para gerenciar contas de desenvolvedor no Gerenciamento de API do Azure][Como criar e usar grupos para gerenciar contas de desenvolvedor no Gerenciamento de API do Azure].

## <a name="view-subscribers"> </a>Ver os assinantes de um produto

A guia **Desenvolvedores** lista os desenvolvedores que assinaram o produto. Os detalhes e configurações de cada desenvolvedor podem ser vistos clicando em seus respectivos nomes. Neste exemplo, nenhum desenvolvedor assinou o produto ainda.

![Desenvolvedores][Desenvolvedores]

## <a name="next-steps"> </a>Próximas etapas

Após as APIs desejadas serem adicionadas e o produto publicado, os desenvolvedores podem assinar o produto e começar a chamar as APIs. Para ver um tutorial que demonstra esses itens, bem como a configuração avançada do produto, consulte [Como criar e definir configurações avançadas no Gerenciamento de API do Azure][Como criar e definir configurações avançadas no Gerenciamento de API do Azure].

  [Criar um produto]: #create-product
  [Adicionar APIs a um produto]: #add-apis
  [Adicionar informações descritivas a um produto]: #add-description
  [Publicar um produto]: #publish-product
  [Tornar um produto visível para os desenvolvedores]: #make-visible
  [Ver os assinantes de um produto]: #view-subscribers
  [Próximas etapas]: #next-steps
  [Criar uma instância de serviço de Gerenciamento de API]: ../api-management-get-started/#create-service-instance
  [Introdução ao Gerenciamento de API do Azure]: ../api-management-get-started
  [Console de Gerenciamento de API]: ./media/api-management-howto-add-products/api-management-management-console.png
  [Produtos]: ./media/api-management-howto-add-products/api-management-products.png
  [Novo produto]: ./media/api-management-howto-add-products/api-management-add-new-product.png
  [1]: ./media/api-management-howto-add-products/api-management-products-page.png
  [Resumo]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
  [Adicionar APIs]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
  [Configurações do produto]: ./media/api-management-howto-add-products/api-management-product-settings.png
  [Publicar produto]: ./media/api-management-howto-add-products/api-management-publish-product.png
  [Visibilidade do produto]: ./media/api-management-howto-add-products/api-management-product-visibility.png
  [Como criar e usar grupos para gerenciar contas de desenvolvedor no Gerenciamento de API do Azure]: ../api-management-howto-create-groups
  [Desenvolvedores]: ./media/api-management-howto-add-products/api-management-developer-list.png
  [Como criar e definir configurações avançadas no Gerenciamento de API do Azure]: ../api-management-howto-product-with-rules