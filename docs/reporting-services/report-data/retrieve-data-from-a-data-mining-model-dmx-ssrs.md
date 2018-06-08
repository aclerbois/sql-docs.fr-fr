---
title: Récupérer des données d’un modèle d’exploration de données (DMX) (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- retrieving report data
- datasets [Reporting Services], with DMX queries
- datasets [Reporting Services], Analysis Services
- queries [Reporting Services], data mining prediction
ms.assetid: d9cd3624-1594-4707-8887-55437dd7e07c
caps.latest.revision: 19
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 53fe62a48a40b768b730f1f16ab038703deda5ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieve-data-from-a-data-mining-model-dmx-ssrs"></a>Récupérer des données d'un modèle d'exploration de données (DMX) (SSRS)
  Pour utiliser les données d’un modèle d’exploration de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans votre rapport, vous devez définir une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ainsi qu’un ou plusieurs datasets de rapport. Lorsque vous créez la définition de la source de données, vous devez spécifier une chaîne de connexion et des informations d'identification pour pouvoir accéder à la source de données à partir de l'ordinateur client.  
  
 Vous pouvez créer une définition de source de données incorporée à utiliser par un seul rapport ou une définition de source de données partagée utilisable par plusieurs rapports. Les procédures de cette rubrique décrivent la création d'une source de données incorporée. Pour plus d’informations sur les sources de données partagées, consultez [Connexions de données ou sources de données incorporées et partagées &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/f417782c-b85a-4c4d-8a40-839176daba56) et [Créer, modifier, puis supprimer des sources de données partagées &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
  
 Après avoir créé une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous pouvez créer un ou plusieurs datasets. Pour chaque dataset, vous utilisez un concepteur de requêtes de prédiction de l'exploration de données (DMX) pour créer une requête DMX qui spécifie la collection de champs. Pour plus d’informations, consultez [Interface utilisateur du Concepteur de requêtes DMX Analysis Services](../../reporting-services/report-data/analysis-services-dmx-query-designer-user-interface.md).  
  
 Le nom du dataset créé apparaît dans le volet des données de rapport sous la forme d'un nœud sous la source de données correspondante.  
  
 Après avoir publié votre rapport, vous pouvez devoir modifier les informations d'identification pour la source de données afin que les autorisations soient valides pour récupérer les données lorsque le rapport s'exécute sur le serveur de rapports.  
  
### <a name="to-create-an-embedded-microsoft-sql-server-analysis-services-data-source"></a>Pour créer une source de données Microsoft SQL Server Analysis Services incorporée  
  
1.  Dans la barre d'outils du volet Données du rapport, cliquez sur **Nouveau**, puis sur **Source de données**.  
  
2.  Dans la boîte de dialogue **Propriétés de la source de données** , tapez un nom dans la zone de texte **Nom** ou acceptez le nom par défaut.  
  
3.  Vérifiez que l’option **Connexion incorporée** est sélectionnée.  
  
4.  Dans la liste déroulante **Type** , sélectionnez **Microsoft SQL Server Analysis Services**.  
  
5.  Spécifiez une chaîne de connexion qui fonctionne avec votre source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
     Contactez l'administrateur de votre base de données pour connaître les informations de connexion et d'identification à utiliser pour se connecter à la source de données. L’exemple de chaîne de connexion suivant spécifie l’exemple de base de données [!INCLUDE[ssSampleDBDWobject](../../includes/sssampledbdwobject-md.md)] sur le client local.  
  
    ```  
    Data Source=localhost;Initial Catalog=AdventureWorksDW2012  
    ```  
  
6.  Cliquez sur **Informations d'identification**.  
  
     Définissez les informations d'identification à utiliser pour se connecter à la source de données. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
    > [!NOTE]  
    >  Pour tester la connexion à la source de données, cliquez sur **Modifier**. Dans la boîte de dialogue **Propriétés de connexion** , cliquez sur **Tester la connexion**. Si le test a réussi, le message « Le test de la connexion a réussi. » s’affiche. Si le test échoue, un message d'avertissement apparaît avec d'autres informations sur la cause de l'échec.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     La source de données apparaît dans le volet des données de rapport.  
  
### <a name="to-create-a-dataset-for-a-microsoft-sql-server-analysis-services"></a>Pour créer un dataset pour Microsoft SQL Server Analysis Services  
  
1.  Dans le volet **Données du rapport** , cliquez avec le bouton droit sur le nom de la source de données qui se connecte à une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis cliquez sur **Ajouter un dataset**.  
  
2.  Dans la boîte de dialogue **Propriétés du dataset** , tapez un nom dans la zone de texte **Nom** .  
  
3.  Dans la zone **Source de données**, vérifiez que le nom est celui d’une source de données qui se connecte à une source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
4.  Cliquez sur **Concepteur de requêtes** pour ouvrir le concepteur de requêtes graphique afin de créer une requête de manière interactive. Si le concepteur de requêtes s’ouvre en mode MDX, cliquez sur **Type de commande DMX** (![Basculer vers la vue langage de requête DMX](../../reporting-services/report-data/media/rsqdicon-commandtypedmx.gif "Basculer vers la vue langage de requête DMX")) dans la barre d’outils pour basculer vers le concepteur de requêtes d’exploration de données. Pour plus d’informations, consultez [Interface utilisateur du Concepteur de requêtes DMX Analysis Services](../../reporting-services/report-data/analysis-services-dmx-query-designer-user-interface.md).  
  
     Vous pouvez également importer une requête DMX existante à partir d’un autre rapport. Pour ce faire, cliquez sur **Importer**, puis naviguez jusqu’au fichier .rdl contenant la requête DMX. L'importation d'une requête à partir d'un fichier .dmx n'est pas prise en charge.  
  
5.  Après avoir créé et exécuté votre requête pour afficher des exemples de résultats, cliquez sur **OK**.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Le dataset et sa collection de champs s'affichent dans le volet des données de rapport sous le nœud de source de données.  
  
## <a name="see-also"></a> Voir aussi  
 [Type de connexion Analysis Services pour DMX &#40;SSRS&#41;](../../reporting-services/report-data/analysis-services-connection-type-for-dmx-ssrs.md)   
 [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)   
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
  
