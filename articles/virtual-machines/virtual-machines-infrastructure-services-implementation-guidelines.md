<properties 
	pageTitle="Diretrizes de implementação dos Serviços de Infraestrutura do Azure" 
	description="Saiba mais sobre as principais diretrizes de design e implementação para a implantação de uma carga de trabalho de TI em serviços de infraestrutura do Azure." 
	documentationCenter=""
	services="virtual-machines" 
	authors="JoeDavies-MSFT" 
	manager="timlt" 
	editor=""/>

<tags 
	ms.service="virtual-machines" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/05/2015" 
	ms.author="josephd"/>

# Diretrizes de implementação dos Serviços de Infraestrutura do Azure
 
Essas diretrizes se concentram nas principais decisões de design e nas tarefas para determinar a variedade de recursos que estão envolvidos na maioria das implementações de serviços de infraestrutura do Azure.

O Azure é uma excelente plataforma para implementar a configuração de verificação de conceito, pois requer muito pouco investimento para testar uma abordagem específica para a implementação de soluções. No entanto, você deve ser capaz de distinguir entre as práticas fáceis de uma verificação de conceito e as práticas mais difíceis e detalhadas para uma implementação totalmente funcional e pronta para a produção de uma carga de trabalho de TI.

Este guia identifica muitas áreas para as quais o planejamento é essencial para o sucesso de uma infraestrutura de TI ou carga de trabalho no Azure. Além disso, ele ajuda na implementação de soluções na plataforma Azure fornecendo um pedido para a criação dos recursos necessários. Embora haja alguma flexibilidade, a Microsoft recomenda que você aplique esta ordem ao seu planejamento e à sua tomada de decisões.

1.	Convenções de nomenclatura
2.	Assinaturas e contas
3.	Armazenamento
4.	Redes virtuais
5.	Serviços de Nuvem
6.	Conjuntos de Disponibilidade
7.	Máquinas virtuais

Estabelecer uma boa convenção de nomenclatura, bem como seguir uma ordem específica e sistemática para criar os recursos no Azure, reduz substancialmente a carga administrativa e aumenta as chances de sucesso de qualquer projeto de implementação.

