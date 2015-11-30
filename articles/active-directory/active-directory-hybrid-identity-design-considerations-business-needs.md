<properties
	pageTitle="Considerações de design da identidade híbrida do Active Directory do Azure - determinar os requisitos de identidade | Microsoft Azure"
	description="Identifique as necessidades de negócios da empresa que levarão você definir os requisitos para o design de identidade híbrida."
	documentationCenter=""
	services="active-directory"
	authors="billmath"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
	ms.date="11/11/2015"
	ms.author="billmath"/>

# Determinar requisitos de identidade para sua solução de identidade híbrida
A primeira etapa na criação de uma solução de identidade híbrida é determinar os requisitos da organização de negócios que aproveitará essa solução. A identidade híbrida começa como uma função de suporte (ela dá suporte para todas as outras soluções de nuvem fornecendo autenticação) e prossegue fornecendo recursos novos e interessantes que desbloqueiam novas cargas de trabalho para os usuários. Essas cargas de trabalho ou os serviços que você deseja adotar para seus usuários determinarão os requisitos de design de identidade híbrida. Esses serviços e cargas de trabalho precisam aproveitar a identidade híbrida no local e na nuvem.

Você precisa abordar esses aspectos principais do negócio para entender o que é um requisito agora e quais são os planos da empresa para o futuro. Se você não tiver a visualização da estratégia de longo prazo para o design de identidade híbrida, a probabilidade é a de que sua solução não seja escalonável conforme o crescimento e a mudança nas necessidades dos negócios. O diagrama abaixo mostra um exemplo de uma arquitetura de identidade híbrida e cargas de trabalho que estão sendo desbloqueadas para os usuários. Isso é apenas um exemplo de todos os recursos novos que podem ser desbloqueados e entregues com uma estratégia de identidade híbrida sólida.
 
