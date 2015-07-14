<properties 
	pageTitle="Perguntas Frequentes: Gerenciamento de senha do AD do Azure | Microsoft Azure" 
	description="Perguntas frequentes (FAQ) sobre o gerenciamento de senhas no Azure AD, incluindo redefinição de senha, registro, relatórios e Write-back para o Active Directory local." 
	services="active-directory" 
	documentationCenter="" 
	authors="asteen" 
	manager="kbrint" 
	editor="billmath"/>

<tags 
	ms.service="active-directory" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/08/2015" 
	ms.author="asteen"/>

# Perguntas frequentes sobre gerenciamento de senhas
Abaixo estão algumas perguntas frequentes sobre tudo relativo ao gerenciamento de senhas.

Se você tiver perguntas sem respostas ou estiver procurando ajuda para um problema específico, leia abaixo para ver se já não cobrimos esse tópico. Se nós ainda não fizemos isso, não se preocupe! Fique à vontade para fazer perguntas que não foram abordadas aqui nos [fóruns do Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) e entraremos em contato assim que possível.

Esta seção de perguntas frequentes é dividida nas seguintes seções:

- [**Perguntas sobre o registro de redefinição de senha**](#password-reset-registration)
- [**Perguntas sobre a redefinição de senha**](#password-reset)
- [**Perguntas sobre relatórios de gerenciamento de senha**](#password-management-reports)
- [**Perguntas sobre o write-back de senha**](#password-writeback)

## Registro de redefinição de senha
 - **P: meus usuários podem registrar seus próprios dados de redefinição de senha?**
 
 > **R:** Sim, se a redefinição de senha estiver habilitada e eles forem licenciados, podem ir para o portal de registro de redefinição de senha em http://aka.ms/ssprsetup para registrar as informações de autenticação que serão usadas na redefinição de senha. Os usuários também podem se registrar se conectando ao painel de acesso em http://myapps.microsoft.com, clicando na guia perfil e clicando na opção Registrar-se para a redefinição de senha. Saiba mais sobre como ter os seus usuários configurados para a redefinição de senha lendo Como ter os usuários configurados para a redefinição de senha.
 
 - **P: posso definir dados de redefinição de senha em nome dos meus usuários?**
 
 > **R:** sim, você pode fazer isso com o DirSync ou com o PowerShell ou através do [Portal de Gerenciamento do Azure](https://manage.windowsazure.com) ou do Portal de administração do Office. Saiba mais sobre esse recurso na postagem do blog Privacidade Melhorada para o MFA do AD do Azure e Números de Telefone de Reposição de Senha e lendo Saiba como os dados são usados pela redefinição de senha.
 
 - **P: posso sincronizar dados de perguntas de segurança do local?**
 
 > **R:** não, isso ainda não é possível, mas estamos considerando a hipótese.
 
 - **P: meus usuários podem registrar dados de forma que outros usuários não possam ver esses dados?**
 
 > **R:** sim. Quando os usuários registram dados usando o Portal de registro de redefinição de senha, eles ficam salvos em campos privados de autenticação visíveis apenas por administradores globais e pelo próprio usuário. Saiba mais sobre esse recurso na postagem do blog Privacidade Melhorada para o MFA do AD do Azure e Números de Telefone de Reposição de Senha e lendo Saiba como os dados são usados pela redefinição de senha.
 
 - **P: meus usuários precisam ser registrados antes de poder usar a redefinição de senha?**
 
 > **R:** não. Se você definir informações de autenticação suficientes no nome deles, os usuários não precisarão se registrar. A redefinição de senha funcionará bem desde que você tenha formatado corretamente os dados armazenados nos campos apropriados no diretório. Saiba que mais sobre isso lendo Saiba como os dados são usados na redefinição de senha.

 - **P: posso sincronizar ou definir os campos Telefone de autenticação, Autenticação de email ou Telefone alternativo em nome dos meus usuários?**
 
 > **R:** não no momento, mas estamos considerando habilitar esse recurso.

 - **P: como o portal de registro sabe quais opções mostrar aos meus usuários?**
 
 > **R:** o portal de registro de redefinição de senha só mostra as opções que você tiver ativado para os usuários na seção Política de redefinição de senha do usuário na guia Configurar do seu diretório. Isso significa que, se você não habilitar, digamos, perguntas de segurança, os usuários não poderão se registrar para essa opção.

 - **P: quando um usuário é considerado registrado?**
 
 > **R:** um usuário é registrado quando ele tem no mínimo X informações definidas, em que X é a Quantidade necessária de métodos de autenticação que você definiu no [Portal de Gerenciamento do Azure](https://manage.windowsazure.com). Para saber mais, confira Personalização da política de redefinição de senha de usuário.


## Redefinição de senha

 - **P: quanto tempo deve levar até que eu receba uma chamada telefônica, um SMS ou um email de redefinição de senha?**
 
 > **R:** email, mensagens SMS e chamadas telefônicas devem levar menos de 1 minuto, normalmente sendo de 5 a 20 segundos. Se você não receber a notificação nesse período de tempo, verifique a pasta Lixo eletrônico, se o número/email de contato é o esperado e se os dados de autenticação no diretório estão formatados corretamente. Para saber mais sobre a formatação de números de telefone e de endereços de email para a redefinição de senha, confira Saiba como os dados são usados na redefinição de senha.

 - **P: quais idiomas são compatíveis com a redefinição de senha?**
 
 > **R:** a interface do usuário de redefinição de senha, as mensagens SMS e as chamadas de voz são localizadas no mesmos 40 idiomas com suporte no Office 365. São eles: árabe, búlgaro, chinês simplificado, chinês tradicional, croata, tcheco, dinamarquês, holandês, inglês, estoniano, finlandês, francês, alemão, grego, hebraico, Hindi, húngaro, indonésio, italiano, japonês, cazaque, coreano, letão, lituano, malaio (Malásia), norueguês (Bokmål), polonês, português (Brasil), português (Portugal), romeno, russo, Sérvio (latino), eslovaco, esloveno, espanhol, sueco, tailandês, turco, ucraniano e vietnamita.

 - **P: que partes da experiência de redefinição de senha ficam personalizadas com a marca quando eu definir marcas organizacionais na guia Configurar no meu diretório?**
 
 > **R:** o portal de redefinição de senha mostrará o logotipo da sua organização e também permitirá que você configure o link Entre em contato com o seu administrador para apontar para um email personalizado ou uma URL. Nenhum email enviado pela redefinição de senha incluirá o logotipo da empresa, cores (nesse caso, vermelho), o nome no corpo do email e o nome De personalizado. Confira abaixo um exemplo com todos os elementos com marca. Para saber mais, leia Personalização da aparência de gerenciamento de senha.

  ![][001]

 - **P: como eu instruo os usuários sobre onde acessar para redefinir as suas senhas?**
 
 > **R:** você pode enviar os usuários para https://passwordreset.microsoftonline.com diretamente ou pode instruí-los a clicar no link Não consegue acessar a sua conta encontrado em qualquer tela de entrada com ID corporativo ou de estudante. Sinta-se livre para publicar esses links (ou criar redirecionamentos de URL para eles) em qualquer local que seja facilmente acessível aos usuários.

 - **P: posso usar essa página em um dispositivo móvel?**
 
 > **R:** sim, essa página funciona em dispositivos móveis.

 - **P: há suporte para desbloqueio das contas locais do active directory quando os usuários redefinirem as suas senhas?**
 
 > **R:** sim, quando um usuário redefine a sua senha e o write-back de senha foi implantado com todas as versões do Azure AD Connect ou versões do Azure AD Sync 1.0.0485.0222 ou posterior, a conta de usuário é desbloqueada automaticamente quando o usuário redefine a senha.

 - **P: como posso integrar a redefinição de senha diretamente à experiência de entrada na área de trabalho do usuário?**
 
 > **R:** isso não é possível atualmente. No entanto, se você precisar desse recurso de qualquer maneira e for um cliente Azure AD Premium, poderá instalar o Microsoft Identity Manager sem custo adicional e implantar a solução de redefinição de senha local encontrada para resolver esse requisito.

 - **P: posso definir perguntas de segurança diferentes para diferentes localidades?**
 
 > **R:** não, isso ainda não é possível, mas estamos considerando a hipótese.

 - **P: quantas perguntas podemos configurar para a opção de autenticação com perguntas de segurança?**
 
 > **R:** você pode configurar até 20 perguntas de segurança personalizadas no [Portal de Gerenciamento do Azure](https://manage.windowsazure.com).

 - **P: qual o tamanho máximo das perguntas de segurança?**
 
 > **R:** as perguntas de segurança podem ter entre 3 e 200 caracteres.

 - **P: qual o tamanho máximo das respostas às perguntas de segurança?**
 
 > **R:** as respostas podem ter entre 3 e 40 caracteres.

 - **P: as respostas duplicadas para perguntas de segurança são rejeitadas?**
 
 > **R:** sim, rejeitamos respostas duplicadas para perguntas de segurança.

 - **P: um usuário pode registrar uma pergunta de segurança mais de uma vez?**
 
 > **R:** não, quando um usuário registra uma pergunta específica, ele não pode se registrar para essa pergunta uma segunda vez.

 - **P: é possível definir um limite mínimo de perguntas de segurança para registro e redefinição?**
 
 > **R:** sim, um limite pode ser definido para o registro e outro para a redefinição. Podem ser necessárias de três a cinco perguntas de segurança para o registro e de três a cinco perguntas para a redefinição.

 - **P: se um usuário tiver registrado mais do que o número máximo de perguntas obrigatórias para a redefinição, como as perguntas de segurança são selecionadas durante a redefinição?**
 
 > **R:** X perguntas de segurança são selecionadas aleatoriamente do número total de perguntas que um usuário tiver registrado, em que X é o número mínimo de perguntas necessárias para a redefinição de senha. Por exemplo, se um usuário tem cinco perguntas de segurança registradas, mas apenas três são obrigatórias para a redefinição, três dessas cinco serão selecionadas aleatoriamente e apresentadas ao usuário no momento da redefinição. Se o usuário der respostas erradas, o processo de seleção ocorre novamente para evitar hammering de perguntas.

 - **P: vocês impedem os usuários de tentar redefinir a senha várias vezes em um curto período de tempo?**
 
 > **R:** sim, existem vários recursos de segurança incorporados à redefinição de senha. Os usuários têm somente cinco tentativas de redefinição de senha em uma mesma hora antes de serem bloqueados por 24 horas. Os usuários somente podem tentar validar um número de telefone cinco vezes em uma mesma hora antes de serem bloqueados por 24 horas. Os usuários somente podem experimentar um único método de autenticação cinco vezes em uma mesma hora antes de serem bloqueados por 24 horas.

 - **P: por quanto tempo vale a senha de uso único por email e SMS?**
 
 > **R:** a duração da sessão para a redefinição de senha é de 105 minutos. Isso significa que, desde o início da operação de redefinição de senha, o usuário tem 105 minutos para redefini-la. A senha de uso único por email e SMS perde a validade depois que esse período de tempo expira.


## Relatórios de gerenciamento de senha

 - **P: quanto tempo leva para que os dados sejam exibidos nos relatórios de gerenciamento de senha?**
 
 > **R:** os dados devem ser exibidos nos relatórios de gerenciamento de senha entre 5 e 10 minutos. Em algumas instâncias, pode levar até uma hora para que sejam exibidos.

 - **P: como posso filtrar os relatórios de gerenciamento de senha?**
 
 > **R:** você pode filtrar os relatórios de gerenciamento de senha clicando na lupa pequena na extremidade direita dos rótulos de coluna, próximo ao início do relatório (veja a captura de tela). Se você quiser fazer uma filtragem mais avançada, pode baixar o relatório do Excel e criar uma tabela dinâmica.

  ![][002]

 - **P: qual o período mais antigo coberto pelos relatórios de gerenciamento de senha?**
 
 > **R:** os relatórios de gerenciamento de senha mostram operações que ocorridas nos últimos 30 dias. Estamos investigando como estender isso para um período de tempo mais longo. Por enquanto, se você precisar arquivar esses dados, pode baixar os relatórios periodicamente e salvá-los em um local separado.

 - **P: existe um número máximo de linhas que podem ser exibidas nos relatórios de gerenciamento de senha?**
 
 > **R:** sim, um máximo de mil linhas pode aparecer nos relatórios de gerenciamento de senha, quer sejam mostrados na interface do usuário ou baixados. Estamos investigando como aumentar esse limite.


## Write-back de senha
 - **P: como funciona os bastidores do Write-back de senha?**
 
 > **R:** confira Como funciona o write-back de senha para obter uma explicação detalhada do que acontece quando você habilitar o write-back de senha e como os dados fluem através do sistema de volta ao seu ambiente local. Confira "Modelo de segurança de write-back de senha" em Como funciona o write-back de senha para saber como podemos garantir que o write-back de senha é um serviço altamente seguro.

 - **P: quanto tempo demora para o write-back de senha funcionar? Há um atraso de sincronização como com a sincronização com hash de senha?**
 
 > **R:** o write-back de senha é instantâneo. É um pipeline síncrono que funciona de forma essencialmente diferente da sincronização com hash de senha. O write-back de senha permite aos usuários obter comentários em tempo real sobre o sucesso da operação de redefinição ou de alteração de senha. O tempo médio para um write-back bem-sucedido de uma senha é abaixo de 500 ms.

 - **P: o Write-back de senha funciona com quais tipos de conta?**
 
 > **R:** o Write-back de senha funciona com usuários federados e sincronizados com hash de senha.

 - **P: o Write-back de senha impõe as políticas de senha do meu domínio?**
 
 > **R:** sim, o Write-back de senha impõe a duração da senha, o histórico, a complexidade, os filtros e qualquer outra restrição que você possa ter sobre as senhas no domínio local.

 - **P: o Write-back de senha é seguro? Como posso ter certeza de que não serei invadido por um hacker?**
 
 > **R:** Sim, o Write-back de senha é muito seguro. Para ler mais sobre as 4 camadas de segurança implementadas pelo serviço de Write-back de senha, confira o "modelo de segurança de write-back de senha" em Como funciona o Write-back de senha


<br/> <br/> <br/>

**Recursos adicionais**


* [O que é gerenciamento de senhas](active-directory-passwords.md)
* [Como funciona o gerenciamento de senhas](active-directory-passwords-how-it-works.md)
* [Introdução ao gerenciamento de senhas](active-directory-passwords-getting-started.md)
* [Personalizar o gerenciamento de senhas](active-directory-passwords-customize.md)
* [Práticas recomendadas de gerenciamento de senhas](active-directory-passwords-best-practices.md)
* [Como obter percepções operacionais com relatórios de gerenciamento de senhas](active-directory-passwords-get-insights.md)
* [Solucionar problemas do Gerenciamento de Senhas](active-directory-passwords-troubleshoot.md)
* [Saiba mais](active-directory-passwords-learn-more.md)
* [Gerenciamento de senhas no MSDN](https://msdn.microsoft.com/library/azure/dn510386.aspx)


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
 

<!---HONumber=62-->