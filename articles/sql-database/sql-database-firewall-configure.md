<properties
   pageTitle="Firewall do Banco de Dados SQL do Azure | Microsoft Azure"
   description="Como configurar o firewall do Banco de Dados SQL do Microsoft Azure."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jeffreyg"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/04/2015"
   ms.author="rickbyh"/>

# Firewall do Banco de Dados SQL do Azure

O Banco de Dados SQL do Microsoft Azure fornece um serviço de banco de dados relacional para o Azure e outros aplicativos baseados na Internet. Para ajudar a proteger seus dados, o firewall do Banco de Dados SQL do Azure impede todo acesso ao seu servidor de Banco de Dados SQL do Azure até que você especifique quais computadores têm permissão. O firewall concede acesso com base no endereço IP de origem de cada solicitação.

Para configurar seu firewall, você deve criar regras de firewall que especifiquem intervalos de endereços IP aceitáveis. Você pode criar regras de firewall nos níveis de servidor e de banco de dados.

- **Regras de firewall no nível do servidor:** essas regras permitem que os clientes acessem todo o servidor do Banco de Dados SQL, ou seja, todos os bancos dentro do mesmo servidor lógico. Essas regras são armazenadas no banco de dados **mestre**.
- **Regras de firewall no nível do banco de dados:** essas regras permitem que os clientes acessem bancos de dados individuais dentro de seu servidor do Banco de Dados SQL. Essas regras são criadas de acordo com o banco de dados e são armazenadas em bancos de dados individuais (incluindo o **mestre**). Essas regras podem ser úteis para restringir o acesso a determinados bancos de dados (seguros) dentro do mesmo servidor lógico.

**Recomendação:** a Microsoft recomenda o uso de regras de firewall no nível do banco de dados sempre que possível, a fim de tornar seu banco de dados mais portátil. Use regras de firewall no nível do servidor quando você tiver muitos bancos de dados com os mesmos requisitos de acesso e não quiser gastar tempo configurando cada um individualmente.

