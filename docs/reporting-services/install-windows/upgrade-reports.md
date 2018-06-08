---
title: Mettre à niveau des rapports | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services], upgrading
- published reports [Reporting Services], upgrades
- custom report items, upgrading
- Reporting Services, upgrades
- upgrading Reporting Services
- snapshots [Reporting Services], upgrading
- report definition files [Reporting Services]
- .rdl files
ms.assetid: a1a10c67-7462-4562-9b07-a8822188a161
caps.latest.revision: 70
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3ae563ddf9c21fc0add692493504dd7ccbdd09fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="upgrade-reports"></a>Mettre à niveau des rapports

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)]

  Les fichiers de définition de rapport (.rdl) existants sont automatiquement mis à niveau de différentes façons :  
  
-   Lorsque vous ouvrez un rapport dans le Concepteur de rapports de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la définition du rapport est mise à niveau vers le schéma RDL actuellement pris en charge. Lorsque vous spécifiez un serveur de rapports [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] dans les propriétés du projet, la définition du rapport est enregistrée dans un schéma compatible avec le serveur cible.  
  
-   Lorsque vous mettez à niveau une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] vers une installation [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] , les rapports et les instantanés existants qui ont été publiés sur un serveur de rapports sont compilés et automatiquement mis à niveau avec le nouveau schéma la première fois qu'ils sont traités. Si un rapport ne peut pas être mis à niveau automatiquement, le rapport est traité à l'aide du mode de compatibilité ascendante. La définition de rapport reste dans le schéma d'origine.  
  
 Les rapports ne sont pas mis à niveau lorsque vous téléchargez un fichier de définition de rapport directement vers le serveur de rapports ou le site SharePoint. La mise à niveau d'une définition de rapport dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] est le seul moyen de mettre à niveau le fichier. rdl.  
  
 Après la mise à niveau d'un rapport en local ou sur le serveur de rapports, vous pouvez noter la présence d'erreurs, d'avertissements et de messages. Cette présence est liée aux améliorations apportées au modèle objet de rapport interne et aux composants de traitement. En effet, des messages apparaissent lors de la détection de problèmes sous-jacents dans le rapport. Pour plus d’informations, consultez [Compatibilité descendante de Reporting Services](../../reporting-services/reporting-services-backward-compatibility.md).  
  
 Pour plus d’informations sur les nouvelles fonctionnalités de [!INCLUDE[ssRSCurrent](../what-s-new-in-sql-server-reporting-services-ssrs.md).  

##  <a name="bkmk_versionsupported"></a> Versions prises en charge par la mise à niveau  
 Les rapports créés dans une version précédente de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent être mis à niveau. Les versions concernées sont les suivantes :  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
##  <a name="bkmk_rdlfiles"></a> Fichiers de définition de rapport (.rdl) et Concepteur de rapports  
 Un fichier de définition de rapport comprend une référence à l'espace de noms RDL qui indique la version du schéma de définition de rapport utilisé pour valider le fichier .rdl.  
  
 Lorsque vous ouvrez un fichier .rdl dans le Concepteur de rapports de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], si le rapport a été créé pour un espace de noms antérieur, le Concepteur de rapports crée automatiquement un fichier de sauvegarde et met à niveau le rapport d'après l'espace de noms actuel. Il s'agit de la seule façon dont vous pouvez mettre à niveau un fichier de définition de rapport.  
  
 Les propriétés de déploiement que vous définissez peuvent affecter le choix du schéma d'enregistrement du fichier de définition de rapport. Pour plus d’informations, consultez [Déploiement et prise en charge des versions dans les outils de données SQL Server &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
 Vous pouvez télécharger un fichier .rdl créé dans une version antérieure de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sur la nouvelle et il est automatiquement mis à niveau lors de la première utilisation. Le serveur de rapports stocke le fichier de définition de rapport dans le format d'origine. Le rapport est automatiquement mis à niveau la première fois où il est affiché, mais le fichier de définition de rapport stocké demeure inchangé.  
  
 Pour identifier le schéma RDL actuel d’un rapport, d’un serveur de rapports ou du Concepteur de rapports, consultez [Rechercher la version du schéma de définition de rapport &#40;SSRS&#41;](../../reporting-services/reports/find-the-report-definition-schema-version-ssrs.md).  
  
##  <a name="bkmk_publishedreports_and_snapshots"></a> Rapports publiés et instantanés de rapport  
 Lors de la première utilisation, le serveur de rapports essaie de mettre à niveau les rapports publiés et les instantanés de rapport existants vers le nouveau schéma de définition de rapport, en ne requérant aucune action spécifique de votre part. Lorsqu'un utilisateur affiche un rapport ou un instantané de rapport, ou lorsque le serveur de rapports traite un abonnement, la mise à niveau s'effectue. La définition de rapport n’est pas remplacée, mais continue à être stockée sur le serveur de rapports dans son schéma d’origine. Si un rapport ne peut pas être mis à niveau, le rapport s'exécute en mode compatibilité descendante.  
  
##  <a name="bkmk_backcompat"></a> Mode compatibilité descendante  
 Un rapport mis à niveau avec succès est traité par le processeur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] . Un rapport qui ne peut pas être mis à niveau est traité par le processeur de rapports [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode compatibilité descendante. Un rapport ne peut pas être traité par les deux processeurs de rapports. Lors de la première utilisation, un rapport est mis à niveau avec succès ou marqué pour la compatibilité descendante.  
  
 Seul le processeur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] prend en charge les nouvelles fonctionnalités. Si un rapport ne peut pas être mis à niveau, vous pouvez toujours consulter le rapport rendu, mais les nouvelles fonctionnalités ne sont pas disponibles. Pour tirer parti des nouvelles fonctionnalités, un rapport doit être mis à niveau avec succès.  
  
