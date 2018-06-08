---
title: Journal des traces du service Report Server | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- logs [Reporting Services], trace
- traces [Reporting Services]
- system information [Reporting Services]
- versions [Reporting Services]
ms.assetid: 2fde08b2-137d-4f4b-88e5-216030216e0d
caps.latest.revision: 52
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: df621b94f8fecf5587cead165b88875b8d7dd4c2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-server-service-trace-log"></a>Report Server Service Trace Log
  Les journaux des traces du serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sont des fichiers texte ASCII qui contiennent des informations détaillées sur les opérations du service Report Server.  Ce fichier contient des informations telles que les opérations effectuées par le service web Report Server, le portail web et le traitement en arrière-plan. Le fichier journal des traces comprend des informations redondantes qui sont consignées dans d'autres fichiers journaux, ainsi que des informations qui ne se trouvent nulle part ailleurs. Les informations du journal des traces sont utiles si vous déboguez une application qui comprend un serveur de rapports, ou si vous essayez de déterminer l’origine d’un problème consigné dans le journal des événements ou le journal des exécutions. Par exemple, lors de la résolution des problèmes liés aux abonnements.  
 
##  <a name="bkmk_view_log"></a> Où se trouvent les fichiers journaux de Report Server ?  
 Les fichiers journaux des traces sont `ReportServerService_<timestamp>.log` et `Microsoft.ReportingServices.Portal.WebHost_<timestamp>.log`, et se trouvent dans le dossier suivant :  
  
 `C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\LogFiles`  
  
 Les journaux des traces sont créés quotidiennement, à partir de la première entrée ayant lieu après minuit (heure locale), et chaque fois que le service redémarre. L'horodateur est basé sur l'heure UTC (Coordinated Universal Time). Le fichier est au format EN-US. Par défaut, les journaux des traces sont limités à 32 mégaoctets et par défaut, ils sont supprimés après 14 jours.  
  
 Visualiser une courte vidéo qui montre l'utilisation de Microsoft Power Query pour afficher des fichiers journaux [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
