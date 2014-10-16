<properties title="Interact with DocumentDB resources" pageTitle="Interact with DocumentDB resources | Azure" description="DocumentDB manages resources--uniquely identified by logical URIs--that developers can interact with using HTTP verbs, request/response headers, and status codes." metaKeywords="" services="documentdb" solutions="data-management" documentationCenter="" authors="bradsev" manager="jhubbard" editor="cgronlun" videoId="" scriptId="" />

<tags ms.service="documentdb" ms.workload="data-services" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="08/20/2014" ms.author="bradsev"></tags>

# Modelo e conceitos de recursos do Banco de Dados de Documentos

O Banco de Dados de Documentos oferece um modelo de programação RESTful simples e aberto em HTTP. As entidades que o Banco de Dados de Documentos gerencia são referidas como **recursos** que são identificados exclusivamente pela sua URI lógica. Os desenvolvedores podem interagir com os recursos usando verbos de HTTP padrão, cabeçalhos de solicitação/resposta e códigos de status. Conforme ilustrado pelo diagrama a seguir, o **modelo de recursos** do Banco de Dados de Documentos é formado por conjuntos de recursos em uma conta do banco de dados, cada um podendo ser acessado por meio de um URI lógico e estável.

> [AZURE.NOTE] Adicionalmente, ele também oferece um protocolo TCP altamente eficiente que também possui o modelo de comunicação RESTful, disponível por meio do SDK do cliente .NET.

![][]
**Modelo de recurso hierárquico em uma conta de banco de dados**

Como cliente do Banco de Dados de Documentos, comece provisionando uma **conta de banco de dados** do Banco de Dados de Documentos usando sua assinatura do Azure. Uma conta do banco de dados pode ser formada por um conjunto de **bancos de dados**, cada um contendo diversas **coleções**, cada uma delas, por sua vez, contendo **procedimentos armazenados, gatilhos, UDFs, documentos** e **anexos** relacionados. Um banco de dados também possui **usuários** associados, cada um com um conjunto de **permissões** para acessar coleções, procedimentos armazenados, gatilhos, UDFs, documentos ou anexos. Enquanto bancos de dados, usuários, permissões e coleções são recursos definidos pelo sistema com esquemas bastante conhecidos, os documentos e anexos possuem conteúdos JSON arbitrários, definidos pelo usuário.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Recurso</strong></p></td>
<td align="left"><p><strong>Descrição</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Banco de dados</strong></p>
<p><strong>Conta</strong></p></td>
<td align="left"><p>Uma conta do banco de dados é associada a uma ou mais unidades de capacidade representando o armazenamento e produtividade do documento provisionado, um conjunto de bancos de dados e armazenamento de blob. Você pode criar uma ou mais contas do banco de dados usando sua assinatura do Azure. Cada conta do banco de dados possui um Nome DNS exclusivo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>Banco de dados</strong></p></td>
<td align="left"><p>Um banco de dados é um contêiner lógico de armazenamento de documentos particionado em coleções. Ele também é um contêiner para usuários.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Usuário</strong></p></td>
<td align="left"><p>O namespace lógico para obter o escopo das permissões.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>Permissão</strong></p></td>
<td align="left"><p>Um token de autorização associado a um usuário para acesso autorizado a um recurso específico.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Coleção</strong></p></td>
<td align="left"><p>Uma coleção é um contêiner de documentos JSON e uma lógica de aplicativo JavaScript associada. Consultas e transações estão dentro do escopo das coleções.</p></td>
</tr>
</tbody>
</table>

## Recursos definidos pelo sistema x usuário

Recursos como contas do banco de dados, bancos de dados, coleções, usuários, permissões, procedimentos armazenados, gatilhos e UDFs, todos possuem um esquema fixo e são chamados de recursos do sistema. Em contraste, recursos como documentos e anexos não possuem restrições sobre o esquema e são exemplos de recursos definidos pelo usuário. No Banco de Dados de Documentos, ambos os recursos definidos pelo sistema e pelo usuário são representados e gerenciados como JSON em conformidade com o padrão.

# Contas do banco de dados

![][1]

