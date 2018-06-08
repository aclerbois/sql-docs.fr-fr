---
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0d6f3ef84c6df07abf30cef16102ef13277bff47
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="log-properties"></a>Propriétés du journal
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge les propriétés de serveur de journal répertoriées dans les tableaux suivants. Pour plus d’informations sur les autres propriétés de serveur et sur la façon de les configurer, consultez [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md).  
  
## <a name="general"></a>Général  
 **Fichier**  
 Propriété de type chaîne qui spécifie le nom du fichier journal du serveur. Cette propriété s'applique uniquement si vous utilisez un fichier sur disque pour l'enregistrement, au lieu d'une table de base de données (comportement par défaut).  
  
 La valeur par défaut de cette propriété est msmdsrv.log.  
  
 **FileBufferSize**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **MessageLogs**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="error-log"></a>Journal des erreurs de SQL Server  
 Vous pouvez définir ces propriétés au niveau de l'instance du serveur pour modifier les valeurs par défaut de la configuration d'erreur qui s'affichent dans d'autres outils et concepteurs. Consultez [Configuration d’erreur pour le Cube, la Partition et le traitement de Dimension &#40;SSAS - multidimensionnel&#41; ](../../analysis-services/multidimensional-models/error-configuration-for-cube-partition-and-dimension-processing.md) et <xref:Microsoft.AnalysisServices.MiningStructure.ErrorConfiguration%2A> pour plus d’informations.  
  
 **ErrorLog\ErrorLogFileName**  
 Propriété utilisée comme valeur par défaut durant une opération de traitement effectuée par le serveur.  
  
 **ErrorLog\ErrorLogFileSize**  
 Propriété utilisée comme valeur par défaut durant une opération de traitement effectuée par le serveur.  
  
 **ErrorLog\KeyErrorAction**  
 Spécifie l’action entreprise par le serveur quand une erreur **KeyNotFound** se produit. Les réponses valides à cette erreur sont :  
  
-   **ConvertToUnknown** indique au serveur d'allouer la valeur de clé d'erreur au membre inconnu.  
  
-   **DiscardRecord** indique au serveur d'exclure l'enregistrement.  
  
 **ErrorLog\KeyErrorLogFile**  
 Il s'agit d'un nom de fichier défini par l'utilisateur qui doit avoir une extension de fichier .log, situé dans un dossier sur lequel le compte de services dispose des autorisations d'accès en lecture/écriture. Ce fichier journal contient uniquement les erreurs générées lors du traitement. Utilisez la Boîte noire si vous avez besoin de plus d'informations.  
  
 **ErrorLog\KeyErrorLimit**  
 Il s'agit du nombre maximal d'erreurs d'intégrité des données que le serveur autorise avant que le traitement échoue. Une valeur égale à -1 indique un nombre illimité. La valeur par défaut est 0, ce qui signifie que le traitement s'arrête après la première erreur. Vous pouvez également définir un nombre entier.  
  
 **ErrorLog\KeyErrorLimitAction**  
 Spécifie l'action entreprise par le serveur lorsque le nombre maximal d'erreurs de clé est atteint. Les réponses valides à cette action sont :  
  
-   **StopProcessing** indique au serveur d'arrêter le traitement lorsque le nombre maximal d'erreurs est atteint.  
  
-   **StopLogging** indique au serveur d'arrêter l'enregistrement des erreurs lorsque le nombre maximal d'erreurs est atteint, mais d'autoriser la poursuite du traitement.  
  
 **ErrorLog\ LogErrorTypes\KeyNotFound**  
 Spécifie l’action entreprise par le serveur quand une erreur **KeyNotFound** se produit. Les réponses valides à cette erreur sont :  
  
-   **IgnoreError** indique au serveur de poursuivre le traitement sans enregistrer l'erreur ou la compter dans le nombre maximal d'erreurs de clé. Lorsque vous ignorez l'erreur, vous permettez au traitement de continuer sans ajouter l'erreur au nombre d'erreurs ou l'enregistrer à l'écran ou dans le fichier journal. L'enregistrement en question rencontre un problème d'intégrité des données et ne peut pas être ajouté à la base de données. L'enregistrement sera ignoré ou agrégé dans le membre inconnu, tel que le détermine la propriété **KeyErrorAction** .  
  
-   **ReportAndContinue** indique au serveur d'enregistrer l'erreur, de la compter dans le nombre maximal d'erreurs de clé et de continuer le traitement. L'enregistrement déclenchant l'erreur est ignoré ou converti en membre inconnu.  
  
-   **ReportAndStop** indique au serveur d'enregistrer l'erreur et d'arrêter le traitement immédiatement, quel que soit le nombre maximal d'erreurs de clé. L'enregistrement déclenchant l'erreur est ignoré ou converti en membre inconnu.  
  
 **ErrorLog\ LogErrorTypes\KeyDuplicate**  
 Spécifie l'action entreprise par le serveur lorsqu'une clé dupliquée est détectée. Les valeurs valides sont **IgnoreError** pour continuer le traitement comme si l'erreur ne s'était pas produite, **ReportAndContinue** pour enregistrer l'erreur et continuer le traitement et **ReportAndStop** pour enregistrer l'erreur et arrêter le traitement immédiatement, même si le nombre d'erreurs est inférieur au nombre maximal.  
  
 **ErrorLog\ LogErrorTypes\NullKeyConvertedToUnknown**  
 Spécifie l'action entreprise par le serveur lorsqu'une clé NULL a été convertie en membre inconnu. Les valeurs valides sont **IgnoreError** pour continuer le traitement comme si l'erreur ne s'était pas produite, **ReportAndContinue** pour enregistrer l'erreur et continuer le traitement et **ReportAndStop** pour enregistrer l'erreur et arrêter le traitement immédiatement, même si le nombre d'erreurs est inférieur au nombre maximal.  
  
 **ErrorLog\ LogErrorTypes\NullKeyNotAllowed**  
 Spécifie l’action effectuée par le serveur quand **NullProcessing** a la valeur **Error** pour un attribut de dimension. Une erreur est générée lorsqu'une valeur NULL n'est pas autorisée dans un attribut donné. Cette propriété de configuration d'erreur informe l'étape suivante, qui consiste à créer un rapport d'erreurs et poursuivre le traitement tant que le nombre maximal d'erreurs n'est pas atteint. Les valeurs valides sont **IgnoreError** pour continuer le traitement comme si l'erreur ne s'était pas produite, **ReportAndContinue** pour enregistrer l'erreur et continuer le traitement et **ReportAndStop** pour enregistrer l'erreur et arrêter le traitement immédiatement, même si le nombre d'erreurs est inférieur au nombre maximal.  
  
 **ErrorLog\ LogErrorTypes\CalculationError**  
 Propriété utilisée comme valeur par défaut durant une opération de traitement effectuée par le serveur.  
  
 **ErrorLog\IgnoreDataTruncation**  
 Propriété utilisée comme valeur par défaut durant une opération de traitement effectuée par le serveur.  
  
## <a name="exception"></a>Exception  
 **Exception\CreateAndSendCrashReports**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Exception\CrashReportsFolder**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Exception\SQLDumperFlagsOn**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Exception\SQLDumperFlagsOff**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Exception\MiniDumpFlagsOn**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Exception\MinidumpErrorList**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="flight-recorder"></a>Boîte noire SQL  
 **FlightRecorder\Enabled**  
 Propriété booléenne qui indique si la fonctionnalité de boîte noire SQL est activée.  
  
 **FlightRecorder\FileSizeMB**  
 Propriété dont la valeur est un entier 32 bits signé qui définit la taille du fichier sur disque de la boîte noire SQL, exprimée en mégaoctets.  
  
 **FlightRecorder\LogDurationSec**  
 Propriété dont la valeur est un entier 32 bits signé qui définit la fréquence en secondes selon laquelle l'enregistrement reprend au début dans la boîte noire SQL.  
  
 **FlightRecorder\SnapshotDefinitionFile**  
 Propriété de type chaîne qui spécifie le nom du fichier de définition d'instantané, qui contient les commandes de découverte envoyées au serveur lorsqu'un instantané est réalisé.  
  
 La valeur par défaut de cette propriété est vide, ce qui correspond au nom de fichier par défaut FlightRecorderSnapshotDef.txt.  
  
 **FlightRecorder\SnapshotFrequencySec**  
 Propriété dont la valeur est un entier 32 bits signé qui définit la fréquence en secondes des instantanés.  
  
 **FlightRecorder\TraceDefinitionFile**  
 Propriété de type chaîne qui spécifie le nom du fichier de définition de trace de la boîte noire SQL.  
  
 La valeur par défaut de cette propriété est vide, ce qui correspond au nom de fichier par défaut FlightRecorderTraceDef.txt.  
  
## <a name="query-log"></a>Journal des requêtes  
 **S'applique à :** Mode serveur multidimensionnel uniquement  
  
 **QueryLog\QueryLogFileName**  
 Propriété de type chaîne qui spécifie le nom du fichier journal des requêtes. Cette propriété s'applique uniquement si vous utilisez un fichier sur disque pour l'enregistrement, au lieu d'une table de base de données (comportement par défaut).  
  
 **QueryLog\QueryLogSampling**  
 Propriété dont la valeur est un entier 32 bits signé qui spécifie le taux d'échantillonnage du journal des requêtes.  
  
 La valeur par défaut de cette propriété, 10, signifie qu'une requête sur 10 est enregistrée.  
  
 **QueryLog\QueryLogFileSize**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **QueryLog\QueryLogConnectionString**  
 Propriété de type chaîne qui spécifie la connexion à la base de données du journal des requêtes.  
  
 **QueryLog\QueryLogTableName**  
 Propriété de type chaîne qui spécifie le nom de la table du journal des requêtes.  
  
 La valeur par défaut de cette propriété est OlapQueryLog.  
  
 **QueryLog\CreateQueryLogTable**  
 Propriété booléenne qui spécifie si la table du journal des requêtes doit être créée.  
  
 La valeur par défaut de cette propriété, False, indique que ne serveur ne crée pas automatiquement la table du journal et n'enregistre pas d'événements de requête.  
  
> [!NOTE]  
>  Pour plus d’informations sur la configuration du journal des requêtes, consultez [Configuration du journal des requêtes Analysis Services](http://go.microsoft.com/fwlink/?LinkId=81890).  
  
## <a name="trace"></a>Trace  
 **Trace\TraceBackgroundDistributionPeriod**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceBackgroundFlushPeriod**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceFileBufferSize**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceFileWriteTrailerPeriod**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceMaxRowsetSize**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceProtocolTraffic**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceQueryResponseTextChunkSize**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceReportFQDN**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceRequestParameters**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Trace\TraceRowsetBackgroundFlushPeriod**  
 Propriété avancée que vous ne devez pas modifier, sauf si vous bénéficiez de l'assistance du support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md)   
 [Déterminer le mode serveur d’une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md)  
  
  
