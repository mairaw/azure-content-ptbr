<properties
   pageTitle="Visão geral da configuração de 'ReliableDictionaryActorStateProvider' dos Reliable Actors do Service Fabric do Azure | Microsoft Azure"
   description="Saiba como configurar atores com monitoração de estado do Service Fabric do Azure do tipo 'ReliableDictionaryActorStateProvider'"
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/28/2015"
   ms.author="sumukhs"/>

# Configurando Atores Confiáveis — ReliableDictionaryActorStateProvider
A configuração padrão do ReliableDictionaryActorStateProvider pode ser modificada modificando-se o arquivo "settings.xml" gerado na raiz pacote do Visual Studio dentro da pasta "Config" para o ator especificado em questão.

O tempo de execução da malha do serviço procura nomes de seção predefinidos no arquivo "settings.xml" e consome os valores de configuração ao criar os componentes de tempo de execução subjacentes.

> [AZURE.NOTE]**NÃO** exclua/modifique os nomes de seção das configurações a seguir no arquivo "settings.xml" que é gerado no Visual Studio Solution.

## Configuração de segurança do replicador
As configurações de segurança do replicador servem para proteger o canal de comunicação que é usado durante a replicação. Isso significa que os serviços não poderão ver o tráfego de replicação uns dos outros, garantindo que os dados que têm alta disponibilidade também estejam seguros. Por padrão, uma seção de configuração de segurança vazia não habilita a segurança de replicação.
### Nome da seção
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## Configuração do replicador
As configurações do replicador servem para configurar o replicador que será responsável por tornar o provedor de estado do ator altamente confiável por meio da replicação e persistência do estado localmente. A configuração padrão é gerada pelo modelo do Visual Studio e deve ser suficiente. Esta seção fala sobre configurações adicionais que estão disponíveis para ajustar o replicador.
### Nome da seção
&lt;ActorName&gt;ServiceReplicatorConfig
### Nomes da configuração

|Nome|Unidade|Valor Padrão|Comentários|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Segundos|0,05|Período de tempo pelo qual o replicador no secundário espera após o recebimento de uma operação antes de enviar novamente uma confirmação ao primário. Todas as outras confirmações a serem enviadas para operações e processadas dentro deste intervalo são enviadas como uma única resposta.||
|ReplicatorEndpoint|N/D|N/A - RequiredParameter|Endereço IP e porta que o replicador primário/secundário usará para se comunicar com outros replicadores no conjunto de réplicas. Eles devem fazer referência a um ponto de extremidade do recurso de TCP no manifesto de serviço. Consulte [Recursos do Manifesto do serviço](service-fabric-service-manifest-resources.md) para saber mais sobre como definir os recursos de ponto de extremidade no manifesto de serviço. |
|MaxReplicationMessageSize|Bytes|50 MB|Tamanho máximo de dados de replicação que podem ser transmitidos em uma única mensagem.|
|MaxPrimaryReplicationQueueSize|Número de operações|8192|Número máximo de operações na fila principal. Uma operação é liberada depois que o replicador primário recebe uma confirmação de todos os replicadores secundários. Esse valor deve ser maior que 64 e uma potência de 2.|
|MaxSecondaryReplicationQueueSize|Número de operações|16384|Número máximo de operações na fila secundária. Uma operação é liberada depois de tornar seu estado de altamente disponível por meio de persistência. Esse valor deve ser maior que 64 e uma potência de 2.|
|CheckpointThresholdInMB|MB|200|Quantidade de espaço de arquivo de log depois que o estado é o ponto de verificação.|
|MaxRecordSizeInKB|KB|1024|Maior tamanho de registro que o replicador pode gravar no log. Esse valor deve ser um múltiplo de 4 e maior que 16.|
|OptimizeLogForLowerDiskUsage|Booleano|verdadeiro|Quando verdadeiro, o log é configurado de forma que o arquivo de log dedicado da replicação seja criado usando um arquivo esparso do NTFS. Isso reduz o uso do espaço real em disco para o arquivo. Quando falso, o arquivo é criado com alocações fixas que fornecem o melhor desempenho de gravação.|
|SharedLogId|guid|""|Especifica um guid exclusivo a ser usado para identificar o arquivo de log compartilhado usado com esta réplica. Normalmente, os serviços não devem usar essa configuração, mas se SharedLogId for especificado, SharedLogPath também deve ser especificado.|
|SharedLogPath|Nome de caminho totalmente qualificado|""|Especifica o caminho totalmente qualificado onde o arquivo de log compartilhado para esta réplica será criado. Normalmente, os serviços não devem usar essa configuração, mas se SharedLogPath for especificado, SharedLogId também deve ser especificado.|


## Exemplo do arquivo de configuração

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## Comentários
BatchAcknowledgementInterval controla a latência de replicação. Um valor '0' resulta na menor latência possível, ao custo de taxa de transferência (como mais mensagens de confirmação devem ser enviadas e processadas, cada uma contendo menos confirmações). Quanto maior o valor para BatchAcknowledgementInterval, maior será a produtividade geral da replicação, ao custo da maior latência de operação. Isso se converte diretamente para a latência de confirmações de transações.

O valor para CheckpointThresholdInMB controla a quantidade de espaço em disco que o replicador pode usar para armazenar as informações de estado no arquivo de log dedicado da replicação. Aumentar para um valor maior que o padrão poderá resultar em tempos de reconfiguração mais rápidos quando uma nova replicação é adicionada ao conjunto devido à transferência de estado parcial que acontece devido à disponibilidade de mais histórico de operações no log, ao mesmo tempo que aumenta potencialmente o tempo de recuperação de uma replicação depois de uma falha.

A configuração do OptimizeForLowerDiskUsage permite que o espaço do arquivo de log seja "excessivamente provisionado", de forma que réplicas ativas possam armazenar mais informações de estado em seus arquivos de log enquanto réplicas inativas usam menos espaço em disco. Embora isso permita a hospedagem de mais réplicas em um nó do que seria possível em uma situação inversa devido à falta de espaço em disco, ao definir OptimizeForLowerDiskUsage como falso, as informações de estado são gravadas nos arquivos de log mais rapidamente.

O MaxRecordSizeInKB define o tamanho máximo de um registro que pode ser gravado pelo replicador no arquivo de log. Na maioria dos casos o tamanho padrão do registro de 1024 KB é ideal, porém, se o serviço estiver fazendo com que itens de dados maiores sejam parte das informações de estado, esse valor talvez precise ser aumentado. Há poucas vantagens em tornar o MaxRecordSizeInKB menor que 1024, já que registros menores usam apenas o espaço necessário para o registro menor. A expectativa é de que isso precise ser alterado somente em casos raros.

As configurações de SharedLogId e SharedLogPath são sempre usadas juntas e permitem que um serviço use um log compartilhado separado do log compartilhado padrão para o nó. Para obter maior eficiência, devem ser especificados o máximo de serviços possível para o mesmo log compartilhado. Arquivos de log compartilhados devem ser colocados em discos que são usados exclusivamente para o arquivo de log compartilhado, de forma que a contenção de movimentação do cabeçote seja reduzida. A expectativa é de que isso precise ser alterado somente em casos raros.

<!---HONumber=Nov15_HO4-->