Você pode criar uma ou mais contas do Banco de Dados de Documentos usando sua assinatura do Azure. Para sua conta do banco de dados, você pode adquirir unidades empilháveis de armazenamento de documento com suporte a SSD e produtividade em termos de CU (Unidades de Capacidade). Com base nas necessidades de escala e desempenho de seu aplicativo, você pode adicionar ou remover CUs de forma incremental. Cada CU vem com um conjunto de coleções elásticas, armazenamento de documentos provisionados com suporte a SSD e produtividade provisionada. O armazenamento provisionado e a capacidade de produtividade associados a uma CU são distribuídos entre as coleções do Banco de Dados de Documentos criadas entre vários bancos de dados em sua conta do banco de dados. A capacidade provisionada sob uma conta do banco de dados fica disponível a todos os bancos de dados e coleções existentes ou que forem criados dentro da conta. Não há um limite prático de escala para o tamanho de uma conta do banco de dados; qualquer número de unidades de capacidade pode ser adicionado ao longo do tempo, sujeito às restrições da oferta. Os recursos gerenciados dentro de uma CU são escalados por meio de um particionamento e replicados para uma alta disponibilidade.

Você pode criar e gerenciar conta(s) de banco de dados do Banco de Dados de Documentos por meio do portal do Azure em [][]<http://portal.azure.com/></a>. Criar e gerenciar uma conta do banco de dados requer acesso administrativo e pode ser feito somente com sua assinatura do Azure. Como parte do provisionamento e gerenciamento de uma conta do banco de dados, você pode configurar e ler as seguintes propriedades:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Nome da Propriedade</strong></p></td>
<td align="left"><p><strong>Descrição</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>Política de Consistência</p></td>
<td align="left"><p>Defina essa propriedade para configurar o nível de consistência padrão para todas as coleções em sua conta do banco de dados. Você pode substituir o nível de consistência com base em cada solicitação usando o cabeçalho de solicitação [x-ms-consistency-level]. No futuro, podemos oferecer suporte à substituição do nível de consistência com base em cada coleção.</p>
<p>Observe que essa propriedade somente se aplica aos <em>recursos definidos pelo usuário</em>. Todos os recursos definidos pelo sistema são configurados para oferecer suporte a leituras/consultas com uma forte consistência.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Chave mestre primária e chave mestre secundária</p></td>
<td align="left"><p>Essas são as chaves mestras primária e secundária, que oferecem acesso administrativo a todos os recursos na conta do banco de dados.</p></td>
</tr>
<tr class="even">
<td align="left"><p>MaxMediaStorageUsageInMB (LEITURA)</p></td>
<td align="left"><p>Quantidade máxima de armazenamento de mídia disponível para a conta do banco de dados</p></td>
</tr>
<tr class="odd">
<td align="left"><p>CurrentMediaStorageUsageInMB (LEITURA)</p></td>
<td align="left"><p>Uso atual do armazenamento de mídia para a conta do banco de dados</p></td>
</tr>
<tr class="even">
<td align="left"><p>CapacityUnitsProvisioned</p></td>
<td align="left"><p>O número total de unidades de capacidade provisionadas na conta do banco de dados</p></td>
</tr>
<tr class="odd">
<td align="left"><p>CapacityUnitsConsumed (LEITURA)</p></td>
<td align="left"><p>O número total de unidades de capacidade consumidas atualmente na conta do banco de dados</p></td>
</tr>
<tr class="even">
<td align="left"><p>ProvisionedDocumentStorageInMB (LEITURA)</p></td>
<td align="left"><p>A quantidade total de armazenamento de documentos (em MB) provisionada entre todas as unidades de capacidade na conta do banco de dados</p></td>
</tr>
<tr class="odd">
<td align="left"><p>ReservedDocumentStorageInMB (LEITURA)</p></td>
<td align="left"><p>A quantidade total de armazenamento de documentos (em MB) reservada entre todas as unidades de capacidade na conta do banco de dados. Diz-se que o armazenamento dos documentos está reservado quando uma coleção contém documentos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>ConsumedDocumentStorageInMB (LEITURA)</p></td>
<td align="left"><p>A quantidade total de armazenamento de documentos (em MB) consumida, de fato, entre todas as unidades de capacidade na conta do banco de dados. Esse é o tamanho dos seus documentos reais e do índice combinados entre várias coleções em uma conta do banco de dados.</p></td>
</tr>
</tbody>
</table>

