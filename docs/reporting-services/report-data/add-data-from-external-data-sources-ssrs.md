---
title: Ajouter des données à partir de sources de données externes (SSRS) | Microsoft Docs
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
ms.assetid: 924a2ec3-150c-4bb2-83c9-4c7b440e8c03
caps.latest.revision: 12
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: fa81a4bd306e62c7f370cb024b5289f8db7421be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-data-from-external-data-sources-ssrs"></a>Ajouter des données à partir de sources de données externes (SSRS)
  Pour récupérer des données à partir d'une source de données externe, vous utilisez une connexion de données. Les informations de connexion de données sont fournies habituellement par le propriétaire de la source de données externe, qui est chargé d'accorder les autorisations nécessaires et de spécifier les types d'informations d'identification à utiliser. Les informations de connexion de données sont enregistrées en tant que source de données de rapport. Le type de source de données spécifie l'extension de données à utiliser pour récupérer les données.  
  
 Pour plus d’informations sur les types de sources de données, consultez [Dans cette section](#InThisSection).  
  
##  <a name="DataAccess"></a> Fonctionnement de la technologie d'accès aux données  
 La récupération des données pour un dataset de rapport requiert plusieurs couches de logiciels d'accès aux données. La liste suivante fournit une description simple de l'utilisation des technologies d'accès aux données par les rapports :  
  
-   **Application et interface utilisateur** Application Générateur de rapports que vous utilisez pour créer une source de données, ajouter une référence à une source de données partagée, ajouter un dataset partagé, ou ajouter une partie de rapport qui inclut les sources de données et datasets dont elle dépend.  
  
-   **Éléments de définition de rapport** Les sources de données et datasets font partie de la définition de rapport. Après la publication d'un rapport sur un serveur de rapports, les sources de données partagées et les datasets partagés sont gérés indépendamment de la définition de rapport.  
  
    -   **Source de données et source de données partagée** Partie d’une définition de rapport qui inclut les informations relatives au type de l’extension pour le traitement des données, les informations de connexion et l’authentification.  
  
    -   **Dataset et collection de champs** Partie d’une définition de rapport qui inclut la requête, la collection de champs et les types de données des champs.  
  
-   **Extensions de données Reporting Services** Extensions de données intégrées installées avec le Générateur de rapports. Une extension de données fournit des fonctionnalités qui gèrent l'authentification, les agrégats de serveur et les paramètres à valeurs multiples.  
  
-   **Fournisseur de données** Logiciel qui gère la connexion et la récupération des données à partir de la source de données externe. Le fournisseur de données définit la syntaxe de la chaîne de connexion. La plupart des extensions de données sont créées au-dessus d'une couche de fournisseur de données.  
  
-   **Source de données externe** Emplacement où récupérer les données du rapport, par exemple une base de données, un fichier, un cube ou un service web.  
  
> [!NOTE]  
>  Lorsque vous n'êtes pas connecté à un serveur de rapports, vous pouvez choisir l'une des extensions de données installées avec le Générateur de rapports. Vous accédez aux données en tant qu'utilisateur unique à l'aide des informations d'identification spécifiques à votre ordinateur. Lorsque vous êtes connecté à un serveur de rapports, vous pouvez choisir l'une des extensions de données installées sur le serveur de rapports. Vous accédez aux données en tant qu'utilisateur faisant partie des multiples utilisateurs qui exécutent le rapport, et vous utilisez les informations d'identification du serveur de rapports. Pour plus d’informations, consultez [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
##  <a name="ReportData"></a> Fonctionnement des données de rapport  
 Dans sa forme la plus simple, un rapport affiche les données d'un dataset de rapport dans une région de données de la page de rapport, autrement dit, dans un tableau, un graphique ou une matrice unique, ou tout autre type de région de données du rapport. Les données d'un dataset de rapport proviennent du premier jeu de résultats retourné par une commande de requête unique qui s'exécute à partir d'un accès en lecture seule à une source de données externe. Chaque région de données s'étend en fonction de toutes les données du dataset à afficher.  
  
 Les données d'un dataset sont essentiellement tabulaires. Les colonnes sont les champs de la requête de dataset. Les lignes proviennent des lignes du jeu de résultats. Vous pouvez utiliser les types de données généralisés suivants dans un rapport :  
  
-   Données rectangulaires. Données d'un jeu de résultats qui a le même nombre de colonnes dans chaque ligne.  
  
-   Les données hiérarchiques sont prises en charge en tant qu'ensemble de lignes aplati.  
  
    -   Les hiérarchies déséquilibrées, où il existe un nombre distinct de colonnes pour chaque ligne de données, ne sont pas prises en charge. Pour certaines extensions de données, cela a des conséquences.  
  
    -   Les extensions de données qui fonctionnent avec les sources de données multidimensionnelles utilisent le protocole XML for Analysis et récupèrent les données en tant qu'ensemble de lignes aplati et non en tant que jeu de cellules.  
  
    -   L'extension de données XML aplatit automatiquement les données XML pour les utiliser dans un rapport. Si la première instance d'un élément XML n'inclut pas tous les attributs ou sous-éléments, les données risquent de ne pas être disponibles en tant que données de rapport.  
  
-   Les données récursives sont prises en charge. Un jeu de résultats qui contient une hiérarchie de données récursives inclut toutes les informations relatives à la structure de la hiérarchie dans un jeu de résultats rectangulaire. Par exemple, la structure de rapport dans une société peut être représentée par un tableau qui comprend deux colonnes : une pour les employés et une autre pour les responsables. Chaque responsable est également un employé ayant un responsable. Le plus haut responsable contient habituellement une valeur Null ou tout autre identificateur qui indique que cet employé n'a aucun responsable.  
  
  
##  <a name="DataTypes"></a> Utilisation des types de données  
 Lorsque vous créez un dataset, les types de données des champs sont mappés à un sous-ensemble de types de données CLR (Common Language Runtime) du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Les types de données qui ne peuvent pas être clairement mappés sont retournés comme chaînes. Pour plus d’informations sur l’utilisation des types de données de champ, consultez [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md). Lorsque vous créez un paramètre, le type de données doit être un type de données de définition de rapport pris en charge. Pour plus d’informations sur le mappage des types de données du fournisseur de données à un paramètre de rapport, consultez [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md).  
  
  
##  <a name="HowTo"></a> Rubriques de procédures  
 Cette section contient des instructions pas à pas sur l'utilisation des connexions de données, des sources de données et des datasets.  
  
 [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Ajouter un filtre à un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="InThisSection"></a> Dans cette section  
 Les rubriques suivantes fournissent des informations sur chaque extension de données intégrée.  
  
|Rubrique|Type de source de données|  
|-----------|----------------------|  
|[Type de connexion SQL Server &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|[Type de connexion Analysis Services pour MDX &#40;SSRS&#41;](../../reporting-services/report-data/analysis-services-connection-type-for-mdx-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)])|  
|[Type de connexion PowerPivot &#40;SSRS&#41;](../../reporting-services/report-data/power-pivot-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|  
|[Type de connexion de liste SharePoint &#40;SSRS&#41;](../../reporting-services/report-data/sharepoint-list-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Liste SharePoint|  
|[Type de connexion SQL Azure &#40;SSRS&#41;](../../reporting-services/report-data/sql-azure-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|[Type de connexion à un entrepôt de données SQL Server Parallel Data Warehouse &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-parallel-data-warehouse-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDWfull](../../includes/ssdwfull-md.md)]|  
|[Type de connexion SAP NetWeaver BI &#40;SSRS&#41;](../../reporting-services/report-data/sap-netweaver-bi-connection-type-ssrs.md)|SAP NetWeaver BI|  
|[Type de connexion Hyperion Essbase &#40;SSRS&#41;](../../reporting-services/report-data/hyperion-essbase-connection-type-ssrs.md)|Hyperion Essbase|  
|[Type de connexion OLE DB &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md)|OLE DB|  
|[Type de connexion ODBC &#40;SSRS&#41;](../../reporting-services/report-data/odbc-connection-type-ssrs.md)|ODBC|  
|[Type de connexion XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-connection-type-ssrs.md)|XML|  
|[Connexion à un modèle de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-model-connection-ssrs.md)|Modèle .smdl|  
  
  
##  <a name="Related"></a> Sections connexes  
 Ces sections de la documentation fournissent des informations de fond d'ordre conceptuel sur les données de rapport, ainsi que des informations sur les procédures de définition, de personnalisation et d'utilisation des parties d'un rapport qui sont liées aux données.  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)|Fournit une vue d'ensemble de l'accès aux données pour votre rapport.|  
|[Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34)|Fournit des informations sur les connexions de données et les sources de données.|  
|[Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)|Fournit des informations sur les datasets incorporés et partagés.|  
|[Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)|Fournit des informations sur la collection de champs de dataset générée par la requête.|  
|[Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|Fournit des informations détaillées sur la prise en charge des plateformes et des versions pour chaque extension de données.|  
|[Vue d’ensemble des extensions pour le traitement des données](../../reporting-services/extensions/data-processing/data-processing-extensions-overview.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [de](http://go.microsoft.com/fwlink/?linkid=121312).|Fournit des informations détaillées sur les extensions de données pour les utilisateurs expérimentés.|  
  
  
## <a name="see-also"></a> Voir aussi  
 [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)   
 [Concepteurs de requêtes &#40;Générateur de rapports&#41;](http://msdn.microsoft.com/library/553f0d4e-8b1d-4148-9321-8b41a1e8e1b9)  
  
  