> [AZURE.NOTE]Os grupos de afinidade não são descritos, pois seu uso foi substituído. Para saber mais, confira [Sobre VNets regionais e grupos de afinidade](https://msdn.microsoft.com/library/azure/jj156085.aspx).

## 1. Convenções de nomenclatura

Antes de criar qualquer coisa no Azure, deve haver uma boa convenção de nomenclatura. Uma convenção de nomenclatura garante que todos os recursos tenham um nome previsível, o que ajuda a reduzir a carga administrativa associada com o gerenciamento desses recursos.

Você pode seguir um conjunto específico de convenções de nomenclatura definido para toda a organização ou para uma determinada conta ou assinatura do Azure. Embora seja fácil para os indivíduos das organizações estabelecerem regras implícitas ao trabalharem com os recursos do Azure, quando uma equipe precisa trabalhar em um projeto no Azure, esse modelo não se adapta bem.

Vocês devem concordar sobre o conjunto de convenções de nomenclatura antecipadamente. Há algumas considerações sobre convenções de nomenclatura que abrangem vários conjuntos de regras que compõem essas convenções.

### Afixos

Ao criar determinados recursos, o Azure usa alguns padrões para simplificar o gerenciamento dos recursos associados a esses recursos. Por exemplo, ao criar a primeira máquina virtual para um novo serviço de nuvem, o Portal de Gerenciamento do Azure tentará usar o nome da máquina virtual como o nome de um novo serviço de nuvem da máquina virtual.

Portanto, é útil identificar os tipos de recursos que precisam de um afixo para identificar esse tipo. Além disso, especifique claramente se o afixo deverá ficar:

- No início do nome (prefixo)
- No final do nome (sufixo)

Por exemplo, veja dois nomes possíveis para um serviço de nuvem que hospeda um mecanismo de cálculo:

- Svc-CalculationEngine (prefixo)
- CalculationEngine-Svc (sufixo)

Os afixos podem se referir a diversos aspectos que descrevam os recursos em questão. A tabela a seguir mostra alguns exemplos normalmente usados.

Aspecto | Exemplos | Observações
--- | --- | ---
Ambiente | dev, stg, prod | Dependendo da finalidade e do nome de cada ambiente.
Local | usw (Oeste dos EUA), use (Leste dos EUA 2) | Dependendo da região do datacenter ou da região da organização.
Componente, serviço ou produto do Azure | Svc para serviço de nuvem, VNet para rede virtual | Dependendo do produto para o qual o recurso oferece suporte.
Função | SQL, ora, sp, iis | Dependendo da função da VM.
Instância | 01, 02, 03 etc. | Para recursos que podem ter mais de uma instância. Por exemplo, servidores Web com balanceamento de carga em um serviço de nuvem.
		
Ao estabelecer as convenções de nomenclatura, verifique se elas determinam claramente quais afixos devem ser usados para cada tipo de recurso e em qual posição (sufixo versus prefixo).

### Datas

Muitas vezes, é importante determinar a data de criação do nome de um recurso. A Microsoft recomenda o formato de data AAAAMMDD. Esse formato garante não apenas que a data completa seja registrada, mas também que dois recursos cujos nomes diferem apenas na data sejam classificados em ordem alfabética e em ordem cronológica ao mesmo tempo.

### Recursos de nomenclatura
Você deve definir cada tipo de recurso na convenção de nomenclatura, que deve ter regras para definir como atribuir nomes a cada recurso criado. Essas regras devem se aplicar a todos os tipos de recursos, por exemplo:

- Assinaturas
- Contas
- Contas de armazenamento
- Redes virtuais
- Sub-redes
- Conjuntos de Disponibilidade
- Serviços de Nuvem
- Máquinas Virtuais
- Pontos de extremidade
- Grupos de segurança de rede
- Funções

Os nomes devem ser os mais descritivos possíveis para garantir que o nome forneça informações suficientes para determinar a qual recurso se refere.

### Nomes de computadores

Quando os administradores criam uma máquina virtual, o Microsoft Azure exige que eles forneçam um nome de máquina virtual. O Microsoft Azure usará o nome da máquina virtual como o nome do recurso da máquina virtual do Azure. O Azure usará o mesmo nome do computador para o sistema operacional instalado na máquina virtual. No entanto, esses nomes nem sempre poderão ser os mesmos.

Nos casos em que uma máquina virtual é criada a partir de um arquivo .VHD que já contenha um sistema operacional, o nome da máquina virtual no Microsoft Azure pode ser diferente do nome do computador do sistema operacional da máquina virtual. Essa situação pode adicionar um grau de dificuldade para o gerenciamento de máquinas virtuais, o que não é recomendável. Verifique sempre se o nome do recurso de máquina virtual do Azure é igual ao nome do computador atribuído ao sistema operacional daquela máquina virtual.

Recomendamos que o nome da Máquina Virtual do Azure seja igual ao nome do computador do sistema operacional subjacente. Por isso, siga as regras de nomenclatura do NetBIOS, descritas em [Convenções de nomenclatura de computadores do Microsoft NetBIOS](https://support.microsoft.com/kb/188997/).

### Nomes de contas de armazenamento

As contas de armazenamento têm regras especiais para os nomes. Você só pode usar letras minúsculas e números e o nome atribuído, concatenado ao serviço (blob, tabela ou fila) e o domínio padrão (core.windows.net) deve processar um nome DNS exclusivo e globalmente válido. Por exemplo, quando a conta de armazenamento é chamada de minhacontadearmazenamento, as URLs resultantes abaixo devem ser nomes DNS válidos e exclusivos: 

- minhacontadearmazenamento.blob.core.windows.net
- minhacontadearmazenamento.table.core.windows.net
- minhacontadearmazenamento.queue.core.windows.net

Além disso, as contas de armazenamento podem aproveitar os contêineres. Elas devem seguir as convenções de nomenclatura, conforme descrito em [Nomenclatura e referência de contêineres, blobs e metadados](https://msdn.microsoft.com/library/azure/dd135715.aspx).

### Nomes de Blocos de Construção do Azure

Os Blocos de Construção do Azure são serviços de nível de aplicativo oferecidos pelo Azure, normalmente aos aplicativos que utilizam os recursos de PaaS, embora os recursos de IaaS possam aproveitar alguns deles, como o SQL Azure, Traffic Manager e outros.

Esses serviços dependem de uma matriz de artefatos que são criados e registrados no Azure. Eles também precisam ser considerados nas convenções de nomenclatura.

### Recapitulação das diretrizes de implementação de convenções de nomenclatura

Decisão:

- Quais são as suas convenções de nomenclatura de recursos do Azure? 

Tarefa:

- Defina as convenções de nomenclatura em termos de afixos, hierarquia, valores de cadeia de caracteres e outras políticas para recursos do Azure.

## 2. Assinaturas e contas

Para trabalhar com o Azure, você precisa de uma ou mais assinaturas do Azure. Os recursos, como serviços de nuvem ou máquinas virtuais, existem no contexto dessas assinaturas.

- Os clientes corporativos normalmente têm um Registro Enterprise, que é o recurso mais alto na hierarquia e está associado a uma ou mais contas. 
- Para consumidores e clientes sem Registro Enterprise, o recurso mais alto é a Conta.
- As assinaturas estão associadas a contas e pode haver uma ou mais assinaturas por conta. O Azure registra as informações de cobrança no nível de assinatura.

Devido ao limite de dois níveis de hierarquia na relação de Conta/Assinatura, é importante alinhar a convenção de nomenclatura das contas e assinaturas às necessidades de cobrança. Por exemplo, se uma empresa global usar o Azure, poderá optar por ter uma conta por região e gerenciar as assinaturas no nível de região.

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/sub01.png)
  
Por exemplo, você pode usar esta estrutura.

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/sub02.png)
  
Seguindo o mesmo exemplo, se uma região decide ter mais de uma assinatura associada a um determinado grupo, então a convenção de nomenclatura deve incorporar uma forma de codificar os adicionais no nome da conta ou da assinatura. Esta organização permite manipular os dados de faturamento para gerar os novos níveis de hierarquia durante os relatórios de cobrança.

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/sub03.png)