Observe que, além de provisionar, configurar e gerenciar sua conta do banco de dados a partir do portal do Azure, também é possível criar e gerenciar programaticamente contas do Banco de Dados de Documentos por meio das [APIs REST do Banco de Dados de Documentos do Azure][], bem como SDKs clientes.

 

# Bancos de dados

Um banco de dados no Banco de Dados de Documentos é um contêiner lógico de uma ou mais coleções e usuários. Você pode criar qualquer número de bancos de dados em uma conta de banco de dados do Banco de Dados de Documentos sujeita a limites de oferta.
![][2]
**Um banco de dados é um contêiner lógico de usuários e coleções**

Um banco de dados pode conter praticamente um armazenamento de documentos ilimitado, particionado por coleções, que formam os domínios de transação para os documentos contidos neles. Um banco de dados do Banco de Dados de Documentos é elástico por padrão, indo de alguns GB a petabytes de armazenamento de documento com suporte de SSD e produtividade provisionada. Diferentemente de um banco de dados em um RDBMS tradicional, um banco de dados no Banco de Dados de Documentos não é vinculado a uma única máquina. Com o Banco de Dados de Documentos, conforme a escala do seu aplicativo precisa crescer, você pode criar mais coleções ou bancos de dados, ou ambos. Na verdade, vários aplicativos primários na Microsoft utilizam o Banco de Dados de Documentos na escala do cliente ao criar bancos de dados do Banco de Dados de Documentos extremamente grandes, cada um contendo milhares de coleções com terabytes de armazenamento de documentos. É possível expandir ou reduzir um banco de dados adicionando ou removendo coleções para atender os requisitos de escada do seu aplicativo. Você pode criar qualquer número de coleções dentro de um banco de dados sujeito à oferta e à quantidade de unidades de capacidade compradas. O armazenamento com suporte de SSD e taxa de transmissão provisionado a você ao comprar unidades de capacidade pode ser dividido entre as coleções nos bancos de dados em sua conta do banco de dados.

Um banco de dados do Banco de Dados de Documentos também é um contêiner de usuários. Um usuário, por sua vez, é um namespace lógico para um conjunto de permissões que oferece uma autorização/acesso refinado a coleções, documentos e anexos.

Assim como com outros recursos no modelo de recursos do Banco de Dados de Documentos, os bancos de dados podem ser criados, substituídos, excluídos, lidos ou enumerados facilmente usando as [APIs REST do Banco de Dados de Documentos do Azure][] ou qualquer SDK do cliente. O Banco de Dados de Documentos garante uma forte consistência para a leitura ou consulta de metadados de um recurso do banco de dados. Excluir um banco de dados automaticamente garante que você não possa acessar qualquer uma das coleções ou usuários contidos nele. Embora o Banco de Dados de Documentos recupere o armazenamento e a produtividade provisionados como parte do banco de dados excluído em segundo plano, o armazenamento provisionado e a produtividade para o banco de dados excluído estão disponíveis imediatamente para uso.

# Coleções

Uma coleção do Banco de Dados de Documentos é um contêiner para seus documentos JSON. Uma coleção também é uma unidade de escala, transações e consulta. Também é possível expandir um banco de dados do Banco de Dados de Documentos adicionando mais coleções. Se o seu aplicativo precisar ser mais escalado, é possível provisionar mais armazenamento de documento com suporte de SSD (e produtividade) e distribuí-lo entre as coleções de um ou mais bancos de dados em sua conta do banco de dados.

## Armazenamento de documento com suporte de SSD elástico

Uma coleção é intrinsicamente elástica; ela cresce e é reduzida automaticamente conforme você adiciona ou remove documentos. Embora o uso primário do Banco de Dados de Documentos o tenha testado com milhares de coleções dentro de um banco de dados, cada um deles indo de alguns gigabytes a terabytes de tamanho, a oferta Standard Preview do Banco de Dados de Documentos atualmente limita a elasticidade de uma determinada coleção a 10GB.

## Indexação automática

O Banco de Dados de Documentos é um verdadeiro sistema de banco de dados livre de esquema. Ele não assume nem requer qualquer esquema para os documentos JSON. Ao incluir documentos em uma coleção, o Banco de Dados de Documentos os indexa automaticamente e eles ficam disponíveis para consulta. A indexação automática de documentos sem exigir esquemas ou índices secundários é uma capacidade chave do Banco de Dados de Documentos e é ativada por técnicas de manutenção de índice com gravação otimizada, livres de bloqueios e estrutura de log. O Banco de Dados de Documentos oferece suporte a um volume permanente de gravações extremamente rápidas ao mesmo tempo em que oferece consultas consistentes. Ambos os armazenamentos de documentos e de índices são usados para calcular o armazenamento consumido por cada coleção. Você pode controlar os compromissos de armazenamento e desempenho associados à indexação configurando a política de indexação para uma coleção.

