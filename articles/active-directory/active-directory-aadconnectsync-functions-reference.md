<properties
	pageTitle="Azure AD Connect Sync: referência de funções | Microsoft Azure"
	description="Referência de expressões de provisionamento declarativo na sincronização do Azure AD Connect."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="StevenPo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="10/13/2015"
	ms.author="markusvi"/>


# Azure AD Connect Sync: referência de funções


No Azure Active Directory Sync, as funções são usadas para manipular um valor de atributo durante a sincronização. <br> A sintaxe das funções é expressa usando o seguinte formato: <br> `<output type> FunctionName(<input type> <position name>, ..)`

Se uma função está sobrecarregada e aceita diversas sintaxes, todas as sintaxes válidas são listadas.<br> As funções são fortemente tipadas e verificam que o tipo passado corresponde ao tipo documentado.<br> Se o tipo não corresponde, um erro é gerado.

Os tipos são expressos com a seguinte sintaxe:

- **bin** - binário
- **bool** - booliano
- **dt** - data/hora UTC
- **enum** - enumeração das constantes conhecidas
- **exp** - expressão, que espera-se que seja avaliada como um valor Booliano
- **mvbin** - binário de vários valores
- **mvstr** - referência de vários valores
- **num** - numérico
- **ref** - referência de valor único
- **str** - cadeia de caracteres de valor único
- **var** - uma variante de (quase) qualquer outro tipo
- **void** - não retorna um valor



## Referência de funções

----------
**Conversão:**