A organização pode ter esta aparência.

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/sub04.png)
 
A Microsoft fornece uma cobrança detalhada por meio de um arquivo para download, para uma única conta ou para todas as contas em um contrato empresarial. Você pode processar esse arquivo, por exemplo, usando o Excel. Esse processo faria a ingestão dos dados, a partição dos recursos que codificam mais de um nível da hierarquia em colunas separadas e usaria uma tabela dinâmica ou do PowerPivot para fornecer recursos dinâmicos de emissão de relatórios.

### Recapitulação das diretrizes de implementação de contas e assinaturas

Decisão:

- De que conjunto de assinaturas e contas você precisa para hospedar a infraestrutura ou carga de trabalho de TI?

Tarefa:

- Crie o conjunto de assinaturas e contas usando a sua convenção de nomenclatura.

## 3. Armazenamento

O armazenamento é uma parte integrante de todas as soluções do Azure, uma vez que não apenas fornece serviços no nível de aplicativo, mas também é parte da infraestrutura de suporte a máquinas virtuais.
 
Há dois tipos de armazenamento disponíveis no Azure: O armazenamento padrão dá acesso a armazenamento de Blob, Tabela, Fila e Arquivo. O armazenamento Premium é projetado para aplicativos de alto desempenho, como SQL Servers em um cluster AlwaysOn, e atualmente oferece suporte somente para discos de Máquina Virtual do Azure.