## Configurando a política de indexação de uma coleção

A política de indexação de cada coleção permite realizar compromissos de desempenho e armazenamento associados à indexação. As seguintes opções estão disponíveis como parte da configuração de indexação:

-   Escolha se a coleção indexa automaticamente todos os documentos ou não. Por padrão, todos os documentos são indexados automaticamente. Você pode escolher desativar a indexação automática e adicionar seletivamente somente documentos específicos para o índice. Da mesma forma, é possível escolher seletivamente excluir somente documentos específicos. Isso pode ser feito definindo a propriedade automática como verdadeira ou falsa na indexingPolicy de uma coleção e usando o cabeçalho de solicitação [x-ms-indexingdirective] ao inserir, substituir ou excluir um documento.
-   Escolha se deseja incluir ou excluir caminhos ou padrões específicos em seus documentos a partir do índice. Isso pode ser feito definindo includedPaths e excludedPaths na indexingPolicy de uma coleção, respectivamente. Também é possível configurar os compromissos de armazenamento e desempenho para as consultas de intervalo e hash para padrões de caminho específicos.
-   Escolha entre atualizações de índice síncronas (consistentes) e assíncronas (lentas). Por padrão, o índice é atualizado sincronamente em cada inserção, substituição ou exclusão de um documento para a coleção. Isso permite que as consultas obedeçam ao mesmo nível de consistência das leituras de documentos. Enquanto o Banco de Dados de Documentos é otimizado para gravação e suporta volumes constantes de gravações de documentos junto com a manutenção síncrona de índice e atendimento a consultas consistentes, você pode configurar determinadas coleções para atualizar seu índice, sem pressa. A indexação lenta aumenta ainda mais o desempenho de gravação, sendo ideal para cenários de ingestão em massa para coleções basicamente de leitura intensa.

Para a versão de teste do Banco de Dados de Documentos, a política de indexação pode ser configurada somente durante a criação de uma coleção. Uma vez que a coleção tenha sido criada, a política não pode ser atualizada.

## Consultando uma coleção

Os documentos dentro de uma coleção podem ter esquemas arbitrários e os documentos podem ser consultados dentro de uma coleção sem oferecer qualquer esquema ou índices secundários de início. É possível consultar a coleção usando a linguagem de consulta SQL do Banco de Dados de Documentos, que oferece operadores hierárquicos e relacionais ricos e extensibilidade por meio de UDFs baseados em JavaScript. A gramática JSON permite modelar documentos JSON como árvores com rótulos como os nós da árvore. Isso é explorado pelas técnicas de indexação automática do Banco de Dados de Documentos bem como pelo dialeto da consulta SQL do Banco de Dados de Documentos. A linguagem de consulta do Banco de Dados de Documentos é formada por três aspectos principais:

1.  Um pequeno conjunto de operações de consulta que é mapeado naturalmente para a estrutura de árvore, incluindo consultas hierárquicas e projeções
2.  Um subconjunto de operações relacionais, incluindo composição, filtragem, projeções, agregados e junções automáticas.
3.  UDFs puras baseadas em JavaScript combinadas a (1) e (2)

O modelo de consulta do Banco de Dados de Documentos tenta criar um equilíbrio entre funcionalidade, eficiência e simplicidade. O mecanismo de banco de dados do Banco de Dados de Documentos compila nativamente e executa as declarações de consulta SQL. É possível consultar uma coleção usando as [APIs REST do Banco de Dados de Documentos do Azure][] ou qualquer um dos SDKs clientes. O SDK do .NET é fornecido com o provedor LINQ. Em uma versão futura, mapeamentos sublinhados nativos serão fornecidos, que podem ser usados a partir dos SDKs de JavaScript do lado do cliente, bem como com os procedimentos e gatilhos armazenados no lado do servidor.

## Transações entre vários documentos

