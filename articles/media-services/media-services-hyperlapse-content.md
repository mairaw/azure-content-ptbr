<properties
	pageTitle="Arquivos de mídia Hyperlapse com o Azure Media Hyperlapse"
	description="O Azure Media Hyperlapse cria vídeos suaves de lapso de tempo de conteúdo de primeira pessoa ou de uma câmera de ação. Este tópico mostra como usar o indexador de mídia."
	services="media-services"
	documentationCenter=""
	authors="asolanki"
	manager="johndeu"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="10/19/2015"   
	ms.author="adsolank"/>


# Arquivos de mídia Hyperlapse com o Azure Media Hyperlapse

O Azure Media Hyperlapse é uma MP (mídia de processador) que cria vídeos suaves de lapso de tempo de conteúdo de primeira pessoa ou de uma câmera de ação. O equivalente baseado em nuvem do [Hyperlapse Pro da Microsoft Research para a área de trabalho e do Hyperlapse Mobile baseado em celular](http://aka.ms/hyperlapse), o Microsoft Hyperlapse para os Serviços de Mídia do Azure utiliza a escala em massa da plataforma de Processamento de Mídia dos Serviços de Mídia do Azure para dimensionar horizontalmente e executar em paralelo o processamento em massa do Hyperlapse.

>[AZURE.IMPORTANT]O Microsoft Hyperlapse para os Serviços de Mídia do Azure está em um estado de visualização pública gratuita. Os trabalhos são limitados aos primeiros 10.000 quadros do ativo de entrada.


> O Microsoft Hyperlapse foi projetado para funcionar melhor em conteúdo de primeira pessoa com uma câmera em movimento. Embora a filmagem de imagens com câmera fotográfica ainda possa funcionar, o desempenho e a qualidade do Processador de Mídia do Azure Media Hyperlapse não podem ser garantidos para outros tipos de conteúdo. Para saber mais sobre o Microsoft Hyperlapse para os Serviços de Mídia do Azure e ver alguns vídeos de exemplo, confira a [postagem de blog introdutória](http://aka.ms/azurehyperlapseblog) da visualização pública.

Um trabalho do Azure Media Hyperlapse utiliza como entrada um arquivo de ativo MP4, MOV ou WMV junto com um arquivo de configuração que especifica quais quadros de vídeo devem ser de lapso de tempo e com qual velocidade (por exemplo, os primeiros 10.000 quadros em 2x). A saída é uma representação estabilizada e de lapso de tempo do vídeo de entrada.

Para as atualizações mais recentes do Azure Media Hyperlapse, consulte [blogs dos serviços de mídia](http://azure.microsoft.com/blog/topics/media-services/).

## Hyperlapse de um ativo

Primeiro, você precisará carregar o arquivo de entrada desejado nos Serviços de Mídia do Azure. Para saber mais sobre os conceitos envolvidos no carregamento e gerenciamento de conteúdo, leia o [artigo sobre gerenciamento de conteúdo](media-services-manage-content.md#upload).

###  <a id="configuration"></a>Predefinição de configuração para Hyperlapse

Quando o conteúdo estiver em sua conta de Serviços de Mídia, você precisará construir a predefinição de configuração. A tabela a seguir explica os campos especificados pelo usuário:

 Campo | Descrição
-------|-------------
StartFrame|O quadro no qual o processamento do Microsoft Hyperlapse deve começar.
NumFrames|O número de quadros a serem processados
Velocidade|O fator de acordo com o qual acelerar o vídeo de entrada.

A seguir há um exemplo de um arquivo de configuração compatível em XML e JSON:

**Predefinição XML:**

	<?xml version="1.0" encoding="utf-16"?>
	<Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
		<Sources>
			<Source StartFrame="0" NumFrames="10000" />
		</Sources>
		<Options>
			<Speed>12</Speed>
		</Options>
	</Preset>

**Predefinição JSON:**

	{
		"Version":1.0,
		"Sources": [
			{
				"StartFrame":0,
				"NumFrames":2147483647
			}
		],
		"Options": {
			"Speed":1,
			"Stabilize":false
		}
	}

###  <a id="sample_code"></a> Microsoft Hyperlapse com o SDK do .NET AMS

O método a seguir carrega um arquivo de mídia como um ativo e cria um trabalho com o Processador de Mídia do Azure Media Hyperlapse.

> [AZURE.NOTE]Você já deve ter um CloudMediaContext no escopo com o nome "contexto" para que esse código funcione. Para saber mais sobre isso, leia o [artigo sobre gerenciamento de conteúdo](media-services-manage-content.md).

> [AZURE.NOTE]O argumento de cadeia de caracteres "hyperConfig" deve ser uma configuração compatível predefinida em JSON ou XML, conforme descrito acima.

	static bool RunHyperlapseJob(string input, string output, string hyperConfig)
	{
		// create asset with input file
		IAsset asset = context
					   .Assets
					   .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

		// grab instances of Azure Media Hyperlapse MP
		IMediaProcessor mp = context
							 .MediaProcessors
							 .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

		// create Job with Hyperlapse task
		IJob job = context
				   .Jobs
				   .Create(String.Format("Hyperlapse {0}", input));

		if (String.IsNullOrEmpty(hyperConfig))
		{
			// config cannot be empty
			return false;
		}

		hyperConfig = File.ReadAllText(hyperConfig);

		ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
												mp,
												hyperConfig,
												TaskOptions.None);
		hyperlapseTask.InputAssets.Add(asset);
		hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
											AssetCreationOptions.None);


		job.Submit();

		// Create progress printing and querying tasks
			Task progressPrintTask = new Task(() =>
			{

				IJob jobQuery = null;
				do
				{
					var progressContext = context;
					jobQuery = progressContext.Jobs
											  .Where(j => j.Id == job.Id)
											  .First();
					Console.WriteLine(string.Format("{0}\t{1}\t{2}",
									  DateTime.Now,
									  jobQuery.State,
									  jobQuery.Tasks[0].Progress));
					Thread.Sleep(10000);
				}
				while (jobQuery.State != JobState.Finished &&
					   jobQuery.State != JobState.Error &&
					   jobQuery.State != JobState.Canceled);
			});
			progressPrintTask.Start();

			Task progressJobTask = job.GetExecutionProgressTask(
												 CancellationToken.None);
			progressJobTask.Wait();

			// If job state is Error, the event handling
			// method for job progress should log errors.  Here we check
			// for error state and exit if needed.
			if (job.State == JobState.Error)
			{
				ErrorDetail error = job.Tasks.First().ErrorDetails.First();
				Console.WriteLine(string.Format("Error: {0}. {1}",
												error.Code,
												error.Message));  
				return false;                  
			}

		DownloadAsset(job.OutputMediaAssets.First(), output);
		return true;
	}

	static void DownloadAsset(IAsset asset, string outputDirectory)
	{
		foreach (IAssetFile file in asset.AssetFiles)
		{
			file.Download(Path.Combine(outputDirectory, file.Name));
		}
	}


	static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
	{
	    IAsset asset = context.Assets.Create(assetName, options);

	    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
	    assetFile.Upload(filePath);

	    return asset;
	}

	static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
	{
	    var processor = context.MediaProcessors
	    .Where(p => p.Name == mediaProcessorName)
	    .ToList()
	    .OrderBy(p => new Version(p.Version))
	    .LastOrDefault();

	    if (processor == null)
	        throw new ArgumentException(string.Format("Unknown media processor",
	                                                   mediaProcessorName));

	    return processor;
	}

### <a id="file_types"></a>Tipos de arquivo com suporte

- MP4
- MOV
- WMV



##Roteiros de aprendizagem dos Serviços de Mídia

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornecer comentários

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->

<!---HONumber=Nov15_HO3-->