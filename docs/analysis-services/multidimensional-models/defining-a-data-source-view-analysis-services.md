---
title: Définition des données de Source de vue (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6966a0763146c9fd787be39d5be011704c048f66
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="defining-a-data-source-view-analysis-services"></a>Définition d'une vue de source de données (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une vue de source de données contient le modèle logique du schéma utilisé par les objets multidimensionnels de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , autrement dit des cubes, des dimensions et des structures d’exploration de données. Une vue de source de données représente la définition, stockée au format XML, des métadonnées de ces éléments de schéma utilisés par le modèle UDM (Unified Dimensional Model) et par les structures d'exploration de données. Une vue de source de données :  
  
-   contient les métadonnées représentant des objets sélectionnés à partir d'une ou de plusieurs sources de données sous-jacentes ou les métadonnées qui seront utilisées pour générer une banque de données relationnelles sous-jacente, si vous suivez l'approche verticale pour la génération du schéma ;  
  
-   peut être élaborée à partir d'une ou plusieurs sources de données, ce qui vous permet de définir des objets multidimensionnels et des objets d'exploration de données qui intègrent des données de diverses sources ;  
  
-   peut contenir des relations, des clés primaires, des noms d'objets, des colonnes calculées et des requêtes qui ne figurent pas dans une source de données sous-jacente et qui existent indépendamment des sources de données sous-jacentes ;  
  
-   ne peut pas être visualisée ou interrogée par les applications clientes.  
  
 Une vue de source de données est un composant requis d'un modèle multidimensionnel. La plupart des développeurs Analysis Services créent une vue de source de données pendant les premières phases de conception de modèle, générant au moins une vue à partir d'une base de données relationnelle externe qui fournit des données sous-jacentes. Toutefois, vous pouvez aussi créer la vue de source de données lors d'une étape ultérieure, en générant le schéma et les structures de base de données sous-jacentes une fois que les dimensions et les cubes sont créés. Cette seconde approche, parfois appelée construction verticale, sert fréquemment à créer des prototypes et des modèles d'analyse. Lorsque vous utilisez cette approche, vous faites appel à l'Assistant Génération de schéma pour créer la vue de source de données et les objets de source de données sous-jacents selon les objets OLAP définis dans un projet ou une base de données Analysis Services. Indépendamment de la méthode et du moment de création d'une vue de source de données, chaque modèle doit avoir une vue avant de pouvoir le traiter.  
  
 Cette rubrique comprend les sections suivantes :  
  
 [Composition de la vue de source de données](#bkmk_dsvdef)  
  
 [Créer une vue DSV à l'aide de l'Assistant Vue de source de données](#bkmk_startWiz)  
  
 [Spécifier des critères de correspondance de nom pour les relations](#bkmk_NameMatch)  
  
 [Ajouter une source de données secondaire](#bkmk_secondaryDS)  
  
##  <a name="bkmk_dsvdef"></a> Composition de la vue de source de données  
 Une vue de source de données contient les éléments suivants :  
  
-   un nom et une description ;  
  
-   une définition de n'importe quel sous-ensemble du schéma récupéré dans une ou plusieurs sources de données, antérieur à et incluant la totalité du schéma, notamment :  
  
    -   les noms de tables ;  
  
    -   les noms de colonnes ;  
  
    -   les types de données ;  
  
    -   la possibilité de valeurs nulles ;  
  
    -   la longueur des colonnes ;  
  
    -   Clés primaires.  
  
    -   les relations entre les clés primaires et les clés étrangères ;  
  
-   les annotations du schéma dans les sources de données sous-jacentes, notamment :  
  
    -   les noms conviviaux des tables, des vues et des colonnes ;  
  
    -   les requêtes nommées qui retournent les colonnes d'une ou plusieurs sources de données (qui apparaissent sous la forme de tables dans le schéma) ;  
  
    -   les calculs nommés qui retournent les colonnes d'une source de données (qui apparaissent sous la forme de colonnes dans les tables ou les vues) ;  
  
    -   les clés primaires logiques (nécessaires si aucune clé primaire n'est définie dans la table sous-jacente ou incluse dans la vue ou la requête nommée) ;  
  
    -   les relations clé primaire logique/clé étrangère entre les tables, les vues et les requêtes nommées.  
  
##  <a name="bkmk_startWiz"></a> Créer une vue DSV à l'aide de l'Assistant Vue de source de données  
 Pour créer une vue de source de données, exécutez l'Assistant Vue de source de données à partir de l'Explorateur de solutions dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
> [!NOTE]  
>  Sinon, commencez par construire des dimensions et des cubes, puis générez une vue de source de données pour le modèle à l'aide de l'Assistant Génération de schéma. Pour plus d’informations, consultez [Assistant Génération de schéma &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/schema-generation-wizard-analysis-services.md).  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Vues des sources de données, puis sélectionnez **Nouvelle vue de source de données**.  
  
2.  Spécifiez un nouvel objet de source de données, ou un objet de source de données existant, qui fournit des informations de connexion à une base de données relationnelle externe (vous ne pouvez sélectionner qu'une seule source de données dans l'Assistant).  
  
3.  Sur la même page, cliquez sur **Avancé** pour sélectionner des schémas spécifiques, appliquer un filtre ou exclure des informations de relation entre tables.  
  
     **Sélectionner les schémas**  
  
     Pour les sources de données volumineuses contenant plusieurs schémas, vous pouvez sélectionner les schémas à utiliser dans une liste séparée par des virgules, sans espaces.  
  
     **Extraire des relations**  
  
     Vous pouvez omettre volontairement les informations de relation entre tables en décochant la case **Extraire les relations** dans la boîte de dialogue Options avancées des vues de source de données, qui vous permet de créer manuellement des relations entre les tables dans le Concepteur de vue de source de données.  
  
4.  **Filtrer les objets disponibles**  
  
     Si la liste des objets disponibles contient un grand nombre d'objets, vous pouvez réduire cette liste en appliquant un filtre simple qui spécifie une chaîne comme critère de sélection. Par exemple, si vous tapez **dbo** et que vous cliquez sur le bouton **Filtre** , seuls les éléments commençant par « dbo » s’affichent dans la liste **Objets disponibles** . Le filtre peut être une chaîne partielle (par exemple, « sal » retourne salaire et salle), mais il ne peut pas inclure plusieurs chaînes ou opérateurs.  
  
5.  Pour les sources de données relationnelles qui n’ont aucune relation entre tables définie, une page **Correspondance de noms** apparaît afin que vous puissiez sélectionner la méthode de correspondance de noms appropriée. Pour plus d’informations, consultez la section [Spécifier des critères de correspondance de nom pour les relations](#bkmk_NameMatch) dans cette rubrique.  
  
##  <a name="bkmk_secondaryDS"></a> Ajouter une source de données secondaire  
 Lorsque vous définissez une vue de source de données qui contient des tables, des vues ou des colonnes de plusieurs sources de données, la première source de données à partir de laquelle vous ajoutez des objets à la vue de source de données est désignée comme étant la source de données primaire (vous ne pourrez pas la changer une fois qu'elle sera définie). Après avoir défini une vue de source de données reposant sur des objets d'une seule source de données, vous pouvez ajouter des objets d'autres sources de données.  
  
 Si une requête de traitement analytique en ligne (OLAP) ou une requête d’exploration de données nécessite des données de plusieurs sources de données dans une seule requête, la source de données primaire doit prendre en charge les requêtes distantes à l’aide d’une requête **OpenRowset**. Il s’agit généralement d’une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par exemple, si vous créez une dimension OLAP qui contient des attributs liés aux colonnes de plusieurs sources de données, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] construit une requête **OpenRowset** pour peupler cette dimension lors du traitement. Cependant, si un objet OLAP peut être peuplé ou si une requête d’exploration de données peut être résolue depuis une seule source de données, aucune requête **OpenRowset** ne sera construite. Dans certaines situations, vous pourrez définir des relations d’attribut pour éviter la création d’une requête **OpenRowset** . Pour plus d’informations sur les relations d’attributs, consultez [Relations d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attribute-relationships.md), [Ajout ou suppression de tables ou de vues dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/adding-or-removing-tables-or-views-in-a-data-source-view-analysis-services.md) et [Définir des relations d’attributs](../../analysis-services/multidimensional-models/attribute-relationships-define.md).  
  
 Pour ajouter des tables et des colonnes d'une deuxième source de données, double-cliquez sur la vue DSV dans l'Explorateur de solutions afin de l'ouvrir dans le concepteur de vue de source de données, puis utilisez la boîte de dialogue Ajouter/supprimer des tables pour inclure des objets d'autres sources de données définies dans votre projet. Pour plus d’informations, consultez [Ajout ou suppression de tables ou de vues dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/adding-or-removing-tables-or-views-in-a-data-source-view-analysis-services.md).  
  
##  <a name="bkmk_NameMatch"></a> Spécifier des critères de correspondance de nom pour les relations  
 Lorsque vous créez une vue DSV, des relations sont créées entre les tables en fonction de contraintes de clé étrangère dans la source de données. Ces relations sont nécessaires au moteur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour construire les requêtes d'exploration de données et de traitement analytique en ligne (OLAP) appropriées. Toutefois, il arrive parfois qu'une source de données contenant plusieurs tables ne comporte aucune contrainte de clé étrangère. Le cas échéant, l'Assistant Vue de source de données vous invite à définir le mode de mise en correspondance des noms de colonnes de différentes tables.  
  
> [!NOTE]  
>  Vous êtes invité à fournir des critères de correspondance de nom uniquement si aucune relation de clé étrangère n'est détectée dans la source de données sous-jacente. Si des relations de clé étrangère sont détectées, elles seront utilisées et vous devrez définir manuellement toute relation supplémentaire à inclure dans la vue de source de données DSV, notamment les clés primaires logiques. Pour plus d’informations, consultez [Définir des relations logiques dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-logical-relationships-in-a-data-source-view-analysis-services.md) et [Définir des clés primaires logiques dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-logical-primary-keys-in-a-data-source-view-analysis-services.md).  
  
 L'Assistant Vue de source de données utilise votre réponse pour faire correspondre les noms de colonnes et créer des relations entre les différentes tables dans la vue DSV. Les critères que vous pouvez spécifier sont répertoriés dans le tableau suivant.  
  
|Critères de correspondance de nom| Description|  
|----------------------------|-----------------|  
|**Nom identique à la clé primaire**|Le nom de la colonne clé étrangère de la table source est le même que le nom de la colonne clé primaire de la table de destination. Par exemple, la colonne de clé étrangère `Order.CustomerID` correspond à la colonne clé primaire `Customer.CustomerID`.|  
|**Nom identique à celui de la table de destination**|Le nom de la colonne clé étrangère de la table source est le même que le nom de la table de destination. Par exemple, la colonne de clé étrangère `Order.Customer` correspond à la colonne clé primaire `Customer.CustomerID`.|  
|**Nom de la table de destination + nom de la clé primaire**|Le nom de la colonne clé étrangère de la table source correspond au nom de la table de destination concaténé avec le nom de la colonne clé primaire. Il est possible de séparer les noms concaténés par un espace ou un trait de soulignement (_). Par exemple, les paires clé étrangère-clé primaire suivantes sont toutes équivalentes :<br /><br /> `Order.CustomerID` et `Customer.ID`<br /><br /> `Order.Customer ID` et `Customer.ID`<br /><br /> `Order.Customer_ID` et `Customer.ID`|  
  
 Le critère que vous sélectionnez modifie le paramètre de propriété **NameMatchingCriteria** de la vue de source de données. Ce paramètre détermine la manière dont l'Assistant ajoute les tables associées. Lorsque vous modifiez la vue de source de données avec le concepteur de vue de source de données, cette spécification détermine la manière dont le concepteur met les colonnes en correspondance afin de créer des relations entre les tables de la vue DSV. Vous pouvez modifier le paramètre de propriété **NameMatchingCriteria** dans le concepteur de vue de source de données. Pour plus d’informations, consultez [Modifier les propriétés d’une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/change-properties-in-a-data-source-view-analysis-services.md).  
  
> [!NOTE]  
>  Une fois que vous avez terminé l'exécution de l'Assistant Vue de source de données, vous pouvez ajouter ou supprimer des relations dans le volet Schéma du Concepteur de vue de source de données. Pour plus d’informations, consultez [Définir des relations logiques dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-logical-relationships-in-a-data-source-view-analysis-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Ajout ou suppression de tables ou de vues dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/adding-or-removing-tables-or-views-in-a-data-source-view-analysis-services.md)   
 [Définir des clés primaires logiques dans une vue de Source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-logical-primary-keys-in-a-data-source-view-analysis-services.md)   
 [Définir des calculs nommés dans une vue de Source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-named-calculations-in-a-data-source-view-analysis-services.md)   
 [Définir des requêtes nommées dans une vue de Source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-named-queries-in-a-data-source-view-analysis-services.md)   
 [Remplacer une Table ou une requête nommée dans une vue de Source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/replace-a-table-or-a-named-query-in-a-data-source-view-analysis-services.md)   
 [Utiliser des diagrammes dans le Concepteur de vue de Source de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/work-with-diagrams-in-data-source-view-designer-analysis-services.md)   
 [Explorer les données dans une vue de Source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/explore-data-in-a-data-source-view-analysis-services.md)   
 [Supprimer une vue de Source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/delete-a-data-source-view-analysis-services.md)   
 [Actualiser le schéma dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/refresh-the-schema-in-a-data-source-view-analysis-services.md)  
  
  
