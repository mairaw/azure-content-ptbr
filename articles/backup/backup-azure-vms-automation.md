<properties
	pageTitle="Implantar e gerenciar backup de VMs do Azure usando o PowerShell | Microsoft Azure"
	description="Saiba como implantar e gerenciar o Backup do Azure usando o PowerShell"
	services="backup"
	documentationCenter=""
	authors="aashishr"
	manager="shreeshd"
	editor=""/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/01/2015"
	ms.author="aashishr"/>


# Implantar e gerenciar o backup de VMs do Azure usando o PowerShell
Este artigo mostra como usar o Azure PowerShell para backup e recuperação de VMs IaaS do Azure.

## Conceitos
Seja [apresentado ao backup de VM IaaS do Azure](backup-azure-vms-introduction.md) na documentação do Backup do Azure. Ela aborda os conceitos básicos sobre por que você deve fazer backup de sua VM, pré-requisitos e limitações.

Para usar efetivamente o PowerShell, é necessário compreender a hierarquia de objetos e de onde começar.

![Hierarquia do Objeto](./media/backup-azure-vms-automation/object-hierarchy.png)

Os dois fluxos mais importantes são habilitar a proteção para uma VM e restaurar dados de um ponto de recuperação. O foco deste artigo é ajudá-lo a se tornar um especialista em trabalhar com os commandlets do PowerShell para habilitar esses dois cenários.


## Configuração e registro
Para começar, habilite os commandlets do Backup do Azure alternando para o modo *AzureResourceManager* usando o commandlet **Switch-AzureMode**:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

As seguintes tarefas de configuração e de registro podem ser automatizadas com o PowerShell:

- Criar um cofre de backup
- Registrando as VMs no serviço de Backup do Azure

### Criar um cofre de backup

> [AZURE.WARNING]Para clientes usando o Backup do Azure pela primeira vez, você precisa registrar o provedor de Backup do Azure para ser usado com sua assinatura. Isso pode ser feito executando o seguinte comando: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"

Você pode criar um novo cofre de backup usando o commandlet **New-AzureRMBackupVault**. O cofre de backup é um recurso do ARM e, portanto, você precisará colocá-lo em um Grupo de Recursos. Em um console do Azure PowerShell com privilégios elevados, execute os seguintes comandos:

```
PS C:\> New-AzureRMResourceGroup –Name “test-rg” –Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

Você pode obter uma lista de todos os cofres de backup em uma determinada assinatura usando o commandlet **Get-AzureRMBackupVault**.

> [AZURE.NOTE]É conveniente armazenar o objeto cofre de backup em uma variável. O objeto cofre é necessário como uma entrada para vários commandlets do Backup do Azure.


### Registrando as VMs
A primeira etapa para configurar o backup com o Backup do Azure é registrar seu computador ou VM em um cofre de Backup do Azure. O commandlet **Register-AzureRMBackupContainer** usa as informações de entrada de uma máquina virtual IaaS do Azure e registra com o cofre especificado. A operação de registro associa a máquina virtual do Azure com o Cofre de backup e controla a VM por meio do ciclo de vida do backup.

Registrar sua VM com o serviço de Backup do Azure cria um objeto de contêiner de nível superior. Um contêiner normalmente contém vários itens que podem ser copiados, mas no caso de VMs haverá apenas um item de backup para o contêiner.

```
PS C:\> $registerjob = Register-AzureRMBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## Fazer backup das VMs do Azure

### Crie uma política de proteção
Não é obrigatório criar uma nova política de proteção para iniciar o backup das suas VMs. O cofre vem com uma 'Política Padrão' que pode ser usado para habilitar a proteção rapidamente e editada posteriormente com os detalhes à direita. Você pode obter uma lista das políticas disponíveis no cofre usando o commandlet **Get-AzureRMBackupProtectionPolicy**:

```
PS C:\> Get-AzureRMBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

Uma política de backup está associada a pelo menos uma política de retenção. A política de retenção define quanto tempo um ponto de recuperação é mantido pelo Backup do Azure. O commandlet **New-AzureRMBackupRetentionPolicy** cria objetos do PowerShell que armazenam informações de política de retenção. Esses objetos de política de retenção são usados como entradas para o commandlet *New-AzureRMBackupProtectionPolicy* ou diretamente com o commandlet *Enable-AzureRMBackupProtection*.

Uma política de backup define quando e com que frequência será feito o backup de um item. O commandlet **New-AzureRMBackupProtectionPolicy** cria um objeto do PowerShell que mantém as informações de política de backup. A política de backup é usada como entrada para o commandlet *Enable-AzureRMBackupProtection*.

```
PS C:\> $Daily = New-AzureRMBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRMBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy ($Daily) -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### Habilitar proteção
Habilitar a proteção envolve dois objetos - o Item e a Política, e ambos precisam pertencer ao mesmo cofre. Depois que a política é associada ao item, o fluxo de trabalho de backup será iniciado no agendamento definido.

