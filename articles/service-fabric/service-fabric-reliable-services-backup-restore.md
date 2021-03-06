<properties
   pageTitle="Reliable Service de Backup e Restauração | Microsoft Azure"
   description="Documentação conceitual dos Reliable Services de Backup e Restauração do Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="subramar,jessebenson"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/01/2015"
   ms.author="mcoskun"/>

# Reliable Services de Backup e Restauração

O Service Fabric é uma plataforma de alta disponibilidade e replica o estado em vários nós a fim de manter essa alta disponibilidade. Portanto, mesmo se um nó do cluster falhar, os serviços continuarão disponíveis. Embora essa redundância interna fornecida pela plataforma possa ser suficiente para algumas pessoas, em certos casos, é recomendável fazer o backup dos dados do serviço (em um repositório externo).

Por exemplo, convém fazer o backup dos dados de um serviço nos seguintes cenários:

* No caso de perda permanente de um cluster inteiro do Service Fabric ou de todos os nós que estão em execução em uma determinada partição. Isso pode acontecer quando, por exemplo, você não estiver replicado geograficamente e todo o cluster estiver em um data center, e o data center inteiro ficar inativo.

* Erros administrativos nos quais o estado é acidentalmente excluído/corrompido. Por exemplo, isso pode ocorrer quando um administrador com privilégios suficientes exclui o serviço por engano.

* Bugs no serviço causando a corrupção de dados. Por exemplo, isso pode ocorrer quando uma atualização de código de serviço inicia a gravação de dados com falha para uma coleção confiável. Nesse caso, convém reverter o código e os dados para um estado anterior.

* Processamento de dados offline. Talvez seja conveniente processar no modo offline os dados de business intelligence que ocorrem separadamente do serviço de geração de dados.

O recurso de Backup/Restauração permite que os serviços criados na API de Reliable Services criem e restaurem os backups. As APIs de backup fornecidas pela plataforma permitem que os backups do estado de uma partição sejam realizados sem bloquear as operações de leitura ou de gravação. As APIs de restauração permitem que o estado da partição seja restaurado a partir de um backup escolhido.


## Como fazer backup

O Autor do serviço tem controle total sobre quando realizar backups e onde os backups serão armazenados.

Para iniciar um backup, o serviço precisa chamar **IReliableStateManager.BackupAsync**. Os backups só podem ser realizados a partir de Réplicas primárias e exigem a concessão do Status de Gravação.

Conforme mostrado abaixo, a sobrecarga mais simples de **BackupAsync** usa a Função<< BackupInfo  bool >> chamada **backupCallback**.

```C#
await this.StateManager.BackupAsync(this.BackupCallbackAsync);
```

**BackupInfo** fornece informações sobre o backup, incluindo o local da pasta na qual o tempo de execução salvou o backup (BackupInfo.Directory). A função de retorno de chamada espera mover o BackupInfo.Directory para um Armazenamento externo ou outro local. Além disso, esta função retorna um booliano que indica se ele foi capaz de mover a pasta de backup para seu local de destino.

O código a seguir demonstra como a backupCallback pode ser usada para carregar o backup no Armazenamento do Azure:

```C#
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, CancellationToken.None);

    return true;
}
```

No exemplo acima, **ExternalBackupStore** é a classe de exemplo usada para realizar a interface com o Armazenamento de Blobs do Azure, e **UploadBackupFolderAsync** é o método que compacta a pasta e a coloca no Armazenamento de Blobs do Azure.

Observação:

- Pode haver apenas um **BackupAsync** por réplica em execução a qualquer momento. Mais de uma chamada de **BackupAsync** por vez lançará **FabricBackupInProgressException** para limitar a execução de backups para apenas uma.

- Se uma réplica passar por failover enquanto um backup estiver em andamento, talvez o backup não tenha sido concluído. Portanto, após a conclusão do failover, é responsabilidade do serviço reiniciar o backup invocando **BackupAsync**, conforme necessário.

## Como restaurar dados

Em geral, os casos em que você talvez precise executar uma operação de restauração se enquadram em uma destas categorias:


1. A partição do serviço perdeu os dados. Por exemplo, o disco de duas entre três réplicas de uma partição (incluindo a réplica primária) é corrompido/apagado. Pode ser necessário que a nova réplica primária restaure os dados a partir de um backup.