##  <a name="bkmk_subreports"></a> Mise à niveau d'un rapport avec les sous-rapports  
 Quand un rapport contient des sous-rapports, l'un des quatre états suivants peut se produire pendant la mise à niveau :  
  
-   Le rapport principal et tous les sous-rapports peuvent être mis à niveau avec succès. Ils sont traités par le processeur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] .  
  
-   Le rapport principal et tous les sous-états ne peuvent pas être mis à niveau. Ils sont traités par le processeur de rapports [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Le rapport principal peut être mis à niveau, mais un ou plusieurs sous-rapports ne peuvent pas être mis à niveau. Le rapport principal est traité par le processeur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] , mais le rapport rendu affiche le message « Erreur : le sous-rapport n'a pas pu être traité » à l'emplacement où le sous-rapport qui n'a pas pu être mis à niveau apparaîtrait.  
  
-   Le rapport principal ne peut pas être mis à niveau, mais un ou plusieurs sous-rapports peuvent l'être. Le rapport principal est traité par le processeur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] , mais le rapport rendu affiche le message « Erreur : le sous-rapport n'a pas pu être traité » à l'emplacement où le sous-rapport apparaîtrait.  
  
 Si le précédent message d'erreur apparaît, vous devez modifier la définition du rapport principal ou du sous-rapport afin que tous les rapports puissent être traités par la même version du processeur de rapports.  
  
 Les rapports d'extraction n'ont pas cette limitation parce qu'ils sont traités en tant que rapports indépendants.  
  
##  <a name="bkmk_CRIs"></a> Mise à niveau d'un rapport avec les éléments de rapport personnalisés  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]Les rapports [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent contenir des éléments de rapport personnalisés proposés par des éditeurs de logiciels tiers et installés par l’administrateur système sur l’ordinateur de création de rapports et sur le serveur de rapports. Les rapports qui contiennent des éléments de rapport personnalisés peuvent être mis à niveau de différentes façons :  
  
-   Un serveur de rapports [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est mis à niveau vers un serveur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] . Les rapports publiés sur le serveur de rapports sont automatiquement mis à niveau lors de la première utilisation.  
  
-   Un rapport [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est chargé sur un serveur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] . Le rapport est automatiquement mis à niveau lors de la première utilisation.  
  