**Sobre federações:** a implementação atual das Federações será descontinuada com as camadas de serviço Web e Business. Considere a implantação de soluções personalizadas de fragmentação para maximizar a escalabilidade, a flexibilidade e o desempenho. Para saber mais sobre a fragmentação personalizada, consulte [Escalando horizontalmente os Bancos de Dados SQL do Azure](https://msdn.microsoft.com/library/dn495641.aspx).

> [AZURE.NOTE]Se você criar uma federação de banco de dados no Banco de Dados SQL do Azure na qual o banco de dados raiz contém regras de firewall no nível do banco de dados, as regras não serão copiadas para os bancos de dados membro da federação. Se você precisar de regras de firewall no nível do banco de dados para os membros da federação, será necessário recriar as regras para os membros da federação. No entanto, se você dividir um membro da federação contendo uma regra de firewall no nível do banco de dados em novos membros da federação usando a Instrução ALTER FEDERATION... SPLIT, os novos membros de destino terão as mesmas regras de firewall no nível do banco de dados do membro da federação de origem. Para saber mais sobre federações, consulte[Federações no Banco de Dados SQL do Azure](https://msdn.microsoft.com/library/hh597452.aspx).

## Visão geral

Inicialmente, todo acesso ao seu servidor de Banco de Dados SQL do Azure é bloqueado pelo firewall. Para começar a usar seu servidor de Banco de Dados SQL do Azure, você deve acessar o Portal de Gerenciamento e especificar uma ou mais regras de firewall no nível do servidor que permitem acesso ao seu servidor de Banco de Dados SQL. Use as regras de firewall para especificar quais intervalos de endereço IP da Internet têm permissão e se os aplicativos do Azure podem tentar se conectar ao seu servidor de Banco de Dados SQL.

No entanto, se você quiser conceder acesso de forma seletiva a apenas um dos bancos de dados em seu servidor de Banco de Dados SQL do Azure, você deve criar uma regra no nível de banco de dados para o banco de dados necessário com um intervalo de endereços IP que vá além do intervalo de endereços IP especificado na regra de firewall no nível de servidor, e certifique-se de que o endereço IP do cliente esteja dentro do intervalo especificado na regra no nível de banco de dados.

As tentativas de conexão da Internet e do Azure devem passar primeiramente pelo firewall antes de poderem acessar seu servidor de Banco de Dados SQL do Azure ou banco de dados, conforme exibido no diagrama a seguir.

   ![sqldb-firewall][1]

## Conectando pela Internet

Quando um computador tenta se conectar ao seu servidor de banco de dados pela Internet, o firewall verifica o endereço IP de origem da solicitação com base no conjunto completo de regras de firewall no nível do servidor e (se necessário) no nível do banco de dados:

- Se o endereço IP da solicitação estiver dentro de um dos intervalos especificados nas regras de firewall no nível do servidor, a conexão será concedida ao seu servidor de Banco de Dados SQL do Azure.

- Se o endereço IP da solicitação não estiver dentro de um dos intervalos especificados nas regras de firewall no nível do servidor, ocorrerá a verificação das regras de firewall no nível do banco de dados. Se o endereço IP da solicitação estiver dentro de um dos intervalos especificados nas regras de firewall no nível do banco de dados, a conexão será concedida apenas ao banco de dados que apresenta uma regra no nível de banco de dados correspondente.

- Se o endereço IP da solicitação não estiver dentro dos intervalos especificados em qualquer uma das regras de firewall no nível do servidor ou do banco de dados, solicitação de conexão falhará.

> [AZURE.NOTE]Para acessar o Banco de Dados SQL do Azure de seu computador local, verifique se o firewall em sua rede e computador local permite a comunicação de saída na porta TCP 1433.
 

## Conexão pelo Azure

Quando um aplicativo do Azure tenta se conectar ao seu servidor de banco de dados, o firewall verifica se há permissão para conexões do Azure. Uma configuração de firewall com endereço inicial e final igual a 0.0.0.0 indica que essas conexões são permitidas. Se a tentativa de conexão não for permitida, a solicitação não alcançará o servidor de Banco de Dados SQL do Azure.

Há duas maneiras para habilitar conexões do Azure no [Portal de Gerenciamento](http://go.microsoft.com/fwlink/p/?LinkID=161793):

- Marque a caixa de seleção**Permitir que serviços do Windows Azure acessem o servidor** durante a criação de um novo servidor.

- Na guia **Configurar** em um servidor, na seção **Serviços Permitidos**, clique em **Sim** para **Serviços do Windows Azure**.

## Criando a primeira regra de firewall no nível do servidor

A primeira configuração de firewall no nível do servidor pode ser criada usando o [Portal de Gerenciamento](http://go.microsoft.com/fwlink/p/?LinkID=161793) ou programaticamente usando a API REST ou o PowerShell do Azure. As regras de firewall no nível de servidor subsequentes podem ser criadas e gerenciados usando esses métodos, por exemplo, por meio de Transact-SQL. Para saber mais sobre regras de firewall no nível do servidor, confira [Como: configurar as definições de firewall (Banco de Dados SQL do Azure)](article:sql-database-configure-firewall-settings).

## Criando regras de firewall no nível do banco de dados

Depois de configurar o primeiro firewall no nível do servidor, convém restringir o acesso a determinados bancos de dados. Se você especificar um intervalo de endereços IP na regra de firewall no nível do banco de dados que está fora do intervalo especificado na regra de firewall no nível do servidor, somente os clientes com endereços IP no intervalo de nível de banco de dados poderão acessar o banco de dados. Você pode ter no máximo 128 regras de firewall no nível do banco de dados para um banco de dados. É possível criar e gerenciar regras de firewall no nível de banco de dados para bancos de dados mestre e de usuário por meio o Transact-SQL. Para saber mais, consulte [Como: definir configurações de firewall (Banco de Dados SQL do Azure)](article:sql-database-configure-firewall-settings)

## Gerenciando programaticamente as regras de firewall

Além do Portal de Gerenciamento do Azure, as regras de firewall podem ser gerenciadas por meio de programação usando Transact-SQL, API REST e PowerShell do Azure. As tabelas a seguir descrevem o conjunto de comandos disponíveis para cada método.


### Transact-SQL

| Exibição do catálogo ou Procedimento armazenado | Nível | Descrição |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall\\\_rules](https://msdn.microsoft.com/library/dn269980.aspx) | Servidor | Exibe as regras de firewall atuais no nível do servidor | | [sp\\\_set\\\_firewall\\\_rule](https://msdn.microsoft.com/library/dn270017.aspx) | Servidor | Cria ou atualiza as regras de firewall no nível do servidor | | [sp\\\_delete\\\_firewall\\\_rule](https://msdn.microsoft.com/library/dn270024.aspx) | Servidor | Remove regras de firewall no nível do servidor | | [sys.database\\\_firewall\\\_rules](https://msdn.microsoft.com/library/dn269982.aspx) | Banco de dados | Exibe as regras de firewall atuais no nível do banco de dados | | [sp\_set\\\_database\\\_firewall\\\_rule](https://msdn.microsoft.com/library/dn270010.aspx) | Banco de dados | Cria ou atualiza as regras de firewall no nível do banco de dados | | [sp\\\_delete\\\_database\\\_firewall\_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Bancos de dados | Remove regras de firewall no nível do banco de dados |

### API REST

| API | Nível | Descrição |
|----------------------|--------|------------------------------------------------------------------|
| [Listar regras de firewall](https://msdn.microsoft.com/library/azure/dn505715.aspx) | Servidor | Exibe as regras de firewall atuais no nível de servidor |
| [Criar uma regra de firewall](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Servidor | Cria ou atualiza as regras de firewall no nível de servidor |
| [Definir regra de firewall](https://msdn.microsoft.com/library/azure/dn505707.aspx) | Servidor | Atualiza as propriedades de uma regra de firewall existente no nível de servidor |
| [Excluir regra de firewall](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Servidor | Remove as regras de firewall no nível de servidor |


### PowerShell do Azure

| Cmdlet | Nível | Descrição |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) | Servidor | Retorna as regras de firewall atuais no nível de servidor |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) | Servidor | Cria as regras de firewall no nível de servidor |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) | Servidor | Atualiza as propriedades de uma regra de firewall existente no nível de servidor |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Servidor | Remove as regras de firewall no nível de servidor |

> [AZURE.NOTE]Pode haver um atraso de até cinco minutos para as alterações nas configurações de firewall serem efetivadas.

## Solucionando problemas com o firewall

Considere os seguintes pontos quando o acesso ao serviço de Banco de Dados SQL do Microsoft Azure não se comportar conforme o esperado:

- **Configuração de firewall local:** antes que o computador possa acessar o Banco de Dados SQL do Azure, talvez seja necessário criar uma exceção de firewall em seu computador para a porta TCP 1433. 

- **Conversão do endereço de rede (NAT):** devido à NAT, o endereço IP usado pelo computador para se conectar ao Banco de Dados SQL do Azure pode ser diferente do endereço IP exibido nas configurações de IP do computador. Para exibir o endereço IP que o computador está usando para se conectar ao Azure, faça logon no Portal de Gerenciamento e navegue até a guia **Configurar** no servidor que hospeda seu banco de dados. Na seção **Endereços IP Permitidos**, o **Endereço IP do Cliente Atual** é exibido. Clique em **Adicionar** aos **Endereços IP Permitidos** para permitir que este computador acesse o servidor.

- **As alterações à lista de permissões ainda não entraram em vigor:** pode ocorrer um atraso de cinco minutos para que as alterações na configuração do firewall do Banco de Dados SQL do Azure entrem em vigor.

- **O logon não está autorizado ou uma senha incorreta foi usada:** se um logon não tiver permissões no servidor de Banco de Dados SQL, ou a senha usada estiver incorreta, a conexão com o servidor de Banco de Dados SQL será negada. Criar uma configuração de firewall apenas oferece aos clientes uma oportunidade de tentar se conectar ao servidor; cada cliente deve fornecer as credenciais de segurança necessárias. Para saber mais sobre a preparação de logons, consulte Gerenciando bancos de dados, logons e usuários no Banco de Dados SQL do Azure.

- **Endereço IP dinâmico:** se você tiver uma conexão com a Internet com endereçamento IP dinâmico e estiver com dificuldades para atravessar o firewall, tente uma das seguintes soluções:

 - Peça ao seu Provedor de serviços de Internet (ISP) o intervalo de endereços IP atribuído aos computadores clientes que acessarão o servidor de Banco de Dados SQL do Azure e, em seguida, adicione o intervalo de endereços IP como uma regra de firewall.

 - Obtenha o endereçamento IP estático para os computadores cliente e, em seguida, adicione os endereços IP como regras de firewall.

## Consulte também

[Como definir as configurações de firewall (Banco de Dados SQL do Azure)](article:sql-database-configure-firewall-settings)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png

<!---HONumber=06-->