2. O serviço inteiro é perdido. Por exemplo, um administrador remove todo o serviço e, portanto, o serviço e os dados precisam ser restaurados.

3. O serviço replica dados corrompidos de aplicativo (por exemplo, devido a um erro de aplicativo). Nesse caso, o serviço precisa ser atualizado/revertido a fim de remover a causa do dano e os dados não corrompidos precisam ser restaurados.

Embora muitas abordagens sejam possíveis, oferecemos alguns exemplos sobre como usar o RestoreAsync para recuperar os cenários acima.

## Perda de dados da partição

Nesse caso, o tempo de execução detectaria automaticamente a perda de dados e chamaria a API **OnDataLossAsync**.

O autor do serviço deve realizar o seguinte para fazer a recuperação: – Substituir **IReliableStateManager** para retornar um novo ReliableStateManager e fornecer uma função de retorno de chamada para ser chamada se houver perda de dados. – Localizar o backup mais recente no local externo que contém os backups do serviço. – Se o estado de backup mais recente estiver por trás da nova primária, retornar falso. Dessa forma, a nova primária não substitui os dados mais antigos. – Baixar o backup mais recente e descompactar o backup na pasta de backups, caso ele esteja compactado. – Chamar **IReliableStateManager.RestoreAsync** com o caminho da pasta de backups. – Retornar verdadeiro, se a restauração for bem-sucedida.

Veja a seguir um exemplo de implementação do método **OnDataLossAsync** junto com a substituição de **IReliableStateManager**.

```C#
protected override IReliableStateManager CreateReliableStateManager()
{
    return new ReliableStateManager(new ReliableStateManagerConfiguration(
            onDataLossEvent: this.OnDataLossAsync));
}

protected override async Task<bool> OnDataLossAsync(CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    await this.StateManager.RestoreAsync(backupFolder);

    return true;
}
```

>[AZURE.NOTE]O RestorePolicy é definido como Seguro por padrão. Isso significa que a API RestoreAsync falhará com ArgumentException se detectar que a pasta de backups contém o estado mais antigo ou igual ao estado contido nesta réplica. RestorePolicy.Force pode ser usado para ignorar a verificação de segurança.

## Serviço perdido ou excluído

Se um serviço for removido, primeiro recrie o serviço antes de restaurar os dados. É importante criar o serviço com a mesma configuração, por exemplo, esquema de particionamento, para que os dados possam ser restaurados perfeitamente. Quando o serviço estiver funcionando, a API para restauração dos dados (**OnDataLossAsync** acima) precisará ser chamada em todas as partições desse serviço. Uma maneira de conseguir isso é usando **FabricClient.ServiceManager.InvokeDataLossAsync** em cada partição.

Neste ponto, a implementação é igual ao cenário anterior. Cada partição deve restaurar o backup mais recente relevante do armazenamento externo. Uma limitação é que a ID de partição pode ter sido alterada, uma vez que o tempo de execução cria IDs de partição dinamicamente). Portanto, o serviço precisa armazenar o nome do serviço e as informações de partição apropriadas para identificar o backup correto mais recente para restauração em cada partição.


## Replicação de dados de aplicativo corrompidos

Se houver um bug na atualização do aplicativo implementado que possa causar danos nos dados, por exemplo, uma atualização do aplicativo pode começar a atualizar todos os números de telefone registrados em um dicionário confiável com um código de área inválido. Nesse caso, os números de telefone inválidos serão replicados, uma vez que o Service Fabric não tem conhecimento da natureza dos dados que estão sendo armazenados.

A primeira coisa a ser feita após a detecção de um bug tão sério e que causa a corrupção de dados é congelar o serviço no nível do aplicativo e, se possível, atualizar para a versão do código do aplicativo que não tenha o bug. No entanto, mesmo depois que o código do serviço é corrigido, os dados ainda podem estar corrompidos e, portanto, talvez seja necessário restaurá-los. Nesses casos, talvez não seja suficiente restaurar o backup mais recente, uma vez que os backups mais recentes também podem estar corrompidos. Assim, é necessário localizar o backup mais recente realizado antes de os dados serem corrompidos.

