﻿<properties 
	pageTitle="Conceitos de serviços de mídia do Azure" 
	description="Este tópico oferece uma visão geral dos conceitos de serviços de mídia do Azure" 
	services="media-services" 
	documentationCenter="" 
	authors="juliako" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/02/2015"
	ms.author="juliako"/>

#Conceitos de serviços de mídia do Azure 

Este tópico fornece uma visão geral dos conceitos mais importantes dos Serviços de Mídia.

##<a id="assets"></a>Ativos \ Armazenamento

###Ativos

Um [Ativo](https://msdn.microsoft.com/library/azure/hh974277.aspx) contém arquivos digitais (incluindo vídeo, áudio, imagens, coleções de miniaturas, faixas de texto e arquivos de legenda codificada) e os metadados sobre esses arquivos. Depois que os arquivos digitais são carregados em um ativo, eles podem ser usados nos fluxos de trabalho de codificação e streaming dos serviços de mídia.

Um ativo é mapeado para um contêiner de blob na conta de armazenamento do Azure e os arquivos no ativo são armazenados como blobs nesse contêiner.

Ao decidir qual conteúdo de mídia para carregar e armazenar em um ativo, as seguintes considerações se aplicam:

- Um ativo deve conter apenas uma instância única e exclusiva do conteúdo de mídia. Por exemplo, uma única edição de um episódio de TV, filme ou anúncio.
- Um ativo não deve conter representações ou edições múltiplas de um arquivo audiovisual. Um exemplo de uso inapropriado de um Ativo é tentar armazenar mais de um episódio de TV, anúncio ou vários ângulos de câmera de uma única produção dentro do ativo. Armazenar representações ou edições diversas de um arquivo audiovisual em um ativo pode resultar em dificuldades ao enviar trabalhos de codificação, transmitir e assegurar a entrega do ativo posteriormente no fluxo de trabalho.  

###Arquivo de ativo 
Um [AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) representa um arquivo de áudio ou vídeo real que é armazenado em um contêiner de blob. Um arquivo de ativo está sempre associado a um ativo, e um ativo pode conter um ou vários arquivos. A tarefa do Codificador dos serviços de mídia falha se um objeto de arquivo de ativo não estiver associado um arquivo digital em um contêiner de blob.

A instância de **AssetFile** e o arquivo de mídia real são dois objetos diferentes. A instância de AssetFile contém metadados sobre o arquivo de mídia, enquanto o arquivo de mídia contém o conteúdo de mídia real.

Você não deve tentar alterar o conteúdo de contêineres de blob que foram gerados pelos serviços de mídia sem o uso de APIs de serviços de mídia.

###Opções de criptografia de ativos

Dependendo do tipo de conteúdo que você deseja carregar, armazenar e fornecer, os Serviços de Mídia fornecem várias opções de criptografia que você pode escolher.

**None** Nenhuma criptografia é usada. Esse é o valor padrão. Observe que ao usar essa opção, seu conteúdo não é protegido quando está em trânsito ou em repouso no armazenamento.

Se você pretende entregar um MP4 usando download progressivo, utilize essa opção para carregar o conteúdo.

**StorageEncrypted** - Use esta opção para criptografar o conteúdo limpo localmente usando a criptografia AES de 256 bits e, em seguida, carregá-lo no armazenamento do Azure onde é armazenado criptografado em rest. Ativos protegidos pela criptografia de armazenamento são descriptografados automaticamente e posicionados em um sistema de arquivos criptografado antes da codificação, então opcionalmente criptografados novamente antes do carregamento como um novo ativo de saída. O caso de uso primário para criptografia de armazenamento é quando você deseja proteger seus arquivos de mídia de entrada de alta qualidade com criptografia forte em repouso no disco. 

Para entregar um ativo de armazenamento criptografado, você deve configurar política de entrega do ativo para que os Serviços de Mídia saibam como você deseja fornecer o conteúdo. Antes que seu ativo possa ser transmitido, o servidor de streaming remove a criptografia de armazenamento e transmite o conteúdo usando a política de entrega especificada (por exemplo, AES, PlayReady ou sem criptografia). 

**CommonEncryption** - use esta opção se você deseja criptografar (ou carregar já criptografado) conteúdo com criptografia comum ou DRM PlayReady (por exemplo, Smooth Streaming protegido com DRM PlayReady).

**EnvelopeEncrypted** - use esta opção se você deseja proteger (ou carregar um já protegido) HTTP Live Streaming (HLS) criptografado com criptografia AES. Observe que se você estiver carregando um HLS já criptografado com AES, ele deve ter sido criptografado pelo Transform Manager.

###Política de acesso 

Uma [AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) define permissões (como leitura, gravação e lista) e a duração do acesso a um ativo. Normalmente, você passaria um objeto AccessPolicy para um localizador que deve ser usado para acessar os arquivos contidos em um ativo.


###Contêiner de blob

Um contêiner de blob fornece um agrupamento de um conjunto de blobs. Contêineres de blob são usados nos serviços de mídia como ponto limite para controle de acesso e localizadores de assinatura de acesso compartilhado (SAS) em ativos. Uma conta de armazenamento do Azure pode conter um número ilimitado de contêineres de blob. Um contêiner pode armazenar um número ilimitado de blobs.

>[AZURE.NOTE]Você não deve tentar alterar o conteúdo de contêineres de blob que foram gerados pelos serviços de mídia sem o uso de APIs de serviços de mídia.

###<a id="locators"></a>Localizadores

[Localizador](https://msdn.microsoft.com/library/azure/hh974308.aspx)es fornecem um ponto de entrada para acessar os arquivos contidos em um ativo. Uma política de acesso é usada para definir as permissões e a duração pela qual um cliente tem acesso a um determinado ativo. Os localizadores podem ter de uma a muitas relações com uma política de acesso, de modo que diferentes localizadores podem fornecer diferentes horas de início e tipos de conexão para diferentes clientes, ao mesmo tempo usando a mesma permissão e as mesmas configurações de duração; no entanto, devido a uma restrição de política de acesso compartilhado definida pelos serviços de armazenamento do Azure, você não pode ter mais do que cinco localizadores exclusivos associados a um determinado ativo ao mesmo tempo. 

Os Serviços de Mídia oferecem suporte a dois tipos de localizadores: Localizadores OnDemandOrigin, usado para transmitir mídia (por exemplo, MPEG DASH, HLS ou Smooth Streaming) ou baixar, de maneira progressiva, mídia e localizadores de URL SAS, usados para carregar ou baixar arquivos de mídia para\do armazenamento do Azure. 

Observe que a permissão de lista (Accesspermissions) não deve ser usada ao criar um localizador OrDemandOrigin. 

###Conta de armazenamento

Todo o acesso ao Armazenamento do Azure é feito por meio de uma conta de armazenamento. Uma conta do Serviço de Mídia pode ser associada a uma ou mais contas de armazenamento. Uma conta pode conter um número ilimitado de contêineres, desde que seu tamanho total esteja abaixo de 500 TB por conta de armazenamento.  Os Serviços de Mídia fornecem ferramentas de nível de SDK que permitem a você gerenciar várias contas de armazenamento e balancear a carga da distribuição de seus ativos durante o carregamento nessas contas com base em métricas ou na distribuição aleatória. Para obter mais informações, consulte Trabalhando com [Armazenamento do Azure](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

##Trabalhos \ tarefas

Um [trabalho](https://msdn.microsoft.com/library/azure/hh974289.aspx) normalmente é usado para processar (por exemplo, indexar ou codificar) uma apresentação de áudio/vídeo. Se você estiver processando vários vídeos, crie um trabalho para cada vídeo a ser codificado.

Um trabalho contém metadados sobre o processamento a ser realizado. Cada Trabalho contém uma ou mais [Tarefas](https://msdn.microsoft.com/library/azure/hh974286.aspx) que especificam uma tarefa de processamento atômica, seus ativos de entrada e ativos de saída, um processador de mídia e suas configurações associadas. Tarefas em um trabalho podem ser encadeadas, em que o ativo de saída de uma tarefa é determinado como o ativo de entrada para a próxima tarefa. Dessa forma, um trabalho pode conter todo o processamento necessário para uma apresentação de mídia.

##<a id="encoding"></a>Codificação \ empacotamento

###Codificação

Para fornecer vídeo digital pela internet, você deve compactar a mídia. Os arquivos de vídeo digital são muito grandes e podem ser muito grandes para entregar pela internet ou para dispositivos de seus clientes para exibir corretamente. As pessoas assistem a vídeos em uma variedade de dispositivos desde TVs com decodificadores de sinais, PCs desktop até tablets e smartphones. Cada um desses dispositivos tem diferentes requisitos de largura de banda e compactação. A codificação é o processo de compactação de áudio e vídeo usando compactadores/descompactadores ou codecs. 

Transcodificação é o processo de pegar um vídeo que foi codificado e codificá-lo novamente em um formato de codificação diferente. Como a maioria das câmeras codifica vídeo até certo ponto, a maioria dos trabalhos de codificação feitos nos Serviços de Mídia do Azure é tecnicamente transcodificação.

###Codecs e formatos de arquivo 

Codecs têm dois componentes: um para compactar arquivos de mídia digital para transmissão e outro para descompactar arquivos de mídia digital para reprodução. Há codecs de áudio para compactar e descompactar áudio e codecs de vídeo para compactar e descompactar vídeo. Os codecs podem usar compactação com ou sem perdas. Codecs sem perdas preservam todas as informações quando ocorre a compressão. Quando o arquivo é descompactado, o resultado é um arquivo idêntico ao da mídia de entrada, o que torna os codecs sem perdas adequados para arquivamento e armazenamento. Codecs com perdas perdem algumas informações durante a codificação e produzem arquivos menores (que o original) à custa da qualidade do vídeo e servem para streaming pela Internet. Os dois principais codecs usados pelo Codificador de Mídia do Azure para codificação são H.264 e VC-1. Outros codecs podem estar disponíveis em nosso ecossistema de parceiros de codificadores. 

###Codificadores de Serviços de Mídia

Para obter informações sobre codificadores para os quais há suporte, consulte [Codificadores](../media-services-encode-asset).


##Streaming ao vivo

###Transcodificador ao vivo

Um codificador ao vivo (ou transcodificador) converte o áudio e/ou vídeo que é transmitido da câmera em um formato de Smooth Streaming ou RTMP com múltiplas taxas de bits. O transcodificador então envia a taxa de bits adaptável RTMP ou Smooth Streaming em um canal dos Serviços de Mídia. Os Serviços de Mídia então transmitem o evento ao vivo.

###Canal

Nos Serviços de Mídia, [Canal](https://msdn.microsoft.com/library/azure/dn783458.aspx)s são responsáveis pelo processamento de conteúdo de transmissão ao vivo. Um Canal fornece um ponto de extremidade de entrada (URL de ingestão) que você então fornece a um transcodificador ao vivo. O canal recebe transmissões ao vivo de entrada do transcodificador ao vivo e o torna disponível para streaming por meio de um ou mais StreamingEndpoints. Canais também fornecem um ponto de extremidade de visualização prévia (URL de visualização prévia) que você usa para visualizar e validar seu fluxo antes de processamento e da entrega.

Você pode obter a URL de ingestão e a URL de visualização prévia quando você cria o canal. Para obter essas URLs, o canal não precisa estar no estado iniciado. Quando estiver pronto para começar a enviar dados de um transcodificador ao vivo para o canal, o canal deve ser iniciado. Depois que o transcodificador ao vivo inicia a ingestão de dados, você pode visualizar o fluxo.

###Programa 

Um [Programa](https://msdn.microsoft.com/library/azure/dn783463.aspx) permite que você controle a publicação e o armazenamento de segmentos em um fluxo ao vivo. Os canais gerenciam os programas. A relação entre canal e programa é muito semelhante à mídia tradicional, onde um canal tem um fluxo constante de conteúdo e um programa tem como escopo algum evento programado naquele canal.
Você pode especificar o número de horas pelo qual deseja manter o conteúdo gravado para o programa, definindo a propriedade **ArchiveWindowLength**. Esse valor pode ser definido entre o mínimo de 5 minutos e o máximo de 25 horas. 

ArchiveWindowLength também determina que a quantidade máxima de clientes de tempo pode buscar de volta no tempo a partir da posição atual em tempo real. Programas podem ser executados sobre o período de tempo especificado, mas o conteúdo que sair do comprimento da janela será continuamente descartado. O valor dessa propriedade também determina por quanto tempo os manifestos do cliente podem crescer.

Cada programa está associado um Ativo. Para publicar o programa, você deve criar um localizador para o ativo associado. Ter esse localizador permitirá que você compile um URL de transmissão que você poderá fornecer aos seus clientes.
Um canal dá suporte a até três programas em execução simultânea, de modo que você pode criar diversos arquivos por meio do mesmo fluxo de entrada. Isso permite que você publique e arquive diferentes partes de um evento, conforme necessário. Por exemplo, o requisito de negócios é arquivar 6 horas de um programa, mas para transmitir, é de apenas os últimos 10 minutos. Para fazer isso, você precisa criar dois programas em execução simultânea. Um programa é definido para arquivar 6 horas do evento, mas o programa não é publicado. Outro programa é definido para arquivar para 10 minutos, e esse programa é publicado.

##Proteção de conteúdo

###Criptografia dinâmica

Os Serviços de Mídia do Microsoft Azure permitem distribuir o conteúdo criptografado dinamicamente com criptografia AES (padrão avançado) (usando chaves de criptografia de 128 bits) e PlayReady DRM. 

No momento, você pode criptografar o seguintes formatos de streaming: HLS, MPEG DASH e Smooth Streaming. Você não pode criptografar o formato de streaming HDSou fazer o download progressivo.

Se você deseja que os Serviços de Mídia criptografem um ativo, você precisa associar a seu ativo de uma chave de criptografia (CommonEncryption ou EnvelopeEncryption) e também configurar políticas de autorização para a chave.

Você também precisa configurar a política de entrega do ativo. Se você deseja transmitir um ativo de armazenamento criptografado, certifique-se de especificar como deseja entregá-lo configurando a política de entrega do ativo.  

Quando um fluxo é solicitado por um player, os serviços de mídia usam a chave especificada para criptografar dinamicamente o conteúdo usando a criptografia AES ou PlayReady. Para descriptografar o fluxo, o player solicitará a chave do serviço de distribuição de chaves. Para decidir se o usuário está autorizado para obter a chave ou não, o serviço avalia as políticas de autorização que você especificou para a chave.

###Serviços de entrega de licenças PlayReady DRM e chaves liberadas AES

Os Serviços de Mídia oferecem um serviço de distribuição de licenças PlayReady e chaves liberadas AES aos clientes autorizados. Você pode usar o Portal de Gerenciamento, a API REST ou o SDK dos Serviços de Mídia para .NET para configurar políticas de autenticação e autorização para suas licenças e chaves.

Observe que se você estiver usando o Portal, poderá configurar uma política AES (que será aplicada a todo o conteúdo criptografado do AES) e uma política do PlayReady (que será aplicada a todo o conteúdo PlayReady criptografado). Use o SDK dos Serviços de Mídia para .NET se quiser mais controle sobre as configurações.

###Modelo de licença do PlayReady

Os Serviços de Mídia fornecem um serviço de distribuição de licenças do PlayReady. Quando o player de usuário final (por exemplo, Silverlight) tenta reproduzir o conteúdo protegido por PlayReady, uma solicitação é enviada para o serviço de entrega de licenças para obter uma licença. Se o serviço de licença aprova a solicitação, ele emite a licença que é enviada ao cliente e pode ser usada para descriptografar e reproduzir o conteúdo especificado.

As licenças contêm os direitos e restrições que você deseja que o tempo de execução do PlayReady DRM imponha quando um usuário está tentando reproduzir conteúdo protegido. Os Serviços de Mídia fornecem APIs que permitem que você configure suas licenças do PlayReady. Para obter mais informações, consulte [Visão geral do modelo de licença do PlayReady dos Serviços de Mídia](https://msdn.microsoft.com/library/azure/dn783459.aspx).

###Restrição de token

A política de autorização de chave de conteúdo pode ter uma ou mais restrições de autorização: abrir, restrição de token ou restrição de IP. A política restrita do token deve ser acompanhada por um token emitido por um Secure Token Service (STS). Os serviços de mídia dão suporte a tokens no formato Simple Web Tokens (SWT) e no formato JSON Web Token (JWT). Os serviços de mídia não fornecem Secure Token Services. Você pode criar um STS personalizado ou usar o Microsoft Azure ACS para emitir tokens. O STS deve ser configurado para criar um token assinado com as a chave especificada e declarações de emissão que você especificou na configuração de restrição do token. O serviço de distribuição de chaves dos serviços de mídia retornará a chave (ou licença) solicitada para o cliente se o token for válido e as declarações no token corresponderem àquelas configurados para a chave (ou licença).
Ao configurar a política restrita do token, você deve especificar os parâmetros da chave de verificação primária, emissor e audiência. A chave de verificação primária contém a chave que o token foi assinado, o emissor é o serviço de token seguro que emite o token. A audiência (às vezes chamada de escopo) descreve a intenção do token ou o recurso que o token autoriza o acesso. O serviço de distribuição de chaves dos serviços de mídia valida que esses valores no token correspondem aos valores no modelo.

##Fornecimento

###<a id="dynamic_packaging"></a>Empacotamento dinâmico

Quando trabalhar com os Serviços de Mídia, é recomendado codificar seus arquivos de mezanino em uma conjunto de MP4 de taxa de bits adaptável e, em seguida, converter o conjunto para o formato desejado usando o [empacotamento dinâmico](https://msdn.microsoft.com/library/azure/jj889436.aspx). 


###Ponto de extremidade de streaming

Um StreamingEndpoint representa um serviço de streaming que pode fornecer conteúdo diretamente a um aplicativo de player de cliente ou para uma CDN (Rede de Distribuição de Conteúdo) para a distribuição (os Serviços de Mídia do Azure agora fornecem a integração com o Azure CDN). O fluxo de saída do serviço StreamingEndpoint pode ser um fluxo ao vivo ou um ativo de vídeo por demanda na sua conta dos Serviços de Mídia. Além disso, você pode controlar a capacidade do serviço StreamingEndpoint para lidar com crescentes necessidades de largura de banda ajustando as unidades de dimensionamento (também conhecido como unidades de streaming). É recomendável alocar uma ou mais unidades de escala para aplicativos no ambiente de produção. As unidades de dimsneionamento fornecem capacidade de egresso dedicada que pode ser comprada em incrementos de 200 Mbps e funcionalidade adicional que, atualmente, inclui recursos de empacotamento dinâmico. 

Se você planeja usar criptografia dinâmica de e/ou de empacotamento dinâmico, você deve ter pelo menos uma unidade de transmissão do ponto de extremidade do qual você planeja transmitir. Para obter mais informações, consulte [Dimensionando as unidades de streaming](../media-services-manage-origins#scale_streaming_endpoints).

###Política de fornecimento de ativos

Uma das etapas do fluxo de trabalho de fornecimento de conteúdo de serviços de mídia está configurando [políticas de entrega de ativos ](https://msdn.microsoft.com/library/azure/dn799055.aspx)que você deseja que sejam transmitidas. A política de entrega de ativos informa aos serviços de mídia como você deseja que o ativo seja entregue: em que protocolo de fluxo seu ativo deve ser dinamicamente empacotado (por exemplo, MPEG DASH, HLS, Smooth Streaming ou todos), se você deseja criptografar dinamicamente seu ativo ou não e como (criptografia de envelope ou comum).

Se você tiver um ativo de armazenamento criptografado, antes que possa ser transmitido seu ativo, o servidor de streaming remove a criptografia de armazenamento e transmite o conteúdo usando a política de entrega especificada. Por exemplo, para entregar o ativo criptografado com chave de criptografia AES (criptografia avançada padrão), defina o tipo de política para DynamicEnvelopeEncryption. Para remover a criptografia de armazenamento e transmitir o ativo claro, defina o tipo de política NoDynamicEncryption. 

###Download progressivo 

O download progressivo permite iniciar a reprodução da mídia antes do arquivo inteiro ter sido baixado. Você só pode baixar apenas progressivamente um arquivo MP4. 

Observe que é necessário descriptografar ativos criptografados se você deseja que estejam disponíveis para download progressivo. 

Para fornecer aos usuários URLs de download progressivo, você deve primeiro criar um localizador OnDemandOrigin. Criar o localizador oferece a você o caminho base para o ativo. Em seguida, é necessário acrescentar o nome do arquivo MP4. Por exemplo:

	http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###URLs de streaming

Transmitindo seu conteúdo para clientes. Para fornecer aos usuários URLs de streaming, você deve primeiro criar um localizador OnDemandOrigin. Criar o localizador oferece a você o caminho base para o ativo que contém o conteúdo que você deseja transmitir. No entanto, para poder transmitir este conteúdo você precisa modificar esse caminho ainda mais. Para construir uma URL completa para o arquivo de manifesto de streaming, você deve concatenar o valor do caminho do localizador e o nome de arquivo de manifesto (ISM). Em seguida, anexe um formato apropriado (se necessário) ao caminho do localizador. 

Você também pode transmitir seu conteúdo por uma conexão SSL. Para fazer isso, certifique-se de que suas URLs de streaming começam com HTTPS.

Observe que você só pode transmitir por SSL se o ponto de extremidade de streaming por meio do qual você pode distribuir o conteúdo tiver sido criado depois de 10 de setembro de 2014. Se suas URLs de streaming baseiam-se nos pontos de extremidade de streaming após 10 de setembro, a URL contém "streaming.mediaservices.windows.net" (o novo formato). URLs de streaming que contêm "origin.mediaservices.windows.net" (o formato antigo) não dão suporte a SSL. Se sua URL está no formato antigo e você deseja ser capaz de transmitir por SSL, crie um novo ponto de extremidade de streaming. Use URLs criadas com base no novo ponto de extremidade de streaming para transmitir seu conteúdo por SSL. 

A lista a seguir descreve os diferentes formatos de streaming e fornece exemplos:

- Smooth Streaming

	{nome do ponto de extremidade de streaming - nome de conta do dos serviços de mídia}.streaming.mediaservices.windows.net/{ID do localizador}/{nome do arqui}.ism/Manifest
		
		http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest


- MPEG DASH

	{nome do ponto de extremidade de streaming - nome de conta dos serviços de mídia}.streaming.mediaservices.windows.net/{ID do localizador}/{nome do arquivo}.ism/Manifest(format=mpd-time-csf)
 
		http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)



- Apple HTTP Live Streaming (HLS) V4

	{nome do ponto de extremidade de streaming - nome de conta dos serviços de mídia}.streaming.mediaservices.windows.net/{ID do localizador}/{nome do arquivo}.ism/Manifest(format=m3u8-aapl)

		http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)



- Apple HTTP Live Streaming (HLS) V3

	{nome do ponto de extremidade de streaming - nome de conta dos serviços de mídia}.streaming.mediaservices.windows.net/{ID do localizador}/{nome do arquivo}.ism/Manifest(format=m3u8-aapl-v3)

		http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

- HDS (apenas para licenciados Adobe PrimeTime/Access)

	{nome do ponto de extremidade de streaming - nome de conta dos serviços de mídia}.streaming.mediaservices.windows.net/{ID do localizador}/{nome do arquivo}.ism/Manifest(format=f4m-f4f)

		http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f) 


<!--HONumber=47-->