As transações do banco de dados oferecem um modelo de programação seguro e previsível para lidar com alterações simultâneas aos dados. No RDBMS, a maneira tradicional de gravar a lógica de negócios é gravar **procedimentos armazenados** e/ou **gatilhos** e enviá-los ao servidor do banco de dados para execução transacional. No RDBMS, o programador do aplicativo precisa lidar com duas linguagens de programação diferentes: (a) a linguagem de programação do aplicativo (não transacional, p. ex., JavaScript, Python, C\#, Java, etc.) e (b) T-SQL, a linguagem de programação transacional, que é executada nativamente pelo banco de dados. Devido ao seu profundo compromisso com JavaScript e JSON diretamente dentro do mecanismo do banco de dados, o Banco de Dados de Documentos oferece um modelo de programação intuitivo para executar a lógica de aplicativos baseados em JavaScript diretamente nas coleções em termos de procedimentos armazenados e gatilhos. Isso permite (a) uma implementação eficiente do controle de simultaneidade, recuperação, indexação automática dos gráficos de objeto JSON diretamente no mecanismo do banco de dados, bem como (b) expressar naturalmente o fluxo de controle, o escopo de variáveis, a designação e integração de primitivas de gerenciamento de exceções com transações do banco de dados diretamente de acordo com a linguagem de programação de JavaScript.

A lógica de JavaScript registrada no nível da coleção pode, então, emitir operações do banco de dados nos documentos da coleção determinada. O Banco de Dados de Documentos encapsula implicitamente os procedimentos de armazenamento baseados em JavaScript e os gatilhos dentro de transações ACID ambientes com isolamento de captura instantânea entre documentos dentro de uma coleção. Durante sua execução, se o JavaScript lançar uma exceção, então, toda a transação será abortada. O modelo de programação resultante é bastante simples, porém, poderoso. Os desenvolvedores de JavaScript obtêm um modelo de programação “durável”, ao mesmo tempo em que utilizam os construtores de linguagem e primitivas de biblioteca com os quais estão familiarizados.

A capacidade de executar JavaScript diretamente dentro do mecanismo do banco de dados no mesmo espaço de endereço que o conjunto de buffer permite uma execução de desempenho e transacional das operações do banco de dados em relação aos documentos de uma coleção. Além disso, o mecanismo de banco de dados do Banco de Dados de Documentos assume um forte compromisso com JSON e o JavaScript elimina qualquer incompatibilidade de impedância entre os sistemas de tipos do aplicativo e do banco de dados.

Após criar uma coleção, você pode registrar procedimentos armazenados, gatilhos e UDFs com uma coleção usando APIs REST do Banco de Dados de Documentos ou qualquer SDK cliente. Após o registro, você pode fazer referência a eles e executá-los. Considere que o seguinte procedimento armazenado gravado inteiramente em JavaScript pega dois argumentos (nome do livro e nome do autor) e cria um novo documento, consulta um documento e, então, o atualiza; tudo sob a égide de uma transação ACID implícita. Em qualquer momento durante a execução, se uma exceção de JavaScript for lançada, toda a transação será abortada.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

O cliente pode “enviar” a lógica de JavaScript acima para o banco de dados para execução transacional por meio do HTTP POST.

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });

Observe que, como o banco de dados compreende nativamente JSON e JavaScript, não há incompatibilidade no sistema de tipos, nem é preciso um “mapeamento OR” ou mágica na geração de códigos.

Os procedimentos armazenados e gatilhos interagem com uma coleção e os documentos em uma coleção por meio de um modelo de objeto bem-definido que expõe o contexto de coleção atual.

As coleções no Banco de Dados de Documentos podem ser criadas, excluídas, lidas ou enumeradas facilmente usando as [APIs REST do Banco de Dados de Documentos do Azure][] ou qualquer SDK cliente. O Banco de Dados de Documentos sempre oferece uma forte consistência para leitura ou consulta dos metadados de uma coleção. Excluir uma coleção automaticamente garante que você não possa acessar qualquer documento, anexo, procedimento armazenado, gatilho e UDFs contidos nela. Embora o Banco de Dados de Documentos recupere o armazenamento e a produtividade provisionados como parte da coleção excluída em segundo plano, o armazenamento provisionado e a produtividade para a coleção excluída estão disponíveis imediatamente para uso.

 

# Procedimentos armazenados, gatilhos e UDFs

