---
title: Type de connexion OLE DB (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d00cb13b-e1c2-4300-a195-3da1430a2df1
caps.latest.revision: 8
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d60ce18616cbd27625dfbdb35e2f1a3f70020fa0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-connection-type-ssrs"></a>Type de connexion OLE DB (SSRS)
  Pour inclure les données d'un fournisseur de données OLE DB, vous devez avoir un dataset basé sur une source de données de rapport de type OLE DB. Ce type de source de données intégré est basé sur l’extension pour le traitement des données OLE DB de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 OLE DB est une technologie d'accès aux données qui permet aux clients de se connecter à divers fournisseurs de données. Après avoir sélectionné le type de source de données OLE DB, vous devez sélectionner un fournisseur de données spécifique. La prise en charge des fonctionnalités telles que les paramètres et les informations d'identification dépend du fournisseur de données que vous sélectionnez.  
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="Connection"></a> Chaîne de connexion  
 La chaîne de connexion de l'extension pour le traitement des données OLE DB dépend du fournisseur de données souhaité. Une chaîne de connexion classique contient des paires nom/valeur prises en charge par le fournisseur de données. Par exemple, la chaîne de connexion suivante spécifie le fournisseur OLE DB pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et la base de données AdventureWorks :  
  
```  
Provider=SQLNCLI10.1;Data Source=server; Initial Catalog=AdventureWorks  
```  
  
 La chaîne de connexion que vous utilisez dépend de la source de données externe à laquelle vous vous connectez. Pour définir des propriétés de chaîne de connexion propres à un fournisseur de données, cliquez sur le bouton **Générer** de la page **Général** de la boîte de dialogue **Propriétés de la source de données** pour ouvrir la boîte de dialogue **Propriétés de connexion** . Définissez des propriétés de source de données étendues via la boîte de dialogue **Propriétés de liaison de données** .  
  
 Pour obtenir des exemples de chaînes de connexion, consultez [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34).  
  
  
##  <a name="Credentials"></a> Informations d'identification  
 Les informations d'identification sont obligatoires pour exécuter des requêtes, afficher l'aperçu du rapport localement et afficher l'aperçu du rapport à partir du serveur de rapports.  
  
 Après avoir publié votre rapport, vous pouvez devoir modifier les informations d'identification pour la source de données afin que les autorisations soient valides pour récupérer les données lorsque le rapport s'exécute sur le serveur de rapports.  
  
 Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) ou [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
###### <a name="special-characters-in-a-password"></a>Caractères spéciaux dans un mot de passe  
 Si vous configurez la source de données OLE DB pour la saisie d'un mot de passe ou l'insertion du mot de passe dans la chaîne de connexion, et si un utilisateur entre le mot de passe avec des caractères spéciaux (par exemple, des signes de ponctuation), certains pilotes de sources de données sous-jacents ne peuvent pas valider les caractères spéciaux. Lors du traitement du rapport, le message « Mot de passe non valide » peut s'afficher et signaler ce problème.  
  
> [!NOTE]  
>  Il est recommandé de ne pas ajouter d'informations de connexion, notamment des mots de passe, à la chaîne de connexion. Le Générateur de rapports comprend un onglet distinct dans la boîte de dialogue **Source de données** , où vous pouvez entrer les informations d'identification.  
  
  
##  <a name="Parameters"></a> Paramètres  
 Certains fournisseurs OLE DB prennent en charge les paramètres nommés et sans nom. Les paramètres sont passés par position via un espace réservé dans la requête. Le caractère d'espace réservé est déterminé par la syntaxe prise en charge par le fournisseur de données.  
  
  
##  <a name="Remarks"></a> Notes  
 OLE DB est une technologie native de création de fournisseurs de données pour des sources de données spécifiques. OLE DB est basé sur les interfaces COM (Component Object Model). OLE DB est une technologie postérieure à ODBC et antérieure aux fournisseurs de données ADO.NET. Les fournisseurs de données OLE DB sont inscrits auprès du système d'exploitation comme tout autre composant COM. Les fournisseurs de données OLE DB sont disponibles auprès de Microsoft et d'éditeurs tiers. Microsoft fournit également MSDASQL, un fournisseur de données OLE DB qui lie la communication aux pilotes ODBC. Pour plus d’informations, consultez [Type de connexion ODBC &#40;SSRS&#41;](../../reporting-services/report-data/odbc-connection-type-ssrs.md).  
  
 Pour récupérer correctement les données de votre choix, vous devez spécifier une syntaxe de requête prise en charge par le fournisseur de données. La prise en charge des paramètres varie selon le fournisseur de données. Pour plus d'informations, consultez les rubriques spécifiques au fournisseur de données sélectionné. Exemple :  
  
-   [Fournisseur OLE DB Analysis Services &#40;Analysis Services – Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/cdeecd50-1d91-4162-a4a2-01c7799b02a8)  
  
-   [Utilisation du fournisseur de données .NET Framework pour Oracle](http://go.microsoft.com/fwlink/?LinkId=112314)  
  
-   [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
 Pour plus d’informations sur les fournisseurs de données OLE DB spécifiques, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
##  <a name="HowTo"></a> Rubriques de procédures  
 Cette section contient des instructions pas à pas sur l'utilisation des connexions de données, des sources de données et des datasets.  
  
 [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Ajouter un filtre à un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="Related"></a> Sections connexes  
 Ces sections de la documentation fournissent des informations de fond d'ordre conceptuel sur les données de rapport, ainsi que des informations sur les procédures de définition, de personnalisation et d'utilisation des parties d'un rapport qui sont liées aux données.  
  
 [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
 Fournit une vue d'ensemble de l'accès aux données pour votre rapport.  
  
 [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34)  
 Fournit des informations sur les connexions de données et les sources de données.  
  
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 Fournit des informations sur les datasets incorporés et partagés.  
  
 [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 Fournit des informations sur la collection de champs de dataset générée par la requête.  
  
 [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 Fournit des informations détaillées sur la prise en charge des plateformes et des versions pour chaque extension de données.  
  
  
## <a name="see-also"></a> Voir aussi  
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  