```
PS C:\> Get-AzureRMBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRMBackupItem | Enable-AzureRMBackupProtection -Policy $newpolicy
```

### Backup inicial
O agendamento de backup se encarregará de fazer a cópia inicial completa do item e a cópia incremental para os backups subsequentes. No entanto, se você quiser forçar o backup inicial para ocorrer em determinado momento ou até mesmo imediatamente, em seguida, use o commandlet **Backup-AzureRMBackupItem**:

```
PS C:\> $container = Get-AzureRMBackupContainer -Vault $backupvault -type AzureVM -name "testvm"
PS C:\> $backupjob = Get-AzureRMBackupItem -Container $container | Backup-AzureRMBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

### Monitoramento de um trabalho de backup
A maioria das operações de longa duração no Backup do Azure são modeladas como um trabalho. Isso facilita acompanhar o andamento sem a necessidade de manter o portal do Azure aberto em todos os momentos.

Para obter o último status de um trabalho em andamento, use o commandlet **Get-AzureRMBackupJob**.

```
PS C:\> $joblist = Get-AzureRMBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

Em vez de sondar esses trabalhos para conclusão – o que é um código adicional desnecessário - é mais simples usar o commandlet **Wait-AzureRMBackupJob**. Quando usado em um script, o commandlet fará uma pausa na execução até que o trabalho seja concluído ou o valor de tempo limite especificado seja atingido.

```
PS C:\> Wait-AzureRMBackupJob -Job $joblist[0] -Timeout 43200
```


## Restaurar uma VM do Azure

Para restaurar dados de backup, é necessário identificar o Item de backup e o ponto de recuperação que mantém os dados pontuais. Essas informações são fornecidas pelo commandlet Restore-AzureRMBackupItem para iniciar uma restauração de dados do cofre para a conta do cliente.

### Selecione a VM

Para obter o objeto do PowerShell que identifica o item correto de backup, você precisa começar do contêiner no cofre e descer para baixo na hierarquia de objetos. Para selecionar o contêiner que representa a VM, use o commandlet **Get-AzureRMBackupContainer** e indique o commandlet **Get-AzureRMBackupItem**.

```
PS C:\> $backupitem = Get-AzureBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRMBackupItem
```

### Escolha um ponto de recuperação

Agora você pode listar todos os pontos de recuperação para o item de backup usando o commandlet **Get-AzureRMBackupRecoveryPoint** e escolha o ponto de recuperação para restaurar. Normalmente os usuários escolhem o ponto mais recente *AppConsistent* na lista.

```
PS C:\> $rp =  Get-AzureRMBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

### Restaurando discos

Há uma diferença importante entre as operações de restauração feitas por meio do portal do Azure e por meio do Azure PowerShell. Com o PowerShell, a operação de restauração para na restauração de discos e configura as informações do ponto de recuperação. Ele não cria uma máquina virtual.

> [AZURE.WARNING]O Restore-AzureRMBackupItem não cria uma VM. Ele restaura apenas os discos para a conta de armazenamento especificada. Este não é o mesmo comportamento que você experimenta no portal do Azure.

```
PS C:\> $restorejob = Restore-AzureRMBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

Você pode obter os detalhes da operação de restauração usando o commandlet **Get-AzureRMBackupJobDetails** depois que o trabalho de restauração for concluído. A propriedade *ErrorDetails* terá as informações necessárias para recriar a VM.

```
PS C:\> $restorejob = Get-AzureRMBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRMBackupJobDetails -Job $restorejob
```

### Compilar a VM

Criar as VMs por meio dos discos restaurados pode ser feito usando os commandlets do PowerShell do Azure ServiceManager mais antigos, os novos modelos de ResourceManager do Azure, ou até mesmo usando o portal do Azure. Em um exemplo rápido, mostraremos como chegar lá usando os commandlets do ServiceManager do Azure.

```
 $properties  = $details.Properties

 $storageAccountName = $properties["TargetStorageAccountName"]
 $containerName = $properties["TargetContainerName"]
 $blobName = $properties["TargetBlobName"]

 Switch-AzureMode AzureServiceManagement

 $keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
 $storageAccountKey = $keys.Primary
 $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


 $destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
 Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


 $obj = [xml](Get-Content $destination_path)
 $pvr = $obj.PersistentVMRole
 $os = $pvr.OSVirtualHardDisk
 $dds = $pvr.DataVirtualHardDisks
 $osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
 $vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

 if (!($dds -eq $null))
 {
	 foreach($d in $dds.DataVirtualHardDisk)
 	 {
		 $lun = 0;
		 if(!($d.Lun -eq $null))
		 {
	 		 $lun = $d.Lun
		 }
		 $name = "panbhadataDisk" + $lun
     Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
     $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
	}
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

Para obter mais informações sobre como criar uma VM por meio dos discos restaurados, leia sobre os seguintes commandlets:

- [Add-AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
- [New-AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
- [New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

<!---HONumber=September15_HO1-->