Conforme descrito na seção anterior, você pode gravar a lógica do aplicativo para ser executada diretamente de uma transação no mecanismo de banco de dados. A lógica do aplicativo pode ser gravada inteiramente em JavaScript e pode ser modelada como um procedimento armazenado, um gatilho ou uma UDF. O código de JavaScript dentro do procedimento armazenado ou de um gatilho pode inserir, substituir, excluir, ler ou consultar documentos dentro de uma coleção. Por outro lado, o JavaScript dentro de uma UDF somente pode realizar cálculos livres de efeitos colaterais ao enumerar os documentos do conjunto de resultados da consulta e produzir outro conjunto de resultados. Para multilocatários, o Banco de Dados de Documentos realiza uma rígida governança de recursos baseada em reserva. Cada procedimento armazenado, gatilho ou UDF obtém um quantum fixo de recursos do sistema operacional para realizar esse trabalho. Além disso, os procedimentos armazenados, gatilhos ou UDFs não podem ser vinculados a bibliotecas de JavaScript externas e são incluídos em listas negras se excederem os orçamentos de recursos alocados para eles. Você pode registrar ou cancelar o registro de procedimentos armazenados, gatilhos ou UDFs com uma coleção por meio das APIs REST. Após o registro, um procedimento armazenado, gatilho ou UDF é pré-compilado e armazenado como um código de byte que é executado posteriormente. A seção a seguir ilustra como você pode usar o SDK de JavaScript do Banco de Dados de Documentos para registrar, executar e cancelar o registro de um procedimento armazenado, gatilho e UDF. O SDK de JavaScript é um wrapper simples nas APIs REST do Banco de Dados de Documentos.

## Registrando um procedimento armazenado

O registro de um procedimento armazenado é realizado ao criar um novo recurso de procedimento armazenado em uma coleção por meio do HTTP POST.

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };

    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

## Executando um procedimento armazenado

A execução de um procedimento armazenado é realizada ao emitir um HTTP POST em relação a um recurso de procedimento armazenado existente ao transferir parâmetros para o procedimento no corpo da solicitação.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

## Cancelando o registro de um procedimento armazenado

O cancelamento do registro de um procedimento armazenado é realizado simplesmente ao emitir um comando HTTP DELETE em relação a um recurso de procedimento armazenado existente.

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });

## Registrando um pré-gatilho

O registro de um gatilho é realizado ao criar um novo recurso de gatilho em uma coleção por meio do HTTP POST. Você pode especificar se o gatilho é um pré ou pós-gatilho e o tipo de operação à qual ele pode ser associado (p. ex., Criar, Substituir, Excluir ou Tudo).

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }

    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

## Executando um pré-gatilho

A execução de um gatilho é realizada especificando o nome de um gatilho existente ao mesmo tempo em que a solicitação POST/PUT/DELETE de um recurso de documento é emitida por meio do cabeçalho de solicitação.

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

## Cancelando o registro de um pré-gatilho

O cancelamento do registro de um gatilho é realizado simplesmente ao emitir um comando HTTP DELETE em relação a um recurso de gatilho existente.

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

## Registrando uma UDF

O registro de uma UDF é realizado ao criar um novo recurso de UDF em uma coleção por meio do HTTP POST.

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

## Executando uma UDF como parte da consulta

Uma UDF pode ser especificada como parte da consulta SQL e é usada como uma maneira de estender a linguagem de consulta SQL central do Banco de Dados de Documentos. Para obter mais detalhes sobre como compor a UDF na linguagem de consulta SQL do Banco de Dados de Documentos, consulte a especificação de linguagem de consulta SQL do Banco de Dados de Documentos.

    var filterQuery = "SELECT mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

## Cancelando o registro de uma UDF

O cancelamento do registro de uma UDF é realizado simplesmente ao emitir um comando HTTP DELETE em relação a um recurso de UDF existente.

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Embora os trechos acima mostrem o registro (POST), cancelamento de registro (PUT), leitura/lista (GET) e execução (POST) por meio do SDK de JavaScript do Banco de Dados de Documentos, você também pode usar as APIs REST ou outros SDKs clientes.

 

# Documentos

Você pode inserir, substituir, excluir, ler, enumerar e consultar documentos JSON arbitrários em uma coleção. O Banco de Dados de Documentos não exige nenhum esquema nem índices secundários a fim de oferecer suporte a consultas em relação a documentos em uma coleção.

