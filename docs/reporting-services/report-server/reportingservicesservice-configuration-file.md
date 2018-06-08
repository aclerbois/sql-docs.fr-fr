---
title: Fichier de configuration ReportingServicesService | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- traces [Reporting Services]
- Report Server Windows service, ReportingServicesService configuration file
- ReportingServicesService configuration file
ms.assetid: 40f4a401-cb61-4c42-b1ec-01acdacdacd1
caps.latest.revision: 41
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 071650750724cf8a58c4ac6377f9e2b1f635fe12
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reportingservicesservice-configuration-file"></a>fichier de configuration ReportingServicesService
 ||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**  SQL Server 2016|
  
Le fichier ReportingServicesService.exe.config contient les paramètres de configuration de la trace.  
  
## <a name="file-location"></a>Emplacement du fichier  
 Ce fichier est situé dans le dossier \Reporting Services\Report Server\Bin.  
  
## <a name="editing-guidelines"></a>Instructions de modification  
 Vous pouvez modifier ce fichier pour renommer le fichier journal ou pour augmenter ou réduire les niveaux de trace. Ne modifiez aucun autre paramètre. Pour obtenir des instructions, consultez [Modifier un fichier de configuration Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md). Pour plus d’informations sur les journaux des traces, consultez [Journal des traces du service Report Server](../../reporting-services/report-server/report-server-service-trace-log.md).  
  
## <a name="example-configuration"></a>Exemple de configuration  
 L'exemple suivant illustre les paramètres et valeurs par défaut du fichier ReportingServicesService.exe.config.  
  
```  
<configSections>  
      <section name="RStrace" type="Microsoft.ReportingServices.Diagnostics.RSTraceSectionHandler,Microsoft.ReportingServices.Diagnostics" />  
</configSections>  
\<system.diagnostics>  
      <switches>  
          <add name="DefaultTraceSwitch" value="3" />  
      </switches>  
\</system.diagnostics>  
<RStrace>  
      <add name="FileName" value="ReportServerService_" />  
      <add name="FileSizeLimitMb" value="32" />  
      <add name="KeepFilesForDays" value="14" />  
      <add name="Prefix" value="tid, time" />  
      <add name="TraceListeners" value="debugwindow, file" />  
      <add name="TraceFileMode" value="unique" />  
      <add name="Components" value="all" />  
</RStrace>  
<runtime>  
      <alwaysFlowImpersonationPolicy enabled="true"/>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
             <dependentAssembly>  
                    <assemblyIdentity name="Microsoft.ReportingServices.Interfaces"  
                        publicKeyToken="89845dcd8080cc91"  
                        culture="neutral" />  
                    <bindingRedirect oldVersion="8.0.242.0"  
                                     newVersion="10.0.0.0"/>  
                    <bindingRedirect oldVersion="9.0.242.0"  
                                     newVersion="10.0.0.0"/>  
             </dependentAssembly>  
      </assemblyBinding>  
      <gcServer enabled="true" />  
</runtime>  
```  
  
## <a name="configuration-settings"></a>Paramètres de configuration  
 Le tableau suivant contient des informations sur des paramètres spécifiques. Les paramètres sont présentés dans l'ordre dans lequel ils apparaissent dans le fichier de configuration.  
  
|Paramètre|Description|  
|-------------|-----------------|  
|**RStrace**|Spécifie les espaces de noms utilisés pour les erreurs et la trace.|  
|**DefaultTraceSwitch**|Spécifie le niveau des informations consignées dans le journal de trace de ReportServerService. Chaque niveau comprend les informations signalées par tous les niveaux inférieurs. La désactivation de la trace n'est pas recommandée. Les valeurs valides sont les suivantes :<br /><br /> 0= Trace désactivée<br /><br /> 1= Exceptions et redémarrages<br /><br /> 2= Exceptions, redémarrages, avertissements<br /><br /> 3= Exceptions, redémarrages, avertissements, messages d'état (par défaut)<br /><br /> 4= Mode commenté|  
|**FileName**|Spécifie la première partie du nom du fichier journal. La valeur spécifiée par **Prefix** complète le reste du nom. Le nom est ReportServerService_ par défaut.|  
|**FileSizeLimitMb**|Spécifie une taille maximale pour le journal de trace. La taille du fichier est exprimée en mégaoctets. Les valeurs valides vont de 0 à un entier maximal. La valeur par défaut est 32.|  
|**KeepFilesForDays**|Spécifie le nombre de jours après lequel supprimer un journal de trace. Les valeurs valides vont de 0 à un entier maximal. La valeur par défaut est 14.|  
|**Prefix**|Spécifie une valeur générée qui distingue une instance de journal d'une autre. Par défaut, des valeurs d'horodatage sont ajoutées aux noms des journaux de trace. Cette valeur est définie sur « tid, time ». Ne modifiez pas ce paramètre.|  
|**TraceListeners**|Spécifie une cible de sortie du contenu du journal de trace. Vous pouvez spécifier plusieurs cibles ; dans ce cas, utilisez la virgule comme séparateur. Les valeurs valides sont les suivantes :<br /><br /> DebugWindow (par défaut)<br /><br /> File (par défaut)<br /><br /> StdOut|  
|**TraceFileMode**|Spécifie si les journaux de trace contiennent des données pour une période de 24 heures. Un seul journal de trace doit exister par composant et par jour. Cette valeur est définie sur « Unique » (par défaut). Ne modifiez pas cette valeur.|  
|**Components**|Spécifie les composants pour lesquels des journaux de trace sont créés. La valeur par défaut est **Tous**. Ce paramètre accepte aussi comme valeur les noms de composants internes. Ne modifiez pas cette valeur.|  
|**Runtime**|Spécifie les paramètres de configuration qui prennent en charge la compatibilité descendante avec la version précédente. Des paramètres d'exécution sont utilisés pour rediriger vers la nouvelle version les demandes qui ciblent la version précédente de Microsoft.ReportingServices.Interfaces.<br /><br /> Tous les paramètres de configuration de cette section sont décrits dans la documentation de produit du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . Pour plus d'informations, recherchez « Runtime Schema Settings » (en anglais) sur le site Web MSDN ou dans la documentation du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] .|  
  
## <a name="see-also"></a> Voir aussi  
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Journal de suivi de service du serveur de rapports](../../reporting-services/report-server/report-server-service-trace-log.md)  
  
  