-   Un rapport [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est ouvert dans le Concepteur de rapports de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Une copie de sauvegarde du rapport original est créée. L'un des deux cas suivants se produit :  
  
    1.  Tous les éléments de rapport personnalisés du rapport n'ont pas de fonctionnalités non prises en charge. Les éléments de rapport personnalisés sont convertis en éléments de rapport dans le nouveau schéma de définition de rapport, de telle sorte que la totalité du rapport soit mise à niveau. Si vous enregistrez le fichier, il est enregistré dans l'espace de noms RDL courant.  
  
    2.  Un ou plusieurs éléments de rapport personnalisés du rapport ont des fonctionnalités non prises en charge. Une boîte de dialogue invite l'utilisateur à convertir les éléments de rapport personnalisés ou à les laisser inchangés.  
  
     Pour plus d'informations, consultez [Ouverture d'un rapport dans le Concepteur de rapports](#OpeningaReport) plus bas dans cette rubrique.  
  
 Pour plus d’informations sur l’identification de l’espace de noms RDL en cours d’un serveur de rapports, de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ou d’un rapport, consultez [Rechercher la version du schéma de définition de rapport &#40;SSRS&#41;](../../reporting-services/reports/find-the-report-definition-schema-version-ssrs.md).  
  
### <a name="upgrading-reports-on-a-report-server"></a>Mise à niveau des rapports sur un serveur de rapports  
 La première fois qu’un rapport [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] s’exécute sur un serveur de rapports mis à niveau vers un serveur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] , le rapport est automatiquement mis à niveau vers l’espace de noms de définition de rapport actuel pris en charge par le serveur de rapports. Le rapport aurait pu exister sur le serveur de rapports avant la mise à niveau, être chargé par le biais du Gestionnaire de rapports ou publié sur le serveur de rapports à partir du Concepteur de rapports de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
 Le tableau suivant répertorie l'action de mise à niveau effectuée par le serveur de rapports pour les types spécifiques d'éléments de rapport personnalisés d'un rapport.  
  
|Type CRI|Action de mise à niveau du serveur de rapports|  
|--------------|----------------------------------|  
|Éléments de rapport personnalisés tiers|Mise à niveau non effectuée.<br /><br /> Traitement par le processeur de rapports [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|  
  
###  <a name="OpeningaReport"></a> Ouverture d'un rapport avec éléments de rapport personnalisés dans le Concepteur de rapports  
 Lorsque vous ouvrez un rapport [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] avec éléments de rapport personnalisés dans le Concepteur de rapports de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], le rapport est mis à niveau vers le nouveau schéma de définition de rapport. Selon les éléments de rapport personnalisés contenus dans le rapport, l'une des actions suivantes a lieu :  
  
-   Éléments de rapport personnalisés tiers détectés. Si la version du CRI installé sur l'ordinateur de création de rapports n'est pas compatible avec le nouveau schéma RDL, l'aire de conception affiche une zone de texte avec une croix de couleur rouge. Vous devez contacter votre administrateur système pour installer les nouvelles versions des éléments de rapport personnalisés des fournisseurs tiers compatibles avec le nouveau schéma RDL.  
  
 L'enregistrement d'un rapport après qu'il a été mis à niveau dans l'environnement de création de rapports est la seule méthode pour mettre à niveau un rapport existant vers le nouveau schéma de définition de rapport.  
  
###  <a name="bkmk_convertCRIdialog"></a> Boîte de dialogue de conversion des éléments de rapport personnalisés  
 Ce rapport contient des éléments de rapport personnalisés (CRI, Custom Report Item) avec des fonctionnalités non prises en charge. Les éléments de rapport personnalisés sont des extensions du langage RDL (Report Definition Language) qui prennent en charge les objets personnalisés qui affichent les données dans un rapport. Les CRI incluent des composants du moment du design et du moment de l'exécution fournis par des éditeurs de logiciels tiers.  
  
> [!NOTE]  
>  Le choix de prendre en charge des éléments de rapport personnalisés sur un serveur de rapports est une décision qui revient à l'administrateur système. Pour permettre l'affichage des éléments de rapport personnalisés (CRI) dans un rapport, les composants CRI doivent être installés sur le client de création de rapports pour l'aperçu d'un rapport et sur le serveur de rapports pour l'affichage d'un rapport publié et téléchargé. Pour plus d’informations, consultez [Éléments de rapport personnalisés](../../reporting-services/custom-report-items/custom-report-items.md) et la documentation de l’éditeur de logiciels tiers.  
  
 Certains CRI peuvent être convertis en éléments de rapport dans le nouveau format de définition de rapport. Utilisez la liste suivante pour décider s'il est nécessaire de convertir les CRI dans ce rapport :  
  
-   **Oui** Choisissez **Oui** pour convertir tous les CRI du rapport, dans la mesure du possible. Les fonctionnalités non prises en charge dans les CRI ne peuvent pas être mises à niveau et peuvent être supprimées du fichier de définition du rapport. Lorsque vous consultez le rapport, vous pouvez voir des différences dans la manière dont les CRI apparaissent dans le rapport.  
  
-   **Non** Choisissez **Non** si vous ne souhaitez pas convertir les CRI dans le rapport. Ces CRI ne peuvent pas être affichés par le processeur de rapports dans leur version actuelle. Si votre administrateur système projette d’installer une nouvelle version du CRI de l’éditeur de logiciels tiers compatible avec le nouveau format de définition de rapport, vous devez choisir **Non**. Tant que de nouvelles versions ne sont pas disponibles, les CRI apparaissent dans le rapport sous la forme d'une zone de texte vide marquée d'une croix (X) rouge.  
  
 Dans les deux cas, le rapport est mis à niveau vers le nouveau format de définition de rapport et une copie de sauvegarde du rapport d’origine est enregistrée au format *\<nom rapport>* `-` Backup.rdl. Si vous enregistrez le rapport dans votre outil de création de rapports, vous enregistrez le rapport mis à niveau dans le nouveau format de définition de rapport. Si vous publiez le rapport, celui-ci est d'abord enregistré sur votre ordinateur, puis publié sur le serveur de rapports. Vous publiez la version mise à niveau du rapport sur le serveur de rapports.  
  
 Si vous n'enregistrez pas le rapport, le rapport d'origine reste inchangé. Toutefois, vous ne pouvez pas modifier ce rapport dans la version SQL Server 2016 de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ou dans un environnement de création de rapports qui utilise un format de définition de rapport plus récent. Vous pouvez continuer à exécuter la version d’origine du rapport en le téléchargeant sur un serveur de rapports [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] à l’aide du portail web. Pour plus d’informations, consultez [Portail web](../../reporting-services/web-portal-ssrs-native-mode.md).  
  
 Pour les rapports que vous téléchargez au lieu de les publier sur un serveur de rapports, le processeur de rapports détermine si le rapport peut être mis à niveau à la première utilisation. Les rapports qui ne peuvent pas être mis à niveau sont traités en mode de compatibilité descendante et continuent à s'afficher comme dans la version antérieure de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  

## <a name="next-steps"></a>Étapes suivantes

[Mettre à niveau et migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Modifications importantes de SQL Server Reporting Services dans SQL Server 2016](../../analysis-services/behavior-changes-to-analysis-services-features-in-sql-server-2016.md)   
[Changements de comportement apportés à SQL Server Reporting Services dans SQL Server 2016](../../analysis-services/behavior-changes-to-analysis-services-features-in-sql-server-2016.md)   
[Fonctionnalités supprimées de SQL Server Reporting Services dans SQL Server 2016](../../reporting-services/behavior-changes-to-sql-server-reporting-services-in-sql-server-2016.md)   
[Éléments de rapport personnalisés](../../reporting-services/custom-report-items/custom-report-items.md)   
[Mettre à niveau une base de données de serveurs de rapports](../../reporting-services/install-windows/upgrade-a-report-server-database.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
