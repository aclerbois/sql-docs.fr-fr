---
title: Résoudre les problèmes liés aux rapports Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a705d103-85b1-49b5-b27f-332b1040d029
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: cb93c59cec663a99ebc0460d948f4c2bd0a78f94
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot--reporting-services-report-issues"></a>Résoudre les problèmes avec les rapports Reporting Services
Cette rubrique vous fournit des conseils pour résoudre les problèmes associés à la conception de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] , l’affichage de l’aperçu d’un rapport, la publication d’un rapport sur un serveur de rapports en mode natif ou en mode SharePoint, l’affichage d’un rapport sur le serveur de rapports ou l’exportation d’un rapport dans un format de fichier différent.  
## <a name="monitor-report-servers"></a>Analyser les serveurs de rapports  
Vous pouvez utiliser des outils système et de base de données pour analyser l'activité du serveur de rapports. Vous pouvez également examiner les fichiers journaux de suivi du serveur de rapports ou interroger le journal des exécutions du serveur de rapports pour obtenir des informations détaillées sur des rapports spécifiques. Si vous utilisez l'Analyseur de performances, vous pouvez ajouter des compteurs de performances pour le service Web Report Server et le service Windows afin d'identifier les goulots d'étranglement dans un traitement à la demande ou planifié.  
Pour plus d’informations, consultez [Analyse des performances d’un serveur de rapports](../../reporting-services/report-server/monitoring-report-server-performance.md).  
  
  
## <a name="view-the-report-server-logs"></a>Afficher les journaux d’un serveur de rapports  
[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] enregistre de nombreux événements internes et externes dans des fichiers journaux. Ceux-ci contiennent des données sur des rapports spécifiques, des informations de débogage, des requêtes et des réponses HTTP, ainsi que des événements du serveur de rapports. Vous pouvez également créer des journaux de performances et sélectionner des compteurs de performances qui spécifient les données à collecter. Le répertoire par défaut des fichiers journaux dans une installation par défaut est `<drive>\Program Files\Microsoft SQL Server\MSRS130.MSSQLSERVER\Reporting Services\LogFiles`.   
  
Pour plus d’informations, consultez [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).  
  
Pour déterminer spécifiquement si les attentes liées à un rapport sont dues à la récupération de données, au traitement de rapport ou au rendu de rapport, utilisez le journal des exécutions. Pour plus d’informations, consultez [Journal d’exécution du serveur de rapports et vue ExecutionLog3].   
  
## <a name="view-the-call-stack-for-report-processing-error-messages-on-the-report-server"></a>Afficher la pile des appels pour les messages d’erreur de traitement des rapports sur le serveur de rapports  
Lorsque vous affichez un rapport publié dans le Gestionnaire de rapports, vous pouvez recevoir un message d'erreur qui représente une erreur générale de traitement ou de rendu. Pour obtenir plus d'informations, vous pouvez consulter la pile des appels.   
  
Pour afficher la pile des appels, ouvrez une session sur le serveur de rapports avec des informations d’identification d’administrateur local, cliquez avec le bouton droit sur la page Gestionnaire de rapports, puis cliquez sur **Afficher la source**. La pile des appels fournit le contexte détaillé du message d'erreur.  
  
## <a name="use-includessmanstudiofullincludesssmanstudiofullmd-to-verify-queries-and-credentials"></a>Utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] pour vérifier les informations d’identification et les requêtes  
Vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] pour valider des requêtes complexes avant de les inclure dans votre rapport.   
  
Pour plus d’informations, consultez [Éditeur de requête du moteur de base de données](../../relational-databases/scripting/database-engine-query-editor-sql-server-management-studio.md) et [Gérer les objets à l’aide de l’Explorateur d’objets](~/ssms/object/manage-objects-by-using-object-explorer.md).  
  
## <a name="analyze-problem-reports-with-report-data-cached-on-the-client"></a>Analyser les rapports de problèmes avec des données de rapport mises en cache sur le client  
Lorsqu’un auteur de rapport crée un rapport dans Business Intelligence Development Studio, le client de création met en cache les données sous forme d’un fichier .rdl.data qui est utilisé lorsque vous affichez l’aperçu d’un rapport. Chaque fois que la requête est modifiée, le cache est mis à jour. Pour déboguer les problèmes de rapport, il peut parfois s'avérer utile d'empêcher l'actualisation des données de rapport de manière à ce que les données ne soient pas modifiées pendant le débogage.   
  
Pour contrôler si [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull.md)] peut uniquement utiliser des données mises en cache, ajoutez la section suivante à devenv.exe.config dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio.md)]. L’emplacement du répertoire par défaut est : `<drive>:Program Files\Microsoft Visual Studio 10.0\Common7\IDE`.   
  
```  
<system.diagnostics>  
      <switches>  
         <add name="Microsoft.ReportDesigner.ReportPreviewStore.ForceCache" value="1" />  
      </switches>  
   </system.diagnostics>  
```  
Tant que la valeur est 1, seules les données de rapport mises en cache sont utilisées. Veillez à supprimer cette section lorsque vous avez terminé de déboguer le rapport.  
  
## <a name="see-also"></a> Voir aussi  
[Erreurs et événements (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect.md)]


