---
title: Enregistrer les opérations dans Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ba0be2d0a46790f1a330a75c25461983e0b7488a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="log-operations-in-analysis-services"></a>Enregistrer les opérations dans Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une instance Analysis Services enregistre les notifications, erreurs et avertissements du serveur dans le fichier msmdsrv.log (un pour chaque instance que vous installez). Les administrateurs se réfèrent à ce fichier journal pour en savoir plus sur les événements ordinaires et extraordinaires. Dans les versions récentes, la journalisation a été améliorée pour inclure davantage d'informations. Désormais, les enregistrements de journaux incluent des informations sur la version et l'édition du produit, ainsi que sur des événements du processeur, de la mémoire, de la connectivité et de blocage. L'article [Améliorations apportées à la journalisation](http://support.microsoft.com/kb/2965035)fournit une liste de tous les changements.  
  
 En plus de la fonctionnalité de journalisation intégrée, de nombreux administrateurs et développeurs utilisent des outils fournis par la communauté Analysis Services pour recueillir des données sur les opérations du serveur, comme **ASTrace**. Pour obtenir des liens de téléchargement, consultez [Exemples de la communauté Microsoft SQL Server : Analysis Services](https://sqlsrvanalysissrvcs.codeplex.com/) .  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Emplacement et types de journaux](#bkmk_location)  
  
-   [Informations générales sur les paramètres de configuration des fichiers journaux](#bkmk_general)  
  
-   [Fichier journal du service MSMDSRV](#bkmk_msmdsrv)  
  
-   [Journaux des requêtes](#bkmk_querylog)  
  
-   [Fichiers de vidage minimal (.mdmp)](#bkmk_mdmp)  
  
-   [Conseils et meilleures pratiques](#bkmk_tips)  
  
> [!NOTE]  
>  Si vous recherchez des informations sur la journalisation, vous serez peut-être intéressé par les opérations de suivi qui montrent les détails de traitement et d'exécution de requêtes. Pour obtenir des informations sur les objets de trace pour le suivi ad hoc et maintenu (par exemple, l’audit de l’accès aux cubes), ainsi que des recommandations sur l’utilisation optimale de Flight Recorder, SQL Server Profiler et xEvents, consultez les liens accessibles dans cette page : [Analyser une instance Analysis Services](../../analysis-services/instances/monitor-an-analysis-services-instance.md).  
  
##  <a name="bkmk_location"></a> Emplacement et types de journaux  
 Analysis Services fournit les journaux décrits ci-dessous.  
  
|Nom de fichier ou emplacement|Type|Utilisé pour|Activé par défaut|  
|---------------------------|----------|--------------|-------------------|  
|Msmdsrv.log|Journal des erreurs|Surveillance de routine et dépannage de base|Oui|  
|Table OlapQueryLog dans une base de données relationnelle|Journal des requêtes|Recueillir les entrées de l'Assistant Optimisation de l'utilisation|non|  
|Fichiers SQLDmp\<guid > .mdmp fichiers|Blocages et exceptions|Dépannage approfondi|non|  
  
 Nous vous recommandons vivement de consulter le lien suivant pour accéder à des ressources supplémentaires non traitées dans la rubrique suivante, qui fournit des [conseils sur la collecte de données initiale depuis le support Microsoft](http://blogs.msdn.com/b/as_emea/archive/2012/01/02/initial-data-collection-for-troubleshooting-analysis-services-issues.aspx).  
  
##  <a name="bkmk_general"></a> Informations générales sur les paramètres de configuration des fichiers journaux  
 Vous trouverez des sections pour chaque journal dans le fichier de configuration de serveur msmdsrv.ini, qui se trouve dans le dossier \Program Files\Microsoft SQL Server\MSAS13.MSSQLSERVER\OLAP\Config. Pour obtenir des instructions sur la modification du fichier, consultez [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md) .  
  
 Dans la mesure du possible, nous vous suggérons de définir des propriétés de journalisation dans la page de propriétés du serveur de Management Studio. Toutefois, dans certains cas, vous devez modifier le fichier msmdsrv.ini directement pour configurer les paramètres qui ne sont pas visibles dans les outils d'administration.  
  
 ![Section du fichier de configuration indiquant des paramètres de journal](../../analysis-services/instances/media/ssas-logfilesettings.png "Section du fichier de configuration indiquant des paramètres de journal")  
  
##  <a name="bkmk_msmdsrv"></a> Fichier journal du service MSMDSRV  
 Analysis Services enregistre les opérations du serveur dans le fichier msmdsrv.log, un par instance, qui se trouve dans \program files\Microsoft SQL Server\\<instance\>\Olap\Log.  
  
 Ce fichier journal est vidé à chaque redémarrage du service. Dans les versions précédentes, les administrateurs redémarraient parfois le service dans le seul but de vider le fichier journal avant que sa taille ne devienne ingérable. Cela n'est plus nécessaire. Les paramètres de configuration, introduits dans SQL Server 2012 SP2 et versions ultérieures, vous permettent de contrôler la taille du fichier journal et son historique :  
  
-   **MaxFileSizeMB** spécifie la taille maximale (en mégaoctets) du fichier journal. La valeur par défaut est 256. Une valeur de remplacement valide doit être un entier positif. Quand la valeur **MaxFileSizeMB** est atteinte, Analysis Services renomme le fichier actuel msmdsrv{horodateur_actuel}.log et crée un nouveau fichier msmdsrv.log.  
  
-   **MaxNumberFiles** spécifie la conservation des anciens fichiers journaux. La valeur par défaut est 0 (option désactivée). Vous pouvez la modifier et affecter un entier positif pour conserver les versions du fichier journal. Quand la valeur **MaxNumberFiles** est atteinte, Analysis Services supprime le fichier ayant l'horodateur le plus ancien.  
  
 Pour utiliser ces paramètres, procédez comme suit :  
  
1.  Dans le Bloc-notes, ouvrez msmdsrv.ini.  
  
2.  Copiez les deux lignes suivantes :  
  
    ```  
    <MaxFileSizeMB>256</MaxFileSizeMB>  
    <MaxNumberOfLogFiles>5</MaxNumberOfLogFiles>  
    ```  
  
3.  Collez les deux lignes dans la section Log de msmdsrv.ini, sous le nom de fichier de msmdsrv.log. Les deux paramètres doivent être ajoutés manuellement. Il n'existe aucun emplacement réservé pour eux dans le fichier msmdsrv.ini.  
  
     Le fichier de configuration modifié doit ressembler à ce qui suit :  
  
    ```  
    <Log>  
    <File>msmdsrv.log</File>  
    <MaxFileSizeMB>256</MaxFileSizeMB>  
    <MaxNumberOfLogFiles>5</MaxNumberOfLogFiles>  
    <FileBufferSize>0</FileBufferSize>  
  
    ```  
  
4.  Modifiez les valeurs si celles qui sont fournies ne vous conviennent pas.  
  
5.  Enregistrez le fichier.  
  
6.  Redémarrage du service.  
  
##  <a name="bkmk_querylog"></a> Journaux des requêtes  
 Le journal des requêtes porte mal son nom, car il n'enregistre pas l'activité des requêtes MDX ou DAX de vos utilisateurs. Au lieu de cela, il recueille des données sur les requêtes générées par Analysis Services, qui sont ensuite utilisées comme entrées de données dans l'Assistant Optimisation de l'utilisation. Les données recueillies dans le journal des requêtes ne sont pas destinées à être analysées directement. Plus précisément, les datasets sont décrits dans des tableaux de bits, avec un chiffre zéro ou un indiquant que les parties du dataset sont comprises dans la requête. Là encore, ces données sont destinées à l'Assistant.  
  
 Pour l'analyse et le dépannage des requêtes, de nombreux développeurs et administrateurs utilisent un outil communautaire, **ASTrace**, pour analyser les requêtes. Vous pouvez également utiliser SQL Server Profiler, xEvents ou une trace Analysis Services. Pour obtenir des liens relatifs au suivi, consultez [Analyser une instance Analysis Services](../../analysis-services/instances/monitor-an-analysis-services-instance.md) .  
  
 Quand faut-il utiliser le journal des requêtes ? Nous vous recommandons d'activer le journal des requêtes dans le cadre d'un exercice de réglage des performances de requête qui inclut l'Assistant Optimisation de l'utilisation. Le journal des requêtes n'existe qu'une fois que vous avez activé la fonctionnalité, créé les structures de données pour la prendre en charge et défini les propriétés utilisées par Analysis Services pour localiser et renseigner le journal.  
  
 Pour activer le journal des requêtes, procédez comme suit :  
  
1.  Créez une base de données relationnelle SQL Server pour stocker le journal des requêtes.  
  
2.  Accordez des autorisations suffisantes sur la base de données au compte de service Analysis Services. Le compte nécessite une autorisation pour créer une table, écrire dans la table et lire à partir de la table.  
  
3.  Dans SQL Server Management Studio, cliquez avec le bouton droit sur **Analysis Services** | **Propriétés** | **Général**, puis affectez la valeur True à **CreateQueryLogTable** .  
  
4.  Éventuellement, modifiez **QueryLogSampling** ou **QueryLogTableName** si vous souhaitez échantillonner les requêtes à une fréquence différente ou utiliser un autre nom pour la table.  
  
 La table du journal des requêtes ne sera créée qu'une fois que vous aurez exécuté suffisamment de requêtes MDX pour répondre aux critères d'échantillonnage. Par exemple, si vous conservez la valeur par défaut (10), vous devez exécuter au moins 10 requêtes pour que la table soit créée.  
  
 Les paramètres du journal des requêtes sont applicables à l'échelle du serveur. Les paramètres que vous spécifiez sont utilisés par toutes les bases de données en cours d'exécution sur ce serveur.  
  
 ![Interrogation des paramètres de journal dans Management Studio](../../analysis-services/instances/media/ssas-querylogsettings.png "paramètres du journal des requêtes dans Management Studio")  
  
 Une fois les paramètres de configuration spécifiés, exécutez une requête MDX à plusieurs reprises. Si l'échantillonnage est défini sur 10, exécutez la requête 11 fois. Vérifiez que la table est créée. Dans Management Studio, connectez-vous au moteur de base de données relationnelle, ouvrez le dossier de base de données, ouvrez le dossier **Tables** et vérifiez que **OlapQueryLog** existe. Si la table n'est pas visible immédiatement, actualisez le dossier pour faire apparaître les modifications apportées à son contenu.  
  
 Laissez le journal des requêtes accumuler suffisamment de données pour l'Assistant Optimisation de l'utilisation. Si les volumes de requêtes sont cycliques, capturez suffisamment de trafic pour avoir un jeu de données représentatif. Pour obtenir des instructions sur la façon d'exécuter l'Assistant, consultez [Aide F1 sur l'Assistant Optimisation de l'utilisation](https://msdn.microsoft.com/library/ms189706.aspx) .  
  
 Pour en savoir plus sur la configuration du journal des requêtes, consultez [Configuration du journal des requêtes Analysis Services](http://technet.microsoft.com/library/Cc917676) . Bien que cet article soit assez ancien, la configuration du journal des requêtes n'a pas changé dans les dernières versions et les informations qu'il contient sont toujours valables.  
  
##  <a name="bkmk_mdmp"></a> Fichiers de vidage minimal (.mdmp)  
 Les fichiers de vidage capturent des données utilisées pour l'analyse des événements extraordinaires. Analysis Services génère automatiquement des vidages minimaux (.mdmp) en réponse à un blocage du serveur, à une exception et à certaines erreurs de configuration. Cette fonctionnalité est activée, mais n'envoie pas automatiquement de rapports d'incidents.  
  
 Vous pouvez configurer les rapports d'incidents via la section Exception du fichier Msmdsrv.ini. Ces paramètres contrôlent la génération des fichiers de vidage de la mémoire. L'extrait suivant montre les valeurs par défaut :  
  
```  
<Exception>  
<CreateAndSendCrashReports>1</CreateAndSendCrashReports>  
<CrashReportsFolder/>  
<SQLDumperFlagsOn>0x0</SQLDumperFlagsOn>  
<SQLDumperFlagsOff>0x0</SQLDumperFlagsOff>  
<MiniDumpFlagsOn>0x0</MiniDumpFlagsOn>  
<MiniDumpFlagsOff>0x0</MiniDumpFlagsOff>  
<MinidumpErrorList>0xC1000000, 0xC1000001, 0xC102003F, 0xC1360054, 0xC1360055</MinidumpErrorList>  
<ExceptionHandlingMode>0</ExceptionHandlingMode>  
<CriticalErrorHandling>1</CriticalErrorHandling>  
<MaxExceptions>500</MaxExceptions>  
<MaxDuplicateDumps>1</MaxDuplicateDumps>  
</Exception>  
```  
  
 **Configurer les rapports d'incidents**  
  
 Sauf instruction contraire fournie par le support Microsoft, la plupart des administrateurs utilisent les paramètres par défaut. Cet article de base de connaissances est ancien, mais il fournit toujours des instructions sur la configuration des fichiers de vidage : [Comment faire pour configurer SQL Server 2005 Analysis Services pour générer des fichiers de vidage de mémoire](http://support.microsoft.com/kb/919711).  
  
 Le paramètre de configuration le plus susceptible d'être modifié est le paramètre **CreateAndSendCrashReports** qui sert à déterminer si un fichier de vidage de la mémoire sera généré.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|Désactive le fichier de vidage de la mémoire. Tous les autres paramètres dans la section Exceptions sont ignorés.|  
|1|(Par défaut) Active mais n'envoie pas le fichier de vidage de la mémoire.|  
|2|Active et envoie automatiquement un rapport d'erreurs à Microsoft.|  
  
 **CrashReportsFolder** est l'emplacement des fichiers de vidage. Par défaut, un fichier .mdmp et les enregistrements de journaux associés se trouvent dans le dossier \Olap\Log.  
  
 **SQLDumperFlagsOn** sert à générer un vidage complet. Par défaut, les vidages complets ne sont pas activés. Vous pouvez affecter la valeur **0x34**à cette propriété.  
  
 Les liens suivants fournissent des explications plus détaillées :  
  
-   [Exploration approfondie de SQL Server avec des minidumps](http://blogs.msdn.com/b/sqlcat/archive/2009/09/11/looking-deeper-into-sql-server-using-minidumps.aspx)  
  
-   [Comment créer un fichier de vidage en mode utilisateur](http://support.microsoft.com/kb/931673)  
  
-   [Comment utiliser l'utilitaire Sqldumper.exe pour générer un fichier de vidage dans SQL Server](http://support.microsoft.com/kb/917825)  
  
##  <a name="bkmk_tips"></a> Conseils et meilleures pratiques  
 Cette section est un récapitulatif des conseils mentionnés dans cet article.  
  
-   Configurez le fichier msmdsrv.log pour contrôler la taille et le numéro du fichier journal msmdsrv. Les paramètres ne sont pas activés par défaut. Veillez à les ajouter comme étapes de post-installation. Consultez [Fichier journal du service MSMDSRV](#bkmk_msmdsrv) dans cette rubrique.  
  
-   Passez en revue ce billet de blog des membres du support technique Microsoft, qui porte sur la [collecte de données initiales](http://blogs.msdn.com/b/as_emea/archive/2012/01/02/initial-data-collection-for-troubleshooting-analysis-services-issues.aspx), pour découvrir les ressources qu’ils utilisent, et obtenir ainsi des informations sur les opérations du serveur.  
  
-   Utilisez ASTrace2012 plutôt qu'un journal des requêtes pour savoir qui interroge les cubes. Le journal des requêtes sert généralement à fournir une entrée pour l'Assistant Optimisation de l'utilisation. Les données qu'il capture ne sont pas faciles à lire ou à interpréter. ASTrace2012 est un outil de la communauté, largement utilisé, qui capture les opérations de requêtes. Consultez [Exemples de la communauté Microsoft SQL Server : Analysis Services](https://sqlsrvanalysissrvcs.codeplex.com/).  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion d’instances Analysis Services](../../analysis-services/instances/analysis-services-instance-management.md)   
 [Introduction à la surveillance d’Analysis Services avec SQL Server Profiler](../../analysis-services/instances/introduction-to-monitoring-analysis-services-with-sql-server-profiler.md)   
 [Propriétés du serveur dans Analysis Services](../../analysis-services/server-properties/server-properties-in-analysis-services.md)  
  
  
