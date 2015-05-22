<properties 
	pageTitle="Como usar o Fiddler para avaliar e testar as APIs REST de Pesquisa do Azure" 
	description="Use o Fiddler para uma abordagem sem código para verificar a disponibilidade de pesquisa do Azure e experimentar as APIs REST." 
	services="search" 
	documentationCenter="" 
	authors="HeidiSteen" 
	manager="mblythe" 
	editor=""/>

<tags 
	ms.service="search" 
	ms.devlang="rest-api" 
	ms.workload="search" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.date="03/12/2015" 
	ms.author="heidist"/>

# Como usar o Fiddler para avaliar e testar as APIs REST de Pesquisa do Azure

Este procedimento usa o Fiddler, disponível como um [download gratuito da Telerik](http://www.telerik.com/fiddler), para emitir solicitações HTTP e exibir as respostas usando a API REST de Pesquisa do Azure, sem precisar escrever nenhum código. As APIs REST do Serviço de Pesquisa do Azure são documentadas no [MSDN](https://msdn.microsoft.com/pt-br/library/azure/dn798935.aspx).

Nas etapas abaixo, você criará um índice, carregará documentos, consultará o índice e consultará o sistema quanto a informações de serviço.

Para concluir essas etapas, você precisará de um serviço de Pesquisa do Azure e `api-key`. Consulte [Criar um serviço de Pesquisa do Azure no portal](search-create-service-portal.md) para obter instruções sobre como começar.

## Crie um índice

1. Inicie o Fiddler. No menu Arquivo, desabilite **Capturar Tráfego** para ocultar atividades HTTP externas não relacionadas à atividade atual. 

3. Na guia Composer, você formulará uma solicitação semelhante a esta:

  	![][1]

2. Selecione **PUT**.

3. Insira uma URL que especifique a URL do serviço, solicite atributos e a versão da API. Alguns aspectos a ter em mente:
   + Use HTTPS como o prefixo
   + O atributo de solicitação é "/indexes/hotels". Isso diz à Pesquisa para criar um índice chamado “hotels”.
   + A versão da API fica em letras minúsculas, especificada como "?api-version=2015-02-28". As versões da API são importantes porque a Pesquisa do Azure implanta atualizações regularmente. Em ocasiões raras, uma atualização do serviço pode introduzir uma alteração de última hora na API. Usando as versões da API, você pode continuar usando sua versão existente e atualizando para a seguinte quando for conveniente.

    A URL completa deve se parecer com o exemplo a seguir:

         https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.	Especifique o cabeçalho da solicitação, substituindo o host e a chave da API por valores que sejam válidos para seu serviço.

        User-Agent: Fiddler
        host: my-app.search.windows.net
        content-type: application/json
        api-key: 1111222233334444

5.	No Corpo da Solicitação, copie os campos que compõem a definição do índice.

         {
        "name": "hotels",  
        "fields": [
          {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
          {"name": "baseRate", "type": "Edm.Double"},
          {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false,},
          {"name": "hotelName", "type": "Edm.String"},
          {"name": "category", "type": "Edm.String"},
          {"name": "tags", "type": "Collection(Edm.String)"},
          {"name": "parkingIncluded", "type": "Edm.Boolean"},
          {"name": "smokingAllowed", "type": "Edm.Boolean"},
          {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
          {"name": "rating", "type": "Edm.Int32"},
          {"name": "location", "type": "Edm.GeographyPoint"}
         ] 
        }

6.	Clique em **Executar**.

Em alguns segundos, você deverá ver uma resposta HTTP 201 na lista de sessões, indicando que o índice foi criado com êxito.

Se obtiver o HTTP 504, veja se a URL especifica HTTPS. Caso veja HTTP 400 ou 404, confira o corpo da solicitação para verificar se não houve erros ao copiar e colar. Um HTTP 403 normalmente indica um problema com a chave de API \(uma chave inválida ou um problema de sintaxe com o modo que a chave de API está especificada\).

## Carregue os documentos

Na guia Composer, sua solicitação para publicar documentos terá a seguinte aparência. O corpo da solicitação contém os dados de pesquisa de 4 hotéis.

   ![][2]

1. Selecione **POST**.

2.	Insira uma URL iniciada por HTTPS, seguida da URL do serviço, seguida de "/indexes/\<'nomedoíndice'\>/docs/index?api-version=2015-02-28". A URL completa deve se parecer com o exemplo a seguir:

        https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.	O Cabeçalho da solicitação deve ser igual ao anterior. Lembre-se de que você substituiu o host e a chave de API por valores que são válidos para seu serviço.

        User-Agent: Fiddler
        host: my-app.search.windows.net
        content-type: application/json
        api-key: 1111222233334444

4.	O Corpo da solicitação contém quatro documentos a serem adicionados ao índice de hotéis.

        {
        "value": [
        {
        	"@search.action": "upload",
        	"hotelId": "1",
        	"baseRate": 199.0,
        	"description": "Best hotel in town",
        	"hotelName": "Fancy Stay",
        	"category": "Luxury",
        	"tags": ["pool", "view", "wifi", "concierge"],
        	"parkingIncluded": false,
        	"smokingAllowed": false,
        	"lastRenovationDate": "2010-06-27T00:00:00Z",
        	"rating": 5,
        	"location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
          },
          {
        	"@search.action": "upload",
        	"hotelId": "2",
        	"baseRate": 79.99,
        	"description": "Cheapest hotel in town",
        	"hotelName": "Roach Motel",
        	"category": "Budget",
        	"tags": ["motel", "budget"],
        	"parkingIncluded": true,
        	"smokingAllowed": true,
        	"lastRenovationDate": "1982-04-28T00:00:00Z",
        	"rating": 1,
        	"location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
          },
          {
        	"@search.action": "upload",
        	"hotelId": "3",
        	"baseRate": 279.99,
        	"description": "Surprisingly expensive",
        	"hotelName": "Dew Drop Inn",
        	"category": "Bed and Breakfast",
        	"tags": ["charming", "quaint"],
        	"parkingIncluded": true,
        	"smokingAllowed": false,
        	"lastRenovationDate": null,
        	"rating": 4,
        	"location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
          },
          {
        	"@search.action": "upload",
        	"hotelId": "4",
        	"baseRate": 220.00,
        	"description": "This could be the one",
        	"hotelName": "A Hotel for Everyone",
        	"category": "Basic hotel",
        	"tags": ["pool", "wifi"],
        	"parkingIncluded": true,
        	"smokingAllowed": false,
        	"lastRenovationDate": null,
        	"rating": 4,
        	"location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
          }
         ]
        }

8.	Clique em **Executar**.

Em alguns segundos, você verá uma resposta HTTP 200 na lista de sessões. Isso indica que os documentos foram criados com êxito. Se você obtiver um 207, houve falha no carregamento de pelo menos um documento. Se você obtiver um 404, há um erro de sintaxe no cabeçalho ou no corpo da solicitação.

## Consulte o índice

Agora que o índice e os documentos foram carregados, você pode consultá-los. Na guia Composer, um comando GET que consulta seu serviço será semelhante ao seguinte:

   ![][3]

1.	Selecione **GET**.

2.	Insira uma URL iniciada por HTTPS, seguida da URL do serviço, seguida por "/indexes/\<'indexname'\>/docs?", seguido de parâmetros de consulta. Para um exemplo, use a URL a seguir, substituindo o nome de host de amostra por um que seja válido para seu serviço.

        https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Esta consulta pesquisa o termo “motel” e recupera categorias facetadas para as classificações.

3.	O Cabeçalho da solicitação deve ser igual ao anterior. Lembre-se de que você substituiu o host e a chave de API por valores que são válidos para seu serviço.

        User-Agent: Fiddler
        host: my-app.search.windows.net
        content-type: application/json
        api-key: 1111222233334444

O código de resposta deve ser 200, e a saída de resposta deve ser semelhante à ilustração a seguir.
 
   ![][4]

O exemplo de consulta a seguir é da [operação Pesquisar Índice \(API da Pesquisa do Azure\)](http://msdn.microsoft.com/library/dn798927.aspx) no MSDN. Muitos dos exemplos de consulta deste tópico incluem espaços, que não são permitidos no Fiddler. Substitua cada espaço por um caractere + antes de colar na cadeia de consulta e tentar realizar a consulta no Fiddler:

**Antes da substituição dos espaços:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Após a substituição dos espaços por +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## Consulte o sistema

Você também pode consultar o sistema para obter informações de contagens de documentos e consumo de armazenamento. Na guia Composer, sua solicitação será semelhante à seguinte, e a resposta retornará uma contagem do número de documentos e do espaço usado.

 ![][5]

1.	Selecione **GET**.

2.	Insira uma URL que inclua a URL do seu serviço, seguida por "/indexes/hotels/stats?api-version=2015-02-28":

        https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28 

3.	Especifique o cabeçalho da solicitação, substituindo o host e a chave da API por valores que sejam válidos para seu serviço.

        User-Agent: Fiddler
        host: my-app.search.windows.net
        content-type: application/json
        api-key: 1111222233334444

4.	Deixe o corpo da solicitação vazio.

5.	Clique em **Executar**. Você deverá ver um código de status HTTP 200 na lista de sessões. Selecione a entrada publicada para seu comando.

6.	Clique na guia **Inspetores** \| **Cabeçalhos** e selecione o formato JSON. Você deverá ver a contagem de documentos e o tamanho do armazenamento \(em KB\).

## Próximas etapas

Os links a seguir fornecem informações adicionais para uma abordagem sem código para gerenciar e usar a Pesquisa do Azure.

-  [Gerencie seu serviço de Pesquisa no Azure](search-manage.md)
-  [Como usar o Chrome Postman com a Pesquisa do Azure](search-chrome-postman.md)

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png


<!--HONumber=54-->