<properties
   pageTitle="Azure AD Connect: Pré-requisitos e hardware | Microsoft Azure"
   description="Descrição do artigo que será exibida nas páginas iniciais e na maioria dos resultados da pesquisa"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="stevenpo"
   editor="curtand"/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="11/16/2015"
   ms.author="andkjell;billmath"/>

# Pré-requisitos do Azure AD Connect
Este tópico descreve os pré-requisitos e requisitos de hardware para o Azure AD Connect.

## Antes de instalar o Azure AD Connect
Antes de instalar o Azure AD Connect, aqui estão algumas coisas que você precisará.

**AD do Azure**

- Uma assinatura do Azure ou uma [Assinatura de avaliação do Azure](http://azure.microsoft.com/pricing/free-trial/) Isso é necessário apenas para acessar o portal do Azure, não para utilizar o Azure AD Connect. Se estiver usando o PowerShell ou o Office 365, você não precisará de uma assinatura do Azure para usar o Azure AD Connect. Se você tiver uma licença do Office 365, também pode usar o portal do Office 365. Com uma licença do Office 365 paga, você também pode entrar no portal do Azure a partir do portal do Office 365.
- [Adicione e verifique o domínio](active-directory-add-domain.md) que você planeja usar no AD do Azure. Por exemplo, se você planeja usar contoso.com para os usuários, em seguida, verifique se este domínio foi verificado e se não está usando apenas o domínio padrão contoso.onmicrosoft.com.
- Um diretório do AD do Azure por padrão permitirá 50 mil objetos. Quando você verificar o domínio, o limite aumentará para 300 mil objetos. Se você precisar de mais objetos no AD do Azure, precisará abrir um caso de suporte para aumentar ainda mais o limite. Se você precisar de mais de 500 mil objetos, precisará de uma licença, como Office 365, AD do Azure Básico, AD do Azure Premium ou Enterprise Mobility Suite.

**Ambiente e servidores locais**

- A versão de esquema do AD e o nível funcional de floresta devem ser o Windows Server 2003 ou posterior. Os controladores de domínio podem executar qualquer versão, desde os requisitos de nível de floresta e de esquema sejam atendidos.
- Se você planeja usar o recurso **write-back de senha**, os Controladores de Domínio devem estar no Windows Server 2008 (com o SP mais recente) ou posterior.
- O Azure AD Connect não pode ser instalado no Small Business Server ou no Windows Server Essentials. O servidor deve estar usando o Windows Server standard ou superior.
- O Azure AD Connect deve ser instalado no Windows Server 2008 ou posterior. Esse servidor pode ser um controlador de domínio ou um servidor membro se usar configurações expressas. Se você usar configurações personalizadas, o servidor também pode ser independente e não precisa ser ingressado em um domínio.
- Se você instalar o Azure AD Connect no Windows Server 2008, certifique-se de aplicar os hotfixes mais recentes do Windows Update. A instalação não poderá ser iniciada com um servidor sem patch.
- Se você planeja usar o recurso **sincronização de senha**, o servidor do Azure AD Connect deve ser o Windows Server 2008 R2 SP1 ou posterior.
- O servidor do Azure AD Connect deve ter o [.Net 4.5.1](#component-prerequisites) ou posterior e o [PowerShell 3.0](#component-prerequisites) ou posterior instalados.
- Se os Serviços de Federação do Active Directory estiverem sendo implantados, os servidores onde o AD FS ou Proxy de Aplicativo Web será instalado devem ser Windows Server 2012 R2 ou posterior. [O gerenciamento remoto do Windows](#windows-remote-management) deve estar habilitado nesses servidores para instalação remota.
- Se o Serviços de Federação do Active Directory estiver sendo implantado, você precisará dos [Certificados SSL](#ssl-certificate-requirements).
- O Azure AD Connect requer um banco de dados do SQL Server para armazenar dados de identidade. Por padrão, um SQL Server 2012 Express LocalDB (uma versão light do SQL Server Express) é instalado e a conta de serviço para o serviço é criada no computador local. O SQL Server Express tem um limite de tamanho de 10GB que permite que você gerencie aproximadamente 100.000 objetos. Se precisar gerenciar um volume maior de objetos de diretório, você precisa apontar o processo de instalação para uma versão diferente do SQL Server. O Azure Connect AD oferece suporte a todos as versões do Microsoft SQL Server do SQL Server 2008 (com SP4) para o SQL Server 2014.

**Contas**

- Uma conta de Administrador Global de AD do Azure para o diretório do AD do Azure com o qual você deseja se integrar.
- Uma conta de Administrador Corporativo do Active Directory local se você usar configurações expressas ou atualizar do DirSync.
- [As contas são Active Directory](active-directory-aadconnect-accounts-permissions.md) se você usar o caminho de instalação de configurações personalizadas.

**Conectividade**

- Se estiver usando um proxy de saída para se conectar à Internet, a configuração a seguir no arquivo **C:\\Windows\\Microsoft.NET\\Framework64\\v4.0.30319\\Config\\machine.config** deve ser adicionada para que o assistente de instalação e a sincronização do Azure AD possam se conectar à Internet e ao Azure AD. Esse texto deve ser inserido na parte inferior do arquivo. Nesse código, &lt;PROXYADRESS&gt; representa o nome de host ou o endereço IP do proxy real.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Se o servidor proxy exigir autenticação, a seção se parecerá com o mostrado abaixo.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Com essa alteração em machine.config, o assistente de instalação e o mecanismo de sincronização responderão às solicitações de autenticação do servidor proxy. Em todas as páginas do assistente de instalação, com exceção da página **Configurar**, as credenciais do usuário conectado são usadas. Na página **Configurar** ao final do assistente de instalação, o contexto é alternado para a [conta de serviço](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) que foi criada.

Veja o MSDN para obter mais informações sobre o [Elemento proxy padrão](https://msdn.microsoft.com/library/kd3cf2ex.aspx).

- Se seu proxy limitar as URLs que podem ser acessadas, as URLs documentadas em [Intervalos de endereços IP e URLs do Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) devem ser abertas no proxy.

**Outros**

- Opcional: uma conta de usuário de teste para verificar a sincronização.

## Pré-requisitos do componente

O Azure AD Connect depende do PowerShell e do .Net 4.5.1. Dependendo da versão do Windows Server, faça o seguinte:

- Windows Server 2012R2
  - PowerShell é instalado por padrão, nenhuma ação é necessária.
  - .NET 4.5.1 e versões posteriores são oferecidas pelo Windows Update. Verifique se você instalou as atualizações mais recentes para o Windows Server no painel de controle.
- Windows Server 2008R2 e Windows Server 2012
  - A versão mais recente do PowerShell está disponível no **Windows Management Framework 4.0**, disponível no [Centro de Download da Microsoft](http://www.microsoft.com/downloads).
  - O .NET 4.5.1 e versões posteriores estão disponíveis no [Centro de Download da Microsoft](http://www.microsoft.com/downloads).
- Windows Server 2008
  - A versão mais recente do PowerShell com suporte está disponível no **Windows Management Framework 3.0**, disponível no [Centro de Download da Microsoft](http://www.microsoft.com/downloads).
 - O .NET 4.5.1 e versões posteriores estão disponíveis no [Centro de Download da Microsoft](http://www.microsoft.com/downloads).

## Gerenciamento Remoto do Windows

 Ao usar o Azure AD Connect para implantar os Serviços de Federação do Active Directory ou o Proxy de Aplicativo Web, verifique os requisitos abaixo para garantir que a conectividade e a configuração sejam bem-sucedidas:

 - Se o servidor de destino for associado ao domínio, verifique se o Windows Remote Managed está habilitado
    - Em uma janela Comando de PSH com privilégio elevado, use o comando `Enable-PSRemoting –force`
 - Se o servidor de destino for um computador WAP não associado ao domínio, haverá alguns requisitos adicionais
 	- No computador de destino (computador WAP):
         - Verifique se o serviço winrm (Windows Remote Management/WS-Management) está em execução por meio do snap-in de Serviços
         - Em uma janela Comando de PSH com privilégio elevado, use o comando `Enable-PSRemoting –force`
    - No computador que está executando o assistente (se o computador de destino não for associado ao domínio ou for de um domínio não confiável):
        - Em uma janela Comando de PSH com privilégio elevado, use o comando `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
 	    - No Gerenciador de Servidores:
 		     - adicione o host DMZ WAP ao pool de computadores (gerenciador de servidores -> Gerenciar -> Adicionar Servidores... use a guia DNS)
 		     - Guia Todos os Servidores do Gerenciador de Servidores: clique com o botão direito do mouse no servidor WAP e escolha Gerenciar como... e digite credenciais locais (não de domínio) para o computador WAP
 		     - Para validar a conectividade PSH remota, na guia Todos os Servidores do Gerenciador de Servidores: clique com o botão direito do mouse no servidor WAP e escolha Windows PowerShell. Uma sessão remota do PSH deverá ser aberta para garantir que sessões remotas do PowerShell possam ser estabelecidas.

## Requisitos de Certificado SSL

**Importante:** é altamente recomendável usar o mesmo certificado SSL em todos os nós do farm do AD FS, bem como em todos os servidores proxy de aplicativo Web.

- O certificado deve ser um certificado X509.
- Você pode usar um certificado autoassinado nos servidores da federação em um ambiente de laboratório de teste. No entanto, para um ambiente de produção, recomendamos que você obtenha o certificado de uma CA pública.
    - Se usar um certificado que não é confiável publicamente, verifique se o certificado instalado em cada servidor proxy de aplicativo Web é confiável no servidor local e em todos os servidores de federação
- A identidade do certificado deve corresponder ao nome do serviço de federação (por exemplo, fs.contoso.com).
    - A identidade é uma extensão SAN (nome alternativo da entidade) do tipo dNSName ou, se não houver entradas de SAN, o nome de entidade especificado como um nome comum.  
    - Várias entradas de SAN podem estar presentes no certificado, desde que uma delas coincide com o nome do serviço de federação.
    - Se você planeja usar o Ingresso no Local de Trabalho, um SAN adicional é necessário com o valor **enterpriseregistration.** seguido do sufixo de nome UPN de sua organização, por exemplo, **enterpriseregistration.contoso.com**.
- Não há suporte para certificados com base em chaves CNG (CryptoAPI de próxima geração) e provedores de armazenamento de chaves. Isso significa que você deve usar um certificado baseado em um CSP (provedor de serviços de criptografia) e não um KSP (provedor de armazenamento de chaves).
- Há suporte para certificados curinga.

## Componentes de suporte do Azure AD Connect

Esta é uma lista de componentes que o Azure AD Connect instalará no servidor onde o Azure AD Connect está instalado. Esta lista é para uma instalação básica do Express. Se você optar por usar um SQL Server diferente na página de serviços de sincronização de instalação, o SQL Express LocalDB não é instalado localmente.

- Utilitários de linha de comando do Microsoft SQL Server 2012
- Microsoft SQL Server 2012 Native Client
- LocalDB do Microsoft SQL Server 2012 Express
- Módulo do Active Directory do Azure para Windows PowerShell
- Assistente de Conexão do Microsoft Online Services para profissionais de TI
- Pacote de redistribuição de Microsoft Visual C++ 2013


## Requisitos de hardware para o Azure AD Connect
A tabela a seguir mostra os requisitos mínimos para o computador de sincronização do Azure AD Connect.

| Número de objetos no Active Directory | CPU | Memória | Tamanho do disco rígido |
| ------------------------------------- | --- | ------ | --------------- |
| Menos de 10.000 | 1,6 GHz | 4 GB | 70 GB |
| 10\.000–50.000 | 1,6 GHz | 4 GB | 70 GB |
| 50\.000–100.000 | 1,6 GHz | 16 GB | 100 GB |
| Para 100.000 ou mais objetos, é necessária a versão completa do SQL Server| | | |
| 100\.000–300.000 | 1,6 GHz | 32 GB | 300 GB |
| 300\.000–600.000 | 1,6 GHz | 32 GB | 450 GB |
| Mais de 600.000 | 1,6 GHz | 32 GB | 500 GB |

Os requisitos mínimos para computadores que executam o AD FS ou servidores de aplicativos Web são os seguintes:

- CPU: Dual-core de 1,6 GHz ou superior
- MEMÓRIA: 2 GB ou superior
- VM do Azure: Configuração A2 ou superior


## Próximas etapas
Saiba mais sobre como [Integrar suas identidades locais com o Active Directory do Azure](active-directory-aadconnect.md).

<!---HONumber=AcomDC_1217_2015-->