<properties 
	pageTitle="Copiar ou mover dados para o Armazenamento com o AzCopy | Microsoft Azure" 
	description="Use o utilitário AzCopy para mover ou copiar dados para ou de conteúdo de blob, tabela e arquivo. Copie dados para o Armazenamento do Azure de arquivos locais ou copie dados dentro na mesma conta ou entre contas de armazenamento. Migre facilmente seus dados para o Armazenamento do Azure." 
	services="storage" 
	documentationCenter="" 
	authors="micurd" 
	manager="jahogg" 
	editor="cgronlun"/>

<tags 
	ms.service="storage" 
	ms.workload="storage" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="12/10/2015" 
	ms.author="micurd"/>

# Transferir dados com o Utilitário de Linha de Comando AzCopy

## Visão geral

AzCopy é um utilitário de linha de comando projetado para cópia de dados para e do armazenamento de Blob, Arquivo e Tabelas do Microsoft Azure. Com o AzCopy, é possível migrar seus dados do sistema de arquivos para o Armazenamento do Azure, ou vice-versa, usando comandos simples e com um desempenho ideal. Você também pode copiar dados de um objeto para outro em sua conta de armazenamento, ou entre contas de armazenamento.

> [AZURE.NOTE]Este guia pressupõe que você tenha o AzCopy 5.0 ou posterior instalado.