As contas de armazenamento são vinculadas às metas de escalabilidade. Confira [Assinatura do Microsoft Azure e limites de serviços, cotas e restrições](../azure-subscription-service-limits.md#storage-limits) para se familiarizar com os limites atuais de armazenamento do Azure. Veja também [Metas de desempenho e escalabilidade do Armazenamento do Azure](../storage-scalability-targets.md).

O Azure cria máquinas virtuais com um disco do sistema operacional, um disco temporário e zero ou mais discos de dados opcionais. O disco do sistema operacional e os discos de dados são blobs do Azure, enquanto o disco temporário é apoiado pelo serviço de armazenamento local referente ao nó onde o computador reside. Isso torna o disco temporário impróprio para dados que devam persistir durante uma reciclagem de sistema, uma vez que a máquina pode migrar silenciosamente de um nó para outro, perdendo os dados desse disco. Não armazene nada na unidade temporária.

Os discos do sistema operacional e os discos de dados têm um tamanho máximo de 1023 GB, já que o tamanho máximo de um blob é de 1024 GB e deve conter os metadados (rodapé) do arquivo VHD (um GB tem 10243 bytes). Você pode implementar a distribuição de discos no Windows para ultrapassar esse limite.

### Discos distribuídos
Além de fornecer a capacidade de criar discos com mais de 1023 GB, em muitos casos, o uso da distribuição para discos de dados poderá melhorar o desempenho, permitindo que vários blobs façam o armazenamento de um único volume. Com isso é feita a paralelização da E/S necessária para gravar e ler dados de um único disco.

O Azure impõe limites na quantidade de discos de dados e largura de banda disponíveis, dependendo do tamanho da máquina virtual. Para obter detalhes, confira [Tamanhos de máquinas virtuais e serviços de nuvem do Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Se você estiver usando a distribuição de disco para os discos de dados do Azure, considere as seguintes diretrizes:

- Os discos de dados devem ter sempre o tamanho máximo (1023 GB)
- Anexe o número máximo de discos de dados permitidos para o tamanho da máquina virtual
- Use a Configuração de Espaços de Armazenamento
- Use a Configuração de Distribuição de Armazenamento
- Evite usar opções de cache de disco de dados do Azure (política de cache = Nenhuma)

Para saber mais, confira [Espaços de armazenamento: design para desempenho](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).

### Várias contas de armazenamento

O uso de várias contas de armazenamento para apoiar os discos associados com muitas máquinas virtuais garante que a E/S agregada desses discos fique bem abaixo das metas de escalabilidade de cada uma dessas contas de armazenamento.

A Microsoft recomenda que você inicie com a implantação de uma máquina virtual para cada conta de armazenamento.

### Design de layout de armazenamento

Para implementar essas estratégias de implementação do subsistema de discos das máquinas virtuais com bom desempenho, a infraestrutura ou carga de trabalho de TI geralmente utilizará muitas contas de armazenamento. Essas contas poderão hospedar muitos blobs VHD. Em alguns casos, mais de um blob é associado a um único volume em uma máquina virtual.

Essa situação pode adicionar complexidade às tarefas de gerenciamento. É essencial criar uma estratégia sólida de armazenamento, incluindo uma nomenclatura apropriada dos discos subjacentes e dos blobs VHD associados.

### Recapitulação das diretrizes de implementação de armazenamento

Decisões:

- Você precisa de distribuição de discos para criar discos com mais de 500 TB?
- Você precisa de distribuição de discos para obter o desempenho ideal para sua carga de trabalho?
- De que conjunto de contas de armazenamento você precisa para hospedar a infraestrutura ou a carga de trabalho de TI?

Tarefa:

- Crie o conjunto de contas de armazenamento usando a sua convenção de nomenclatura. Você pode usar o Portal de Visualização do Azure, o Portal de Gerenciamento do Azure ou o cmdlet do PowerShell **New-AzureStorageAccount**.

## 4. Serviços de Nuvem

Os serviços de nuvem são um componente fundamental no Azure, tanto para os serviços de PaaS como de IaaS. Para PaaS, os serviços de nuvem representam uma associação de funções cujas instâncias podem se comunicar entre si. Os serviços de nuvem estão associados a um endereço IP virtual público (VIP) e um balanceador de carga, que usa o tráfego de entrada da Internet e faz o balanceamento da carga para as funções configuradas para receber esse tráfego.

No caso de IaaS, os serviços de nuvem oferecem uma funcionalidade semelhante, embora, na maioria dos casos, a funcionalidade do balanceador de carga seja usada para encaminhar o tráfego para determinadas portas TCP ou UDP da Internet para as várias máquinas virtuais desse serviço de nuvem.

Os nomes dos serviços de nuvem são especialmente importantes em IaaS porque o Azure os usa como parte da convenção de nomenclatura padrão para discos. O nome do serviço de nuvem pode conter apenas letras, números e hifens. O primeiro e o último caractere no campo devem ser uma letra ou um número.

O Microsoft Azure expõe os nomes dos serviços de nuvem, já que eles são associados ao VIP, no domínio "cloudapp.net". Para uma melhor experiência do usuário do aplicativo, deve ser configurado um nome intuitivo conforme necessário para substituir o nome totalmente qualificado do serviço de nuvem. Isso normalmente é feito com um registro CNAME no seu DNS público que mapeia o nome DNS público do recurso (por exemplo, www.contoso.com) para o nome DNS do serviço de nuvem que hospeda o recurso (por exemplo, o serviço de nuvem que hospeda os servidores Web de www.contoso.com).

Além disso, a convenção de nomenclatura usada para os serviços de nuvem pode precisar tolerar exceções porque os nomes de serviços de nuvem devem ser exclusivos entre todos os outros serviços de nuvem do Microsoft Azure, independentemente do locatário do Microsoft Azure.

As assinaturas do Azure podem suportar um máximo de 200 serviços em nuvem.

### Recapitulação das diretrizes de implementação de serviços de nuvem

Decisão:

- De que conjunto de serviços de nuvem você precisa para hospedar a infraestrutura ou a carga de trabalho de TI? 

Tarefa:

- Crie o conjunto de serviços de nuvem usando a sua convenção de nomenclatura. Você pode usar o Portal de Gerenciamento do Azure ou o cmdlet do PowerShell **New-AzureService**.

## 5. Redes Virtuais

A próxima etapa lógica é criar as redes virtuais necessárias para dar suporte às comunicações entre as máquinas virtuais na solução. Embora seja possível hospedar várias máquinas virtuais de uma carga de trabalho de TI apenas em um serviço de nuvem, as redes virtuais são recomendadas.

As redes virtuais são um contêiner de máquinas virtuais para as quais você também pode especificar sub-redes, endereçamento personalizado e opções de configuração de DNS. As máquinas virtuais dentro da mesma rede virtual podem se comunicar diretamente com outros computadores na mesma rede virtual, independentemente do serviço de nuvem do qual eles são membros. Dentro da rede virtual, essa comunicação permanece privada, sem necessidade de a comunicação percorrer os pontos de extremidade públicos. Essa comunicação pode ocorrer por meio do endereço IP ou pelo nome, usando um servidor DNS instalado na rede virtual ou no local, se a máquina virtual estiver conectada à rede corporativa.

### Conectividade de site
Se os computadores e usuários locais não exigem conectividade contínua com as máquinas virtuais em uma rede virtual do Azure, crie uma rede virtual somente em nuvem.

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/vnet01.png)
 
Normalmente isso é usado para cargas de trabalho para a Internet, como um servidor Web baseado na Internet. Você pode gerenciar essas máquinas virtuais usando conexões de Área de Trabalho Remota, sessões remotas do PowerShell, conexões Secure Shell (SSH) e conexões VPN ponto a site.

Como elas não se conectam à sua rede local, as redes virtuais somente em nuvem podem usar qualquer parte do espaço de endereço IP privado.

Se os computadores e usuários locais exigirem conectividade contínua para máquinas virtuais em uma rede virtual do Azure, crie uma rede virtual entre locais e conecte-se à sua rede local com uma Rota Expressa ou uma conexão VPN site a site.

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/vnet02.png)
 
Nessa configuração, a rede virtual do Azure é essencialmente uma extensão baseada em nuvem da sua rede local.