Alguns componentes que são parte da arquitetura de identidade híbrida ![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## Determinar as necessidades de negócios
Cada empresa terá requisitos diferentes, mesmo que essas empresas façam parte do mesmo setor; os requisitos reais dos negócios podem variar. Você ainda pode aproveitar as práticas recomendadas do setor, mas acaba sendo as necessidades da empresa que levarão você a definir os requisitos para o design de identidade híbrida.

Não deixe de responder às seguintes perguntas para identificar suas necessidades de negócios:

- Sua empresa está procurando reduzir os custos operacionais de TI?
- Sua empresa está procurando proteger ativos de nuvem (aplicativos SaaS, infraestrutura)?
- Sua empresa está procurando modernizar sua TI?
  - Seus usuários são mais exigentes e exigem da TI que esta crie exceções na sua rede de perímetro para permitir outro tipo de tráfego a fim de acessar recursos diferentes?
  - Sua empresa tem aplicativos herdados que precisavam ser publicados para esses usuários modernos, mas não são fáceis de reescrever?
  - Sua empresa precisa realizar todas essas tarefas e colocá-las sob controle ao mesmo tempo?
- Sua empresa está procurando proteger as identidades dos usuários e reduzir riscos trazendo novas ferramentas que utilizam a experiência de segurança do Microsoft Azure no local?
- Sua empresa está tentando se livrar das temidas contas "externas" no local e movê-las para a nuvem onde não são mais uma ameaça inativa dentro do seu ambiente local?

## Analisar a infraestrutura de identidades no local
Agora que você tem uma ideia sobre os requisitos de negócios da empresa, precisa avaliar sua infraestrutura de identidade no local. Essa avaliação é importante para definir os requisitos técnicos para integrar sua solução de identidade atual ao sistema de gerenciamento de identidade de nuvem. Não deixe de responder às seguintes perguntas:

- Que solução de autenticação e autorização sua empresa usa no local? 
- Sua empresa possui atualmente algum serviço de sincronização local?
- Sua empresa usa qualquer IdP (provedor de identidade de terceiros)?

Você também precisa estar ciente dos serviços de nuvem que sua empresa pode ter. É muito importante executar uma avaliação para entender a integração atual com os modelos SaaS, IaaS ou PaaS em seu ambiente. Não deixe de responder às seguintes perguntas durante essa avaliação: - Sua empresa tem alguma integração com um provedor de serviço de nuvem? - Em caso afirmativo, quais serviços estão sendo usados? - A integração está atualmente em produção ou é um piloto?


>[AZURE.NOTE]Se você não tiver um mapeamento preciso de todos os seus aplicativos e serviços de nuvem, pode usar a ferramenta Cloud App Discovery. Essa ferramenta pode fornecer a seu departamento de TI visibilidade de todos os negócios da organização e aplicativos de nuvem do consumidor. Isso facilita ainda mais a descoberta de TI de sombra em sua organização, incluindo detalhes sobre padrões de uso e quaisquer usuários que acessam seus aplicativos na nuvem. Para acessar essa ferramenta, visite [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## Avaliar os requisitos de integração de identidade
Em seguida, você precisa avaliar os requisitos de integração de identidade. Essa avaliação é importante para definir os requisitos técnicos de como os usuários serão autenticados, qual será a aparência da presença da organização na nuvem, como a organização permitirá a autorização e como será a experiência do usuário. Não deixe de responder às seguintes perguntas:

- Sua organização usará federação, autenticação padrão ou ambos?
- A federação é um requisito? Pelo seguinte:
 - SSO baseada em Kerberos
 - Sua empresa tem um aplicativo local (seja criado internamente ou de terceiros) que usa SAML ou recursos de federação semelhantes.
 - MFA usando cartões inteligentes. RSA SecurID, etc.
 - Regras de acesso para cliente que abordam as perguntas a seguir:
     1. Posso bloquear todo o acesso externo ao Office 365 baseado no endereço IP do cliente?
     1. Posso bloquear todo o acesso externo ao Office 365 exceto o ActiveSync do Exchange?
     1. Posso bloquear todo o acesso externo ao Office 365, exceto aplicativos baseados em navegador (OWA, SPO)
     1. Pode bloquear todo o acesso externo ao Office 365 de membros de grupos do AD designados
- Preocupações de auditoria/segurança
- Investimento já existente na autenticação federada
- Qual nome a nossa organização usará para nosso domínio na nuvem?
- A organização tem um domínio personalizado?
    1. Esse domínio é público e verificável facilmente usando DNS?
    1. Se não for, você tem um domínio público que possa ser usado para registrar um UPN alternativo no AD?
- Os identificadores de usuário são consistentes para a representação de nuvem? 
- A empresa possui aplicativos que requerem integração com serviços de nuvem?
- A organização possui vários domínios e todos eles usarão a autenticação federada ou padrão?

## Avaliar aplicativos executados em seu ambiente
Agora que você tem uma ideia sobre suas instalações e infraestrutura de nuvem, precisa avaliar os aplicativos executados nesses ambientes. Essa avaliação é importante para definir os requisitos técnicos para integrar esses aplicativos para o sistema de gerenciamento de identidade na nuvem. Não deixe de responder às seguintes perguntas:

- Onde nossos aplicativos residirão?
- Os usuários acessarão os aplicativos no local? Na nuvem? Ou ambos?
- Há planos para levar as cargas de trabalho de aplicativo existentes e movê-las para a nuvem?
- Há planos para desenvolver novos aplicativos que residirão no local ou na nuvem que usará autenticação de nuvem?

## Avaliar os requisitos do usuário
Você também precisa avaliar os requisitos do usuário. Essa avaliação é importante para definir as etapas que serão necessárias para a integração e para ajudar os usuários com a transição para a nuvem. Não deixe de responder às seguintes perguntas:

- Os usuários acessarão aplicativos no local?
- Os usuários acessarão aplicativos na nuvem?
- Como os usuários normalmente entram em seu ambiente local?
- Como os usuários entrarão na nuvem?

>[Observação] Faça anotações de cada resposta e entenda o raciocínio por trás da resposta. [Determinar os requisitos de resposta a incidentes](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) vai avaliar as opções disponíveis e os prós e contras de cada opção. Depois de responder a essas perguntas, você selecionará a opção que melhor se ajusta às necessidades da sua empresa.

## Próximas etapas
[Determinar os requisitos de sincronização de diretório](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## Consulte também
[Visão geral de considerações de design](active-directory-hybrid-identity-design-considerations-directory-overview.md)

<!---HONumber=Nov15_HO3-->