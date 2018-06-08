---
title: Indicateurs de performance clés | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6eaaf0fc4589fb07484dd10479ded4956650b245
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="kpis"></a>Indicateurs de performance clés
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Un *indicateur de performance clé* (KPI), dans un modèle tabulaire, permet de mesurer les performances d’une valeur, définies par une mesure de *base* par rapport à une valeur *cible* , également définie par une mesure ou par une valeur absolue. Cet article fournit des concepteurs de modèles tabulaires une présentation de base des indicateurs de performance clés dans un modèle tabulaire.  
  
##  <a name="bkmk_benefits"></a> Avantages  
 Dans la terminologie d'entreprise, un indicateur de performance clé (KPI) est une mesure quantifiable des performances d'objectifs économiques. Un KPI est fréquemment évalué dans le temps. Par exemple, le service commercial d'une organisation peut utiliser un KPI pour mesurer tous les mois la marge brute réelle par rapport à la marge brute planifiée. Le service comptable peut mesurer tous les mois les dépenses par rapport aux recettes pour évaluer les coûts ; et le service des ressources humaines peut mesurer tous les trimestres le taux de rotation du personnel. Chacun est un exemple de KPI. Les décideurs utilisent fréquemment des KPI qui sont regroupés dans un tableau de bord de l'entreprise pour obtenir une synthèse historique rapide et précise de l'activité ou déterminer les tendances.  
  
 Un indicateur de performance clé dans un modèle tabulaire inclut les éléments suivants :  
  
 **Valeur de base**  
 Une valeur de base est définie par une mesure qui restitue une valeur. Cette valeur, par exemple, peut être un agrégat des ventes réelles ou une mesure calculée comme les bénéfices pour une période donnée.  
  
 **Valeur cible**  
 Une valeur cible est définie par une mesure qui restitue une valeur, ou par une valeur absolue. Par exemple, une valeur cible peut être le montant par lequel les cadres d'une organisation veulent augmenter les ventes ou le bénéfice.  
  
 **Seuils d'état**  
 Un seuil d'état est défini par la plage entre un seuil minimal et un seuil maximal, ou par une valeur fixe. Le seuil d'état s'affiche sous la forme d'un graphique pour aider les utilisateurs à déterminer facilement l'état de la valeur de base comparée à la valeur cible.  
  
##  <a name="bkmk_example"></a> Exemple  
 La responsable commerciale d'AdventureWorks souhaite créer un tableau croisé dynamique qu'elle pourra utiliser pour déterminer rapidement si ses commerciaux atteignent leurs quotas de vente pendant une période donnée (généralement annuelle). Pour chaque commercial, elle souhaite que le tableau croisé dynamique affiche le montant total des ventes en dollars, le montant du quota des ventes en dollars et un graphique simple indiquant l'état de l'employé, c'est-à-dire où il se situe par rapport au quota des ventes. Elle souhaite obtenir des données annuelles.  
  
 Pour cela, la directrice commerciale demande l'aide du développeur de solutions de décisionnel de son organisation pour ajouter un KPI Ventes au modèle tabulaire AdventureWorks. Le directeur des ventes utilise Excel pour vous connecter pour le modèle tabulaire Adventure Works comme source de données et créer un tableau croisé dynamique avec les champs (mesures et KPI) et les segments pour analyser ou non de la force de vente atteint ses quotas.  
  
 Dans le modèle, une mesure de la colonne SalesAmount dans la table FactResellerSales est créée, indiquant le montant des ventes réel en dollars pour chaque employé. Cette mesure va définir la valeur de base du KPI.  
  
 La mesure Sales est créée au moyen de la formule suivante :  
  
```  
Sales:=Sum(FactResellerSales[SalesAmount])  
```  
  
 La colonne SalesAmountQuota de la table FactSalesQuota indique le quota sur le montant des ventes défini pour chaque employé. Les valeurs de cette colonne serviront de mesure cible (valeur) dans le KPI.  
  
 La mesure SalesAmountQuota est créée au moyen de la formule suivante :  
  
```  
Target SalesAmountQuota:=Sum(FactSalesQuota[SalesAmountQuota])  
```  
  
> [!NOTE]  
>  Il existe une relation entre la colonne EmployeeKey du tableau FactSalesQuota et la colonne EmployeeKey du tableau DimEmployees. Cette relation est requise afin que chaque commercial inclus dans le tableau DimEmployee soit représenté dans le tableau FactSalesQuota.  
  
 Maintenant que les mesures ont été créées pour servir de valeur de base et de valeur cible du KPI, la mesure Sales est étendue à un nouveau KPI des ventes. Dans le KPI Sales, la mesure Target SalesAmountQuota est définie comme valeur cible. Le seuil d'état est défini comme une plage en pourcentage dont la cible est 100 %, signifiant que les ventes réelles définies par la mesure Sales correspondent au quota défini dans la mesure SalesAmoutnQuota cible. Les pourcentages haut et bas sont définis sur la barre d'état et un type de graphique est sélectionné.  
  
 La responsable commerciale peut maintenant créer un tableau croisé dynamique en ajoutant la valeur de base, la valeur cible et l'état du KPI aux champs de valeurs. La colonne Employees est ajoutée au champ RowLabel et la colonne CalendarYear est ajoutée en tant que segment.  
  
 Elle peut ainsi segmenter par an le montant des ventes, le quota des ventes et l'état réels de chaque employé. Elle peut ensuite analyser les tendances des ventes de chaque année pour déterminer si elle doit rectifier le quota d'un employé donné.  
  
##  <a name="bkmk_create"></a> Create and edit KPIs  
 Pour créer des indicateurs de performance clés, dans le générateur de modèles, vous allez utiliser la boîte de dialogue Indicateur de performance clé. Étant donné que les indicateurs de performance clés doivent être associés à une mesure, vous créez un indicateur de performance clé en étendant une mesure qui prend une valeur de base, puis en créant une mesure qui correspond à une valeur cible ou en entrant une valeur absolue. Une fois la mesure de base (valeur) et la valeur cible définies, vous pouvez définir les paramètres de seuil d'état entre les valeurs de base et cibles. L'état est affiché dans un format graphique à l'aide d'icônes sélectionnables, de barres, de graphiques ou de couleurs. Les valeurs de base et cibles, ainsi que l'état, peuvent ensuite être ajoutés à un rapport ou un tableau croisé dynamique en tant que valeurs pouvant être segmentées par rapport à d'autres champs de données.  
  
 Pour afficher la boîte de dialogue Indicateur de performance clé, dans la grille de mesures d'une table, cliquez avec le bouton droit sur une mesure qui servira de valeur de base, puis sélectionnez **Créer un KPI**. Après qu'une mesure a été étendue à un KPI en tant que valeur de base, une icône s'affiche en regard du nom de la mesure dans la grille de mesures afin d'identifier la mesure comme associée à un indicateur de performance clé.  
  
##  <a name="bkmk_related_tasks"></a> Tâches associées  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Créer et gérer des indicateurs de performance clés](../../analysis-services/tabular-models/create-and-manage-kpis-ssas-tabular.md)|Décrit comment créer un indicateur de performance clé avec une mesure de base, une mesure cible et des seuils d'état.|  
  
## <a name="see-also"></a>Voir aussi  
 [Mesures](../../analysis-services/tabular-models/measures-ssas-tabular.md)   
 [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)  
  
  
