---
title: Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: fcca7243-a702-4725-8e6f-cf118e988acf
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 58b46f8ae50aaf31318d75a45ad767170c15b575
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-dataset-filters-data-region-filters-and-group-filters"></a>Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes
  Dans un rapport, un filtre est une partie d'un dataset, d'une région de données ou d'un groupe de régions de données que vous créez pour limiter les données utilisées dans le rapport. Les filtres vous aident à contrôler les données de rapport si vous ne pouvez pas modifier la requête de dataset, par exemple, si vous utilisez un dataset partagé.  
  
 Les filtres vous aident à contrôler l'affichage et le traitement des données dans un rapport. Vous pouvez spécifier des filtres pour un dataset, une région de données, un groupe ou toute association de ces éléments.  
  
 Pour plus d’informations, consultez [Ajouter un filtre à un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md) et [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="When"></a> Choix de la planification d'un filtre  
 Spécifiez des filtres pour les éléments de rapport lorsque vous ne pouvez pas filtrer les données à la source. Par exemple, utilisez des filtres de rapport lorsque la source de données ne prend pas en charge les paramètres de requête, lorsque que vous devez exécuter des procédures stockées et ne pouvez pas modifier la requête ou encore lorsqu'un instantané de rapport paramétrable affiche des données personnalisées pour les différents utilisateurs.  
  
 Vous pouvez filtrer les données de rapport avant ou après leur récupération pour un dataset de rapport. Pour filtrer les données avant qu'elles ne soient récupérées, modifiez la requête pour chaque dataset. Lorsque vous filtrez des données dans la requête, vous les filtrez au niveau de la source de données, ce qui réduit le volume de données à récupérer et à traiter dans un rapport. Pour filtrer les données après leur récupération, créez des expressions de filtre dans le rapport. Vous pouvez définir des expressions de filtre pour un dataset, une région de données ou un groupe, y compris les groupes de détail. Vous pouvez également inclure des paramètres dans les expressions de filtre, ce qui permet de filtrer des données pour des valeurs ou des utilisateurs spécifiques, par exemple en filtrant sur une valeur qui identifie l'utilisateur qui consulte le rapport.  
  
##  <a name="Where"></a> Choix de l'emplacement de définition d'un filtre  
 Déterminez l'emplacement où vous souhaitez définir un filtre en fonction de l'effet que vous souhaitez obtenir dans votre rapport. Au moment de l'exécution, le processeur de rapports applique les filtres dans l'ordre suivant : sur le dataset, puis sur la région de données, puis sur les groupes dans l'ordre de haut de bas de chaque hiérarchie de groupe. Dans une table, une matrice ou une liste, les filtres des groupes de lignes, des groupes de colonnes et des groupes adjacents sont appliqués indépendamment. Dans un graphique, les filtres des groupes de catégories et des groupes de séries sont appliqués indépendamment. Quand le processeur de rapports applique le filtre, toutes les équations de filtre sont appliquées dans l’ordre où elles sont définies sur la page **Filtre** de la boîte de dialogue **Propriétés** pour chaque élément de rapport, ce qui revient à les combiner avec des opérations booléennes AND.  
  
 La liste suivante compare l'effet de la définition de filtres sur les différents éléments de rapport :  
  
-   **Sur le dataset** : définissez un filtre sur le dataset quand vous souhaitez filtrer de la même façon une ou plusieurs régions de données liées à un même dataset. Par exemple, définissez le filtre sur le dataset lié à à la fois à une table qui affiche des chiffres des ventes et à un graphique qui affiche les mêmes données.  
  
-   **Sur la région de données** : définissez un filtre sur la région de données quand vous souhaitez qu’une ou plusieurs régions de données liées à un même dataset donnent une vue différente du dataset. Par exemple, définissez le filtre sur une région des données de table pour afficher les dix magasins totalisant le plus de ventes et sur une autre région des données de table pour afficher les dix magasins totalisant le moins de ventes, le tout dans le même rapport.  
  
-   **Sur les groupes de lignes ou de colonnes d’une région de données de tableau matriciel** : définissez un filtre sur un groupe quand vous souhaitez inclure ou exclure certaines valeurs dans une expression de groupe, afin de contrôler les valeurs de groupe qui apparaissent dans la table, la matrice ou la liste.  
  
-   **Sur le groupe de détails d’une région de données de tableau matriciel** : définissez un filtre sur le groupe de détails quand vous disposez de plusieurs groupes de détails pour une région de données et si souhaitez que chaque groupe de détails affiche un ensemble de données différent issu du dataset.  
  
-   **Sur les groupes de séries ou de catégories d’une région de données de graphique** : définissez un filtre sur un groupe de séries ou de catégories quand vous souhaitez inclure ou exclure certaines valeurs dans une expression de groupe afin de contrôler les valeurs qui s’affichent dans le graphique.  
  
 Retour au début  
  
##  <a name="FilterEquations"></a> Fonctionnement d'une équation de filtre  
 Au moment de l'exécution, le processeur de rapports convertit la valeur dans le type de données spécifié, puis utilise l'opérateur spécifié pour comparer l'expression et la valeur. La liste suivante décrit chaque élément de l'équation de filtre :  
  
-   **Expression** : définit ce sur quoi vous filtrez. En général, il s'agit d'un champ de dataset.  
  
-   **Type de données** : spécifie le type de données à utiliser quand l’équation de filtre est évaluée au moment de l’exécution par le processeur de rapports. Le type de données sélectionné doit être pris en charge par le schéma de définition de rapport.  
  
-   **Opérateur** : définit comment comparer les deux parties de l’équation de filtre.  
  
-   **Valeur** : définit l’expression à utiliser dans la comparaison.  
  
 Les sections suivantes décrivent chaque élément de l'équation de filtre :  
  
### <a name="expression"></a>Expression  
 Lorsque l'équation de filtre est évaluée par le processeur de rapports au moment de l'exécution, les types de données pour l'expression et la valeur doivent être identiques. Le type de données du champ sélectionné pour **Expression** est déterminé par l’extension de traitement des données ou le fournisseur de données utilisée pour extraire les données de la source de données. Le type de données de l’expression entrée pour **Valeur** est déterminé par les valeurs par défaut de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Les choix proposés pour le type de données sont déterminés par les types de données pris en charge pour une définition de rapport. Les valeurs de la base de données peuvent être converties par le fournisseur de données en un type CLR.  
  
### <a name="data-type"></a>Type de données  
 Pour que le processeur de rapports puisse comparer deux valeurs, les types de données doivent être identiques. Le tableau suivant répertorie le mappage entre les types de données CLR et les types de données de définition de rapport. Les données que vous récupérez à partir d'une source de données peuvent être converties en un type de données différent lorsqu'il s'agit de données de rapport.  
  
|**Type de données de schéma de définition de rapport**|**Type CLR**|  
|--------------------------------------------|-----------------------|  
|**Booléen**|**Booléen**|  
|**DateTime**|**DateTime**, **DateTimeOffset**|  
|**Integer**|**Int16**, **Int32**, **UInt16**, **Byte**, **SByte**|  
|**Float**|**Single**, **Double**, **Decimal**|  
|**Texte**|**String**, **Char**, **GUID**, **Timespan**|  
  
 Dans les cas où vous devez spécifier un type de données, vous pouvez spécifier votre propre conversion dans la partie Valeur de l’expression.  
  
### <a name="operator"></a>Opérateur  
 Le tableau suivant répertorie les opérateurs que vous pouvez utiliser dans une équation de filtre et ceux que le processeur de rapports utilise pour évaluer l'équation de filtre.  
  
|Opérateur|Action|  
|--------------|------------|  
|**Equal, Like, NotEqual, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual**|Compare l'expression à une valeur.|  
|**TopN, BottomN**|Compare l’expression à une valeur de type **Entier** .|  
|**TopPercent, BottomPercent**|Compare l’expression à une valeur de type **Entier** ou **Flottant** .|  
|**Entre**|Teste si l'expression est comprise entre deux valeurs (y compris ces valeurs).|  
|**Dans**|Teste si l'expression est contenue dans un jeu de valeurs.|  
  
### <a name="value"></a>Valeur  
 L’expression Valeur spécifie la dernière partie de l’équation de filtre. Le processeur de rapports convertit l'expression évaluée dans le type de données que vous avez spécifié, puis évalue l'équation de filtre entière pour déterminer si les données spécifiées dans le champ Expression sont filtrées ou non.  
  
 Pour effectuer une conversion dans un type de données autre que CLR standard, vous devez modifier l'expression afin de procéder à une conversion explicite dans un type de données spécifique. Vous pouvez utiliser les fonctions de conversion répertoriées dans la boîte de dialogue **Expression** sous **Fonctions communes**, **Conversion**. Par exemple, pour un champ `ListPrice` qui représente les données stockées avec un type de données **monétaire** sur une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l’extension de traitement des données retourne la valeur du champ en utilisant le type de données <xref:System.Decimal> . Pour définir qu’un filtre doit utiliser seulement des valeurs supérieures à **50000,00 $** dans la devise du rapport, convertissez la valeur en Décimal en utilisant l’expression `=CDec(50000.00)`.  
  
 Cette valeur peut également inclure une référence de paramètre permettant à un utilisateur de sélectionner de manière interactive une valeur de filtre.  
  
 Retour au début  
  
## <a name="see-also"></a> Voir aussi  
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)  
  
  
