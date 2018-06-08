---
title: Recherche floue, transformation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.fuzzylookuptrans.f1
- sql13.dts.designer.fuzzylookuptransformation.referencetable.f1
- sql13.dts.designer.fuzzylookuptransformation.columns.f1
- sql13.dts.designer.fuzzylookuptransformation.advanced.f1
helpviewer_keywords:
- cleaning data
- comparing data
- token delimiters [Integration Services]
- temporary indexes [Integration Services]
- temporary tables [Integration Services]
- Fuzzy Lookup transformation
- reference tables [Integration Services]
- match similar data [Integration Services]
- replacing missing values
- correcting data [Integration Services]
- cache [Integration Services]
- standardizing data [Integration Services]
- lookups [Integration Services]
- confidence scores [Integration Services]
- fuzzy matches
- missing values replaced [Integration Services]
- similarity thresholds [Integration Services]
ms.assetid: 019db426-3de2-4ca9-8667-79fd9a47a068
caps.latest.revision: 75
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d4d43876a38188ce181c80bd625c50128bbc608c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fuzzy-lookup-transformation"></a>transformation de recherche floue
  La transformation de recherche floue effectue des tâches de nettoyage des données telles que la standardisation des données, la correction des données et la fourniture de données manquantes.  
  
> [!NOTE]  
>  Pour plus d’informations sur la transformation de recherche floue, y compris les limitations en termes de performances et de mémoire, consultez le livre blanc, [Présentation des transformations Fuzzy Lookup (recherche approximative) et Fuzzy Grouping (regroupement approximatif) dans les services DTS (Data Transformation Services) de SQL Server 2005](http://go.microsoft.com/fwlink/?LinkId=96604).  
  
 La transformation de recherche floue diffère de la transformation de recherche par son utilisation de la correspondance approximative. La transformation de recherche utilise une équijointure pour localiser les enregistrements équivalents dans la table de référence. Elle retourne les enregistrements avec au moins un enregistrement correspondant et les enregistrements sans enregistrements correspondants. Par contre, la transformation de recherche floue utilise la correspondance floue pour renvoyer un ou plusieurs résultats dont la correspondance est proche dans la table de référence.  
  
 Une transformation de recherche floue suit souvent une transformation de recherche dans le flux de données d'un package. D'abord, la transformation de recherche tente de trouver une correspondance exacte. Si elle échoue, la transformation de recherche floue fournit des correspondances proches dans la table de référence.  
  
 La transformation doit accéder à la source de données de référence contenant les valeurs utilisées pour nettoyer et étendre les données d'entrée. La source de données de référence doit être une table d’une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La correspondance entre la valeur dans une colonne d'entrée et la valeur dans la table de référence doit être une correspondance exacte ou approximative. Cependant, la transformation nécessite au moins une correspondance de colonne pour être configurée en correspondance floue. Si vous ne souhaitez utiliser que la correspondance exacte, utilisez plutôt la transformation de recherche.  
  
 Cette transformation a une entrée et une sortie.  
  
 Seules les colonnes d’entrée avec les types de données **DT_WSTR** et **DT_STR** peuvent être utilisées dans la correspondance approximative. La correspondance exacte peut être appliquée aux colonnes de n’importe quel type de données, à l’exception des types **DT_TEXT**, **DT_NTEXT**et **DT_IMAGE**. Pour plus d’informations, consultez [Integration Services Data Types](../../../integration-services/data-flow/integration-services-data-types.md). Les colonnes participant à la jointure entre l'entrée et la table de référence doivent avoir des types de données compatibles. Par exemple, il est valide d’effectuer une jointure entre une colonne ayant le type de données DTS **DT_WSTR** et une colonne ayant le type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **nvarchar** , mais pas d’effectuer une jointure entre une colonne ayant le type de données **DT_WSTR** et une colonne ayant le type de données **int** .  
  
 Vous pouvez personnaliser cette transformation en spécifiant le volume maximum de mémoire, l'algorithme de comparaison de lignes et la mise en cache d'index et de tables de référence utilisés par la transformation.  
  
 Vous pouvez configurer la quantité de mémoire utilisée par la transformation de recherche floue en définissant la propriété personnalisée MaxMemoryUsage. Vous pouvez spécifier le nombre de mégaoctets (Mo) ou utiliser la valeur 0, ce qui permet à la transformation d'utiliser une quantité de mémoire dynamique basée sur ses besoins et sur la mémoire physique disponible. La propriété personnalisée MaxMemoryUsage peut être mise à jour par une expression de la propriété lors du chargement du package. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41; ](../../../integration-services/expressions/integration-services-ssis-expressions.md), [Utiliser des expressions de propriété dans les packages](../../../integration-services/expressions/use-property-expressions-in-packages.md) et [Propriétés personnalisées des transformation](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
## <a name="controlling-fuzzy-matching-behavior"></a>Contrôle du comportement de la correspondance floue  
 La transformation de recherche floue comprend trois fonctionnalités de personnalisation de la recherche qu'elle effectue : le nombre maximum de correspondances à retourner par ligne d'entrée, les séparateurs de jetons et les seuils de similarité.  
  
 La transformation renvoie zéro ou plusieurs correspondances, jusqu'au nombre de correspondances spécifiées. La spécification du nombre maximal de correspondances ne garantit pas que la transformation renvoie le nombre maximal de correspondances. Elle garantit uniquement que la transformation renvoie un nombre inférieur ou égal de correspondances. Si vous définissez le nombre maximal de correspondances à une valeur supérieure à 1, la sortie de la transformation peut inclure plusieurs lignes par recherche et certaines des lignes peuvent être des doublons.  
  
 La transformation fournit un ensemble par défaut de délimiteurs utilisés pour marquer les données, mais vous pouvez ajouter des délimiteurs de jetons pour répondre aux besoins de vos données. La propriété Delimiters contient les délimiteurs par défaut. La création de jetons est importante car elle définit les unités qui sont comparées entre elles au sein des données.  
  
 Les seuils de similarité peuvent être définis au niveau du composant et de la jointure. La similarité au niveau de la jointure n'est disponible que lorsque la transformation effectue une correspondance floue entre des colonnes dans l'entrée et la table de référence. La plage de similarité va de 0 à 1. Plus le seuil s'approche de 1, plus les lignes et les colonnes doivent être similaires pour être répertoriées comme doublons. Vous spécifiez le seuil de similarité en définissant la propriété MinSimilarity au niveau du composant et de la jointure. Pour obtenir la similarité spécifiée au niveau du composant, toutes les lignes doivent montrer une similarité à travers toutes les correspondances, qui soit supérieure ou égale au seuil de similarité spécifié au niveau du composant. En d'autres termes, vous ne pouvez pas spécifier une correspondance très proche au niveau du composant à moins que les correspondances au niveau de la ligne et de la jointure ne soient également proches.  
  
 Chaque correspondance comprend un score de similarité et un score de confiance. Le score de similarité est une mesure mathématique de la similarité de texture entre l'enregistrement d'entrée et l'enregistrement que la transformation de recherche floue renvoie de la table de référence. Le score de confiance est une mesure de la probabilité qu'une valeur particulière soit la meilleure correspondance parmi les correspondances trouvées dans la table de référence. Le score de confiance affecté à un enregistrement dépend des autres enregistrements correspondants renvoyés. Par exemple, la correspondance entre *St.* et *Saint* renvoie un faible score de similarité indépendamment des autres correspondances. Si *Saint* est la seule correspondance renvoyée, le score de confiance est élevé. Si *Saint* et *St.* apparaissent tous les deux dans la table de référence, la confiance est élevée pour *St.* et faible pour *Saint* . Cependant, une similarité élevée ne signifie pas forcément une confiance élevée. Par exemple, si vous cherchez la valeur *Chapitre 4*, les résultats renvoyés *Chapitre 1*, *Chapitre 2*et *Chapitre 3* possèdent un score de similarité élevé mais un score de confiance faible, car aucun des résultats ne se dégage clairement comme étant la meilleure correspondance.  
  
 Le score de similarité est représenté par une valeur décimale entre 0 et 1, où la valeur 1 signifie que la correspondance entre la valeur dans la colonne d'entrée et la valeur dans la table de référence est exacte. Le score de confiance, qui est également une valeur décimale entre 0 et 1, indique la confiance par rapport à la correspondance. Si aucune correspondance utilisable n'est trouvée, des scores de similarité et de confiance de 0 sont affectés à la ligne, et les colonnes de sortie copiées à partir de la table de référence contiendront des valeurs NULL.  
  
 Parfois, la transformation de recherche floue peut ne pas localiser de correspondances appropriées dans la table de référence. Cela peut être le cas si la valeur d'entrée utilisée dans une recherche est un mot seul et court. Par exemple, *helo* n’est pas mis en correspondance avec *hello* dans une table de référence quand aucun autre jeton n’est présent dans cette colonne ni dans aucune autre colonne de la ligne.  
  
 Les colonnes de sortie de transformation comprennent les colonnes d'entrée marquées comme colonnes SQL directes, les colonnes sélectionnées dans la table de recherche et les colonnes supplémentaires suivantes :  
  
-   **_Similarity**, une colonne décrivant la similarité entre des valeurs des colonnes d’entrée et de référence.  
  
-   **_Confidence**, une colonne décrivant la qualité de la correspondance.  
  
 La transformation utilise la connexion à la base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour créer les tables temporaires utilisées par l'algorithme de correspondance approximative.  
  
## <a name="running-the-fuzzy-lookup-transformation"></a>Exécution de la transformation de recherche floue  
 Lorsque le package exécute la transformation pour la première fois, la transformation copie la table de référence, ajoute une clé avec un type de données entier à la nouvelle table et génère un index sur la colonne clé. Ensuite, la transformation génère un index appelé index de correspondances sur la copie de la table de référence. L'index de correspondances stocke les résultats de la création de jetons pour les valeurs dans les colonnes d'entrée de la transformation. La transformation utilise ensuite les jetons dans l'opération de recherche. L’index de correspondances est une table dans une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Lorsque le package est exécuté de nouveau, la transformation peut utiliser un index de correspondances existant ou en créer un nouveau. Si la table de référence est statique, le package peut éviter le processus éventuellement coûteux de recréation de l'index pour des sessions répétées de nettoyage des données. Si vous choisissez d'utiliser un index existant, celui-ci est créé lors de la première exécution du package. Si les transformations de recherche floue utilisent la même table de référence, elles peuvent toutes utiliser le même index. Pour réutiliser l'index, les opérations de recherche doivent être identiques ; la recherche doit utiliser les mêmes colonnes. Vous pouvez nommer l’index et sélectionner la connexion à la base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour enregistrer l’index.  
  
 Si la transformation enregistre l'index de correspondances, il peut être géré automatiquement. Cela signifie que chaque fois qu'un enregistrement dans la table de référence est mis à jour, l'index de correspondances est également mis à jour. La gestion de l'index de correspondances peut réduire le temps de traitement, car l'index ne doit pas être recréé à chaque exécution du package. Vous pouvez spécifier comment la transformation gère l'index de correspondances.  
  
 Le tableau suivant décrit les options de l'index de correspondances.  
  
|Option|Description|  
|------------|-----------------|  
|**GenerateAndMaintainNewIndex**|Créer un nouvel index, l'enregistrer et le gérer. La transformation installe des déclencheurs dans la table de référence pour garder la table de référence et la table d'index synchronisées.|  
|**GenerateAndPersistNewIndex**|Créer un nouvel index et l'enregistrer, sans le gérer.|  
|**GenerateNewIndex**|Créer un nouvel index, sans l'enregistrer.|  
|**ReuseExistingIndex**|Réutiliser un index existant.|  
  
### <a name="maintenance-of-the-match-index-table"></a>Maintenance de la table d'index de correspondances  
 L’option **GenerateAndMaintainNewIndex** installe des déclencheurs dans la table de référence pour garder la table de référence et la table d’index synchronisées. Si vous devez supprimer le déclencheur installé, vous devez exécuter la procédure stockée **sp_FuzzyLookupTableMaintenanceUnInstall** et fournir le nom spécifié dans la propriété MatchIndexName comme valeur de paramètre d’entrée.  
  
 Vous ne devez pas supprimer la table d’index de correspondances gérée avant d’exécuter la procédure stockée **sp_FuzzyLookupTableMaintenanceUnInstall** . Si la table d'index de correspondances est supprimée, les déclencheurs de la table de référence ne seront pas exécutés correctement. Toutes les mises à jour suivantes de la table de référence échoueront tant que vous ne supprimerez pas les déclencheurs dans la table de référence.  
  
 La commande SQL TRUNCATE TABLE n'appelle pas de déclencheurs DELETE. Si la commande TRUNCATE TABLE est utilisée sur la table de référence, celle-ci ne sera plus synchronisée avec l'index de correspondances et la transformation de recherche floue échouera. Lorsque les déclencheurs qui gèrent la table d'index de correspondances sont installés sur la table de référence, vous devez utiliser la commande SQL DELETE au lieu de TRUNCATE TABLE.  
  
> [!NOTE]  
>  Lorsque vous sélectionnez **Conserver l’index stocké** sous l’onglet **Table de référence** de **l’Éditeur de transformation de recherche floue**, la transformation utilise des procédures stockées managées pour maintenir l’index. Ces procédures stockées managées utilisent la fonctionnalité de l’intégration du common language runtime (CLR) dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Par défaut, l'intégration CLR dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est désactivée. Pour utiliser la fonctionnalité **Conserver l’index stocké** , vous devez activer l’intégration du CLR. Pour plus d’informations, consultez [Activation de l’intégration du CLR](../../../relational-databases/clr-integration/clr-integration-enabling.md).  
>   
>  Dans la mesure où l’option **Conserver l’index stocké** requiert l’intégration du CLR, cette fonctionnalité n’est effective que lorsque vous sélectionnez une table de référence dans une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour laquelle l’intégration du CLR est activée.  
  
## <a name="row-comparison"></a>Comparaison de lignes  
 Lorsque vous configurez la transformation de configuration floue, vous pouvez spécifier l'algorithme de comparaison utilisé par la transformation pour rechercher les enregistrements correspondants dans la table de référence. Si vous définissez la propriété Exhaustive comme ayant la valeur **True**, la transformation compare chaque ligne de l’entrée à chaque ligne de la table de référence. Cet algorithme de comparaison peut produire des résultats plus précis, mais peut ralentir la transformation, sauf si le nombre de lignes dans la table de référence est faible. Si la propriété Exhaustive a pour valeur **True**, toute la table de référence est chargée dans la mémoire. Pour éviter les problèmes de performance, il est recommandé de définir la propriété Exhaustive avec la valeur **True** seulement au cours du développement du package.  
  
 Si la propriété Exhaustive a pour valeur **False**, la transformation de recherche floue renvoie uniquement les correspondances ayant au moins un jeton indexé ou une sous-chaîne (la sous-chaîne est appelée un *q-gram*) en commun avec l’enregistrement d’entrée. Pour améliorer l'efficacité des recherches, seul un sous-ensemble des jetons de chaque ligne de la table est indexé dans la structure d'index inversée dont se sert la transformation de recherche floue pour rechercher les correspondances. Lorsque le dataset d’entrée est petit, vous pouvez affecter à Exhaustive la valeur **True** afin d’éviter les correspondances manquantes pour lesquelles il n’existe aucun jeton commun dans la table d’index.  
  
## <a name="caching-of-indexes-and-reference-tables"></a>Mise en cache des index et tables de référence  
 Lorsque vous configurez la transformation de recherche floue, vous pouvez spécifier si la transformation met partiellement en cache l'index et la table de référence dans la mémoire avant que la transformation soit exécutée. Si vous définissez la propriété WarmCaches avec la valeur **True**, l’index et la table de référence sont chargés dans la mémoire. Lorsque l’entrée a de nombreuses lignes, la définition de la propriété WarmCaches avec la valeur **True** peut améliorer les performances de la transformation. Lorsque le nombre de lignes d’entrée est faible, la définition de la propriété WarmCaches avec la valeur **False** peut accélérer la réutilisation d’un index de grande taille.  
  
## <a name="temporary-tables-and-indexes"></a>Tables et index temporaires  
 Lors de l'exécution, la transformation de recherche floue crée des objets temporaires tels que des tables et des index, dans la base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à laquelle la transformation se connecte. La taille de ces tables et index temporaires est proportionnelle au nombre de lignes et de jetons dans la table de référence et au nombre de jetons que la transformation de recherche floue crée ; ils peuvent donc occuper un volume important de l'espace disque. La transformation exécute également des requêtes sur ces tables temporaires. Vous devez donc envisager de connecter la transformation de recherche floue à une instance d’une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui ne soit pas en production, notamment si le serveur de production dispose d’un espace disque disponible limité.  
  
 Les performances de cette transformation peuvent s'améliorer si les tables et les index qu'elle utilise sont situés sur l'ordinateur local. Si la table de référence que la transformation de recherche floue utilise se trouve sur le serveur de production, vous devez envisager de copier la table sur un serveur qui ne soit pas un serveur production et de configurer la transformation de recherche floue pour accéder à la copie. Ce faisant, vous pouvez empêcher les requêtes de recherche de consommer des ressources sur le serveur de production. En outre, si la transformation de recherche floue gère l’index de correspondances (c’est-à-dire si MatchIndexOptionsis a pour valeur **GenerateAndMaintainNewIndex**), la transformation peut verrouiller la table de référence durant l’opération de nettoyage des données et empêcher les autres utilisateurs et applications d’accéder à la table.  
  
## <a name="configuring-the-fuzzy-lookup-transformation"></a>Configuration de la transformation de recherche floue  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la définition des propriétés d’un composant de flux de données, consultez [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="fuzzy-lookup-transformation-editor-reference-table-tab"></a>Éditeur de transformation de recherche floue (onglet Table de référence)
  Utilisez l’onglet **Table de référence** de la boîte de dialogue **Éditeur de transformation de recherche floue** pour définir la table source et l’index à utiliser pour la recherche. La source de données de référence doit être une table d’une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  La transformation de recherche floue crée une copie de travail de la table de référence. Les index décrits ci-dessous sont créés dans cette table de travail en utilisant une table spéciale et non un index [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] standard. La transformation ne modifie pas les tables sources existantes si vous ne sélectionnez pas **Conserver l’index stocké**. Dans ce cas, elle crée un déclencheur dans la table de référence qui met à jour la table de travail et la table d'index de recherche en fonction des modifications apportées à la table de référence.  
  
> [!NOTE]  
>  Les propriétés **Exhaustive** et **MaxMemoryUsage** de la transformation de recherche floue ne sont pas disponibles dans **l’Éditeur de transformation de recherche floue**, mais elles peuvent être définies à l’aide de **l’Éditeur avancé**. De plus, une valeur supérieure à 100 pour **MaxOutputMatchesPerInput** peut uniquement être spécifiée dans **l’Éditeur avancé**. Pour plus d’informations sur ces propriétés, consultez la section Transformation de recherche floue dans [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md).  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions OLE DB**  
 Sélectionnez un gestionnaire de connexions OLE DB existant dans la liste ou créez une connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Crée une connexion en utilisant la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** .  
  
 **Créer un nouvel index**  
 Indiquez que la transformation doit créer un nouvel index à utiliser pour la recherche.  
  
 **Nom de la table de référence**  
 Sélectionnez la table existante à utiliser comme table de référence (recherche).  
  
 **Stocker un nouvel index**  
 Sélectionnez cette option pour enregistrer le nouvel index de recherche.  
  
 **Nom du nouvel index**  
 Si vous avez décidé d'enregistrer le nouvel index de recherche, tapez un nom descriptif.  
  
 **Conserver l’index stocké**  
 Si vous avez décidé d'enregistrer le nouvel index de recherche, indiquez si vous voulez également que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] conserve l'index.  
  
> [!NOTE]  
>  Quand vous sélectionnez **Conserver l’index stocké** sous l’onglet **Table de référence** de **l’Éditeur de transformation de recherche floue**, la transformation utilise des procédures stockées gérées pour tenir l’index à jour. Ces procédures stockées managées utilisent la fonctionnalité de l’intégration du common language runtime (CLR) dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Par défaut, l'intégration CLR dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est désactivée. Pour utiliser la fonctionnalité **Conserver l’index stocké** , vous devez activer l’intégration du CLR. Pour plus d’informations, consultez [Activation de l’intégration du CLR](../../../relational-databases/clr-integration/clr-integration-enabling.md).  
>   
>  L’option **Conserver l’index stocké** nécessitant l’intégration du CLR, cette fonctionnalité n’est effective que quand vous sélectionnez une table de référence dans une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour laquelle l’intégration du CLR est activée.  
  
 **Utiliser l'index existant**  
 Indiquez que la transformation doit utiliser un index de recherche existant.  
  
 **Nom d'un index existant**  
 Dans la liste, sélectionnez un index de recherche existant.  
  
## <a name="fuzzy-lookup-transformation-editor-columns-tab"></a>Éditeur de transformation de recherche floue (onglet Colonnes)
  L'onglet **Colonnes** de la boîte de dialogue **Éditeur de transformation de recherche floue** vous permet de définir les propriétés des colonnes d'entrée et de sortie.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Faites glisser les colonnes d'entrée sur les colonnes de recherche afin de les y connecter. Ces colonnes doivent avoir le même type de données pris en charge. Sélectionnez une ligne de mappage et cliquez avec le bouton droit de la souris pour modifier les mappages dans la boîte de dialogue [Créer des relations](../../../integration-services/data-flow/transformations/create-relationships.md) .  
  
 **Nom**  
 Permet d'afficher les noms des colonnes d'entrée disponibles.  
  
 **Transfert direct**  
 Permet d'indiquer s'il convient d'inclure les colonnes d'entrée dans la sortie de la transformation.  
  
 **Colonnes de recherche disponibles**  
 Ces cases à cocher vous permettent de choisir les colonnes sur lesquelles les opérations de recherche floue doivent porter.  
  
 **colonne de recherche**  
 Permet de choisir les colonnes de recherche floue dans la liste des colonnes disponibles de la table de référence. Vos sélections se reflètent dans les sélections des cases à cocher tirées de la table **Colonnes de recherche disponibles** . En sélectionnant une colonne dans la table **Colonnes de recherche disponibles** , vous créez ainsi une colonne de sortie contenant la valeur de la colonne issue de la table de référence de chaque ligne retournée y correspondant.  
  
 **Alias de sortie**  
 Permet de saisir un alias pour la sortie de chaque colonne de recherche. La valeur par défaut correspond au nom de la colonne de recherche suivi d'une valeur d'index numérique. Vous pouvez cependant choisir tout autre nom descriptif s'il reste unique.  
  
## <a name="fuzzy-lookup-transformation-editor-advanced-tab"></a>Éditeur de transformation de recherche floue (onglet Avancé)
  Utilisez l’onglet **Avancé** de la boîte de dialogue **Éditeur de transformation de recherche floue** pour définir les paramètres relatifs à une recherche floue.  
  
### <a name="options"></a>Options  
 **Correspondances maximales à afficher par recherche**  
 Indiquez le nombre maximal de correspondances que la transformation peut retourner pour chaque ligne d'entrée. La valeur par défaut est **1**.  
  
 **Seuil de similarité**  
 Définissez le seuil de similarité au niveau du composant à l'aide du curseur. Plus la valeur est proche de 1, plus la valeur de recherche doit être proche de la valeur source pour constituer une correspondance. Si vous augmentez le seuil, vous pouvez améliorer la vitesse de correspondance étant donné qu'un plus petit nombre d'enregistrements candidats doit être pris en compte.  
  
 **Séparateurs de jetons**  
 Indiquez les séparateurs utilisés par la transformation pour marquer les valeurs de colonne.  
  
## <a name="see-also"></a> Voir aussi  
 [Transformation de recherche](../../../integration-services/data-flow/transformations/lookup-transformation.md)   
 [Fuzzy Grouping Transformation](../../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