[CBool](#cbool) &nbsp;&nbsp;&nbsp;&nbsp; [CDate](#cdate) &nbsp;&nbsp;&nbsp;&nbsp; [CGuid](#cguid) &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; [ConvertFromBase64](#convertfrombase64) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToBase64](#converttobase64) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertFromUTF8Hex](#convertfromutf8hex) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToUTF8Hex](#converttoutf8hex) &nbsp;&nbsp;&nbsp;&nbsp; [CNum](#cnum) &nbsp;&nbsp;&nbsp;&nbsp; [CRef](#cref) &nbsp;&nbsp;&nbsp;&nbsp; [CStr](#cstr) &nbsp;&nbsp;&nbsp;&nbsp; [StringFromGuid](#StringFromGuid) &nbsp;&nbsp;&nbsp;&nbsp; [StringFromSid](#stringfromsid)

**Data / hora:**

[DateAdd](#dateadd) &nbsp;&nbsp;&nbsp;&nbsp; [DateFromNum](#datefromnum) &nbsp;&nbsp;&nbsp;&nbsp; [FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Now](#now) &nbsp;&nbsp;&nbsp;&nbsp; [NumFromDate](#numfromdate)

**Diretório**

[DNComponent](#dncomponent) &nbsp;&nbsp;&nbsp;&nbsp; [DNComponentRev](#dncomponentrev) &nbsp;&nbsp;&nbsp;&nbsp; [EscapeDNComponent](#escapedncomponent)

**Inspeção:**

[IsBitSet](#isbitset) &nbsp;&nbsp;&nbsp;&nbsp; [IsDate](#isdate) &nbsp;&nbsp;&nbsp;&nbsp; [IsEmpty](#isempty) &nbsp;&nbsp;&nbsp;&nbsp; [IsGuid](#isguid) &nbsp;&nbsp;&nbsp;&nbsp; [IsNull](#isnull) &nbsp;&nbsp;&nbsp;&nbsp; [IsNullOrEmpty](#isnullorempty) &nbsp;&nbsp;&nbsp;&nbsp; [IsNumeric](#isnumeric) &nbsp;&nbsp;&nbsp;&nbsp; [IsPresent](#ispresent) &nbsp;&nbsp;&nbsp;&nbsp; [IsString](#isstring)

**Matemática:**

[BitAnd](#bitand) &nbsp;&nbsp;&nbsp;&nbsp; [BitOr](#bitor) &nbsp;&nbsp;&nbsp;&nbsp; [RandomNum](#randomnum)

**De valores múltiplos**

[Contains](#contains) &nbsp;&nbsp;&nbsp;&nbsp; [Count](#count) &nbsp;&nbsp;&nbsp;&nbsp; [Item](#item) &nbsp;&nbsp;&nbsp;&nbsp; [ItemOrNull](#itemornull) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [RemoveDuplicates](#removeduplicates) &nbsp;&nbsp;&nbsp;&nbsp; [Split](#split)

**Fluxo do programa:**

[Error](#error) &nbsp;&nbsp;&nbsp;&nbsp; [IIF](#iif) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)


**Texto**

[GUID](#guid) &nbsp;&nbsp;&nbsp;&nbsp; [InStr](#instr) &nbsp;&nbsp;&nbsp;&nbsp; [InStrRev](#instrrev) &nbsp;&nbsp;&nbsp;&nbsp; [LCase](#lcase) &nbsp;&nbsp;&nbsp;&nbsp; [Left](#left) &nbsp;&nbsp;&nbsp;&nbsp; [Len](#len) &nbsp;&nbsp;&nbsp;&nbsp; [LTrim](#ltrim) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp;&nbsp;&nbsp; [PadLeft](#padleft) &nbsp;&nbsp;&nbsp;&nbsp; [PadRight](#padright) &nbsp;&nbsp;&nbsp;&nbsp; [PCase](#pcase) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [ReplaceChars](#replacechars) &nbsp;&nbsp;&nbsp;&nbsp; [Right](#right) &nbsp;&nbsp;&nbsp;&nbsp; [RTrim](rtrim) &nbsp;&nbsp;&nbsp;&nbsp; [Trim](#trim) &nbsp;&nbsp;&nbsp;&nbsp; [UCase](#ucase) &nbsp;&nbsp;&nbsp;&nbsp; [Word](#word)

----------
### BitAnd

**Descrição:**<br> a função BitAnd define os bits especificados em um valor.

**Sintaxe:**<br> `num BitAnd(num value1, num value2)`

- value1, value2: valores numéricos que devem ser agrupados com AND

**Comentários:**<br> esta função converte ambos os parâmetros para a representação binária e define um bit como:

- 0 - se um ou ambos os bits correspondentes na *máscara* e no *sinalizador* são 0
- 1 - se ambos os bits correspondentes são 1.

Em outras palavras, ele retorna 0 em todos os casos, exceto quando os bits correspondentes de ambos os parâmetros são 1.

**Exemplo:**<br> `BitAnd(&HF, &HF7)`<br> Retorna 7, já que os hexadecimais "F" AND "F7" são avaliados como esse valor.

----------
### BitOr

**Descrição:** <br> a função BitAnd define os bits especificados em um valor.

**Sintaxe:** <br> `num BitOr(num value1, num value2)`

- value1, value2: valores numéricos que devem ser agrupados com OR

**Comentários:** <br> Esta função converte ambos os parâmetros para a representação binária e define um bit como 1 se um ou ambos os bits correspondentes na máscara e sinalizador forem 1; ou então define o bit como 0, se ambos os bits correspondentes forem 0. <br> Em outras palavras, ele retorna 1 em todos os casos, exceto naqueles em que os bits correspondentes de ambos os parâmetros são 0.

----------
### CBool

**Descrição:**<br> a função CBool retorna um valor booliano com base na expressão avaliada

**Sintaxe:** <br> `bool CBool(exp Expression)`

**Comentários:**<br> se a expressão é avaliada como um valor diferente de zero, CBool retorna True; caso contrário, retorna False.


**Exemplo:**<br> `CBool([attrib1] = [attrib2])` <br>

Retorna True se ambos os atributos têm o mesmo valor.




----------
### CDate

**Descrição:**<br> a função CDate retorna, de uma cadeia de caracteres, um atributo DateTime UTC. DateTime não é um tipo de atributo nativo no Sync, mas é usado por algumas funções.

**Sintaxe:**<br> `dt CDate(str value)`

- Value: uma cadeia de caracteres com uma data, hora e opcionalmente um fuso horário

**Comentários:**<br> a cadeia de caracteres retornada é sempre em UTC.

**Exemplo:**<br> `CDate([employeeStartTime])` <br> retorna um DateTime com base na hora de início do funcionário

`CDate("2013-01-10 4:00 PM -8")` <br> Retorna um DateTime que representa "2013-01-11 12:00 AM"




----------
### CGuid

**Descrição:**<br> a função CGuid converte a representação de cadeia de caracteres de um GUID em sua representação binária.

**Sintaxe:**<br> `bin CGuid(str GUID)GUID`

- Uma cadeia de caracteres formatada nesse padrão: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx ou {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}




----------
### Contém:

**Descrição:**<br> a função Contains localiza uma cadeia de caracteres dentro de um atributo com vários valores

**Sintaxe:**<br> `num Contains (mvstring attribute, str search)` - diferencia maiúsculas de minúsculas<br> `num Contains (mvstring attribute, str search, enum Casetype)`<br> `num Contains (mvref attribute, str search)` - diferencia maiúsculas de minúsculas

- attribute: o atributo com vários valores pelo qual pesquisar.<br>
- search: cadeia de caracteres a localizar no atributo.<br>
- Casetype: CaseInsensitive ou CaseSensitive.<br>

Retorna o índice no atributo com vários valores em que a cadeia de caracteres foi encontrada. Se a cadeia de caracteres não for encontrada, 0 será retornado.


**Comentários:**<br> para atributos com vários valores de cadeia de caracteres, a pesquisa encontrará subcadeias de caracteres nos valores.<br> Para atributos de referência, a cadeia de caracteres pesquisada deve corresponder exatamente ao valor para que sejam considerados como uma correspondência.

**Exemplo:**<br> `IIF(Contains([proxyAddresses],”SMTP:”)>0,[proxyAddresses],Error(“No primary SMTP address found.”))`<br> se o atributo proxyAddresses tem um endereço de email principal (indicado por letras maiúsculas "SMTP:"), o atributo proxyAddress é retornado; caso contrário, um erro é retornado.




----------
### ConvertFromBase64

**Descrição:**<br> a função ConvertFromBase64 converte o valor codificado base64 especificado em uma cadeia de caracteres regular.

**Sintaxe:**<br>`str ConvertFromBase64(str source)` - adota Unicode para a codificação de <br> `str ConvertFromBase64(str source, enum Encoding)`

- source: cadeia de caracteres codificada em Base64<br>
- Codificação: Unicode, ASCII, UTF8

**Exemplo**<br> `ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`<br> `ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Ambos os exemplos retornam "*Hello world!*"




----------
### ConvertFromUTF8Hex

**Descrição:**<br> a função ConvertFromUTF8Hex converte o valor codificado em UTF8 hexadecimal especificado em uma cadeia de caracteres.

**Sintaxe:**<br> `str ConvertFromUTF8Hex(str source)`

- source: cadeia de caracteres codificada em UTF8 de 2 bytes

**Comentários:**<br> a diferença entre essa função e ConvertFromBase64(,UTF8) é que o resultado é amigável para o atributo DN.<br> Esse formato é usado pelo Active Directory do Azure como DN.

**Exemplo:**<br> `ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`<br> retorna "*Hello world!*"




----------
### ConvertToBase64

**Descrição:** <br> a função ConvertToBase64 converte uma cadeia de caracteres em uma cadeia de caracteres Unicode em base64.<br> Converte o valor de uma matriz de inteiros em sua representação equivalente de cadeia de caracteres, que é codificada com dígitos em base 64.

**Sintaxe:** <br> `str ConvertToBase64(str source)`

**Exemplo:** <br> `ConvertToBase64("Hello world!")` <br> retorna "SABlAGwAbABvACAAdwBvAHIAbABkACEA"




----------
### ConvertToUTF8Hex

**Descrição:**<br> a função ConvertToUTF8Hex converte uma cadeia de caracteres em um valor codificado em UTF8 hexadecimal.

**Sintaxe:**<br> `str ConvertToUTF8Hex(str source)`

**Comentários:**<br> o formato de saída dessa função é usado pelo Active Directory do Azure como formato do atributo DN.

**Exemplo:** <br> `ConvertToUTF8Hex("Hello world!")` <br> retorna 48656C6C6F20776F726C6421




----------
### Contagem

**Descrição:**<br> a função Count retorna o número de elementos em um atributo com valores múltiplos

**Sintaxe:** <br> `num Count(mvstr attribute)`




----------
### CNum

**Descrição:** <br> a função CNum usa uma cadeia de caracteres e retorna um tipo de dados numérico.

**Sintaxe:** <br> `num CNum(str value)`




----------
### CRef

**Descrição:** <br> converte uma cadeia de caracteres em um atributo de referência

**Sintaxe:** <br> `ref CRef(str value)`

**Exemplo:** <br> `CRef(“CN=LC Services,CN=Microsoft,CN=lcspool01, CN=Pools,CN=RTC Service,” & %Forest.LDAP%)`




----------
### CStr

**Descrição:** <br> a função CStr o converte em um tipo de dados String.

**Sintaxe:** <br> `str CStr(num value)` <br> `str CStr(ref value)` <br> `str CStr(bool value)` <br>

- valor: pode ser um valor numérico, o atributo de referência ou booliano.

**Exemplo:** <br> `CStr([dn]) <br>` poderia retornar “cn=Joe,dc=contoso,dc=com”




----------
### DateAdd

**Descrição:** <br> retorna um parâmetro Date, contendo uma data à qual um intervalo de tempo especificado foi adicionado.

**Sintaxe:** <br> `dt DateAdd(str interval, num value, dt date)`

- interval: expressão de cadeia de caracteres que é o intervalo de tempo que você deseja adicionar. A cadeia de caracteres deve ter um dos seguintes valores:
 - aaaa Ano
 - q - Trimestre
 - m - Mês
 - y - Dia do ano
 - d - Dia
 - w - Dia da semana
 - ww - Semana
 - h - Hora
 - m - Minuto
 - s - Segundo
- valor: O número de unidades que você deseja adicionar. Ele pode ser positivo (para obter datas no futuro) ou negativo (para obter datas no passado).
- date: DateTime, representando a data à qual o intervalo é adicionado.

**Exemplo:** <br> `DateAdd("m", 3, CDate("2001-01-01"))` <br> adiciona 3 meses e retorna para um DateTime que representa "1/4/2001"




----------
### DateFromNum

**Descrição:** <br> A função DateFromNum converte um valor, de formato de data do AD, em um tipo DateTime.

**Sintaxe:** <br> `dt DateFromNum(num value)`

**Exemplo:** <br> `DateFromNum([lastLogonTimestamp])` <br> `DateFromNum(129699324000000000)` <br> retorna um DateTime que representa 2012-01-01 23:00:00




----------
### DNComponent

**Descrição:** <br> a função DNComponent retornará o valor de um componente DN especificado, saindo da esquerda.

**Sintaxe:** <br> `str DNComponent(ref dn, num ComponentNumber)`

- dn: o atributo de referência a interpretar
- ComponentNumber: o componente no DN a retornar

**Exemplo:** <br> `DNComponent([dn],1)` <br> se dn é “cn=Joe,ou=…, ele retorna Joe




----------
### DNComponentRev

**Descrição:** <br> a função DNComponentRev retornará o valor de um componente DN especificado, saindo da direita (o final).

**Sintaxe:** <br> `str DNComponentRev(ref dn, num ComponentNumber)` <br> `str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- dn: o atributo de referência a interpretar
- ComponentNumber - o componente no DN a retornar
- Opções: DC – ignorar todos os componentes com “dc=”

**Exemplo:** <br> `If dn is “cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com” then DNComponentRev([dn],3)` <br> `DNComponentRev([dn],1,”DC”)` <br> ambos retornam EUA.




----------
### Erro

**Descrição:**<br> a função Error é usada para retornar um erro personalizado.

**Sintaxe:** <br> `void Error(str ErrorMessage)`

**Exemplo:** <br> `IIF(IsPresent([accountName]),[accountName],Error(“AccountName is required”))` <br> se o atributo accountName não estiver presente, gere um erro no objeto.




----------
### EscapeDNComponent

**Descrição:** <br> a função EscapeDNComponent usa um componente de um DN e escapa-o para que ele possa ser representado no LDAP.

**Sintaxe:** <br> `str EscapeDNComponent(str value)`

**Exemplo:** <br> `EscapeDNComponent(“cn=” & [displayName]) & “,” & %ForestLDAP%` <br> garante que o objeto possa ser criado em um diretório LDAP, mesmo que o atributo displayName tenha caracteres que devam ser escapados no protocolo LDAP.




----------
### FormatDateTime

**Descrição:** <br> a função FormatDateTime é usada para formatar um DateTime para uma cadeia de caracteres com um formato especificado

**Sintaxe:** <br> `str FormatDateTime(dt value, str format)`

- value: um valor no formato DateTime <br>
- format: uma cadeia de caracteres que representa o formato para o qual converter.

**Comentários:** <br> Os possíveis valores para o formato podem ser encontrados aqui: [Formatos de data/hora definidos pelo usuário (função Format)](http://msdn2.microsoft.com/library/73ctwf33(VS.90).aspx)

**Exemplo:** <br>

`FormatDateTime(CDate(“12/25/2007”),”yyyy-mm-dd”)` <br> Resulta em "2007-12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),”yyyyMMddHHmmss.0Z”)` <br> Pode resultar em "20140905081453.0Z"




----------
### GUID

**Descrição:** <br> A função GUID gera um novo GUID aleatório

**Sintaxe:** <br> `str GUID()`




----------
### IIF

**Descrição:** <br> A função IIF retorna um valor de um conjunto de valores possíveis com base em uma condição especificada.

**Sintaxe:** <br> `var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- condition: qualquer valor ou expressão que pode ser avaliada como true ou false.
- valueIfTrue: um valor que será retornado se condition for avaliada como true.
- valueIfFalse: um valor que será retornado se condition for avaliada como false.

**Exemplo:** <br> `IIF([employeeType]=“Intern”,”t-“&[alias],[alias])` <br> Retorna o alias de um usuário com"t-" adicionado ao seu início se o usuário for um estagiário; caso contrário, retorna o alias do usuário como está.




----------
### InStr

**Descrição:** <br> A função InStr localiza a primeira ocorrência de uma subcadeia de caracteres em uma cadeia de caracteres

**Sintaxe:** <br>

`num InStr(str stringcheck, str stringmatch)` <br> `num InStr(str stringcheck, str stringmatch, num start)` <br> `num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- stringcheck: cadeia de caracteres a ser pesquisada <br>
- stringmatch: cadeia de caracteres a ser localizada <br>
- start: posição inicial para localizar a subcadeia de caracteres <br>
- compare: vbTextCompare ou vbBinaryCompare

**Comentários:** <br> Retorna a posição em que a subcadeia de caracteres tiver sido encontrada ou 0 se ela não tiver sido encontrada.

**Exemplo:** <br> `InStr("The quick brown fox","quick")` <br> É avaliado como 5

`InStr("repEated","e",3,vbBinaryCompare)` <br> É avaliado como 7




----------
### InStrRev

**Descrição:** <br> A função InStrRev localiza a última ocorrência de uma subcadeia de caracteres em uma cadeia de caracteres

**Sintaxe:** <br> `num InstrRev(str stringcheck, str stringmatch)` <br> `num InstrRev(str stringcheck, str stringmatch, num start)` <br> `num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- stringcheck: cadeia de caracteres a ser pesquisada <br>
- stringmatch: cadeia de caracteres a ser localizada <br>
- start: posição inicial para localizar a subcadeia de caracteres <br>
- compare: vbTextCompare ou vbBinaryCompare

**Comentários:** <br> Retorna a posição em que a subcadeia de caracteres tiver sido encontrada ou 0 se ela não tiver sido encontrada.

**Exemplo:** <br> `InStrRev("abbcdbbbef","bb")` <br> Retorna 7.




----------
### IsBitSet

**Descrição:** <br> A função IsBitSet testa se um bit está definido ou não

**Sintaxe:** <br> `bool IsBitSet(num value, num flag)`

- value: um valor numérico que é evaluated.flag: um valor numérico que contém o bit a ser avaliado

**Exemplo:** <br> `IsBitSet(&HF,4)` <br> Retorna True porque o bit "4" está definido no valor hexadecimal "F"




----------
### IsDate

**Descrição:** <br> A função IsDate será avaliada como True se a expressão puder ser avaliada como um tipo DateTime.

**Sintaxe:** <br> `bool IsDate(var Expression)`

**Comentários:** <br> Usada para determinar se CDate() será bem-sucedida.




----------
###IsEmpty

**Descrição:** <br> A função IsEmpty será avaliada como True se o atributo estiver presente no CS ou MV, mas será avaliada como uma cadeia de caracteres vazia.

**Sintaxe:** <br> `bool IsEmpty(var Expression)`




----------
###IsGuid

**Descrição:** <br> A função IsGuid será avaliada como true se a cadeia de caracteres puder ser convertida em um GUID.

**Sintaxe:** <br> `bool IsGuid(str GUID)`

**Comentários:** <br> Um GUID é definido como uma cadeia de caracteres seguindo um destes padrões: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx ou {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

usada para determinar se CGuid() será bem-sucedida.

**Exemplo:** <br> `IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)` <br> Se StrAttribute tiver um formato de GUID, ela retornará uma representação binária; caso contrário, retornará um valor nulo.




----------
###IsNull

**Descrição:** <br> A função IsNull retornará true se a expressão for avaliada como nula.

**Sintaxe:** <br> `bool IsNull(var Expression)`

**Comentários:** <br> Para um atributo, um valor nulo é expresso pela ausência do atributo.

**Exemplo:** <br> `IsNull([displayName])` <br> Retornará True se o atributo não estiver presente no CS ou MV.




----------
###IsNullOrEmpty

**Descrição:** <br> A função IsNullOrEmpty retornará true se a expressão for nula ou for uma cadeia de caracteres vazia.

**Sintaxe:** <br> `bool IsNullOrEmpty(var Expression)`

**Comentários:** <br> Para um atributo, isso poderia ser avaliado como True se o atributo estivesse ausente ou presente, mas fosse uma cadeia de caracteres vazia.<br> O inverso dessa função é chamado de IsPresent.

**Exemplo:** <br> `IsNull([displayName])` <br> Retornará True se o atributo não estiver presente ou for uma cadeia de caracteres vazia no CS ou MV.




----------
### IsNumeric

**Descrição:** <br> A função IsNumeric retorna um valor booliano que indica se uma expressão pode ser avaliada como um tipo numérico.

**Sintaxe:** <br> `bool IsNumeric(var Expression)`

**Comentários:**<br> Usada para determinar se CNum() terá sucesso ao analisar a expressão.




----------
### IsString

**Descrição:** <br> A função IsString será avaliada como True se a expressão puder ser avaliada como um tipo de cadeia de caracteres.

**Sintaxe:** <br> `bool IsString(var expression)`

**Comentários:** <br> Usada para determinar se CStr() terá sucesso ao analisar a expressão.




----------
### IsPresent

**Descrição:** <br> A função IsPresent retornará true se a expressão for avaliada como uma cadeia de caracteres que não é nula e não está vazia.

**Sintaxe:** <br> `bool IsPresent(var expression)`

**Comentários:** <br> O inverso dessa função é chamado de IsNullOrEmpty.

**Exemplo:** <br>

`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`




----------
### Item

**Descrição:** <br> A função Item retorna um item de um atributo/cadeia de caracteres com valores múltiplos.

**Sintaxe:** <br> `var Item(mvstr attribute, num index)`

- attribute: atributo com valores múltiplos <br>
- index: índice para um item na cadeia de caracteres com vários valores.

**Comentários:** <br> A função Item é útil com a função Contains, desde que a última função retorne o índice para um item no atributo de valores múltiplos.

Gera um erro se o índice está fora dos limites.

**Exemplo:** <br> `Mid(Item([proxyAddress],Contains([proxyAddress], ”SMTP:”)),6)` <br> Retorna o endereço de email principal.




----------
### ItemOrNull

**Descrição:** <br> A função ItemOrNull retorna um item de um atributo/cadeia de caracteres com valores múltiplos.

**Sintaxe:** <br> `var ItemOrNull(mvstr attribute, num index)`

- attribute: atributo com valores múltiplos <br>
- index: índice para um item na cadeia de caracteres com vários valores.

**Comentários:** <br> A função ItemOrNull é útil com a função Contains, desde que a última função retorne o índice para um item no atributo de valores múltiplos.

Retorna um valor nulo se o índice está fora dos limites.




----------
### Join

**Descrição:** <br> A função Join usa uma cadeia de caracteres de valores múltiplos e retorna uma cadeia de caracteres de valor único com separador especificado inserido entre cada item.

**Sintaxe:** <br> `str Join(mvstr attribute)` <br> `str Join(mvstr attribute, str Delimiter)`

- attribute: um atributo de valores múltiplos contendo cadeias de caracteres a serem unidas. <br>
- delimiter: qualquer cadeia de caracteres usada para separar as subcadeias de caracteres na cadeia de caracteres retornada. Se omitido, o caractere de espaço (" ") é usado. Se o Delimitador é uma cadeia de caracteres de comprimento zero ("") ou Nada, todos os itens na lista são concatenados sem delimitadores.

**Comentários**<br> Há paridade entre as funções Join e Split. A função Join pega uma matriz de cadeias de caracteres e une-as usando uma cadeia de caracteres do delimitador, para retornar uma única cadeia de caracteres. A função Split pega uma cadeia de caracteres e a separa no delimitador, para retornar uma matriz de cadeias de caracteres. No entanto, uma diferença importante é que a Join pode concatenar cadeias de caracteres com qualquer cadeia de caracteres delimitadora, enquanto Split só pode separar cadeias de caracteres usando um único caractere delimitador.

**Exemplo:** <br> `Join([proxyAddresses],”,”)` <br> Pode retornar: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com”




----------
### LCase

**Descrição:** <br> A função LCase converte todos os caracteres de uma cadeia em letras minúsculas.

**Sintaxe:** <br> `str LCase(str value)`

**Exemplo:** <br> `LCase(“TeSt”)` <br> Retorna "test".




----------
### Left

**Descrição:** <br> A função Left retorna um número especificado de caracteres do lado esquerdo de uma cadeia de caracteres.

**Sintaxe:** <br> `str Left(str string, num NumChars)`

- string: a cadeia de caracteres da qual retornar caracteres <br>
- NumChars: um número que identifica o número de caracteres a ser retroando do início (esquerda) da cadeia de caracteres

**Comentários:** <br> Uma cadeia de caracteres que contém os primeiros caracteres numChars na cadeia de caracteres:

- Se numChars = 0, retorne a cadeia de caracteres vazia.
- Se numChars < 0, retorne a cadeia de caracteres de entrada.
- Se a cadeia de caracteres for nula, retorne a cadeia de caracteres vazia.

Se a cadeia de caracteres contém menos caracteres do que o número especificado em numChars, uma cadeia de caracteres idêntica à cadeia de caracteres (ou seja, que contém todos os caracteres no parâmetro 1) será retornada.

**Exemplo:** <br> `Left(“John Doe”, 3)` <br> Retorna "Joh".




----------
### Len

**Descrição:** <br> A função Len retorna o número de caracteres em uma cadeia de caracteres.

**Sintaxe:** <br> `num Len(str value)`

**Exemplo:** <br> `Len(“John Doe”)` <br> Retorna 8.




----------
### LTrim

**Descrição:** <br> A função LTrim remove espaços em branco à esquerda de uma cadeia de caracteres.

**Sintaxe:** <br> `str LTrim(str value)`

**Exemplo:** <br> `LTrim(“ Test ”)` <br> Retorna "Test"




----------
### Mid

**Descrição:** <br> A função Mid retorna um número especificado de caracteres de uma posição especificada em uma cadeia de caracteres.

**Sintaxe:** <br> `str Mid(str string, num start, num NumChars)`

- string: a cadeia de caracteres da qual retornar caracteres <br>
- start: um número que identifica a posição inicial na cadeia de caracteres da qual retornar caracteres
- NumChars: um número que identifica o número de caracteres a ser retornado da posição


**Comentários:** <br> Retorna caracteres numChars desde a posição inicial na cadeia de caracteres.<br> Uma cadeia de caracteres contendo caracteres numChars desde a posição inicial na cadeia de caracteres:

- Se numChars = 0, retorne a cadeia de caracteres vazia.
- Se numChars < 0, retorne a cadeia de caracteres de entrada.
- Se start > o comprimento da cadeia de caracteres, retorne a cadeia de caracteres de entrada.
- Se start <= 0, retorne a cadeia de caracteres de entrada.
- Se a cadeia de caracteres for nula, retorne a cadeia de caracteres vazia.

Se não houver caracteres numChar restantes na cadeia de caracteres na posição inicial, serão retornados tantos caracteres quanto for possível retornar.

**Exemplo:** <br>

`Mid(“John Doe”, 3, 5)` <br> Retorna "hn Do".

`Mid(“John Doe”, 6, 999)` <br> Retorna "Doe"




----------
### Now

**Descrição:** <br> A função Now retorna um DateTime especificando a data e hora atuais, de acordo a data e hora do sistema do seu computador.

**Sintaxe:** <br> `dt Now()`




----------
### NumFromDate

**Descrição:** <br> A função NumFromDate retorna uma data no formato de data do Active Directory.

**Sintaxe:** <br> `num NumFromDate(dt value)`


**Exemplo:** <br> `NumFromDate(CDate("2012-01-01 23:00:00"))` <br> Retorna 129699324000000000.




----------
### PadLeft

**Descrição:** <br> A função PadLeft preenche à esquerda uma cadeia de caracteres até um comprimento especificado usando um caractere de preenchimento fornecido.

**Sintaxe:** <br> `str PadLeft(str string, num length, str padCharacter)`

- string: a cadeia de caracteres a ser preenchida. <br>
- length: um inteiro que representa o comprimento desejado da cadeia de caracteres. <br>
- padCharacter: uma cadeia de caracteres que consiste em um único caractere a ser usado como o caractere de preenchimento



----------
### Comentários

- Se o comprimento da cadeia de caracteres for menor que length, padCharacter será acrescentado repetidamente ao início (esquerda) da cadeia de caracteres até que ela tenha um comprimento igual a length.
- PadCharacter pode ser um caractere de espaço, mas não pode ser um valor nulo.
- Se o comprimento da cadeia de caracteres é igual ou maior que length, a cadeia de caracteres é retornada inalterada.
- Se a cadeia de caracteres tem um comprimento maior que ou igual a length, uma cadeia de caracteres idêntica à cadeia de caracteres em questão será retornada.
- Se o comprimento da cadeia de caracteres for menor que length, uma nova cadeia de caracteres do comprimento desejado é retornada, contendo a cadeia de caracteres preenchida com um padCharacter.
- Se a cadeia de caracteres é nula, a função retorna uma cadeia de caracteres vazia.

**Exemplo:** <br> `PadLeft(“User”, 10, “0”)` <br> Retorna "000000User".




----------
### PadRight

**Descrição:** <br> A função PadRight preenche à direita uma cadeia de caracteres até um comprimento especificado usando um caractere de preenchimento fornecido.

**Sintaxe:** <br> `str PadRight(str string, num length, str padCharacter)`

- string: a cadeia de caracteres a preencher.
- length: um inteiro que representa o comprimento da cadeia de caracteres desejado.
- padCharacter: uma cadeia de caracteres que consiste em um único caractere a ser usado como o caractere de preenchimento

**Comentários:**

- Se o comprimento da cadeia de caracteres for menor que length, padCharacter será acrescentado repetidamente ao final (direita) da cadeia de caracteres até que ela tenha um comprimento igual a length.
- padCharacter pode ser um caractere de espaço, mas não pode ser um valor nulo.
- Se o comprimento da cadeia de caracteres é igual ou maior que length, a cadeia de caracteres é retornada inalterada.
- Se a cadeia de caracteres tem um comprimento maior que ou igual a length, uma cadeia de caracteres idêntica à cadeia de caracteres em questão será retornada.
- Se o comprimento da cadeia de caracteres for menor que length, uma nova cadeia de caracteres do comprimento desejado é retornada, contendo a cadeia de caracteres preenchida com um padCharacter.
- Se a cadeia de caracteres é nula, a função retorna uma cadeia de caracteres vazia.


**Exemplo:** <br> `PadRight(“User”, 10, “0”)` <br> Retorna "User000000".




----------
### PCase

**Descrição:** <br> A função PCase converte em letras maiúsculas o primeiro caractere de cada palavra delimitada por espaço em uma cadeia de caracteres , enquanto todos os outros caracteres são convertidos em letras minúsculas.

**Sintaxe:** <br> `String PCase(string)`

**Exemplo:** <br> `PCase(“TEsT”)` <br> Retorna "Test".




----------
### RandomNum

**Descrição:** <br> A função RandomNum retorna um número aleatório entre um intervalo especificado.

**Sintaxe:** <br> `num RandomNum(num start, num end)`

- start: um número que identifica o limite inferior do valor aleatório a ser gerado <br>
- end: um número que identifica o limite superior do valor aleatório a gerar

**Exemplo:** <br> `Random(100,999)` <br> Retorna 734.




----------
### RemoveDuplicates

**Descrição:** <br> A função RemoveDuplicates usa uma cadeia de caracteres com valores múltiplos e verifica se cada valor é exclusivo.

**Sintaxe:** <br> `mvstr RemoveDuplicates(mvstr attribute)`

**Exemplo:** <br> `RemoveDuplicates([proxyAddresses])` <br> Retorna um atributo proxyAddress corrigido em que todos os valores duplicados foram removidos.




----------
### Substitua

**Descrição:**<br> A função Replace substitui todas as ocorrências de uma cadeia de caracteres por outra cadeia de caracteres.

**Sintaxe:** <br> `str Replace(str string, str OldValue, str NewValue)`

- string: uma cadeia de caracteres na qual substituir valores. <br>
- OldValue: a cadeia de caracteres pela qual pesquisar e a qual substituir. <br>
- NewValue: a cadeia de caracteres a substituir.


**Comentários:** <br> A função reconhece os seguintes monikers especiais:

- \\n - Nova linha
- \\r - Retorno de carro
- \\t - Guia


**Exemplo:** <br>

`Replace([address],”\r\n”,”, “)` <br> Substitui CRLF por uma vírgula e um espaço e pode levar a "One Microsoft Way, Redmond, WA, USA"




----------
### ReplaceChars

**Descrição:** <br> A função ReplaceChars substitui todas as ocorrências de caracteres encontradas na cadeia de caracteres ReplacePattern.

**Sintaxe:** <br> `str ReplaceChars(str string, str ReplacePattern)`

- string: uma cadeia de caracteres na qual substituir caracteres.
- ReplacePattern: uma cadeia de caracteres que contém um dicionário com caracteres a substituir.

O formato é {origem1}:{destino1},{origem2}:{destino2},{origemN},{destinoN}, em que a origem é o caractere a localizar e destino é a cadeia de caracteres com a qual trabalhar.


**Comentários:**

- A função considera cada ocorrência de origens definidas e as substitui pelos destinos.
- A origem deve ter exatamente um caractere (unicode).
- A origem não pode ser vazia nem maior que um caractere (erro de análise).
- O destino pode ter vários caracteres, por exemplo, ö:oe, β:ss.
- O destino pode ser vazio, o que indica que o caractere deve ser removido.
- A origem diferencia maiúsculas de minúsculas e deve ser uma correspondência exata.
- A , (vírgula) e : (dois-pontos) são caracteres reservados e não podem ser substituídos usando essa função.
- Espaços e outros caracteres em branco na cadeia de caracteres ReplacePattern são ignorados.


**Exemplo:** <br> '%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o'

`ReplaceChars(”Räksmörgås”,%ReplaceString%)` <br> Retorna Raksmorgas

`ReplaceChars(“O’Neil”,%ReplaceString%)` <br> Retorna "ONeil", o único tique é definido para ser removido.




----------
### Right

**Descrição:** <br> A função Right retorna um número especificado de caracteres do lado direito (final) de uma cadeia de caracteres.

**Sintaxe:** <br> `str Right(str string, num NumChars)`

- string: a cadeia de caracteres da qual retornar caracteres
- NumChars: um número que identifica o número de caracteres a ser retornado do final (direita) da cadeia de caracteres

**Comentários:** <br> Os caracteres de NumChars são retornados desde a última posição da cadeia de caracteres.

Uma cadeia de caracteres que contém os últimos caracteres numChars na cadeia de caracteres:

- Se numChars = 0, retorne a cadeia de caracteres vazia.
- Se numChars < 0, retorne a cadeia de caracteres de entrada.
- Se a cadeia de caracteres for nula, retorne a cadeia de caracteres vazia.

Se a cadeia de caracteres contém menos caracteres do que o número especificado em NumChars, uma cadeia de caracteres idêntica à cadeia de caracteres será retornada.

**Exemplo:** <br> `Right(“John Doe”, 3)` <br> Retorna "Doe".




----------
### RTrim

**Descrição:** <br> A função RTrim remove espaços em branco ao final de uma cadeia de caracteres.

**Sintaxe:** <br> `str RTrim(str value)`

**Exemplo:** <br> `RTrim(“ Test ”)` <br> Retorna "Test".




----------
### Divisão

**Descrição:**<br> A função Split usa uma cadeia de caracteres separada por um delimitador e a transforma em uma cadeia de caracteres de valores múltiplos.


**Sintaxe:** <br> `mvstr Split(str value, str delimiter)` <br? `mvstr Split(str value, str delimiter, num limit)`

- valor: a cadeia de caracteres com um caractere delimitador para separar.
- delimitador: um único caractere a ser usado como o delimitador.
- limite: número máximo de valores a serem retornados.

**Exemplo:** <br> `Split(“SMTP:john.doe@contoso.com,smtp:jd@contoso.com”,”,”)` <br> Retorna uma cadeia de caracteres com valores múltiplos e 2 elementos úteis para o atributo proxyAddress




----------
### StringFromGuid

**Descrição:**<br> A função StringFromGuid usa um GUID binário e o converte em uma cadeia de caracteres

**Sintaxe:** <br> `str StringFromGuid(bin GUID)`




----------
### StringFromSid

**Descrição:** <br> A função StringFromSid converte uma matriz de bytes ou uma matriz de bytes com valores múltiplos que contém um identificador de segurança em uma cadeia de caracteres ou cadeia de caracteres com valores múltiplos.

**Sintaxe:** <br> `str StringFromSid(bin ObjectSID)` <br> `mvstr StringFromSid(mvbin ObjectSID)`




----------
### Switch

**Descrição:**<br> A função Switch é usada para retornar um único valor com base em condições avaliadas.

**Sintaxe:** <br> `var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- expr: expressão variante que você deseja avaliar.
- value: valor a ser retornado se a expressão correspondente for True.

**Comentários:** <br> A lista de argumentos da função Switch consiste em pares de expressões e valores. As expressões são avaliadas da esquerda para a direita e o valor associado à primeira expressão avaliada como True é retornado. Se as partes não tiverem pares adequados, ocorrerá um erro em tempo de execução.

Por exemplo, se expr1 for True, o comutador retornará valor1. Se expr-1 for False, mas expr-2 for True, Switch retorna valor-2 e assim por diante.

Switch retorna um valor nulo se: - nenhuma das expressões é True. - A primeira expressão True tem um valor correspondente que é nulo.

O Switch avalia todas as expressões, mesmo que retorne apenas uma delas. Por essa razão, você deve tomar cuidado com efeitos colaterais indesejáveis. Por exemplo, se a avaliação de qualquer expressão resulta em um erro de divisão por zero, ocorrerá um erro.

Valor também pode ser a função Error, que retornaria uma cadeia de caracteres personalizada.

**Exemplo:** <br> `Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error(“Unknown city”))` <br> Retorna o idioma falado em algumas das maiores cidades; caso contrário, retorna um erro.




----------
### Trim

**Descrição:** <br> A função Trim remove espaços em branco à esquerda e à direita de uma cadeia de caracteres.

**Sintaxe:** <br> `str Trim(str value)` <br> `mvstr Trim(mvstr value)`

**Exemplo:** <br> `Trim(“ Test ”)` <br> Retorna "Test".

`Trim([proxyAddresses])` <br> Remove espaços à direita e à esquerda para cada valor no atributo proxyAddress.




----------
### UCase

**Descrição:** <br> A função UCase converte todos os caracteres de uma cadeia de caracteres em letras maiúsculas.

**Sintaxe:** <br> `str UCase(str string)`

**Exemplo:** <br> `UCase(“TeSt”)` <br> Retorna "TEST".




----------
### Word

**Descrição:**<br> A função Word retorna uma palavra contida em uma cadeia de caracteres com base nos parâmetros que descrevem os delimitadores a serem usados e o número de palavras a serem retornadas.

**Sintaxe:** <br> `str Word(str string, num WordNumber, str delimiters)`

- string: a cadeia de caracteres da qual retornar uma palavra
- WordNumber: um número que identifica o número de palavras que deve ser retornado.
- delimitadores: uma cadeia de caracteres que representa o delimitador(es) que deve ser usado para identificar palavras

**Comentários:** <br> Cada cadeia de caracteres separada por um dos caracteres nos delimitadores na cadeia de caracteres é identificada como palavra:

- Se number < 1, retorna uma cadeia de caracteres vazia.
- Se a cadeia de caracteres for nula, retorna a cadeia de caracteres vazia.

Se a cadeia de caracteres contém um número menor que o número de palavras ou cadeia de caracteres não contém quaisquer palavras identificadas por delimitadores, uma cadeia de caracteres vazia é retornada.


**Exemplo:** <br> `Word(“The quick brown fox”,3,” “)` <br> Retorna "brown"

`Word(“This,string!has&many seperators”,3,”,!&#”)` <br> Retornaria "has"


## Recursos adicionais

* [Noções básicas sobre expressões de provisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Connect Sync: personalizando opções de sincronização](active-directory-aadconnectsync-whatis.md)
* [Integração de suas identidades locais com o Active Directory do Azure](active-directory-aadconnect.md)


<!--Image references-->

<!---HONumber=AcomDC_1203_2015-->