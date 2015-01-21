﻿
1. Faça logon no [Portal de Gerenciamento do Azure], clique em **Serviços Móveis**, clique em seu serviço móvel, em seguida, clique na guia **Painel**.

	![Select your mobile service](./media/mobile-services-enable-source-control/mobile-services-selection.png)

2. (Opcional) Se você já tiver configurado as credenciais de controle do código-fonte de Serviços Móveis ou Websites para sua assinatura do Azure, poderá pular para a etapa 4. Caso contrário, clique em **Configurar o controle do código-fonte** em **Visão rápida** e clique em **Sim** para confirmar.

	![Set up source control](./media/mobile-services-enable-source-control/mobile-setup-source-control.png)


3. Forneça um **Nome de usuário, uma ****Nova senha**, confirme a senha e clique no botão de seleção. 

	![Set source control credentials](./media/mobile-services-enable-source-control/mobile-source-control-credentials.png)

	O repositório Git é criado no seu serviço móvel. Anote as credenciais que acabou de fornecer, pois você as utilizará para acessar esse e outros repositórios de Serviços Móveis em sua assinatura.

4. Clique na guia **Configurar** e observe os campos **Controle do código-fonte**.

	![Configure source control](./media/mobile-services-enable-source-control/mobile-source-control-configure.png)

	A URL do repositório Git é exibida. Você usará essa URL para clonar o repositório para o computador local.

Com o controle do código-fonte habilitado em seu serviço móvel, você poderá usar o Git para clonar o repositório para o computador local.
 

<!--HONumber=35.1-->