<properties
	pageTitle="Backup e restauração do SQL Server | Microsoft Azure"
	description="Descreve as considerações de backup e restauração para bancos de dados do SQL Server em execução em Máquinas Virtuais do Azure."
	services="virtual-machines"
	documentationCenter="na"
	authors="rothja"
	manager="jeffreyg"
	editor="monicar"
	tags="azure-service-management" />

<tags
	ms.service="virtual-machines"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="11/13/2015"
	ms.author="jroth" />

# Backup e restauração para o SQL Server em Máquinas Virtuais do Azure

## Visão geral

Fazer backup dos dados em bancos de dados do SQL Server é uma parte importante da estratégia de proteção contra perda de dados devido a erros de aplicativo ou do usuário. Isso também é verdadeiro para o SQL Server em execução em máquinas virtuais do Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Modelo do Gerenciador de Recursos.

Para o SQL Server em execução em VMs do Azure, você pode usar o backup nativo e técnicas de restauração usando discos anexados para o destino dos arquivos de backup. No entanto, há um limite para o número de discos que você pode anexar a uma máquina virtual do Azure, com base no [tamanho da máquina virtual](virtual-machines-size-specs.md). Também é necessário considerar a sobrecarga de gerenciamento de disco.

A partir do SQL Server 2014, você pode fazer backup e restauração para o Armazenamento de Blobs do Microsoft Azure. O SQL Server 2016 também fornece aprimoramentos para essa opção. Além disso, para os arquivos de banco de dados armazenados no armazenamento de Blobs do Microsoft Azure, o SQL Server 2016 fornece uma opção de backups quase imediatos e restaurações rápidas usando instantâneos do Azure. Este artigo fornece uma visão geral dessas opções. Encontre mais informações em [Backup e restauração do SQL Server com o Serviço de Armazenamento de Blob do Microsoft Azure](https://msdn.microsoft.com/library/jj919148(v=sql.130).aspx).

>[AZURE.NOTE]Para ver uma discussão sobre as opções de backup de bancos de dados muito grandes, veja [Estratégias de backup do banco de dados SQL Server de vários terabytes para Máquinas Virtuais do Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

As seções abaixo incluem informações específicas para as diferentes versões do SQL Server com suporte em uma máquina virtual do Azure.

## Considerações de backup quando arquivos de banco de dados estão armazenados no Serviço Blob do Microsoft Azure

Os motivos para a execução de backups de banco de dados e a tecnologia de backup subjacente em si muda quando os arquivos de banco de dados são armazenados no Armazenamento de Blobs do Microsoft Azure. Para obter mais informações sobre como armazenar arquivos de banco de dados no armazenamento de blob do Azure, veja [Arquivos de dados do SQL Server no Azure](https://msdn.microsoft.com/library/jj919148.aspx).

- Você não precisa executar backups de banco de dados para fornecer proteção contra falhas de hardware ou de mídia, pois o Microsoft Azure fornece essa proteção como parte do seu serviço.

- Você ainda precisará fazer backups de banco de dados para proporcionar proteção contra erros de usuário ou para fins de arquivamento, regulamentos ou fins administrativos.

- Você pode executar backups quase imediatos e restaurações rápidas usando o recurso de Backup de instantâneo do arquivo do SQL Server no Microsoft SQL Server 2016 Community Technology Preview 3 (CTP3). Para obter mais informações, veja [Backups de instantâneo de arquivo para arquivos de banco de dados no Azure](https://msdn.microsoft.com/library/mt169363.aspx).

## Backup e Restauração no Microsoft SQL Server 2016 Community Technology Preview 3 (CTP3)

O Microsoft SQL Server 2016 Community Technology Preview 3 (CTP3) dá suporte ao recurso de [backup e restauração com blobs do Azure](https://msdn.microsoft.com/library/jj919148.aspx) encontrados no SQL Server 2014 e descritos abaixo. Porém ele também inclui os seguintes aprimoramentos:

- **Distribuição**: ao fazer backup para o armazenamento de blob do Microsoft Azure, o SQL Server 2016 dá suporte ao backup para vários blobs, a fim de habilitar o backup de grandes bancos de dados, até um máximo de 12,8 TB.

- **Backup de instantâneo**: com o uso de instantâneos do Azure, o Backup de Instantâneo de Arquivo do SQL Server fornece backups quase imediatos e restaurações rápidas para os arquivos de banco de dados armazenados por meio do serviço de armazenamento de Blob do Azure. Essa funcionalidade permite simplificar suas políticas de backup e restauração. O backup de instantâneo de arquivo também dá suporte à recuperação pontual. Para obter mais informações, veja [Backups de instantâneo para arquivos de banco de dados no Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).

- **Agendamento de Backup Gerenciado**: o Backup de Gerenciado do SQL Server no Azure agora dá suporte a agendamentos personalizados. Para obter mais informações, veja [Backup Gerenciado do SQL Server para o Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).

>[AZURE.NOTE]Para obter um tutorial dos recursos do SQL Server 2016 ao usar o armazenamento de Blob do Azure, veja [Tutorial: usando o serviço de armazenamento de Blob do Microsoft Azure com bancos de dados SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## Backup e Restauração no SQL Server 2014

O SQL Server 2014 inclui o seguinte aprimoramento:

1. **Backup e restauração para o Azure**:

 - Agora há suporte para o *Backup do SQL Server para URL* no SQL Server Management Studio. A opção de backup para o Azure agora está disponível ao usar a tarefa de Backup ou Restauração, ou ainda o Assistente de plano de manutenção no SQL Server Management Studio. Para obter mais informações, veja [Backup do SQL Server para URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - O *Backup Gerenciado do SQL Server para Azure* traz uma nova funcionalidade que permite o gerenciamento de backup automatizado. Isso é especialmente útil para automatizar o gerenciamento de backups para instâncias do SQL Server 2014 em execução em um computador do Azure. Para obter mais informações, veja [Backup Gerenciado do SQL Server para o Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - O *Backup Automatizado* fornece automação adicional para habilitar automaticamente o *Backup Gerenciado do SQL Server para o Azure* em todos os bancos de dados novos e existentes para uma VM do SQL Server no Azure. Para obter mais informações, veja [Backup Automatizado para o SQL Server em Máquinas Virtuais do Azure](virtual-machines-sql-server-automated-backup.md).
 - Para obter uma visão geral de todas as opções de Backup do SQL Server 2014 no Azure, veja [Backup e restauração do SQL Server com o serviço de armazenamento de blob do Microsoft Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Criptografia**: o SQL Server 2014 dá suporte à criptografia de dados durante a criação de um backup. Ele dá suporte a vários algoritmos de criptografia e ao uso de um certificado ou chave assimétrica. Para obter mais informações, veja [Criptografia de backup](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## Backup e Restauração no SQL Server 2012

Para obter informações detalhadas sobre o Backup e restauração do SQL Server no SQL Server 2012, veja [Backup e restauração de Bancos de Dados do SQL Server (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

A partir do SQL Server 2012 SP1 Atualização Cumulativa 2, você pode fazer o backup e restauração a partir do Serviço de Armazenamento de Blobs do Azure. Esse aprimoramento pode ser usado para fazer backup de bancos de dados do SQL Server em um SQL Server em execução em uma máquina virtual do Azure ou em uma instância local. Para obter mais informações, veja [Backup e restauração do SQL Server com o Serviço de Armazenamento de Blob do Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Alguns dos benefícios de usar o serviço de armazenamento de Blobs do Azure incluem a capacidade de ignorar o limite de 16 discos para discos anexados, a facilidade de gerenciamento, a disponibilidade direta do arquivo de backup para outra instância do SQL Server em execução em uma máquina virtual do Azure ou para instâncias locais para a migração ou recuperação de desastres. Para obter uma lista completa dos benefícios de usar um serviço de armazenamento de blob do Azure para backups do SQL Server, veja a seção *Benefícios* em [Backup e restauração do SQL Server com o serviço de armazenamento de blob do Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Para ver as práticas recomendadas e informações de solução de problemas, veja [Práticas recomendadas de Backup e restauração (Serviço de Armazenamento de Blob do Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## Backup e restauração em outras versões do SQL Server com suporte em uma Máquina Virtual do Azure

Para Backup e restauração do SQL Server no SQL Server 2008 R2, veja [Fazer backup e restauração de bancos de dados no SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Para Backup e restauração do SQL Server no SQL Server 2008, veja [Fazer backup e restauração de bancos de dados no SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## Próximas etapas

Se ainda estiver planejando a implantação do SQL Server em uma VM do Azure, você encontrará orientações de provisionamento no seguinte tutorial: [Provisionando uma Máquina Virtual do SQL Server no Azure](virtual-machines-provision-sql-server.md).

Embora o backup e a restauração possam ser usados para migrar seus dados, há caminhos de migração de dados potencialmente mais fácil para o SQL Server em uma VM do Azure. Para ver uma discussão completa sobre as opções de migração e suas recomendações, veja [Migrando um banco de dados para o SQL Server em uma VM do Azure](virtual-machines-migrate-onpremises-database.md).

Examine outros [recursos para executar o SQL Server em Máquinas Virtuais do Azure](virtual-machines-sql-server-infrastructure-services.md).

<!---HONumber=Nov15_HO4-->