Como eles se conectam à sua rede local, as redes virtuais entre instalações devem usar uma parte do espaço de endereço usado pela sua organização que é exclusiva e a infraestrutura de roteamento deve oferecer suporte ao roteamento do tráfego para essa parte, encaminhando-o para o seu dispositivo VPN local.

Para permitir que os pacotes viajem da sua rede virtual entre instalações para a sua rede local, você deve configurar o conjunto de prefixos de endereços locais relevantes como parte da definição da Rede Local para a rede virtual. Dependendo do espaço de endereço da rede virtual e do conjunto de locais relevantes na instalação, pode haver vários prefixos de endereço na Rede Local.

Você pode converter uma rede virtual somente em nuvem em uma rede virtual entre locais, mas provavelmente será necessário renumerar seu espaço de endereço de rede virtual, suas sub-redes e as máquinas virtuais que usam endereços IP estáticos atribuídos pelo Azure, conhecidos como IPs Dinâmicos (DIPs). Portanto, considere com atenção o tipo de redes virtuais de que você precisa (somente em nuvem versus entre instalações) antes de criá-las.

### Sub-redes
As sub-redes permitem organizar os recursos que estão relacionados, seja logicamente (por exemplo, uma sub-rede para máquinas virtuais associadas ao mesmo aplicativo) ou fisicamente (por exemplo, uma sub-rede por serviço de nuvem), ou empregar técnicas de isolamento de sub-redes para maior segurança.

Para redes virtuais entre instalações, você deve criar sub-redes com as mesmas convenções que você usa para recursos locais, tendo em mente que **o Azure sempre usa os três primeiros endereços IP do espaço de endereços de cada sub-rede**. Para determinar o número de endereços necessários para a sub-rede, conte o número de máquinas virtuais de que você precisa agora, faça uma estimativa do crescimento futuro e, em seguida, use a tabela a seguir para determinar o tamanho da sub-rede.
 
Número de máquinas virtuais necessárias | Número de bits de host necessários | Tamanho da sub-rede 
--- | --- | --- 
1 – 3 | 3 | / 29
4 – 11 | 4 | / 28
12 – 27 | 5 | / 27
28-59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE]Para sub-redes locais normais, o número máximo de endereços de host para uma sub-rede com n bits de host é 2<sup>n</sup> – 2. Para uma sub-rede do Azure, o número máximo de endereços de host para uma sub-rede com n bits de host é 2<sup>n</sup> – 5 (2 mais 3 para os endereços que o Azure usa em cada sub-rede).

Se você escolher um tamanho de sub-rede que seja muito pequeno, precisará renumerar e reimplantar as máquinas virtuais na sub-rede.

### Recapitulação das diretrizes de implementação de redes virtuais

Decisões:

- Que tipo de rede virtual é necessário para hospedar sua infraestrutura ou carga de trabalho de TI (somente em nuvem ou entre instalações)?
- Para redes virtuais entre instalações, quanto espaço de endereço você precisa para hospedar as sub-redes e máquinas virtuais agora e para uma expansão razoável no futuro?

Tarefas:

- Defina o espaço de endereço da rede virtual.
- Defina o conjunto de sub-redes e o espaço de endereço para cada uma.
- Para redes virtuais entre instalações, defina o conjunto de espaços de endereços de redes locais para as instalações que as máquinas virtuais da rede virtual precisam acessar.
- Crie a rede virtual usando a sua convenção de nomenclatura. Você pode usar o Portal de Visualização do Azure ou o Portal de Gerenciamento do Azure.

## 6. Conjuntos de Disponibilidade

No Azure PaaS, os serviços de nuvem contêm uma ou mais funções que executam o código do aplicativo. As funções podem ter uma ou mais instâncias de máquinas virtuais que a malha provisiona automaticamente. A qualquer momento, o Azure pode atualizar as instâncias nessas funções, mas como elas fazem parte da mesma função, o Azure sabe que não deve atualizar todas ao mesmo tempo para impedir uma interrupção de serviço para a função.

No Azure IaaS, o conceito de função não é importante, já que cada máquina virtual de IaaS representa uma função com uma única instância. Para indicar ao Azure para não desativar dois ou mais computadores associados ao mesmo tempo (por exemplo, para atualizações do sistema operacional do nó onde eles residem), foi introduzido o conceito de conjuntos de disponibilidade. O conjunto de disponibilidade informa ao Azure para não desativar todos os computadores do mesmo conjunto de disponibilidade ao mesmo tempo para impedir uma interrupção de serviço. Os membros de máquina virtual de um conjunto de disponibilidade têm um contrato de nível de serviço com tempo de atividade de 99,95%.

Os conjuntos de disponibilidade devem fazer parte do planejamento de alta disponibilidade da solução. Um conjunto de disponibilidade é definido como o conjunto de máquinas virtuais em um único serviço de nuvem que têm o mesmo nome de conjunto de disponibilidade. Depois de criar os serviços de nuvem, você pode criar conjuntos de disponibilidade.