Sendo um verdadeiro serviço de banco de dados aberto, o Banco de Dados de Documentos não inverte nenhum tipo de dados especializado (p. ex., de data e hora) ou codificações específicas para documentos JSON. Observe que o Banco de Dados de Documentos não requer nenhuma convenção JSON especial para codificar os relacionamentos entre vários documentos; a linguagem de consulta SQL do Banco de Dados de Documentos oferece operadores de consulta hierárquica e relacional bastante poderosos para consultar e projetar documentos sem nenhuma anotação especial ou necessidade de codificar relacionamentos entre documentos usando propriedades distintas.

Assim como todos os outros recursos, os documentos podem ser criados, substituídos, excluídos, lidos, enumerados e consultados facilmente usando as APIs REST ou qualquer SDK cliente. Excluir um documento libera imediatamente a cota correspondente a todos os anexos aninhados. O nível de consistência de leitura dos documentos segue a Política de Consistência da conta do banco de dados. Essa política pode ser substituída com base em cada solicitação, dependendo dos requisitos de consistência de dados de seu aplicativo. Ao consultar documentos, a consistência de leitura segue o conjunto do modo de indexação na coleção. Para fins de “consistência”, a Política de Consistência da conta é seguida.

# Anexos e mídia

O Banco de Dados de Documentos permite armazenar blobs/mídias binários/as no Banco de Dados de Documentos ou em seu próprio armazenamento de mídia. Ele também permite representar os metadados de uma mídia de acordo com um documento especial chamado anexo. Um anexo no Banco de Dados de Documentos é um documento (JSON) especial que faz referência à mídia/ao blob armazenada/o em outro lugar. Um anexo é simplesmente um documento especial que captura os metadados (p. ex., localização, autor, etc.) de uma mídia armazenada em um armazenamento remoto de mídia.

Considere um aplicativo de leitura social que utiliza o Banco de Dados de Documentos para armazenar anotações de tintas e metadados incluindo comentários, destaques, favoritos, classificações, preferências/não preferências, etc., associados a um e-book de um determinado usuário.

-   O conteúdo do livro em si é armazenado no armazenamento de mídia disponível como parte da conta do Banco de Dados de Documentos ou de um armazenamento remoto de mídia.
-   Um aplicativo poderá armazenar os metadados de cada usuário como um documento distinto, por exemplo, os metadados de Joe para o book1 são armazenados em um documento cuja referência é /colls/joe/docs/book1.
-   Anexos indicando para as páginas de conteúdo de um determinado livro de um usuário são armazenados no documento correspondente, p. ex., /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2, etc.

Observe que os exemplos utilizam IDs amigáveis para transmitir a hierarquia de recursos. Os recursos são acessados por meio das APIs REST usando IDs de recurso exclusivos.

Para a mídia gerenciada pelo Banco de Dados de Documentos, a propriedade \_media do anexo fará referência à mídia por meio de seu URI. O Banco de Dados de Documentos irá garantir que a mídia será jogada na lixeira quando todas as referências pendentes forem ignoradas. O Banco de Dados de Documentos gera automaticamente o anexo ao fazer o upload da nova mídia e preencher \_media para indicar a mídia recém-adicionada. Se escolher armazenar a mídia em um armazenamento de blob remoto por conta própria (p. ex., OneDrive, Azure Storage, DropBox, etc.), você ainda poderá usar os anexos para fazer referência à mídia. Nesse caso, você criará o anexo por conta própria e preencherá a propriedade its \_media.

Assim como todos os outros recursos, os anexos podem ser criados, substituídos, excluídos, lidos ou enumerados facilmente usando as APIs REST ou qualquer SDK cliente. Assim como com os documentos, o nível de consistência de leitura dos anexos segue a Política de Consistência na conta do banco de dados. Essa política pode ser substituída com base em cada solicitação, dependendo dos requisitos de consistência de dados de seu aplicativo. Ao consultar anexos, a consistência de leitura segue o conjunto do modo de indexação na coleção. Para fins de “consistência”, a Política de Consistência da conta é seguida.

 

# Usuários

