<properties
   pageTitle="Conectar um dispositivo usando C no mbed | Microsoft Azure"
   description="Descreve como conectar um dispositivo à solução pré-configurada de monitoramento remoto do Azure IoT Suite usando um aplicativo escrito em C em um dispositivo mbed."
   services=""
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="na"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/10/2015"
   ms.author="dobett"/>


# Conectar seu dispositivo à solução pré-configurada de monitoramento remoto do IoT Suite

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## Compilar e executar a solução de exemplo do C no mbed

As instruções a seguir descrevem as etapas para conectar um dispositivo [Freescale FRDM-K64F habilitado para mbed][lnk-mbed-home] para a solução de monitoramento remoto.

### Conecte o dispositivo ao computador e à rede

1. Conecte o dispositivo mbed à sua rede usando um cabo Ethernet. Essa etapa é necessária porque o aplicativo de exemplo requer acesso à internet.

2. Consulte [Introdução ao mbed][lnk-mbed-getstarted] para conectar o dispositivo mbed ao seu computador.

3. Se o seu computador executar o Windows, consulte [Configuração de PC][lnk-mbed-pcconnect] para configurar o acesso de porta serial ao dispositivo mbed.

### Crie um projeto mbed e importe o código de exemplo

1. No navegador da Web, vá até o [site de desenvolvedor](https://developer.mbed.org/) mbed.org. Se você ainda não se inscreveu, verá uma opção para criar uma nova conta (ele é gratuito). Caso contrário, faça logon com suas credenciais de conta. Em seguida, clique em **Compilador** no canto superior direito da página. Isso deve levá-lo até a interface de gerenciamento do espaço de trabalho.

2. Verifique se a plataforma de hardware que você está usando é exibida no canto superior direito da janela ou clique no ícone no canto superior direito para selecionar a plataforma de hardware.

3. Clique em **Importar** no menu principal. Clique em **Clique aqui** para importar do link de URL ao lado do logotipo de globo mbed.

    ![][6]

4. Na janela pop-up, insira o link para o código de exemplo https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/

    ![][7]

5. Você pode ver no compilador mbed que a importação desse projeto importou várias bibliotecas. Algumas são fornecidas e mantidas pela equipe do Azure IoT ([azureiot\_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub\_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub\_amqp\_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [iothub\_http\_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_http_transport/), [proton-c-mbed](https://developer.mbed.org/users/AzureIoTClient/code/proton-c-mbed/)), enquanto outras são bibliotecas de terceiros disponíveis no catálogo de bibliotecas mbed.

    ![][8]

6. Abra remote\_monitoring\\remote\_monitoring.c e localize o seguinte código no arquivo:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Substitua [Id do dispositivo], [chave do dispositivo] com os dados do dispositivo.

8. Use os dados do dispositivo Nomedohost do Hub IoT para preencher o nome e o sufixo IoTHub. Por exemplo, se seu nome de host do Hub IoT for Contoso.azure-devices.net, Contoso é o nome IoTHub e tudo depois dele é o sufixo:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "Contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### Compile e execute o programa

1. Clique em **Compilar** para criar o programa. Você pode ignorar os avisos com segurança, mas se a compilação gerar erros, corrija-os antes de continuar.

2. Se a compilação for bem-sucedida, um arquivo .bin será gerado com o nome do seu projeto. Copie o arquivo .bin para o dispositivo. Salvar o arquivo .bin no dispositivo faz com que a sessão de terminal atual para o dispositivo seja redefinida. Ao se reconectar, redefina o terminal novamente de forma manual ou inicie um novo terminal. Isso permite que o dispositivo mbed seja redefinido e inicie a execução do programa.

3. Conecte-se ao dispositivo usando um aplicativo de cliente SSH, como o PuTTY. Você pode determinar qual porta serial o dispositivo usa verificando o Gerenciador de Dispositivos do Windows:


4. Em PuTTY, clique no tipo de conexão **Serial**. O dispositivo provavelmente se conectará em 115200; portanto, insira esse valor na caixa **Velocidade**. Em seguida, clique em **Abrir**:

    ![][11]

5. O programa inicia a execução. Talvez seja necessário redefinir a placa (pressione CTRL+Break ou pressione o botão de redefinição da placa) se o programa não iniciar automaticamente quando você se conectar.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration

<!---HONumber=Nov15_HO4-->