### Recapitulação das diretrizes de implementação de conjuntos de disponibilidade

Decisão:

- Quantos conjuntos de disponibilidade são necessários para as várias funções e camadas da sua infraestrutura ou em carga de trabalho de TI?

Tarefa:

- Defina o conjunto de conjuntos de disponibilidade usando a sua convenção de nomenclatura. Você pode associar uma máquina virtual a um conjunto quando criar as máquinas virtuais ou associar uma máquina virtual a um conjunto de disponibilidade depois que a máquina virtual tiver sido criada.

## 7. Máquinas Virtuais

No Azure PaaS, o Azure gerencia as máquinas virtuais e seus discos associados. Você deve criar e nomear os serviços de nuvem e as funções e, em seguida, o Azure criará instâncias associadas a essas funções. No caso do Azure IaaS, cabe a você fornecer nomes para os serviços de nuvem, máquinas virtuais e discos associados.

Para reduzir a carga administrativa, o Portal de Gerenciamento do Azure usará o nome do computador como uma sugestão para o nome padrão do serviço de nuvem associado (caso o cliente opte por criar um novo serviço de nuvem como parte do assistente de criação de máquinas virtuais).

Além disso, o Azure nomeia os discos e seus blobs VHD de apoio usando uma combinação do nome do serviço de nuvem, do nome do computador e da data de criação.

Em geral, o número de discos será muito maior do que a quantidade de máquinas virtuais. Tome cuidado ao manipular máquinas virtuais para evitar os discos órfãos. Além disso, os discos podem ser excluídos sem excluir o blob de suporte. Se esse for o caso, o blob permanecerá na conta de armazenamento até ser excluído manualmente.

### Recapitulação das diretrizes de implementação de máquinas virtuais

Decisão:

- De quantas máquinas virtuais você precisa para acomodar a infraestrutura ou a carga de trabalho de TI?

Tarefas:

- Defina o nome de cada máquina virtual usando a sua convenção de nomenclatura.
- Crie as suas máquinas virtuais com o Portal de Visualização do Azure, o Portal de Gerenciamento do Azure ou o cmdlet do PowerShell **New-AzureVM**.

## Exemplo de uma carga de trabalho de TI: o mecanismo de análise financeira Contoso

A Contoso Corporation desenvolveu um mecanismo de análise financeira de última geração com algoritmos proprietários de ponta para ajudar na negociação do mercado de futuros. Eles desejam disponibilizar esse mecanismo para seus clientes como um conjunto de servidores no Azure, que consistem em:

- Dois (e eventualmente mais) servidores Web baseados no IIS executando serviços Web personalizados em uma camada da Web
- Dois (e, eventualmente, mais) servidores de aplicativos baseados no IIS que executam os cálculos em uma camada de aplicativo
- Um cluster do SQL Server 2014 com grupos de disponibilidade AlwaysOn (dois servidores SQL e uma testemunha do nó principal) que armazena dados históricos e contínuos de cálculo em uma camada de banco de dados
- Dois controladores de domínio do Active Directory para uma floresta independente e um domínio na camada de autenticação, que é exigido pelo agrupamento do SQL Server
- Todos os servidores estão localizados em duas sub-redes; uma sub-rede front-end para os servidores Web e uma sub-rede de back-end para os servidores de aplicativos, um cluster do SQL server 2014 e controladores de domínio

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/example-tiers.png)
 
É preciso fazer o balanceamento de carga da entrada de tráfego seguro da Web dos clientes Contoso na Internet. O tráfego de solicitações de cálculo na forma de solicitações HTTP dos servidores Web precisa ser equilibrada entre os servidores de aplicativos. Além disso, o mecanismo deve ser projetado para alta disponibilidade.

O projeto resultante deve incorporar:

- Uma assinatura e uma conta do Contoso Azure
- Contas de armazenamento
- Uma rede virtual com duas sub-redes
- Um conjunto de serviços de nuvem
- Conjuntos de disponibilidade para os conjuntos de servidores com funções semelhantes
- Máquinas virtuais

Todos os itens acima seguirão estas convenções de nomenclatura Contoso:

- A Contoso usa [local]-[carga de trabalho TI]-[recurso Azure] como prefixo. Neste exemplo, "azfae" (Mecanismo de Análise Financeira do Azure) é o nome da carga de trabalho de TI e "use" (Leste dos EUA 2) é o local, já que a maioria dos clientes iniciais da Contoso está na Costa Leste dos Estados Unidos.
- As contas de armazenamento usam contosoazfaeusesa[descrição] Observe que a palavra contoso foi adicionada ao prefixo para garantir a exclusividade e que os nomes das contas de armazenamento não permitem o uso de hifens.
- Os serviços de nuvem usam contoso-azfae-use-cs-[descrição] Observe que ccontoso foi adicionada ao prefixo para garantir a exclusividade.
- As redes virtuais usam AZFAE-USE-VN[número].
- Os conjuntos de disponibilidade usam azfae-use-as-[função].
- Os nomes de máquinas virtuais usam azfae-use-vm -[vmname].