Caso não saiba ao certo quais backups estão corrompidos, implemente um novo cluster do Service Fabric e restaure os backups das partições afetadas, assim como o cenário descrito anteriormente para "Serviço Excluído". Para cada partição, comece a restaurar os backups a partir do mais recente. Caso encontre um backup sem danos, mova ou exclua todos os outros backups mais recentes dessa partição. Repita esse processo para cada partição. Agora, quando **OnDataLossAsync** for chamado na partição do cluster de produção, o último backup localizado no armazenamento externo será aquele escolhido de acordo com o processo acima.

Agora, as etapas no "Serviço Excluído" podem ser usadas para restaurar o estado do backup do serviço para o estado anterior à corrupção do estado pelo código com bug.

Observe que:

- Sempre que você restaura há uma chance do backup restaurado ser mais antigo do que o estado da partição antes de os dados serem perdidos. Por isso, essa Restauração deve ser usada apenas como último recurso para recuperar o máximo possível de dados.

- A cadeia de caracteres que representa o caminho da pasta de backup e os caminhos dos arquivos dentro da pasta de backup podem ser maiores do que 255 caracteres, dependendo do caminho de FabricDataRoot e do comprimento do nome do Tipo de Aplicativo. Isso pode fazer com que alguns métodos .Net, como **Directory.Move**, lancem o **PathTooLongException**. Uma solução alternativa é chamar diretamente as APIs do kernel32 como **CopyFile**.


## Nos bastidores: mais detalhes sobre backup e restauração

### Backup
O Gerenciador de Estado Confiável permite a criação de backups consistentes sem bloquear as operações de leitura e gravação. Para fazer isso, ele utiliza um mecanismo de ponto de verificação e persistência de log. O Gerenciador de Estado Confiável usa pontos de verificação (leves) difusos em determinados pontos para aliviar a pressão do log transacional e melhorar os tempos de recuperação. Quando IReliableStateManager.**BackupAsync** é chamado, o Gerenciador de Estado Confiável instrui todos os Objetos Confiáveis a copiar seus arquivos de ponto de verificação mais recentes em uma pasta de backup local. Em seguida, o Gerenciador de Estado Confiável copia todos os registros de log desde o "ponteiro inicial" até o registro de log mais recente na pasta de backup. Como todos os registros de log, até o último, são incluídos no backup, e o Gerenciador de Estado Confiável preserva o Log Write-Ahead, o Gerenciador de Estado Confiável garante que todas as transações confirmadas (CommitAsync retornou com êxito) sejam incluídas no backup.

As transações confirmadas após o **BackupAsync** ser chamado podem ou não estar no backup. Após o preenchimento da pasta de backup local pela plataforma (ou seja, backup local concluído pelo tempo de execução), o retorno de chamada de backup do serviço é invocado. Esse retorno de chamada é responsável por mover a pasta de backups para um local externo, por exemplo, o Armazenamento do Azure.

### Restaurar

O Gerenciador de Estado Confiável permite a restauração de um backup usando a API IReliableStateManager.RestoreAsync. O método RestoreAsync só pode ser chamado dentro do método **OnDataLossAsync**. O bool retornado por **OnDataLossAsync** indica se o serviço restaurou seu estado a partir de uma fonte externa. Quando o **OnDataLossAsync** retorna verdadeiro, o Service Fabric recompila todas as outras réplicas a partir da primária. O Service Fabric garante que as réplicas que devem receber **OnDataLossAsync** façam primeiro a transição para a função primária, embora não recebam status de leitura ou de gravação. Isso significa que, para os implementadores de StatefulService, o RunAsync não será chamado até que **OnDataLossAsync** seja concluído com êxito. Em seguida, **OnDataLossAsync** será invocado na nova primária. A API continua sendo chamada, até que um serviço conclua essa API com êxito, retornando verdadeiro ou falso, e conclua a reconfiguração relevante.


Primeiro, RestoreAsync descarta todos os estados existentes na réplica Primária na qual foi chamado. Depois, o Gerenciador de Estado Confiável cria todos os Objetos Confiáveis que existem na pasta de backup. Em seguida, os Objetos Confiáveis são instruídos a restaurar a partir dos pontos de verificação na pasta de backup. Finalmente, o Gerenciador de Estado Confiável recupera seu próprio estado a partir dos registros de log na pasta de backup e executa a recuperação. Como parte do processo de recuperação, as operações que começaram do "ponto de partida" e confirmaram os registros de log na pasta de backup são reproduzidas aos Objetos Confiáveis. Essa etapa garante que o estado recuperado seja consistente.

<!---HONumber=AcomDC_1203_2015-->