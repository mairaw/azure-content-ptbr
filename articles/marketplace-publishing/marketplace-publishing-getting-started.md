<properties
   pageTitle="Visão geral de como criar e implantar uma oferta para o Marketplace | Microsoft Azure"
   description="Entender as etapas necessárias para se tornar um vendedor da Microsoft aprovado e criar e implantar uma imagem de máquina virtual, o modelo, serviço de dados ou o serviço de desenvolvedor no Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace-publishing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/09/2015"
   ms.author="hascipio" />

# Como publicar uma oferta no Microsoft Azure Marketplace
Este artigo é fornecido para ajudar um vendedor a criar e implantar sua solução por exemplo, a imagem de máquina virtual única, modelo de solução, serviço de desenvolvedor ou serviço de dados no Azure Marketplace para outros parceiros e clientes do Azure adquirir e utilizar.

A primeira coisa que você gostaria de fazer como um publicador é definir que tipo de solução sua empresa está oferecendo. O Azure Marketplace dá suporte a vários tipos diferentes de soluções e cada uma delas requer um conjunto ligeiramente diferente de trabalho para publicar com êxito no Marketplace.

**Tipos de soluções:**

- Serviços de dados
- Serviços para Desenvolvedores
- Máquinas Virtuais
- Modelos de solução

Algumas etapas são compartilhadas entre os diferentes tipos de soluções. Isso fornece uma breve visão geral das etapas que você precisará para concluir qualquer tipo de solução.

**Antes de começar qualquer trabalho no Azure Marketplace, você deve ser pré-aprovado. Não aplicável para publicadores de serviço de dados.**

|| Imagem da Máquina Virtual | Serviço de desenvolvedor | Serviço de dados | Modelo de solução | |---|---|---|---|---| | **Obter pré-aprovação** | [Certificado pelo Microsoft Azure][link-certification] | [Certificado pelo Microsoft Azure][link-certification] | n/a | [Certificado Pelo Microsoft Azure][link-certification] | | **Etapa 1. Registrar conta do vendedor** | [Conta do vendedor da Microsoft: criação e registro][link-accts] | [Conta do vendedor da Microsoft: criação e registro][link-accts] | [Conta do vendedor da Microsoft: criação e registro][link-accts] | [Conta do vendedor da Microsoft: criação e registro][link-accts] | |**Etapa 2. Criar sua oferta**| [Pré-requisitos gerais não técnicos](marketplace-publishing-pre-requisites.md)| [Pré-requisitos gerais não técnicos](marketplace-publishing-pre-requisites.md)| [Pré-requisitos gerais não técnicos](marketplace-publishing-pre-requisites.md)| [Pré-requisitos gerais não técnicos](marketplace-publishing-pre-requisites.md)| | | [Pré-requisitos técnicos VM][link-single-vm-prereq] | Pré-requisitos técnico do serviço do desenvolvedor | Pré-requisitos técnicos de serviço de dados | [Pré-requisitos técnicos do modelo de solução](marketplace-publishing-solution-template-creation-prerequisites.md) | | |[a imagem da VM publicando guia][link-single-vm] | Guia de publicação do serviço de desenvolvedor | Guia de publicação do modelo de solução | [Guia de publicação do modelo de solução](marketplace-publishing-solution-template-creation.md) | | | [Guia de conteúdo de Marketing do Azure Marketplace][link-pushstaging] | [Guia de conteúdo de Marketing do Azure Marketplace][link-pushstaging] | [Guia de conteúdo de Marketing do Azure Marketplace][link-pushstaging] | [Guia de conteúdo de Marketing do Azure Marketplace][link-pushstaging] | | **Etapa 3. Envie por push sua oferta para Preparação** | [Sua oferta VM na Preparação](marketplace-publishing-vm-image-test-in-staging.md) | Teste sua oferta de serviço do desenvolvedor na Preparação | Teste sua oferta de serviço de dados em Preparação | [Teste o seu Modelo de Solução em Preparação](marketplace-publishing-solution-template-test-in-staging.md) | | **Etapa 4. Implante sua oferta no Marketplace** | [Implante sua oferta no Marketplace][link-pushprod] | [Implante sua oferta no Marketplace][link-pushprod] | [Implante sua oferta no Marketplace][link-pushprod] | [Implante sua oferta no Marketplace][link-pushprod] |

## Suporte
- [Obtenha suporte como um Editor][suppt-general]
- [Entendendo o relatório de Percepções do Vendedor][suppt-rpt-insights]
- [Entendendo os relatórios de pagamento][suppt-rpt-payouts]
- [Solução comuns de problemas de publicação no Marketplace][suppt-common]

## Recursos adicionais
- Para saber mais sobre os Portais usados, visite [Portais que você precisará](marketplace-publishing-portals.md)

**Máquinas virtuais**

- [Configurando o PowerShell do Azure](marketplace-publishing-powershell-setup.md)
- [Criando uma máquina virtual no local da imagem VM](marketplace-publishing-vm-image-creation-on-premise.md)
- [Criar uma máquina virtual que executa o Windows no Portal de Visualização do Azure](../virtual-machines-windows-tutorial/)

[suppt-general]: marketplace-publishing-get-publisher-support.md
[suppt-rpt-insights]: marketplace-publishing-report-seller-insights.md
[suppt-rpt-payouts]: marketplace-publishing-report-payout.md
[suppt-common]: marketplace-publishing-support-common-issues.md
[link-certification]: marketplace-publishing-azure-certification.md
[link-accts]: marketplace-publishing-accounts-creation-registration.md
[link-single-vm]: marketplace-publishing-vm-image-creation.md
[link-single-vm-prereq]: marketplace-publishing-vm-image-creation-prerequisites.md
[link-multi-vm]: marketplace-publishing-solution-template-creation.md
[link-multi-vm-prereq]: marketplace-publishing-solution-template-creation-prerequisites.md
[link-datasvc]: marketplace-publishing-data-service-creation.md
[link-datasvc-prereq]: marketplace-publishing-data-service-creation-prerequisites.md
[link-devsvc]: marketplace-publishing-dev-service-creation.md
[link-devsvc-prereq]: marketplace-publishing-dev-service-creation-prerequisites.md
[link-pushstaging]: marketplace-publishing-push-to-staging.md
[link-pushprod]: marketplace-publishing-push-to-production.md

<!---HONumber=Oct15_HO3-->