### Assinaturas e contas do Azure

A Contoso está usando sua assinatura do Enterprise, chamada Contoso Enterprise Subscription, para fornecer cobrança para essa carga de trabalho de TI.

### Contas de armazenamento

A Contoso determinou que precisavam duas contas de armazenamento:

- **contosoazfaeusesawebapp** para o armazenamento padrão dos servidores Web, servidores de aplicativos e controles de domínio e seus discos de dados extras
- **contosoazfaeusesasqlclust** para o armazenamento premium do cluster do SQL Server e os discos de dados extras

A Contoso criou as duas contas de armazenamento com estes comandos do PowerShell:

	New-AzureStorageAccount -StorageAccountName "contosoazfaeusesawebapp" -Location "East US 2"
	New-AzureStorageAccount -StorageAccountName "contosoazfaeusesasqlclust" -Location "East US 2" -Type Premium_LRS

### Uma rede virtual com sub-redes

Como a rede virtual não precisa de conectividade contínua com a rede local da Contoso, a Contoso decidiu por uma rede virtual somente em nuvem.

Criaram uma rede virtual somente em nuvem com as seguintes configurações usando o Portal de Visualização do Azure:

- Nome: AZFAE-USE-VN01
- Local: Leste dos EUA 2
- Espaço de endereço da rede virtual: 10.0.0.0/8
- Primeira sub-rede:
	- Nome: FrontEnd
	- Espaço de endereço: 10.0.1.0/24
- Segunda sub-rede:
	- Nome: BackEnd
	- Espaço de endereço: 10.0.2.0/24

### Serviços de Nuvem

A Contoso decidiu por dois serviços de nuvem:

- **contoso-azfae-use-cs-frontend** para os servidores Web front-end
- **contoso-azfae-use-cs-backend** para os servidores de aplicativos back-end, servidores de cluster do SQL Server e controladores de domínio

A Contoso criou os serviços de nuvem com estes comandos do PowerShell:

	New-AzureService -Service "contoso-azfae-use-cs-frontend" -Location "East US 2"
	New-AzureService -Service "contoso-azfae-use-cs-backend" -Location "East US 2"

### Conjuntos de Disponibilidade

Para manter a alta disponibilidade de todos os quatro níveis de seu mecanismo de análise financeira, a Contoso decidiu por quatro conjuntos de disponibilidade:

- **azfae-use-as-dc** para os controladores de domínio
- **azfae-use-as-web** para os servidores da Web
- **azfae-use-as-app** para os servidores de aplicativos
- **azfae-use-as-sql** para os servidores do cluster do SQL Server

Esses conjuntos de disponibilidade serão criados junto com as máquinas virtuais.

### Máquinas virtuais

A Contoso decidiu sobre os seguintes nomes para suas máquinas virtuais do Azure:

- **azfae-use-vm-dc01** para o primeiro controlador de domínio
- **azfae-use-vm-dc02** para o segundo controlador de domínio
- **azfae uso-vm-web01** para o primeiro servidor Web
- **azfae uso-vm-web02** para o segundo servidor Web
- **azfae uso-vm-app01** para o primeiro servidor de aplicativo
- **azfae uso-vm-app02** para o segundo servidor de aplicativo
- **azfae-use-vm-sql01** para o primeiro servidor SQL do cluster do SQL Server
- **azfae-use-vm-sql02** para o segundo servidor SQL do cluster do SQL Server
- **azfae-use-vm-sqlmn01** para a testemunha do nó principal do cluster do SQL Server

Veja abaixo a configuração resultante.

![](./media/virtual-machines-infrastructure-services-implementation-guidelines/example-config.png)
 
Essa configuração inclui:

- Uma rede virtual somente em nuvem com duas sub-redes (front-end e back-end)
- Dois serviços de nuvem
- Duas contas de armazenamento
- Quatro conjuntos de disponibilidade, um para cada camada do mecanismo de análise financeira
- As máquinas virtuais das quatro camadas
- Um conjunto de balanceamento de carga externo para tráfego da Web baseado em HTTPS da Internet para os servidores Web
- Um conjunto de balanceamento de carga interno para tráfego da Web dos servidores Web para os servidores de aplicativos