[![consultez une vidéo ilustrant Power Query et les fichiers journaux ssrs](../../reporting-services/report-server/media/generic-video-thumbnail.png)](https://technet.microsoft.com/library/sql-server-reporting-services-log-files-and-microsoft-power-query.aspx)  [Utiliser Microsoft Power Query pour afficher les fichiers journaux de Reporting Services](https://technet.microsoft.com/library/sql-server-reporting-services-log-files-and-microsoft-power-query.aspx)
  
##  <a name="bkmk_trace_configuration_settings"></a> Paramètres de configuration des traces  
 Le comportement du journal des traces est géré dans le fichier de configuration **ReportingServicesService.exe.config**. Le fichier de configuration se trouve dans le chemin d'accès de dossier suivant :  
  
 `\Program Files\Microsoft SQL Server\MSRS13.<instance name>\Reporting Services\ReportServer\bin`.  
  
 L’exemple suivant illustre la structure XML des paramètres **RStrace** . La valeur de **DefaultTraceSwitch** détermine le type d’information qui est ajouté au journal. À l’exception de l’attribut **Components** , les valeurs de **RStrace** sont identiques dans tous les fichiers de configuration.  
  
```  
  \<system.diagnostics>
    <switches>
      <add name="DefaultTraceSwitch" value="3" />
    </switches>
  \</system.diagnostics>
  <RStrace>
    <add name="FileName" value="ReportServerService_" />
    <add name="FileSizeLimitMb" value="32" />
    <add name="KeepFilesForDays" value="14" />
    <add name="Prefix" value="appdomain, tid, time" />
    <add name="TraceListeners" value="file" />
    <add name="TraceFileMode" value="unique" />
    <add name="Components" value="all:3" />
  </RStrace>
```  
  
 Le tableau suivant fournit des informations sur chaque paramètre.  
  
|Paramètre|Description|Valeurs|  
|-------------|-----------------|------------|  
|**RStrace**|Spécifie les espaces de noms utilisés pour les erreurs et la trace.||  
|**DefaultTraceSwitch**|Spécifie le niveau des informations consignées dans le journal de trace de ReportServerService. Chaque niveau comprend les informations signalées par tous les niveaux inférieurs. La désactivation de la trace n'est pas recommandée.|Les valeurs valides sont :<br /><br /> <br /><br /> 0 = Trace désactivée. Le fichier journal ReportServerService est activé par défaut. Pour le désactiver, définissez le niveau de trace à 0.<br /><br /> 1= Exceptions et redémarrages<br /><br /> 2= Exceptions, redémarrages, avertissements<br /><br /> 3= Exceptions, redémarrages, avertissements, messages d'état (par défaut)<br /><br /> 4= Mode commenté|  
|**FileName**|Spécifie la première partie du nom du fichier journal. La valeur spécifiée par **Prefix** complète le reste du nom.||  
|**FileSizeLimitMb**|Spécifie une taille maximale pour le journal de trace. La taille du fichier est exprimée en mégaoctets.<br /><br /> Vous pouvez surveiller la taille du fichier en définissant des niveaux de trace (de 0 à 4) pour contrôler la quantité de contenu enregistrée. Vous pouvez également spécifier les composants faisant l'objet d'une trace. Si la taille maximale du fichier journal est atteinte avant le délai d'expiration de 14 jours, les entrées les plus anciennes sont remplacées par les nouvelles entrées.|Les valeurs valides vont de 0 à un entier maximal. La valeur par défaut est 32. Si vous spécifiez 0 ou un nombre négatif, le serveur de rapports considère que la valeur est égale à 1.|  
|**KeepFilesForDays**|Spécifie le nombre de jours après lequel supprimer un journal de trace.|Les valeurs valides vont de 0 à un entier maximal. La valeur par défaut est 14. Si vous spécifiez 0 ou un nombre négatif, le serveur de rapports considère que la valeur est égale à 1.|  
|**Prefix**|Spécifie une valeur générée qui distingue une instance de journal d'une autre.|Par défaut, des valeurs d'horodatage sont ajoutées aux noms des journaux de trace. Cette valeur est définie sur « appdomain, tid, time ». Ne modifiez pas ce paramètre.|  
|**TraceListeners**|Spécifie une cible de sortie du contenu du journal de trace. Vous pouvez spécifier plusieurs cibles ; dans ce cas, utilisez la virgule comme séparateur.|Les valeurs valides sont :<br /><br /> <br /><br /> DebugWindow<br /><br /> File (par défaut)<br /><br /> StdOut|  
|**TraceFileMode**|Spécifie si les journaux de trace contiennent des données pour une période de 24 heures. Un seul journal de trace doit exister par composant et par jour.|Cette valeur est définie sur « Unique » (par défaut). Ne modifiez pas cette valeur.|  
|**Catégorie de composant**|Spécifie les composants pour lesquels les informations du journal des traces sont générées, ainsi que le niveau des traces, dans le format suivant :<br /><br /> \<catégorie de composant>:\<tracelevel><br /><br /> Vous pouvez spécifier l’ensemble ou une partie des composants (**all**, **RunningJobs**, **SemanticQueryEngine**, **SemanticModelGenerator**). Si vous ne voulez pas générer les informations relatives à un composant spécifique, désactivez les traces de ce composant (par exemple, « SemanticModelGenerator:0 »). Ne désactivez pas le suivi pour le composant **all**.<br /><br /> Vous pouvez définir « SemanticQueryEngine:4 » si vous voulez afficher les instructions Transact-SQL qui sont générées pour chaque requête sémantique. Les instructions Transact-SQL sont enregistrées dans le journal des traces. L'exemple suivant illustre le paramètre de configuration qui ajoute les instructions Transact-SQL au journal :<br /><br /> \<add name="Components" value="all,SemanticQueryEngine:4" />|Catégories de composants pouvant être définies :<br /><br /> <br /><br /> Le composant**All** est utilisé pour effectuer le suivi de l’activité générale du serveur de rapports pour tous les processus qui ne se retrouvent pas dans les catégories spécifiques.<br /><br /> Le composant**RunningJobs** sert à effectuer le suivi d’une opération de rapport ou d’abonnement en cours.<br /><br /> Le composant**SemanticQueryEngine** sert à effectuer le suivi d’une requête sémantique qui est traitée quand un utilisateur effectue une exploration de données ad hoc dans un rapport basé sur un modèle.<br /><br /> Le composant**SemanticModelGenerator** est utilisé pour effectuer le suivi de la génération de modèle.<br /><br /> Le composant**http** sert à activer le fichier journal HTTP Report Server. Pour plus d'informations, consultez [Report Server HTTP Log](../../reporting-services/report-server/report-server-http-log.md).|  
|Valeur**tracelevel** pour les catégories de composants|\<catégorie de composant>:\<tracelevel><br /><br /> <br /><br /> Si vous n’ajoutez pas de niveau de suivi au composant, la valeur spécifiée pour **DefaultTraceSwitch** est utilisée. Par exemple, si vous spécifiez « all,RunningJobs,SemanticQueryEngine,SemanticModelGenerator », tous les composants utilisent le niveau des traces par défaut.|Valeurs valides pour le niveau de trace :<br /><br /> <br /><br /> 0= Trace désactivée<br /><br /> 1= Exceptions et redémarrages<br /><br /> 2= Exceptions, redémarrages, avertissements<br /><br /> 3= Exceptions, redémarrages, avertissements, messages d'état (par défaut)<br /><br /> 4= Mode commenté<br /><br /> La valeur par défaut pour Report Server est : « all:3 ».|  
  
##  <a name="bkmk_add_custom"></a> Ajout d'un paramètre de configuration personnalisé destiné à spécifier l'emplacement des fichiers de vidage  
 Vous pouvez ajouter un paramètre personnalisé visant à définir l'emplacement que Dr Watson pour Windows utilise pour stocker les fichiers de vidage. Le paramètre personnalisé est **Directory**. L’exemple suivant illustre l’utilisation de ce paramètre de configuration dans la section **RStrace** :  
  
```  
<add name="Directory" value="U:\logs\" />  
```  
  
 Pour plus d'informations, consultez l' [article 913046 de la Base de connaissances](http://support.microsoft.com/?kbid=913046) sur le site Web de [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
##  <a name="bkmk_log_file_fields"></a> Champs du fichier journal  
 Un journal des traces contient les champs suivants :  
  
-   informations système, notamment le système d'exploitation, la version, le nombre de processeurs et la mémoire ;  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ;  
  
-   événements consignés dans le journal des applications ;  
  
-   exceptions générées par le serveur de rapports ;  
  
-   avertissements relatives aux ressources insuffisantes consignés par un serveur de rapports ;  
  
-   enveloppes SOAP entrantes et enveloppes SOAP sortantes résumées ;  
  
-   en-tête HTTP, trace de la pile et informations de suivi de débogage.  
  
 Vous pouvez consulter les informations du journal des traces pour déterminer si une remise de rapport s'est produite, qui a reçu le rapport et combien de tentatives de remises ont été effectuées. Les journaux de suivi enregistrent également l'activité d'exécution des rapports et les variables d'environnement qui sont en vigueur pendant le traitement des rapports. Les erreurs et les exceptions sont également consignées dans les journaux de suivi. Par exemple, vous pouvez trouver des erreurs de délai d’attente pour des rapports (indiquées par une entrée **ThreadAbortExceptions** ).  

## <a name="previous-versions"></a>Versions précédentes
Dans les versions antérieures de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)], il existait plusieurs fichiers journaux des traces, un pour chaque application. Les fichiers suivants sont obsolètes et ne sont plus créés dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures :
+ ReportServerWebApp_*\<timestamp>*.log
+ ReportServer_*\<timestamp>*.log
+ ReportServerService_main_*\<timestamp >*.log
  
## <a name="see-also"></a> Voir aussi  
 [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Guide de référence des erreurs et des événements &#40;Reporting Services&#41;](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
 D’autres questions ? [Essayez le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
  