A visualização da biblioteca de Movimentação de Dados do Armazenamento do Microsoft Azure agora está disponível para download por meio do [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement). A Biblioteca de Movimentação de Dados é a biblioteca principal subjacente ao AzCopy. O código-fonte está disponível no [GitHub](https://github.com/Azure/azure-storage-net-data-movement). Para obter mais informações, veja [Apresentando a Visualização da Biblioteca de Movimentação de Dados do Armazenamento do Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).

## Baixar e instalar o AzCopy

1. Baixar a [versão mais recente do AzCopy](http://aka.ms/downloadazcopy)
2. Execute a instalação. Por padrão, o AzCopy é instalado no `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy\AzCopy.exe` (em um computador que executa o Windows de 64 bits) ou no `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy\AzCopy.exe` (em um computador que executa o Windows de 32 bits). No entanto, é possível alterar o caminho da instalação por meio do assistente de instalação.
3. Se quiser, você pode alterar o local da instalação do AzCopy para o caminho do sistema.

## Entenda a sintaxe da linha de comando do AZCopy

Em seguida, abra uma janela de comando e navegue até o diretório de instalação do AzCopy no computador, em que o executável `AzCopy.exe` está localizado. A sintaxe básica dos comandos do AzCopy é:

	AzCopy /Source:<source> /Dest:<destination> /Pattern:<filepattern> [Options]

> [AZURE.NOTE]A partir da versão 3.0.0 do AzCopy, a sintaxe da linha de comando do AzCopy exige que todos os parâmetros sejam especificados para incluir o nome do parâmetro, *por exemplo*, `/ParameterName:ParameterValue`.

## Escrever seu primeiro comando AzCopy

Uma maneira fácil de experimentar o AzCopy é carregar um arquivo do sistema de arquivos local para o armazenamento de Blobs. Em uma janela de console, execute o seguinte comando, primeiro substituindo os nomes dos recursos abaixo por seus próprios nomes de recurso válidos:
	
	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt

Observe que, ao copiar um único arquivo, você precisará especificar a opção `/Pattern` com o nome do arquivo. Mais exemplos estão disponíveis nas seções a seguir.

## Introdução aos parâmetros

Os parâmetros do AzCopy são descritos na tabela abaixo. Também é possível digitar um dos comandos a seguir na linha de comando para obter ajuda no uso do AzCopy:

- Para obter ajuda detalhada da linha de comando para o AzCopy: `AzCopy /?`
- Para obter ajuda detalhada para qualquer parâmetro do AzCopy: `AzCopy /?:SourceKey`
- Para obter exemplos da linha de comando: `AzCopy /?:Samples` 

<table>
  <tr>
    <th>Nome da opção</th>
    <th>Descrição</th>
    <th>Aplicável ao armazenamento de blob (S/N)</th>
    <th>Aplicável ao armazenamento de arquivo (S/N)</th>
    <th>Aplicável ao armazenamento de tabelas (S/N)</th>
  </tr>
  <tr>
    <td><b>/Source:&lt;source></b></td>
    <td>Especifica os dados de origem para cópia. A origem pode ser um diretório do sistema de arquivos, um contêiner de blob, um diretório virtual de blob, um compartilhamento de arquivos de armazenamento, um diretório de arquivos de armazenamento ou uma tabela do Azure.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/Dest:&lt;destination></b></td>
    <td>Especifica o destino da cópia. O destino pode ser um diretório do sistema de arquivos, um contêiner de blob, um diretório virtual de blob, um compartilhamento de arquivos de armazenamento, um diretório de arquivos de armazenamento ou uma tabela do Azure.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/Pattern:&lt;file-pattern></b></td>
      <td>
          Especifica um padrão de arquivo que indica quais arquivos devem ser copiados. O comportamento do parâmetro /Pattern é determinado pelo local dos dados de origem e pela presença da opção do modo recursivo. O modo recursivo é especificado pela opção /S.
          <br /><br />
          Se a origem especificada for um diretório no sistema de arquivos, os curingas padrão estão em vigor, e o padrão de arquivo fornecido é comparado com os arquivos dentro do diretório. Se a opção /S for especificada, o AzCopy também compara o padrão especificado com todos os arquivos em todas as subpastas do diretório.
          <br /><br />
          Se a origem especificada for um contêiner de blob ou um diretório virtual, os curingas não são aplicados. Se a opção /S for especificada, o AzCopy interpreta o padrão do arquivo especificado como um prefixo de blob. Se a opção /S não for especificada, o AzCopy compara o padrão do arquivo com os nomes de blob exatos.
          <br /><br />
          Se a origem especificada for um compartilhamento de arquivos do Azure, você deve especificar o nome exato do arquivo (por exemplo, abc.txt) para copiar um único arquivo ou especificar a opção /S para copiar todos os arquivos recursivamente no compartilhamento. A tentativa de especificar, ao mesmo tempo, um padrão de arquivo e uma opção /S resultará em erro.
          <br /><br />
          O AzCopy diferencia maiúsculas de minúsculas quando /Source é um contêiner de blob ou diretório virtual de blob, e não diferencia maiúsculas de minúsculas em todos os outros casos.
          <br/><br />
          O padrão de arquivo usado quando nenhum padrão de arquivo é especificado é *.* para um local do sistema de arquivos, ou um prefixo vazio para um local de armazenamento do Azure. Não é possível especificar diversos padrões para os arquivos.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/DestKey:&lt;storage-key></b></td>
    <td>Especifica a chave de conta de armazenamento do recurso de destino.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td class="auto-style1"><b>/DestSAS:&lt;sas-token></b></td>
    <td class="auto-style1">Especifica uma SAS (Assinatura de acesso compartilhado) com as permissões de LEITURA e GRAVAÇÃO para o destino (se for aplicável). Coloque a SAS entre aspas duplas, porque ela pode conter caracteres de linha de comando especiais.<br /><br />
        Se o recurso de destino for um contêiner de blob, compartilhamento de arquivo ou tabela, você poderá especificar essa opção seguida pelo token da SAS, ou a SAS como parte do URI do contêiner de blob, compartilhamento de arquivo ou tabela de destino, sem essa opção.<br /><br />
        Se a origem e o destino forem blobs, ambos devem residir na mesma conta de armazenamento.</td>
    <td class="auto-style1">S</td>
    <td class="auto-style1">S<br /></td>
    <td class="auto-style1">S<br /></td>
  </tr>
  <tr>
    <td><b>/SourceKey:&lt;storage-key></b></td>
    <td>Especifica a chave de conta de armazenamento para o recurso de origem.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/SourceSAS:&lt;sas-token></b></td>
    <td>Especifica uma Assinatura de acesso compartilhado com as permissões de LEITURA e LISTAGEM para a origem (se for aplicável). Coloque a SAS entre aspas duplas, porque ela pode conter caracteres de linha de comando especiais.
        <br /><br />
        Se o recurso da fonte foir um contêiner de blob e não for fornecida uma chave nem uma SAS, o contêiner será lido por meio de acesso anônimo.
        <br /><br />
        Se a origem for um compartilhamento de arquivo ou uma tabela, será necessário fornecer uma chave ou SAS.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/S</b></td>
    <td>Especifica o modo recursivo para operações de cópia. No modo recursivo, o AzCopy copiará todos os blobs ou arquivos correspondentes ao padrão do arquivo, inclusive os presentes nas subpastas.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/BlobType:&lt;block | page | append></b></td>
    <td>Especifica se o blob de destino é um blob de blocos, um blob de páginas ou um blob anexo. <br /><br />
	Essa opção só é aplicável no carregamento de blob. Do contrário, um erro é gerado. Se o destino for um blob e essa opção não estiver especificada, por padrão, o AzCopy criará um blob de blocos.</td>
    <td>S</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/CheckMD5</b></td>
    <td>Calcula um hash MD5 para dados baixados e certifica que o hash MD5 armazenado no blob, ou a propriedade Content-MD5 do arquivo, corresponde ao hash calculado. Como, por padrão, a verificação MD5 permanece desativada, você deve especificar essa opção para realizar a verificação MD5 ao baixar os dados.
	<br /><br />
    O Azure Storage não assegura que o hash MD5 armazenado para o blob ou o arquivo esteja atualizado. É de responsabilidade do cliente atualizar o MD5 sempre que o blob ou o arquivo é modificado.
	<br /><br />
    O AzCopy sempre define a propriedade Content-MD5 para um blob ou um arquivo do Azure depois de carregá-lo no serviço.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/Snapshot</b></td>
    <td>Indica se é necessário transferir ou não os instantâneos. Essa opção só é válida quando a origem é um blob. 
        <br /><br />
        Os instantâneos de blob transferidos são renomeados neste formato: [nome-blob](tempo-instantâneo)[extensão]. 
        <br /><br />
        Por padrão, os instantâneos não são copiados.</td>
    <td>S</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/V:verbose log-file</b></td>
    <td>Produz mensagens de status detalhadas em um arquivo de log.
	<br /><br />Por padrão, o arquivo de log detalhado é chamado <code>AzCopyVerbose.log</code> em <code>%LocalAppData%\Microsoft\Azure\AzCopy</code>. Se você especificar um local de arquivo existente para essa opção, o log detalhado será acrescentado a esse arquivo.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/Z:[pasta-de-ficheiro-diário]</b></td>
    <td>Especifica uma pasta de arquivo de diário para retomar uma operação.<br />
        O AzCopy sempre dará suporte à retomada caso uma operação tenha sido interrompida.<br />
        Se essa opção não for especificada ou for especificada sem um caminho de pasta, o AzCopy criará o arquivo de diário no local padrão, que é <code>%LocalAppData%\Microsoft\Azure\AzCopy</code>.<br />
        Sempre que você emite um comando para o AzCopy, ele verifica se um arquivo de diário existe na pasta padrão ou se está em uma pasta especificada por meio dessa opção. Se o arquivo de diário não estiver em nenhum dos lugares, o AzCopy tratará a operação como nova e gerar um novo arquivo de diário.
        <br /><br />
		Se o arquivo de diário existir, o AzCopy verificará se a linha de comando inserida corresponde à linha de comando no arquivo de diário. Se as duas linhas de comando forem correspondentes, o AzCopy retomará a operação incompleta. Se elas não forem correspondentes, será solicitado que você substitua o arquivo de diário para iniciar uma nova operação ou que cancele a operação atual. 
        <br /><br />
        O arquivo de diário é excluído mediante a conclusão bem-sucedida da operação.
		<br /><br />
		A retomada de uma operação de um arquivo de diário criado por uma versão anterior do AzCopy não é compatível.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/@:parameter-file</b></td>
    <td>Especifica um arquivo que contém parâmetros. O AzCopy processa os parâmetros no arquivo como se eles tivessem sido especificados na linha de comando.<br /> 
		Em um arquivo de resposta, é possível especificar vários parâmetros em um único arquivo ou especificar cada parâmetro na própria linha. Um parâmetro individual não pode abranger várias linhas. 
        <br /><br />
		Os arquivos de resposta podem incluir linhas de comentários iniciadas pelo símbolo <code>#</code>. 
        <br /><br />
        É possível especificar vários arquivos de resposta. No entanto, o AzCopy não permite arquivos de resposta aninhados.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/Y</b></td>
    <td>Suprime todas as solicitações de confirmação do AzCopy.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/L</b></td>
    <td>Especifica uma operação de listagem apenas. Nenhum dado é copiado.
    <br /><br />
    O AzCopy interpretará o uso dessa opção como uma simulação para execução da linha de comando sem essa opção /L, e contará quantos objetos serão copiados. Você pode especificar a opção /V ao mesmo tempo para verificar quais objetos serão copiados no log detalhado.
    <br /><br />
    O comportamento dessa opção também é determinado pelo local dos dados de origem e pela presença da opção no modo recursivo /S e pela opção de padrão de arquivo /Pattern.
    <br /><br />
    O AzCopy exige a permissão de LISTAGEM e de LEITURA deste local de origem ao usar essa opção.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/MT</b></td>
    <td>Define a hora da última modificação do arquivo baixado como a mesma do blob de origem ou do arquivo.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XN</b></td>
    <td>Exclui um recurso de origem mais novo. O recurso não será copiado se a hora da última modificação na origem for igual ou mais recente do que o destino.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XO</b></td>
    <td>Exclui um recurso de origem mais antigo. O recurso não será copiado se a hora da última modificação da origem for igual ou anterior à do destino.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/A</b></td>
    <td>Carrega apenas arquivos que tenham o atributo Archive definido.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/IA:[RASHCNETOI]</b></td>
    <td>Carrega apenas arquivos que tenham algum dos atributos especificados definido.<br /><br />
        Entre os atributos disponíveis estão:  
        <br /><br />
        R&#160;&#160;&#160;Arquivos somente leitura
        <br />
        A&#160;&#160;&#160;Arquivos prontos para arquivamento
        <br />
        S&#160;&#160;&#160;Arquivos de sistema
        <br />
        H&#160;&#160;&#160;Arquivos ocultos
        <br />
        C&#160;&#160;&#160;Arquivo compactado
        <br />
        N&#160;&#160;&#160;Arquivos normais
        <br />
        E&#160;&#160;&#160;Arquivos criptografados
        <br />
        T&#160;&#160;&#160;Arquivos temporários
        <br />
        O&#160;&#160;&#160;Arquivos offline
        <br />
        I&#160;&#160;&#160;Arquivos não indexados</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XA:[RASHCNETOI]</b></td>
    <td>Exclui arquivos que tenham qualquer dos atributos especificados definido.<br /><br />
        Entre os atributos disponíveis estão:  
        <br /><br />
        R&#160;&#160;&#160;Arquivos somente leitura  
        <br />
        A&#160;&#160;&#160;Arquivos prontos para arquivamento  
        <br />
        S&#160;&#160;&#160;Arquivos de sistema  
        <br />
        H&#160;&#160;&#160;Arquivos ocultos  
        <br />
        C&#160;&#160;&#160;Arquivo compactado  
        <br />
        N&#160;&#160;&#160;Arquivos normais  
        <br />
        E&#160;&#160;&#160;Arquivos criptografados  
        <br />
        T&#160;&#160;&#160;Arquivos temporários  
        <br />
        O&#160;&#160;&#160;Arquivos offline  
        <br />
        I&#160;&#160;&#160;Arquivos não indexados</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/Delimiter:&lt;delimiter></b></td>
    <td>Indica o caractere delimitador usado para delimitar diretórios virtuais em um nome de blob.<br /><br />
        Por padrão, o AzCopy usa / como o caractere delimitador. No entanto, o AzCopy dá suporte ao uso de qualquer caractere comum (como @, # ou %) como delimitador. Se precisar incluir um desses caracteres especiais na linha de comando, coloque o nome do arquivo entre aspas duplas. 
        <br /><br />
        Essa opção só é aplicável para o download de blobs.</td>
    <td>S</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/NC:&lt;number-of-concurrents></b></td>
    <td>Especifica o número de operações simultâneas.
        <br /><br />
        Por padrão, o AzCopy inicia uma determinada quantidade de operações simultâneas para aumentar a taxa de transferência dos dados. O grande número de operações simultâneas em um ambiente com baixa largura de banda pode sobrecarregar a conexão de rede, evitando que as operações sejam concluídas. Limite operações simultâneas com base na largura de banda real de rede disponível.
        <br /><br />
		O limite máximo de operações simultâneas é 512.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/SourceType:Blob|Table</b></td>
    <td>Especifica se o recurso <code>origem</code> é um blob disponível no ambiente de desenvolvimento local, em execução no emulador de armazenamento.</td>
    <td>S</td>
    <td>N</td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/DestType:Blob|Table</b></td>
    <td>Especifica se o recurso <code>destino</code> é um blob disponível no ambiente de desenvolvimento local, em execução no emulador de armazenamento.</td>
    <td>S</td>
    <td>N</td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><strong>/PKRS:&lt;"key1#key2#key3#..."></strong></td>
    <td>Divide o intervalo de chaves de partição para possibilitar a exportação paralela dos dados, o que aumenta a velocidade dessa operação.
        <br /><br />
        Se essa opção não for especificada, o AzCopy usa um único thread para exportar entidades de tabela. Por exemplo, se o usuário especifica /PKRS:"aa#bb", o AzCopy inicia três operações simultâneas.
        <br /><br />
        Cada operação exporta um dos três intervalos de chaves de partição, como mostramos abaixo: 
        <br /><br />
        &#160;&#160;&#160;[&lt;primeira chave da partição>, aa) 
        <br /><br />
        &#160;&#160;&#160;[aa, bb)
        <br /><br />
        &#160;&#160;&#160;[bb, &lt;última chave da partição>] </td>
    <td>N</td>
    <td>N</td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><strong>/SplitSize:</strong><file-size><strong>&lt;file-size></strong></td>
    <td>Especifica o tamanho da divisão do arquivo exportado em MB, o valor mínimo permitido é de 32.
        <br /><br />
        Se essa opção não for especificada, o AzCopy exporta os dados da tabela para um único arquivo.
        <br /><br />
        Se os dados da tabela forem exportados para um blob e o tamanho do arquivo exportado alcançar o limite de 200 GB, o AzCopy divide o arquivo exportado, mesmo que essa opção não seja especificada. </td>
    <td>N</td>
    <td>N</td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/EntityOperation:&lt;InsertOrSkip | InsertOrMerge | InsertOrReplace> </b>
</td>
    <td>Especifica o comportamento da importação dos dados da tabela.
        <br /><br />
        InsertOrSkip — Ignora uma entidade existente ou insere uma nova entidade, caso ela não exista na tabela.
        <br /><br />
        InsertOrMerge — Mescla uma entidade existente ou insere uma nova entidade, caso ela não exista na tabela.
        <br /><br />
        InsertOrReplace — Substitui uma entidade existente ou insere uma nova entidade, caso ela não exista na tabela. </td>
    <td>N</td>
    <td>N</td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/Manifest:&lt;manifest-file></b></td>
    <td>Especifica o arquivo de manifesto para a operação de exportação e importação de tabela.
	<br /><br />
    Essa opção é opcional durante a operação de exportação. O AzCopy gerará um arquivo de manifesto com nome predefinido se essa opção não for especificada.
    <br /><br />
    Essa opção é exigida durante a operação de importação para localização dos arquivos de dados.</td>
    <td>N</td>
    <td>N</td>
    <td>S<br /></td>
  </tr>
  <tr>
    <td><b>/SyncCopy</b></td>
    <td>Indica se devem ser copiados de forma síncrona blobs ou arquivos entre dois pontos de extremidade de armazenamento do Azure. <br /><br />
		O AzCopy por padrão usa cópia assíncrona no servidor. Especifique essa opção para executar uma cópia síncrona, que baixa blobs ou arquivos para a memória local e, em seguida, carrega-as para o armazenamento do Azure.
		<br /><br />
		Você pode usar essa opção ao copiar arquivos no armazenamento de Blob no armazenamento de arquivo ou do armazenamento de Blob para armazenamento de arquivos ou vice-versa.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/SetContentType:&lt;content-type></b></td>
    <td>Especifica o tipo de conteúdo MIME para blobs ou arquivos de destino. <br /><br />
		O AzCopy define o tipo de conteúdo para um blob ou arquivo <code>application/octet-stream</code> por padrão. Você pode definir o tipo de conteúdo para todos os blobs ou arquivos explicitamente especificando um valor para essa opção. 
		<br /><br />
		Se você especificar essa opção sem um valor, AzCopy definirá cada blob ou tipo de conteúdo do arquivo de acordo com a sua extensão de arquivo.</td>
    <td>S</td>
    <td>S<br /></td>
    <td>N</td>
  </tr>
    <tr>
    <td><b>/PayloadFormat:&lt;JSON | CSV></b></td>
    <td>Especifica o formato do arquivo de dados exportados da tabela.<br /><br />
    Se essa opção não for especificada, por padrão, o AzCopy exportará o arquivo de dados da tabela no formato JSON.</td>
    <td>N</td>
    <td>N</td>
    <td>S<br /></td>
  </tr>
</table>
<br/>

## Limite gravações simultâneas durante a cópia de dados

Ao copiar blobs ou arquivos usando o AZCopy, lembre-se de que outro aplicativo pode estar modificando os dados enquanto você os copia. Se possível, verifique se os dados que está copiando não estão sendo modificados durante a cópia. Por exemplo, ao copiar um VHD associado a uma máquina virtual do Azure, verifique se nenhum outro aplicativo está gravando no VHD, no momento. Uma boa maneira de fazer isso é ceder o recurso para ser copiado. Também é possível criar um instantâneo do VHD primeiro e, em seguida, copiar o instantâneo.

Se não for possível evitar que outros aplicativos gravem em blobs ou arquivos enquanto são copiados, lembre-se que, quando o trabalho terminar, os recursos copiados não poderão mais ter paridade total com os recursos de origem.

## Copie blobs do Azure com o AzCopy

Os exemplos abaixo demonstram vários cenários para copiar blobs com o AzCopy.

### Copiar um único blob

**Carregue um arquivo do sistema de arquivos no armazenamento de blob:**
	
	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt

**Baixe um blob do armazenamento de blob no sistema de arquivos:**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Para saber mais sobre como trabalhar com as chaves de acesso de armazenamento, veja [Exibir, copiar e regenerar chaves de acesso de armazenamento](../storage-create-storage-account/#regeneratestoragekeys).

### Copiar um blob por meio da cópia no servidor

Uma operação de cópia no servidor é realizada quando você copia um blob dentro de uma conta de armazenamento ou em contas de armazenamento. Para obter mais informações sobre operações de cópia no servidor, consulte [Introdução ao blob de cópia assíncrona entre contas](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx).

**Copie um blob dentro de uma conta de armazenamento:**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt 

**Copie um blob entre contas de armazenamento:**

	AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
 
### Copiar um blob da região secundária 

Se a conta de armazenamento tiver armazenamento com redundância geográfica com acesso de leitura habilitado, será possível copiar dados da região secundária.

**Copie um blob para a conta primária por meio da secundária:**

	AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

**Baixe um blob na secundária para um arquivo no sistema de arquivos:**

	AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

### Carregar um arquivo em um novo contêiner de blob ou diretório virtual

**Carregue um arquivo em um novo contêiner de blob**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mynewcontainer /DestKey:key /Pattern:abc.txt

Se o contêiner de destino especificado não existir, o AzCopy o criará e carregará o arquivo nele.

**Carregue um arquivo em um novo diretório virtual de blob**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Observe que, se o diretório virtual especificado não existir, o AzCopy carregará o arquivo para incluir o diretório virtual em seu nome (*por exemplo*, `vd/abc.txt` no exemplo acima).

### Baixar um blob em uma nova pasta

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Se a pasta `C:\myfolder` ainda não existir, o AzCopy a criará no sistema de arquivos e baixará `abc.txt ` na nova pasta.

### Carregar arquivos e subpastas de um diretório em um contêiner de maneira recursiva

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

A especificação da opção `/S` copia o conteúdo do diretório especificado para o armazenamento de Blobs de maneira recursiva, o que significa que todas as subpastas e seus arquivos serão copiados também. Por exemplo, suponhamos que os seguintes arquivos residam na pasta `C:\myfolder`:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\subfolder\a.txt
	C:\myfolder\subfolder\abcd.txt

Depois da operação de cópia, o contêiner incluirá os seguintes arquivos:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

### Carregar arquivos de um diretório em um contêiner de maneira não recursiva

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key

Se você não especificar a opção `/S` na linha de comando, o AzCopy não copiará de maneira recursiva. Apenas os arquivos no diretório especificado são copiados. Subpastas e seus arquivos NÃO são copiados. Por exemplo, vamos supor que a pasta `C:\myfolder` contenha os seguintes arquivos:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\subfolder\a.txt
	C:\myfolder\subfolder\abcd.txt

Depois da operação de cópia, o contêiner incluirá os seguintes arquivos:

	abc.txt
	abc1.txt
	abc2.txt

### Baixar todos os blobs de um contêiner em um diretório no sistema de arquivos de maneira recursiva

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Suponhamos que os seguintes blobs residam no contêiner especificado:

	abc.txt
	abc1.txt
	abc2.txt
	vd1\a.txt
	vd1\abcd.txt

Após a operação de cópia, o diretório `C:\myfolder` incluirá os seguintes arquivos:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\vd1\a.txt
	C:\myfolder\vd1\abcd.txt

### Baixar todos os blobs de um diretório de blob virtual em um diretório no sistema de arquivos de maneira recursiva

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer/vd1/ /Dest:C:\myfolder /SourceKey:key /S

Suponhamos que os seguintes blobs residam no contêiner especificado:

	abc.txt
	abc1.txt
	abc2.txt
	vd1\a.txt
	vd1\abcd.txt

Após a operação de cópia, o diretório `C:\myfolder` incluirá os arquivos a seguir. Apenas os blobs no diretório virtual são copiados:

	C:\myfolder\a.txt
	C:\myfolder\abcd.txt

### Carregar arquivos correspondentes ao padrão de arquivo especificado em um contêiner de maneira recursiva 

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Vamos supor que a pasta `C:\myfolder` contenha os seguintes arquivos:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt
	C:\myfolder\xyz.txt
	C:\myfolder\subfolder\a.txt
	C:\myfolder\subfolder\abcd.txt

Depois da operação de cópia, o contêiner incluirá os seguintes arquivos:

	abc.txt
	abc1.txt
	abc2.txt
	subfolder\a.txt
	subfolder\abcd.txt
	
### Baixar blobs com o prefixo especificado no sistema de arquivos de maneira recursiva

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Suponhamos que os blobs a seguir residam no contêiner especificado. Todos os blobs que começam com o prefixo `a` serão copiados:

	abc.txt
	abc1.txt
	abc2.txt
	xyz.txt
	vd1\a.txt
	vd1\abcd.txt

Após a operação de cópia, a pasta `C:\myfolder` incluirá os seguintes arquivos:

	C:\myfolder\abc.txt
	C:\myfolder\abc1.txt
	C:\myfolder\abc2.txt

O prefixo se aplica ao diretório virtual, que forma a primeira parte do nome do blob. No exemplo mostrado acima, como não corresponde ao prefixo especificado, o diretório virtual não é copiado.


### Copiar um blob e seus instantâneos para outra conta de armazenamento

	AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Depois da operação de cópia, o contêiner de destino incluirá o blob e seus instantâneos. Supondo que o blob no exemplo acima tenha dois instantâneos, o contêiner incluirá os seguintes blob e instantâneos:

	abc.txt
	abc (2013-02-25 080757).txt
	abc (2014-02-21 150331).txt


### Usar um arquivo de resposta para especificar parâmetros de linha de comando

	AzCopy /@:"C:\myfolder\abc.txt"

É possível incluir qualquer parâmetro de linha de comando do AZCopy em um arquivo de resposta. O AzCopy processa os parâmetros no arquivo como se eles tivessem sido especificados na linha de comando, realizando uma substituição direta pelo conteúdo do arquivo.

**Especificar um ou mais arquivos de resposta de linha única**

Vamos supor que haja um arquivo de resposta chamado `source.txt` que especifique um contêiner de origem:

	/Source:http://myaccount.blob.core.windows.net/mycontainer

E um arquivo de resposta chamado `dest.txt` que especifica uma pasta de destino no sistema de arquivos:

	/Dest:C:\myfolder

E um arquivo de resposta chamado `options.txt` que especifica opções para o AzCopy:

	/S /Y

Para chamar o AzCopy usando esses arquivos de resposta, que residem em um diretório `C:\responsefiles`, use este comando:

	AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

O AzCopy processa esse comando assim como faria se você tivesse incluído todos os parâmetros individuais na linha de comando:

	AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

**Especifique um arquivo de resposta multilinhas**

Vamos supor que um arquivo de resposta chamado `copyoperation.txt` contenha as linhas a seguir. Todo parâmetro do AzCopy é especificado na própria linha:

	/Source:http://myaccount.blob.core.windows.net/mycontainer
	/Dest:C:\myfolder
	/SourceKey:<sourcekey>
	/S 
	/Y

Para chamar o AzCopy usando esses arquivos de resposta, use este comando:

	AzCopy /@:"C:\responsefiles\copyoperation.txt"

O AzCopy processa esse comando assim como faria se você tivesse incluído todos os parâmetros individuais na linha de comando:

	AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

Cada parâmetro do AzCopy deve ser especificado todo em uma única linha. Por exemplo, o AzCopy falhará se você dividir o parâmetro em duas linhas, conforme mostrado aqui para o parâmetro `/sourcekey`:

	http://myaccount.blob.core.windows.net/mycontainer
 	C:\myfolder
	/sourcekey:
	[sourcekey]
	/S 
	/Y

### Especificar uma SAS (Assinatura de Acesso Compartilhado)
	
**Especificar uma SAS para o contêiner de origem usando a opção /sourceSAS**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /DestC:\myfolder /SourceSAS:SAS /S

**Especificar uma SAS para o contêiner de origem no URI do contêiner de origem**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

**Especificar uma SAS para o contêiner de destino usando a opção /destSAS**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer1 /DestSAS:SAS /Pattern:abc.txt

**Especificar uma SAS para os contêineres de origem e destino**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

### Especificar uma pasta de arquivo de diário

Sempre que você emite um comando para o AzCopy, ele verifica se um arquivo de diário existe na pasta padrão ou se está em uma pasta especificada por meio dessa opção. Se o arquivo de diário não estiver em nenhum dos lugares, o AzCopy tratará a operação como nova e gerar um novo arquivo de diário.

Se o arquivo de diário existir, o AzCopy verificará se a linha de comando inserida corresponde à linha de comando no arquivo de diário. Se as duas linhas de comando forem correspondentes, o AzCopy retomará a operação incompleta. Se elas não forem correspondentes, será solicitado que você substitua o arquivo de diário para iniciar uma nova operação ou que cancele a operação atual.

**Usar o local padrão para o arquivo de diário**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Se você omitir a opção `/Z` ou especificar a opção `/Z` sem o caminho da pasta, conforme mostrado acima, o AzCopy criará o arquivo de diário no local padrão, que é `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Se o arquivo de diário já existir, o AzCopy retomará a operação com base no arquivo de diário.

**Especificar um local padrão para o arquivo de diário**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Este exemplo criará o arquivo de diário se ele ainda não existir. Se ele existir, o AzCopy retomará a operação com base no arquivo de diário.

**Retomar uma operação do AzCopy**

	AzCopy /Z:C:\journalfolder\

Este exemplo retoma a última operação, cuja conclusão pode ter falhado.


### Gerar um arquivo de log

**Gravar o arquivo de log detalhado no local padrão**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Se você especificar a opção `/V` sem fornecer um caminho de arquivo para o log detalhado, o AzCopy criará o arquivo de log no local padrão, que é `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

**Gravar o arquivo de log detalhado em um local personalizado**

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Observe que, se você especificar um caminho relativo depois da opção `/V`, como `/V:test/azcopy1.log`, o log detalhado será criado no diretório de trabalho atual dentro de uma subpasta chamada `test`.


### Definir a hora da última modificação dos arquivos baixados como sendo a mesma dos blobs de origem

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

### Excluir blobs da operação de cópia com base na hora da última modificação

Especifique a opção `/MT` para comparar a hora da última modificação do blob de origem com a do arquivo de destino.

**Excluir blobs cuja hora da última modificação for a mesma ou mais recente do que o arquivo de destino**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

**Excluir blobs cuja hora da última modificação for a mesma ou mais antiga do que o arquivo de destino**

	AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

### Especificar o número de operações simultâneas para começar

A opção `/NC` especifica o número de operações de cópia simultâneas. Por padrão, o AzCopy começará operações simultâneas com oito vezes o número de processadores de núcleo disponíveis. Se estiver executando o AzCopy em uma rede de baixa largura de banda, você pode especificar um número menor para essa opção a fim de evitar uma falha causada pela concorrência de recursos.


### 	Executar o AzCopy em recursos de blob no emulador de armazenamento

	AzCopy /Source:https://127.0.0.1:10004/myaccount/myfileshare/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

### Copiar blobs de forma síncrona entre dois pontos de extremidade do armazenamento do Azure

O AzCopy por padrão copia dados entre dois pontos de extremidade de armazenamento assincronamente. Portanto, a operação de cópia será executada em segundo plano usando capacidade de largura de banda extra sem nenhum SLA em termos da velocidade de como o blob será copiado e o AzCopy verificará periodicamente o status da cópia até que a cópia esteja concluída ou tenha ocorrido uma falha.

A opção `/SyncCopy` garante que a operação de cópia obterá uma velocidade consistente. O AzCopy realiza a cópia síncrona baixando os blobs para copiar da fonte especificada para a memória local, e, em seguida, carregá-los para o destino de armazenamento de Blob.

	AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

Observe que `/SyncCopy` pode gerar custo de saída adicional comparando a cópia assíncrona. A abordagem recomendada é usar essa opção na VM do Azure que está na mesma região que a sua conta de armazenamento de origem para evitar o custo de saída.

### Especificar o tipo de conteúdo MIME de um blob de destino

Por padrão, o AzCopy define o tipo de conteúdo de um blob de destino como `application/octet-stream`. A partir da versão 3.1.0, você pode especificar explicitamente o tipo de conteúdo por meio da opção `/SetContentType:[content-type]`. Essa sintaxe define o tipo de conteúdo para todos os blobs em uma operação de cópia.

	AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Se você especificar `/SetContentType` sem um valor, o AzCopy definirá o tipo de conteúdo de cada blob ou arquivo de acordo com sua extensão de arquivo.

	AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## Copie arquivos no armazenamento de arquivos do Azure com o AzCopy

Os exemplos abaixo demonstram vários cenários para copiar arquivos do Azure com o AzCopy.

### Baixar um arquivo de um compartilhamento de arquivos do Azure no sistema de arquivos

	AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Observe que, se a origem especificada for um compartilhamento de arquivos do Azure, você deve especificar o nome exato do arquivo, (*por exemplo*, `abc.txt`) para copiar um único arquivo ou especificar a opção `/S` para copiar todos os arquivos do compartilhamento de maneira recursiva. A tentativa de especificar um padrão de arquivo e uma opção `/S` simultaneamente resultará em um erro.

### Baixar arquivos e pastas de um compartilhamento de arquivos do Azure no sistema de arquivos, de maneira recursiva, especificará a assinatura de acesso

	AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceSAS:SAS /S

Nenhuma pasta vazia será copiada.


### Carregar arquivos e pastas do sistema de arquivos no compartilhamento de arquivos do Azure de maneira recursiva

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Nenhuma pasta vazia será copiada.


### Carregar arquivos correspondentes ao padrão de arquivo especificado em um compartilhamento de arquivos do Azure de maneira recursiva

	AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

### Copiar arquivos de forma assíncrona no Armazenamento de arquivos do Azure

O Armazenamento de arquivos do Azure oferece suporte à cópia assíncrona no lado do servidor.

Cópia assíncrona do Armazenamento de arquivos para o Armazenamento de arquivos:

	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

Cópia assíncrona do Armazenamento de arquivos para o Blob de blocos:
  
	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Cópia assíncrona do Blob de blocos/páginas para o Armazenamento de arquivos:

	AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

Observe que não há suporte para a cópia assíncrona do Armazenamento de arquivos para o Blob de páginas.

### Copiar arquivos de forma síncrona no armazenamento de arquivos do Azure

Além da cópia assíncrona, o usuário também pode especificar a opção `/SyncCopy` para copiar dados do Armazenamento de Arquivos para o Armazenamento de Arquivos, do Armazenamento de Arquivos para o Armazenamento de Blobs e vice-versa de forma síncrona. O AzCopy faz isso baixando os dados de origem para a memória local e carregando-os novamente no destino.

	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

	AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
	
	AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Durante a cópia do Armazenamento de Arquivos para o Armazenamento de Blobs, o tipo de blob padrão é o blob de blocos, e o usuário pode especificar a opção `/BlobType:page` para alterar o tipo de blob de destino.

Observe que `/SyncCopy` pode gerar custo de saída adicional comparando a cópia assíncrona. A abordagem recomendada é usar essa opção na VM do Azure que está na mesma região que a sua conta de armazenamento de origem para evitar o custo de saída.


## Copiar entidades em uma tabela do Azure com o AzCopy

Os exemplos abaixo demonstram vários cenários para copiar entidades de tabela do Azure com o AzCopy.

### Exportar entidades para o sistema de arquivos local

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

O AzCopy grava um arquivo de manifesto na pasta de destino ou o contêiner de blob especificado. O arquivo do manifesto é usado pelo processo de importação para localizar os arquivos de dados necessários e executar a validação de dados durante o processo de importação. O arquivo de manifesto usa a seguinte convenção de nomenclatura por padrão:

	<account name>_<table name>_<timestamp>.manifest

O usuário também pode especificar a opção `/Manifest:<manifest file name>` para definir o nome do arquivo de manifesto.

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest


### Exportar entidades para o formato de arquivo de dados JSON e CSV

Por padrão, o AzCopy exporta entidades de Tabela para arquivos JSON. O usuário pode especificar a opção `/PayloadFormat:JSON|CSV` para decidir o tipo de arquivo de dados exportado.

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Ao especificar o formato de carga CSV, além dos arquivos de dados com a extensão `.csv` encontrados no local especificado pelo parâmetro `/Dest`, o AzCopy gerará um arquivo de esquema com a extensão `.schema.csv` para cada arquivo de dados. Observe que o AzCopy não inclui o suporte para "importar" o arquivo de dados CSV, você pode usar o formato JSON para exportar e importar dados de tabela.

### Exportar entidades para um blob do Azure

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

O AzCopy gera um arquivo de dados JSON na pasta local ou no contêiner do blob, seguindo esta convenção de nomenclatura:

	<account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

O arquivo de dados JSON gerado segue o formato de carga para metadados mínimos. Para obter detalhes sobre esse formado de carga, confira [Formato de carga para operações do serviço Tabela](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Observe que, ao exportar Entidades da Tabela de Armazenamento para o Blob de Armazenamento, primeiro, o AzCopy exportará as entidades da Tabela para arquivos de dados temporários locais e, em seguida, os carregará no Blob; esses arquivos de dados temporários são colocados na pasta de arquivos do diário com o caminho padrão “<code>%LocalAppData%\\Microsoft\\Azure\\AzCopy</code>” e você pode especificar a opção /Z:[pasta-de-arquivo-do-diário] para alterar o local da pasta de arquivo do diário e, portanto, alterar o local dos arquivos de dados temporários. O tamanho dos arquivos de dados temporários é decidido pelo tamanho das entidades da tabela e pelo tamanho especificado com a opção /SplitSize, embora o arquivo de dados temporários no disco local será excluído imediatamente depois que ele tiver sido carregado para o Blob, verifique que você tem espaço suficiente no disco local para armazenar esses arquivos de dados temporários antes de serem excluídos,

### Dividir os arquivos exportados

	AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

O AzCopy usa um *índice de volume* nos nomes dos arquivos de dados da divisão para diferenciar diversos arquivos. O índice do volume é composto por duas partes: um *índice de intervalo de chaves de partição* e um *índice de arquivos de divisão*. Os dois índices são de base zero.

O índice do intervalo de chaves de partição será 0 se o usuário não especificar a opção `/PKRS` (apresentada na próxima seção).

Por exemplo, vamos supor que o AzCopy gere dois arquivos de dados depois que o usuário especificar a opção `/SplitSize`. Os nomes dos arquivos de dados resultantes podem ser:

	myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
	myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Observe que o menor valor possível para a opção `/SplitSize` é 32 MB. Se o destino especificado for um armazenamento de Blobs, o AzCopy dividirá o arquivo de dados quando alcançar o tamanho limite do blob (200 GB), sem levar em conta se o usuário especificou ou não a opção `/SplitSize`.

### Exportar entidades simultaneamente

	AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

O AzCopy iniciará operações simultâneas para exportar entidades quando o usuário especificar a opção `/PKRS`. Cada operação exporta um intervalor de chaves de partição.

Observe que a opção `/NC` também controla a quantidade de operações simultâneas. O AzCopy usa a quantidade de processadores de núcleo como o valor padrão de `/NC` ao copiar entidades de tabela, mesmo que `/NC` não tenha sido especificado. Quando o usuário especifica a opção `/PKRS`, o AzCopy usa o menor valor dos dois valores (intervalos de chaves de partição versus operações simultâneas especificadas implícita ou explicitamente) para determinar quantas operações simultâneas devem ser iniciadas. Para obter mais detalhes, digite `AzCopy /?:NC` na linha de comando.

### Importar entidades simultaneamente

	AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace 

A opção `/EntityOperation` indica como inserir entidades na tabela. Os valores possíveis são:

- `InsertOrSkip`: ignora uma entidade existente ou insere uma nova entidade, caso ela não exista na tabela.
- `InsertOrMerge`: mescla uma entidade existente ou insere uma nova entidade, caso ela não exista na tabela.
- `InsertOrReplace`: substitui uma entidade existente ou insere uma nova entidade, caso ela não exista na tabela.

Observe que não é possível especificar a opção `/PKRS` no cenário de importação. Ao contrário do cenário de exportação, no qual é necessário especificar a opção `/PKRS` para iniciar operações simultâneas, por padrão, o AzCopy iniciará as operações simultâneas quando você importar as entidade. A quantidade padrão de operações simultâneas iniciadas é igual à quantidade de processadores de núcleo. No entanto, você pode especificar uma quantidade diferente com a opção `/NC`. Para obter mais detalhes, digite `AzCopy /?:NC` na linha de comando.


## Problemas Conhecidos e Práticas Recomendadas

#### Execute uma instância de AzCopy em um computador.
O AzCopy foi projetado para maximizar a utilização de recursos de seu computador para acelerar a transferência de dados; recomendamos que você execute apenas uma instância do AzCopy em um único computador e especifique a opção `/NC` se precisar de mais operações simultâneas. Para obter mais detalhes, digite `AzCopy /?:NC` na linha de comando.

#### Habilite algoritmos MD5 compatíveis com FIPS para o AzCopy quando você "Usar algoritmos compatíveis com FIPS para criptografia, hash e assinatura".
Por padrão, o AzCopy usa a implementação MD5 do .NET para calcular o MD5 ao copiar objetos, mas há alguns requisitos de segurança que precisam do AzCopy para permitir a configuração de MD5 compatível com FIPS.

Você pode criar um arquivo app.config `AzCopy.exe.config` com a propriedade `AzureStorageUseV1MD5` e reservá-lo com o AzCopy.exe.

	<?xml version="1.0" encoding="utf-8" ?>
	<configuration>
	  <appSettings>
	    <add key="AzureStorageUseV1MD5" value="false"/>
	  </appSettings>
	</configuration>

Para a propriedade "AzureStorageUseV1MD5" • True - valor padrão, o AzCopy usará a implementação MD5 de .NET. • False – o AzCopy usará o algoritmo MD5 compatível com FIPS.

Observe que os algoritmos compatíveis com FIPS estão desabilitados por padrão em seu computador com Windows. Digite secpol.msc na janela Executar e marque essa opção em Configuração de Segurança -> Segurança -> Políticas Locais > Opções de Segurança > Criptografia do Sistema: usar algoritmos compatíveis com FIPS para criptografia, hash e assinatura.

## Versões do AzCopy

> [AZURE.NOTE]Recomendamos a instalação da versão mais recente do AzCopy para obter melhor desempenho e novos recursos.

| Versão | O que há de novo | Versão da Biblioteca do Cliente .NET referenciada | Versão da API REST de armazenamento de destino |
|---------|-----------------------------------------------------------------------------------------------------------------|--------|----------|
| [**V5.0.0**](http://xdmrelease.blob.core.windows.net/azcopy-5-0-0/MicrosoftAzureStorageTools.msi) | **Versão atual. Inclui todas as funcionalidades da V4.2.0. Todos os recursos de armazenamento de Arquivo e Tabela agora são GA.** | **V6.0.0** | **2015-04-05**
| [V4.2.0](http://xdmrelease.blob.core.windows.net/azcopy-4-2-0-preview/MicrosoftAzureStorageTools.msi) | Inclui todas as funcionalidades da V3.2.0. Também oferece suporte a SAS de compartilhamento do arquivo de armazenamento, cópia assíncrona do armazenamento de arquivos, exportação de entidades de tabela para CSV e especificação do nome do manifesto ao exportar entidades de Tabela | V5.0.0 | 2015-02-21
| [V3.2.0](http://xdmrelease.blob.core.windows.net/azcopy-3-2-0/MicrosoftAzureStorageTools.msi) | Oferece suporte ao Blob anexo e configuração de MD5 compatível com FIPS | V5.0.0 | 2015-02-21
| [V4.1.0](http://xdmrelease.blob.core.windows.net/azcopy-4-1-0-preview/MicrosoftAzureStorageTools.msi) | Inclui todas as funcionalidades da V3.1.0. Suporta cópia de blobs e arquivos de forma síncrona e especifica o tipo de conteúdo para blobs e arquivos de destino | V4.3.0 | 2014-02-14
| [V3.1.0](http://xdmrelease.blob.core.windows.net/azcopy-3-1-0/MicrosoftAzureStorageTools.msi) | Suporta cópia de blobs de forma síncrona e especifica o tipo de conteúdo para blobs de destino.| V4.3.0 | 2014-02-14
| [V4.0.0](http://xdmrelease.blob.core.windows.net/azcopy-4-0-0-preview/MicrosoftAzureStorageTools.msi) | Inclui todas as funcionalidades do V3.0.0. Também permite copiar arquivos de ou para o armazenamento de arquivos do Azure e copiar entidades de ou para o armazenamento de tabela do Azure.| V4.2.1 | 2014-02-14
| [V3.0.0](http://xdmrelease.blob.core.windows.net/azcopy-3-0-0/MicrosoftAzureStorageTools.msi) | Modifica a sintaxe da linha de comando do AzCopy para solicitar os nomes do parâmetro e redefine a ajuda da linha de comando. Essa versão dá suporte apenas copiar para e do armazenamento de blob do Azure.| V4.2.1 | 2014-02-14
| V2.5.1 | Otimiza o desempenho com o uso das opções /xo e /xn. Corrige bugs relacionados a caracteres especiais nos nomes dos arquivos de origem e os danos nos arquivos de diário quando o usuário insere sintaxe errada na linha de comando.| V4.1.0 | 2014-02-14
| V2.5.0 | Otimiza o desempenho para cenários de cópia em larga escala e apresenta diversos aperfeiçoamentos de usabilidade importantes.| V4.1.0 | 2014-02-14
| V2.4.1 | Permite a especificação da pasta de destino no assistente de instalação.| V4.0.0 | 2014-02-14
| V2.4.0 | Permite o upload e o download de arquivos para armazenamento de arquivo do Azure.| V4.0.0 | 2014-02-14
| V2.3.0 | Permite contas de armazenamento de redundância geográfica com acesso de leitura.| V3.0.3 | 2013-08-15
| V2.2.2 | Atualizado para usar a versão da biblioteca do cliente de armazenamento do Azure 3.0.3.| V3.0.3 | 2013-08-15
| V2.2.1 | Problema de desempenho corrigido durante a cópia de arquivos muito grandes dentro da mesma conta de armazenamento.| V2.1.0 |
| V2.2 | Permite a configuração do delimitador de diretório virtual para nomes de blob. Permite a especificação do caminho de arquivo de diário.| V2.1.0 |
| V2.1 | Oferece mais de 20 opções para permitir operações de upload, download e cópia de blob de maneira eficiente.| V2.0.5 |


## Próximas etapas

Para obter mais informações sobre o Armazenamento do Azure e o AzCopy, consulte os recursos a seguir.

### Documentação do Armazenamento do Azure:

- [Introdução ao Armazenamento do Azure](storage-introduction.md)
- [Como usar o Armazenamento de blob do .NET](storage-dotnet-how-to-use-blobs.md)
- [Como usar o Armazenamento de Arquivos no .NET](storage-dotnet-how-to-use-files.md)
- [Como usar o Armazenamento de Tabela do .NET](storage-dotnet-how-to-use-tables.md)
- [Como criar, gerenciar ou excluir uma conta de armazenamento](storage-create-storage-account.md)
- [Usar o Serviço de Importação/Exportação para transferir dados para o Armazenamento de Blobs](storage-import-export-service.md)

### Postagens de blog de armazenamento do Azure:
- [DML: apresentando a versão de visualização da biblioteca de movimentação de dados do armazenamento do azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Introducing synchronous copy and customized content type (AzCopy: introdução a cópia síncrona e tipo de conteúdo personalizado)](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support (AzCopy: anúncio de disponibilidade geral do AzCopy 3.0 mais versão de teste do AzCopy 4.0 com suporte para Tabela e Arquivo)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Optimized for Large-Scale Copy Scenarios (AzCopy: otimizado para cenários de cópia em larga escala)](http://go.microsoft.com/fwlink/?LinkId=507682)
- [Apresentando o serviço de arquivo do Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
- [AzCopy: Support for read-access geo-redundant storage (AzCopy: suporte para o armazenamento com redundância geográfica com acesso de leitura)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Transfer data with re-startable mode and SAS token (AzCopy: transferir dados com o modo reiniciável e token de SAS)](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Using cross-account Copy Blob (AzCopy: usando blob de cópia em várias contas)](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Uploading/downloading files for Azure Blobs (AzCopy: Upload/download de arquivos para Blobs do Azure)](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

<!---HONumber=AcomDC_1217_2015-->