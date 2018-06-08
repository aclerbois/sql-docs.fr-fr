---
title: Type de connexion Analysis Services pour MDX (SSRS) | Microsoft Docs
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
ms.assetid: bd2e7148-3124-4e07-9734-22333127c3be
caps.latest.revision: 10
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 553fb7ac9b7346330a3fed798eba0d1e039fe267
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="analysis-services-connection-type-for-mdx-ssrs"></a>Type de connexion Analysis Services pour MDX (SSRS)
  Pour inclure des données d’un cube [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans votre rapport, vous devez avoir un dataset basé sur une source de données de rapport de type [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Ce type de source de données intégré est basé sur l'extension de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez récupérer les métadonnées relatives aux dimensions, hiérarchies, niveaux, indicateurs de performance clés (KPI), mesures et attributs d'un cube [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] afin de les utiliser comme données de rapport.  
  
 Cette extension pour le traitement des données prend en charge des paramètres à valeurs multiples, des agrégats de serveur et des informations d'identification qui sont gérés séparément de la chaîne de connexion.  
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="Connection"></a> Chaîne de connexion  
 Quand vous vous connectez à un cube [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous vous connectez à l’objet de base de données d’une instance d’Analysis Services sur un serveur. La base de données peut avoir plusieurs cubes. Vous spécifiez le cube dans le concepteur de requêtes lorsque vous générez la requête. L’exemple suivant affiche une chaîne de connexion :  
  
```  
data source=<server name>;initial catalog=<database name>  
```  
  
 Pour obtenir d’autres exemples sur les chaînes de connexion, consultez [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34).  
  
  
##  <a name="Credentials"></a> Informations d'identification  
 Les informations d'identification sont obligatoires pour exécuter des requêtes, afficher l'aperçu du rapport localement et afficher l'aperçu du rapport à partir du serveur de rapports.  
  
 Après avoir publié votre rapport, vous pouvez devoir modifier les informations d'identification pour la source de données afin que les autorisations soient valides pour récupérer les données lorsque le rapport s'exécute sur le serveur de rapports.  
  
 Sur un client de création de rapports, les options suivantes sont disponibles pour spécifier des informations d'identification :  
  
-   Utilisateur Windows actuel (également appelé sécurité intégrée).  
  
-   Utiliser un nom d'utilisateur et un mot de passe enregistrés.  
  
-   demander à l'utilisateur des informations d'identification ; Cette option prend uniquement en charge la sécurité intégrée de Windows.  
  
-   Aucune information d'identification n'est requise. Pour utiliser cette option, vous devez avoir configuré le compte d'exécution sans assistance sur le serveur de rapports. Pour plus d’informations, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md) dans la [documentation Reporting Services](http://go.microsoft.com/fwlink/?linkid=121312) sur msdn.microsoft.com.  
  
 Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) ou [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
  
##  <a name="Query"></a> Requêtes  
 Après avoir établi une connexion de données à une source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous créez un dataset et définissez une requête MDX (Multidimensional Expression) qui spécifie les données à récupérer à partir du cube. Utilisez le concepteur de requêtes graphique MDX pour parcourir les différentes structures de données sous-jacentes de la source de données et effectuer une sélection.  
  
 Vous pouvez spécifier une requête de plusieurs façons :  
  
-   Générer une requête de manière interactive. Le concepteur de requêtes MDX Analysis Services prend en charge les vues suivantes :  
  
    -   **Mode Création** Faites glisser des dimensions, des membres, des propriétés de membre, des mesures et des indicateurs de performance clés du navigateur de métadonnées vers le volet **Données** dans le but de générer une requête MDX. Faites glisser les membres calculés du volet CalculatedMembers vers le volet Données pour définir d’autres champs de dataset.  
  
    -   **Affichage des requêtes** Faites glisser des dimensions, des membres, des propriétés de membre, des mesures et des indicateurs de performance clés du navigateur de métadonnées vers le volet Requête dans le but de générer une requête MDX. Il est possible de modifier le texte MDX directement dans le volet Requête. Faites glisser les membres calculés du volet CalculatedMembers vers le volet Requête pour définir d’autres champs de dataset.  
  
     Pour plus d’informations, consultez [Interface utilisateur du Concepteur de requêtes MDX Analysis Services &#40;Générateur de rapports&#41;](http://msdn.microsoft.com/library/7e288eee-2d37-485e-a6a0-dbba5e041e26).  
  
-   Importer une requête MDX existante à partir d'un rapport. Utilisez le bouton **Importer une requête** pour rechercher un fichier .rdl et importer une requête. Vous pouvez importer une requête à partir d'un rapport qui contient un dataset incorporé basé sur une source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . L'importation d'une requête MDX directement à partir d'un fichier .mdx n'est pas prise en charge.  
  
 Au moment de la conception, exécutez la requête pour afficher un jeu de résultats. Les résultats de la requête sont automatiquement récupérés comme un ensemble de lignes aplati. Les colonnes dans le jeu de résultats d'une requête remplissent la collection de champs pour un dataset. Après avoir généré la requête, affichez la collection de champs de dataset générée à partir des métadonnées dans le volet des données de rapport. Lorsque le rapport s'exécute, les données réelles sont retournées à partir de la source de données externe.  
  
 L’extension pour le traitement des données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge les propriétés de champ de dataset étendues. Il s'agit des valeurs disponibles dans la source de données externe mais qui ne s'affichent pas dans le volet des données de rapport. Vous pouvez utiliser les propriétés de champ étendues prises en charge par l’extension pour le traitement des données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans votre rapport par le biais de la collection **Fields** intégrée. Pour les propriétés qui comprennent des valeurs dans la source de données, vous pouvez accéder à des valeurs de propriété prédéfinies telles que **FormattedValue**, **Color**ou **UniqueName**. Pour plus d’informations, consultez [Propriétés de champ étendues pour une base de données Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
  
##  <a name="Parameters"></a> Paramètres  
 Pour inclure les paramètres de requête, créez un filtre dans la zone de filtre du concepteur de requêtes et marquez le filtre en tant que paramètre. Pour chaque filtre, un dataset est créé automatiquement afin de fournir les valeurs disponibles. Par défaut, ces datasets n'apparaissent pas dans le volet Données du rapport. Pour plus d’informations, consultez [Définir des paramètres dans le Concepteur de requêtes MDX pour Analysis Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/define-parameters-in-the-mdx-query-designer-for-analysis-services.md) et [Afficher des datasets masqués pour les valeurs de paramètres des données multidimensionnelles &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/show-hidden-datasets-for-parameter-values-multidimensional-data.md).  
  
 Par défaut, chaque paramètre de rapport a le type de données **Texte**. Après avoir créé les paramètres de rapport, vous devrez peut-être modifier les valeurs par défaut. Pour plus d'informations, consultez [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
  
##  <a name="Remarks"></a> Notes  
 L'extension de données Analysis Services est basée sur le protocole XMLA (XML for Analysis). Les jeux de résultats de cubes sont récupérés via le protocole XMLA sous la forme d'un ensemble de lignes aplati. Les hiérarchies déséquilibrées ne sont pas prises en charge. Pour plus d’informations, consultez [Hiérarchies déséquilibrées](../../analysis-services/multidimensional-models/user-defined-hierarchies-ragged-hierarchies.md).  
  
 Vous pouvez également récupérer les données d’un cube [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à partir du type de source de données OLE DB. Pour plus d’informations, consultez [Type de connexion OLE DB &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md).  
  
 Pour plus d’informations sur la prise en charge des versions, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
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
  
 [Propriétés de champ étendues pour une base de données Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md)  
 Fournit des informations sur les champs supplémentaires disponibles via le fournisseur de données XMLA.  
  
 [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) dans la section [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de la [documentation en ligne](http://go.microsoft.com/fwlink/?linkid=121312) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 Fournit des informations détaillées sur la prise en charge des plateformes et des versions pour chaque extension de données.  
  
  
## <a name="see-also"></a> Voir aussi  
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  