Um usuário no Banco de Dados de Documentos representa um namespace lógico para permissões de agrupamentos. Um usuário no Banco de Dados de Documentos pode ser correspondente a um usuário em um sistema de gerenciamento de identidade ou uma função de aplicativo predefinida. Para o Banco de Dados de Documentos, um usuário simplesmente representa uma abstração para agrupar um conjunto de permissões em um banco de dados.

Para implementar multilocatários para seu aplicativo, você pode criar usuários no Banco de Dados de Documentos que correspondem aos seus usuários reais ou aos locatários de seu aplicativo. Você pode, então, criar permissões para um determinado usuário que corresponde ao controle de acesso nas várias coleções, documentos, anexos, etc.

Como seus aplicativos precisam ser escalados conforme o crescimento do usuário, você pode adotar várias maneiras de fragmentar seus dados. Você pode modelar cada um de seus usuários para

-   Cada usuário é mapeado a um banco de dados
-   Cada usuário é mapeado para uma coleção,
-   Documentos correspondentes a vários usuários para uma coleção dedicada ou
-   Documentos de modelo correspondentes a múltiplos usuários para um conjunto de coleções

Independentemente da estratégia de fragmentação específica escolhida, você pode modelar seus usuários reais como usuários no banco de dados do Banco de Dados de Documentos e associar permissões de refinamento a cada usuário.

![][3]
**Estratégias de fragmentação e usuários de modelagem**

Assim como todos os outros recursos, os usuários no Banco de Dados de Documentos podem ser criados, substituídos, excluídos, lidos ou enumerados facilmente usando as APIs REST ou qualquer SDK cliente. O Banco de Dados de Documentos sempre oferece uma forte consistência para leitura ou consulta dos metadados de um recurso do usuário. Vale destacar que excluir um usuário automaticamente assegura que você não poderá acessar nenhuma das permissões contidas nele. Embora o Banco de Dados de Documentos recupere a cota das permissões como parte do usuário excluído em segundo plano, as permissões excluídas estão disponíveis imediatamente mais uma vez para uso.

# Permissões

Da perspectiva de controle de acesso, recursos como contas do banco de dados, bancos de dados, usuários e permissões são considerados recursos *administrativos*, uma vez que requerem permissões administrativas. Por outro lado, recursos que incluem as coleções, documentos, anexos, procedimentos armazenados, gatilhos e UDFs têm seu escopo definido em um determinado banco de dados e são considerados como *recursos do aplicativo*. Correspondendo aos dois tipos de recursos e às funções que os acessam (a saber, o administrador e o usuário), o modelo de autorização define dois tipos de *chaves de acesso*: *chave mestra* e *chave de recurso*. A chave mestre é uma parte da conta do banco de dados e é fornecida ao desenvolvedor (ou administrador) que está provisionando a conta do banco de dados. Essa chave mestre possui uma semântica do administrador, e ela pode ser usada para autorizar o acesso aos recursos administrativos e do aplicativo. Em contraste, uma chave de recurso é uma chave de acesso granular que permite o acesso a um recurso de aplicativo *específico*. Portanto, ela captura a relação entre o usuário de um banco de dados e as permissões que o usuário possui para um recurso específico (p. ex., coleção, documento, anexo, procedimento armazenado, gatilho ou UDF).

A única maneira de obter uma chave de recurso é criar um recurso de permissão em um determinado usuário. Observe que, a fim de criar ou recuperar uma permissão, uma chave mestre deve ser apresentada no cabeçalho de autorização. Um recurso de permissão vincula o recurso, seu acesso e o usuário. Após criar um recurso de permissão, o usuário só precisa apresentar a chave de recurso associada para obter acesso ao recurso relevante. Portanto, uma chave de recurso pode ser visualizada como uma representação lógica e compacta do recurso de permissão.

Assim como todos os outros recursos, as permissões no Banco de Dados de Documentos podem ser criadas, substituídas, excluídas, lidas ou enumeradas facilmente usando as APIs REST ou qualquer SDK cliente. O Banco de Dados de Documentos sempre oferece uma forte consistência para leitura ou consulta dos metadados de uma permissão.

  []: ./media/documentdb-resources/resources1.png
  [1]: ./media/documentdb-resources/resources2.png
  []: http://portal.azure.com/
  [APIs REST do Banco de Dados de Documentos do Azure]: http://go.microsoft.com/fwlink/p/?LinkID=402413
  [2]: ./media/documentdb-resources/resources3.png
  [3]: ./media/documentdb-resources/resources4.png