Esses comandos do PowerShell do Azure criam as máquinas virtuais nesta configuração para as contas de armazenamento criadas anteriormente, os serviços de nuvem e a rede virtual.

	#Specify the storage account for the web and application servers
	Set-AzureSubscription –SubscriptionName "Contoso Enterprise Subscription" -CurrentStorageAccountName "contosoazfaeusesawebapp"
	
	#Specify the cloud service name for the web servers
	$ServiceName="contoso-azfae-use-cs-frontend"
	
	#Get the image string for the latest version of the Windows Server 2012 R2 Datacenter image in the gallery
	$image= Get-AzureVMImage | where { $_.ImageFamily -eq "Windows Server 2012 R2 Datacenter" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
	
	#Create the first web server
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the first web server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-web01 -InstanceSize large -ImageName $image -AvailabilitySetName azfae-use-as-web
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password 
	$vm1 | Set-AzureSubnet -SubnetNames FrontEnd
	$vm1 | Add-AzureEndpoint -Name Web1 -Protocol tcp -LocalPort 443 -PublicPort 443 -LBSetName "WebSet" -DefaultProbe
	New-AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Create the second web server 
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the second web server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-web02 -InstanceSize Large -ImageName $image -AvailabilitySetName azfae-use-as-web
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password 
	$vm1 | Set-AzureSubnet -SubnetNames FrontEnd
	$vm1 | Add-AzureEndpoint -Name Web2 -Protocol tcp -LocalPort 443 -PublicPort 443 -LBSetName "WebSet" -DefaultProbe
	New-AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Specify the cloud service name for the application, SQL server, and authentication tiers
	$ServiceName="contoso-azfae-use-cs-backend"
	
	#Create the first domain controller server
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the first domain controller server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-dc01 -InstanceSize Small -ImageName $image -AvailabilitySetName azfae-use-as-dc
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password 
	$vm1 | Set-AzureSubnet -SubnetNames BackEnd
	$vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 100 -DiskLabel AppFiles –LUN 0 -HostCaching None
	New-AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Create the second domain controller server
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the second domain controller server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-dc02 -InstanceSize Small -ImageName $image -AvailabilitySetName azfae-use-as-dc
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password 
	$vm1 | Set-AzureSubnet -SubnetNames BackEnd
	$vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 100 -DiskLabel AppFiles –LUN 0 -HostCaching None
	New-	AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Create an internal load balancer instance for the application server tier 
	Add-AzureInternalLoadBalancer -ServiceName $ServiceName -InternalLoadBalancerName "AppTierILB" –SubnetName BackEnd –StaticVNetIPAddress 10.0.2.100
	
	#Create the first application server
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the first application server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-app01 -InstanceSize Large -ImageName $image -AvailabilitySetName azfae-use-as-app
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password 
	$vm1 | Set-AzureSubnet -SubnetNames BackEnd
	$vm1 | Add-AzureEndpoint -Name App1 -Protocol tcp -LocalPort 80 -PublicPort 80 -LBSetName "AppSet" -InternalLoadBalancerName "AppTierILB" -DefaultProbe
	$vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 500 -DiskLabel AppFiles –LUN 0 -HostCaching None
	New-	AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Create the second application server 
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the second application server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-app02 -InstanceSize Large -ImageName $image -AvailabilitySetName azfae-use-as-app
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password 
	$vm1 | Add-AzureEndpoint -Name App2 -Protocol tcp -LocalPort 80 -PublicPort 80 -LBSetName "AppSet" -InternalLoadBalancerName "AppTierILB" -DefaultProbe
	$vm1 | Set-AzureSubnet -SubnetNames BackEnd
	$vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 500 -DiskLabel AppFiles –LUN 0 -HostCaching None
	New-AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Specify the premium storage account for the SQL Server cluster
	Set-AzureSubscription –SubscriptionName "Contoso Enterprise Subscription" -CurrentStorageAccountName "contosoazfaeusesasqlclust"
	
	#Create the majority node witness server for the SQL Server cluster
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the majority node witness server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-sqlmn01 -InstanceSize Medium -ImageName $image -AvailabilitySetName azfae-use-as-sql
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password 
	$vm1 | Set-AzureSubnet -SubnetNames BackEnd
	New-AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Change the image string for the latest version of the SQL Server 2014 image in the gallery
	$image= Get-AzureVMImage | where { $_.ImageFamily -eq "SQL Server 2014 RTM Standard on Windows Server 2012 R2" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
	
	#Create the first SQL Server
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the first SQL Server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-sql01 -InstanceSize A5 -ImageName $image  -AvailabilitySetName azfae-use-as-sql
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password
	$vm1 | Set-AzureSubnet -SubnetNames BackEnd
	$vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1000 -DiskLabel SQLFiles –LUN 0 -HostCaching None
	New-AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01
	
	#Create the second SQL Server
	$cred1=Get-Credential –Message "Type the name and password of the local administrator account for the second SQL Server."
	$vm1=New-AzureVMConfig -Name azfae-use-vm-sql02 -InstanceSize A5 -ImageName $image  -AvailabilitySetName azfae-use-as-sql
	$vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password
	$vm1 | Set-AzureSubnet -SubnetNames BackEnd
	$vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1000 -DiskLabel SQLFiles –LUN 0 -HostCaching None
	New-AzureVM –ServiceName $ServiceName -VMs $vm1 -VNetName AZFAE-USE-VN01

## Recursos adicionais

[Assinatura e limites de serviços, cotas e restrições do Microsoft Azure](../azure-subscription-service-limits.md#storage-limits)

[Tamanhos de máquinas virtuais e serviços de nuvem do Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx)

[Metas de desempenho e escalabilidade do Armazenamento do Azure](../storage-scalability-targets.md)

<!--HONumber=54--> 