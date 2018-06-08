---
title: Agrégation, transformation | Microsoft Docs
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
- sql13.dts.designer.aggregatetrans.f1
- sql13.dts.designer.aggregationtransformation.aggregations.f1
- sql13.dts.designer.aggregationtransformation.advanced.f1
helpviewer_keywords:
- IsBig property
- aggregate functions [Integration Services]
- Aggregate transformation [Integration Services]
- large data, SSIS transformations
ms.assetid: 2871cf2a-fbd3-41ba-807d-26ffff960e81
caps.latest.revision: 59
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0a8e1f5981bdaf8cec7b263a9894ce3d1c4c7b04
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="aggregate-transformation"></a>Transformation d'agrégation
  La transformation d’agrégation applique des fonctions d’agrégation, comme Average, aux valeurs des colonnes et copie les résultats dans la sortie de la transformation. Outre les fonctions d'agrégation, cette transformation propose la clause GROUP BY qui permet de spécifier des groupes auxquels appliquer l'agrégation.  
  
## <a name="operations"></a>Opérations  
 La transformation d'agrégation prend en charge les opérations suivantes.  
  
|Opération|Description|  
|---------------|-----------------|  
|Group by|Divise les datasets en groupes. Les colonnes contenant tout type de données peuvent être utilisées pour le regroupement. Pour plus d’informations, consultez [GROUP BY &#40;Transact-SQL&#41;](../../../t-sql/queries/select-group-by-transact-sql.md).|  
|SUM|Additionne les valeurs dans une colonne. Seules les colonnes dont les données sont numériques peuvent être additionnées. Pour plus d’informations, consultez [SUM &#40;Transact-SQL&#41;](../../../t-sql/functions/sum-transact-sql.md).|  
|Moyenne|Retourne la moyenne des valeurs d'une colonne. La moyenne ne peut être calculée que sur les colonnes dont les données sont numériques. Pour plus d’informations, consultez [AVG &#40;Transact-SQL&#41;](../../../t-sql/functions/avg-transact-sql.md).|  
|Compter|Retourne le nombre d'éléments figurant dans un groupe. Pour plus d’informations, consultez [COUNT &#40;Transact-SQL&#41;](../../../t-sql/functions/count-transact-sql.md).|  
|Count distinct|Retourne le nombre de valeurs non NULL uniques d'un groupe.|  
|Minimum|Renvoie la valeur minimale figurant dans un groupe. Pour plus d’informations, consultez [MIN &#40;Transact-SQL&#41;](../../../t-sql/functions/min-transact-sql.md). À la différence de la fonction Transact-SQL MIN, cette opération peut être utilisée uniquement avec des données de type numérique, date et heure.|  
|Maximum|Renvoie la valeur maximale figurant dans un groupe. Pour plus d’informations, consultez [MAX &#40;Transact-SQL&#41;](../../../t-sql/functions/max-transact-sql.md). À la différence de la fonction Transact-SQL MAX, cette opération peut être utilisée uniquement avec des données de type numérique, date et heure.|  
  
 La transformation d'agrégation traite les valeurs null de la même manière que le moteur de base de données relationnelle [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Ce comportement est défini dans la norme SQL-92. Les règles suivantes s'appliquent :  
  
-   Dans une clause GROUP BY, les valeurs NULL sont traitées comme toute autre valeur de colonne. Si la colonne de regroupement contient plusieurs valeurs null, ces valeurs sont placées dans un seul groupe.  
  
-   Dans les fonctions COUNT (nom de colonne) et COUNT (nom de colonne DISTINCT), les valeurs NULL sont ignorées et le résultat ne tient pas compte des lignes contenant des valeurs NULL dans la colonne nommée.  
  
-   Dans la fonction COUNT (*), toutes les lignes sont comptées, y compris celles contenant des valeurs NULL.  
  
## <a name="big-numbers-in-aggregates"></a>Nombres élevés dans les agrégats  
 Une colonne peut contenir des valeurs numériques nécessitant une attention particulière en raison de leur grandeur ou de leur précision. La transformation d’agrégation inclut la propriété IsBig que vous pouvez définir sur les colonnes de sortie pour appeler un traitement spécial des nombres élevés ou de grande précision. Si la valeur d’une colonne dépasse 4 milliards ou si une précision plus grande que le type de données float est requise, vous devez affecter la valeur 1 à IsBig.  
  
 Le fait d’affecter la valeur 1 à IsBig affecte la sortie de la transformation d’agrégation de la manière suivante :  
  
-   Le type de données DT_R8 est utilisé à la place du type de données DT_R4.  
  
-   Les résultats de l'opération Count sont stockés en tant que type de données DT_UI8.  
  
-   Les résultats de l'opération Comptage de valeurs sont stockés en tant que type de données DT_UI4.  
  
> [!NOTE]  
>  Vous ne pouvez pas affecter la valeur 1 à IsBig pour les colonnes utilisées avec les opérations GROUP BY, Maximum ou Minimum.  
  
## <a name="performance-considerations"></a>Considérations relatives aux performances  
 La transformation d'agrégation comprend un ensemble de propriétés que vous pouvez définir pour améliorer ses performances.  
  
-   Quand vous effectuez une opération **Group by** , définissez les propriétés Keys ou KeysScale du composant et les sorties du composant. Grâce à Keys, vous pouvez spécifier le nombre exact de clés que la transformation doit normalement traiter. (Dans ce contexte, Keys fait référence au nombre de groupes attendus d’une opération **Group by**.) Grâce à KeysScale, vous pouvez spécifier un nombre approximatif de clés. Quand vous spécifiez une valeur appropriée pour Keys ou KeyScale, vous améliorez les performances car la transformation peut allouer la mémoire adéquate pour les données mises en cache par la transformation.  
  
-   Quand vous effectuez une opération **Comptage de valeurs** , définissez les propriétés CountDistinctKeys ou CountDistinctScale du composant. Grâce à CountDistinctKeys, vous pouvez indiquer le nombre exact de clés que la transformation doit normalement traiter pour une opération Count Distinct. (Dans ce contexte, CountDistinctKeys fait référence au nombre de valeurs distinctes attendues d’une opération **Comptage de valeurs**.) La propriété CountDistinctScale permet d’indiquer un nombre approximatif de clés pour une opération Count Distinct. Quand vous spécifiez une valeur appropriée pour CountDistinctKeys ou CountDistinctScale, vous améliorez les performances car la transformation peut allouer la mémoire adéquate pour les données mises en cache par la transformation.  
  
## <a name="aggregate-transformation-configuration"></a>Configuration de la transformation d'agrégation  
 La transformation d'agrégation est configurée au niveau de la transformation, de la sortie et de la colonne.  
  
-   Au niveau de la transformation, vous configurez la transformation d'agrégation pour les performances en spécifiant les valeurs suivantes :  
  
    -   Nombre de groupes attendus d’une opération **Group by**  
  
    -   Nombre de valeurs distinctes attendues d’une opération **Count Distinct**  
  
    -   Pourcentage par lequel la mémoire peut être étendue pendant l'agrégation.  
  
     La transformation d'agrégation peut également être configurée de manière à générer un avertissement au lieu d'échouer lorsque la valeur d'un diviseur est zéro.  
  
-   Au niveau de la sortie, vous configurez la transformation d’agrégation pour les performances en spécifiant le nombre de groupes attendus d’une opération **Group by** . La transformation d'agrégation prend en charge plusieurs sorties ; chacune pouvant être configurée différemment.  
  
-   Au niveau de la colonne, vous spécifiez les valeurs suivantes :  
  
    -   agrégation que la colonne effectue ;  
  
    -   options de comparaison de l'agrégation.  
  
 Vous pouvez également configurer la transformation d'agrégation pour les performances en spécifiant les valeurs suivantes :  
  
-   Nombre de groupes attendus d’une opération **Group by** dans la colonne  
  
-   Nombre de valeurs distinctes attendues d’une opération **Count Distinct** dans la colonne  
  
 Vous pouvez également identifier des colonnes en tant que IsBig si une colonne contient des valeurs numériques élevées ou de grande précision.  
  
 La transformation d'agrégation est asynchrone, ce qui signifie qu'elle ne consomme pas les données et ne les publie pas ligne par ligne. Au lieu de cela, elle consomme tout l'ensemble de lignes, effectue les regroupements et les agrégations, puis publie les résultats.  
  
 Cette transformation ne transmet aucune colonne, mais en crée de nouvelles dans le flux de données pour les données qu'elle publie. Seules les colonnes d'entrée auxquelles des fonctions d'agrégation s'appliquent ou les colonnes d'entrée que la transformation utilise pour le regroupement sont copiées dans la sortie de la transformation. Par exemple, l’entrée d’une transformation d’agrégation peut contenir trois colonnes : **PaysRegion**, **Ville**et **Population**. La transformation effectue un regroupement à partir de la colonne **PaysRegion** et applique la fonction Sum à la colonne **Population** . La sortie n’inclut donc pas la colonne **Ville** .  
  
 Vous pouvez également ajouter plusieurs sorties à la transformation d'agrégation et diriger chaque agrégation vers une sortie différente. Par exemple, si la transformation d’agrégation applique les fonctions Sum et Average, chaque agrégation peut être dirigée vers une sortie différente.  
  
 Vous pouvez appliquer plusieurs agrégations à une même colonne d'entrée. Par exemple, si vous souhaitez calculer la somme et la moyenne d’une colonne d’entrée nommée **Ventes**, vous pouvez configurer la transformation pour qu’elle applique les fonctions Sum et Average à la colonne **Ventes** .  
  
 La transformation d'agrégation comporte une entrée et une ou plusieurs sorties. Elle ne prend pas en charge de sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Agréger les valeurs dans un dataset à l'aide de la transformation d'agrégation](../../../integration-services/data-flow/transformations/aggregate-values-in-a-dataset-by-using-the-aggregate-transformation.md)  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
-   [Trier des données pour les transformations de fusion et de jointure de fusion](../../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 [Agréger les valeurs dans un dataset à l'aide de la transformation d'agrégation](../../../integration-services/data-flow/transformations/aggregate-values-in-a-dataset-by-using-the-aggregate-transformation.md)  
  
## <a name="aggregate-transformation-editor-aggregations-tab"></a>Éditeur de transformation d'agrégation (onglet Agrégations)
  Utilisez l’onglet **Agrégations** de la boîte de dialogue **Éditeur de transformation d’agrégation** pour spécifier les colonnes destinées à l’agrégation et les propriétés de cette agrégation. Vous pouvez appliquer plusieurs agrégations. Ce type de transformation ne génère pas d'erreur de sortie.  
  
> [!NOTE]  
>  Les options concernant le nombre et l’échelle de clés d’une part et le nombre et l’échelle de clés distinctes d’autre part s’appliquent au niveau du composant quand ces options sont indiquées sous l’onglet **Avancé** , au niveau de la sortie quand elles sont précisées dans l’affichage avancé de l’onglet **Agrégations** ou encore au niveau des colonnes quand elles sont spécifiées dans la liste de colonnes se trouvant dans la partie inférieure de l’onglet **Agrégations** .  
>   
>  Dans la transformation d’agrégation, **Clés** et **Échelle de clé** font référence au nombre de groupes attendus d’une opération **Regrouper par** . **Nombre de clés distinctes** et **Échelle de nombre des valeurs distinctes** font référence au nombre de valeurs distinctes attendues d’une opération **Comptage de valeurs** .  
  
### <a name="options"></a>Options  
 **Avancé / Simple**  
 Permet d'afficher ou de masquer les options permettant de configurer plusieurs agrégations dans le cas de sorties multiples. Par défaut, les options avancées sont masquées.  
  
 **Nom d'agrégation**  
 Dans l'écran Avancé, permet d'attribuer un nom convivial à l'agrégation.  
  
 **Grouper par colonnes**  
 Dans l’écran Avancé, permet de sélectionner les colonnes afin de regrouper les éléments d’après la liste **Colonnes d’entrée disponibles** , comme décrit ci-dessous.  
  
 **Échelle de clé**  
 Dans l'écran Avancé, permet de spécifier aussi éventuellement le nombre de clés adéquat que l'agrégation peut écrire. Par défaut, la valeur de cette option est **Non spécifié**. Si la valeur des propriétés **Échelle de clé** et **Clés** sont toutes deux définies, c’est celle de la propriété **Clés** qui prévaut.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Non spécifié|La propriété Échelle de clé n'est pas utilisée.|  
|Faible|L’agrégation peut écrire environ 500 000 clés.|  
|Moyenne|L'agrégation peut écrire environ 5 000 000 clés.|  
|Élevée|L'agrégation peut écrire plus de 25 000 000 clés.|  
  
 **Clés**  
 Dans l'écran Avancé, permet d'indiquer éventuellement le nombre exact de clés que l'agrégation peut écrire. Si la valeur des propriétés **Échelle de clé** et **Clés** sont toutes deux précisées, celle de la propriété **Clés** prévaut alors.  
  
 **Colonnes d’entrée disponibles**  
 Permet de sélectionner les colonnes dans la liste des colonnes d'entrée disponibles en cochant ou décochant les cases du tableau.  
  
 **Colonne d'entrée**  
 Permet de sélectionner des colonnes dans la liste des colonnes d'entrée disponibles.  
  
 **Alias de sortie**  
 Permet de saisir un alias pour chaque colonne. Par défaut, il s'agit du nom de la colonne d'entrée ; vous pouvez néanmoins choisir un nom unique et descriptif.  
  
 **Opération**  
 Permet de choisir dans la liste parmi les opérations disponibles, d'après le tableau suivant.  
  
|Opération|Description|  
|---------------|-----------------|  
|**GroupBy**|Divise les datasets en groupes. Les colonnes incluant tout type de données peuvent être utilisées pour le regroupement. Pour plus d'informations, consultez GROUP BY.|  
|**Sum**|Additionne les valeurs dans une colonne. Seules les colonnes dont les données sont numériques peuvent être additionnées. Pour plus d'informations, consultez SUM.|  
|**Moyenne**|Retourne la moyenne des valeurs d'une colonne. La moyenne ne peut être calculée que sur les colonnes dont les données sont numériques. Pour plus d'informations, consultez AVG.|  
|**Count**|Retourne le nombre d'éléments figurant dans un groupe. Pour plus d'informations, consultez COUNT.|  
|**CountDistinct**|Retourne le nombre de valeurs non NULL uniques d'un groupe. Pour plus d'informations, consultez COUNT et Distinct.|  
|**Minimum**|Renvoie la valeur minimale figurant dans un groupe. Cette opération se restreint aux données numériques.|  
|**Maximum**|Renvoie la valeur maximale figurant dans un groupe. Cette opération se restreint aux données numériques.|  
  
 **Indicateurs de comparaison**  
 Si vous sélectionnez **Group by**, permet d’utiliser les cases à cocher afin de contrôler le mode d’évaluation de la comparaison effectuée par la transformation. Pour plus d’informations sur les options de comparaison de chaînes, consultez [Comparaison des données chaînes](../../../integration-services/data-flow/comparing-string-data.md).  
  
 **Count Distinct Scale**  
 Permet éventuellement de spécifier le nombre approximatif de valeurs distinctes que l'agrégation peut écrire. Par défaut, la valeur de cette option est **Non spécifié**. Si la valeur des propriétés **CountDistinctScale** et **CountDistinctKeys** sont toutes deux précisées, celle de la propriété **CountDistinctKeys** prévaut alors.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Non spécifié|La propriété **CountDistinctScale** n’est pas utilisée.|  
|Faible|L'agrégation peut écrire environ 500 000 valeurs distinctes.|  
|Moyenne|L’agrégation peut écrire environ 5 000 000 de valeurs distinctes.|  
|Élevée|L'agrégation peut écrire plus de 25 000 000 valeurs distinctes.|  
  
 **Count Distinct Keys**  
 Permet de spécifier éventuellement le nombre exact de valeurs distinctes que l'agrégation peut écrire. Si la valeur des propriétés **CountDistinctScale** et **CountDistinctKeys** sont toutes deux précisées, celle de la propriété **CountDistinctKeys** prévaut alors.  
  
## <a name="aggregate-transformation-editor-advanced-tab"></a>Éditeur de transformation d'agrégation (onglet Avancé)
  L’onglet **Avancé** de la boîte de dialogue **Éditeur de transformation d’agrégation** permet de définir les propriétés des composants, de spécifier des agrégations et de définir les propriétés des colonnes d’entrée et de sortie.  
  
> [!NOTE]  
>  Les options concernant le nombre et l’échelle de clés d’une part, et le nombre et l’échelle de clés distinctes d’autre part, s’appliquent au niveau du composant quand ces options sont indiquées sous l’onglet **Avancé** , au niveau de la sortie quand elles sont précisées dans l’affichage avancé de l’onglet **Agrégations** , ou encore au niveau des colonnes quand elles sont spécifiées dans la liste de colonnes se trouvant dans la partie inférieure de l’onglet **Agrégations** .  
>   
>  Dans la transformation d’agrégation, **Clés** et **Échelle de clé** font référence au nombre de groupes attendus d’une opération **Regrouper par** . **Nombre de clés distinctes** et **Échelle de nombre des valeurs distinctes** font référence au nombre de valeurs distinctes attendues d’une opération **Comptage de valeurs** .  
  
### <a name="options"></a>Options  
 **Échelle de clé**  
 Permet de spécifier le nombre approximatif de clés attendu par l'agrégation (facultatif). La transformation utilise ces informations afin d'optimiser la taille initiale de son cache. Par défaut, la valeur de cette option est **Non spécifié**. Si les deux options **Échelle de clé** et **Nombre de clés** sont spécifiées, **Nombre de clés** est prioritaire.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Non spécifié|La propriété **KeyScale** n’est pas utilisée.|  
|Faible|L’agrégation peut écrire environ 500 000 clés.|  
|Moyenne|L'agrégation peut écrire environ 5 000 000 clés.|  
|Élevée|L'agrégation peut écrire plus de 25 000 000 clés.|  
  
 **Nombre de clés**  
 Permet de spécifier le nombre exact de clés attendu par l'agrégation (facultatif). La transformation utilise ces informations afin d'optimiser la taille initiale de son cache. Si les deux options **Échelle de clé** et **Nombre de clés** sont spécifiées, **Nombre de clés** est prioritaire.  
  
 **Échelle de nombre des valeurs distinctes**  
 Permet éventuellement de spécifier le nombre approximatif de valeurs distinctes que l'agrégation peut écrire. Par défaut, la valeur de cette option est **Non spécifié**. Si les deux options **Échelle de nombre des valeurs distinctes** et **Nombre de clés distinctes** sont spécifiées, **Nombre de clés distinctes** est prioritaire.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Non spécifié|La propriété CountDistinctScale n'est pas utilisée.|  
|Faible|L'agrégation peut écrire environ 500 000 valeurs distinctes.|  
|Moyenne|L’agrégation peut écrire environ 5 000 000 de valeurs distinctes.|  
|Élevée|L'agrégation peut écrire plus de 25 000 000 valeurs distinctes.|  
  
 **Nombre de clés distinctes**  
 Permet de spécifier éventuellement le nombre exact de valeurs distinctes que l'agrégation peut écrire. Si les deux options **Échelle de nombre des valeurs distinctes** et **Nombre de clés distinctes** sont spécifiées, **Nombre de clés distinctes** est prioritaire.  
  
 **Facteur d'extension automatique**  
 Utilisez une valeur comprise entre 1 et 100 afin de spécifier le pourcentage selon lequel la mémoire peut être étendue pendant l'agrégation. Par défaut, la valeur de cette option est **25 %**.  
  
## <a name="see-also"></a> Voir aussi  
 [Flux de données](../../../integration-services/data-flow/data-flow.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
