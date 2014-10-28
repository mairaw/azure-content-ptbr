Estas etapas criam um novo [ApiController][ApiController] personalizado, que envia notificações por push ao aplicativo. Você pode implementar o mesmo código em um [TableController][TableController] ou em qualquer lugar nos seus serviços de back-end.

1.  No Gerenciador de Soluções do Visual Studio, clique com o botão direito do mouse na pasta Controladores do projeto de serviço móvel, expanda **Adicionar** e clique em **Novo Item Scaffold**.

    Isso exibe a caixa de diálogo Adicionar Scaffold.

2.  Expanda **Serviços Móveis do Azure** e clique em **Controlador Personalizado de Serviços Móveis do Azure**, em **Adicionar**, forneça um **Nome do controlador** de `NotifyAllUsersController` e clique em **Adicionar** novamente.

    ![Caixa de diálogo API da Web adiciona Scaffold.][Caixa de diálogo API da Web adiciona Scaffold.]

    Isso cria uma nova classe de controlador vazia chamada **NotifyAllUsersController**.

3.  No novo arquivo de projeto NotifyAllUsersController.cs, adicione as seguintes instruções **usando**:

        using Newtonsoft.Json.Linq;
        using System.Threading.Tasks;

4.  Adicione o seguinte código que implementa o método POST no controlador:

        public async Task<bool> Post(JObject data)
        {
            try
            {
                // Define the XML paylod for a WNS native toast notification 
                // that contains the value supplied in the POST request.
                string wnsToast = 
                    string.Format("<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<toast><visual><binding template=\"ToastText01\">" + 
                    "<text id=\"1\">{0}</text></binding></visual></toast>", 
                    data.GetValue("toast").Value<string>());

                // Define the WNS native toast with the payload string.
                WindowsPushMessage message = new WindowsPushMessage();
                message.XmlPayload = wnsToast;

                // Send the toast notification.
                await Services.Push.SendAsync(message);
                return true;
            }
            catch (Exception e)
            {
                Services.Log.Error(e.ToString());
            }
            return false;
        }

    > [WACOM.NOTE]este método POST pode ser chamado por qualquer cliente que tem a chave do aplicativo, o que não é seguro. Para garantir o pontod e extremidade, aplique o atributo `[AuthorizeLevel(AuthorizationLevel.User)]` ao método ou classe para requerer autenticação.

  [ApiController]: http://go.microsoft.com/fwlink/p/?LinkId=512673
  [TableController]: http://msdn.microsoft.com/pt-br/library/azure/dn643359.aspx
  [Caixa de diálogo API da Web adiciona Scaffold.]: ./media/mobile-services-dotnet-backend-update-server-push-vs2013/add-custom-api-controller.png