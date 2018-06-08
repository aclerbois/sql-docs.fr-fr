---
title: Hiérarchies | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e6ce881fb0b423744316027faeec7210ae76f296
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="hierarchies"></a>Hierarchies
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les hiérarchies, dans les modèles tabulaires, sont des métadonnées qui définissent les relations entre deux colonnes ou plus d'une table. Les hiérarchies peuvent apparaître séparément des autres colonnes dans une liste de champs de clients de création de rapports, ce qui les rend faciles à parcourir par les utilisateurs du client et à inclure dans un rapport.  
  
##  <a name="bkmk_benefits"></a> Avantages  
 Les tables peuvent inclure des dizaines, voire des centaines de colonnes avec des noms de colonne inhabituels sans ordre apparent. Cela peut entraîner une apparence non ordonnée dans les listes de champs de clients de création de rapports, ce qui complique la tâche des utilisateurs qui recherchent et souhaitent inclure des données dans un rapport. Les hiérarchies peuvent fournir une vue simple et intuitive d'une structure de données autrement plus complexe.  
  
 Par exemple, dans un tableau de dates, vous pouvez créer une hiérarchie de calendrier. « Année civile » est utilisé comme niveau parent de premier niveau, avec « Mois », « Semaine » et « Jour » inclus comme niveaux enfants (Année civile->Mois->Semaine->Jour). Cette hiérarchie montre une relation logique entre « Année civile » et « Jour ». Un utilisateur client peut ensuite sélectionner l'année civile d'une liste de champs pour inclure tous les niveaux dans un tableau croisé dynamique, ou développer la hiérarchie, puis sélectionner uniquement les niveaux particuliers à inclure dans le tableau croisé dynamique.  
  
 Chaque niveau dans une hiérarchie étant une représentation d'une colonne dans une table, le niveau peut être renommé. Bien que cette opération ne concerne pas seulement les hiérarchies (toute colonne peut être renommée dans un modèle tabulaire), renommer les niveaux de la hiérarchie peut permettre aux utilisateurs de rechercher et d’inclure plus facilement des niveaux dans un rapport. Renommer un niveau ne renomme pas la colonne qu'il référence ; cela rend simplement le niveau plus identifiable. Dans notre hiérarchie Année civile, par exemple, dans la table de date de la vue de données, les colonnes CalendarYear, CalendarMonth, CalendarWeek et CalendarDay ont été renommées en Année civile, Mois, Semaine et Jour de manière à les rendre plus facilement identifiables. Renommer des niveaux présente un autre avantage, celui d'assurer la cohérence dans les rapports, étant donné que les utilisateurs seront moins souvent amenés à modifier les noms de colonnes pour les rendre plus lisibles dans les tableaux croisés dynamiques, les graphiques, etc.  
  
 Les hiérarchies peuvent être incluses dans des perspectives. Les perspectives définissent des sous-ensembles visualisables d'un modèle et des points de vue du modèle focalisés sur un domaine d'activité ou sur une application. Par exemple, une perspective peut fournir aux utilisateurs une liste visuelle (hiérarchie) d'éléments de données requis pour des exigences de création de rapports spécifiques. Pour plus d’informations, consultez [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md).  
  
 Les hiérarchies ne sont pas censées être utilisées comme mécanisme de sécurité, mais en tant qu'outil pour améliorer le confort de l'utilisateur. Toute la sécurité d'une hiérarchie donnée est héritée du modèle sous-jacent. Les hiérarchies ne peuvent pas autoriser l'accès aux objets de modèle auxquels un utilisateur n'a pas accès. La sécurité de la base de données model doit d'abord être résolue, avant que l'utilisateur puisse accéder aux objets du modèle par le biais d'une hiérarchie. Les rôles de sécurité peuvent permettre de sécuriser les métadonnées et les données de modèle. Pour plus d’informations, consultez [rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md).  
  
##  <a name="bkmk_define"></a> Defining hierarchies  
 Vous créez et gérez des hiérarchies à l'aide du générateur de modèles dans la vue de diagramme. La création et la gestion des hiérarchies n'est pas prise en charge dans le générateur de modèles dans la vue de données. Pour afficher le concepteur de modèles dans la Vue de diagramme, dans le menu **Modèle** , pointez sur **Vue du modèle**, puis cliquez sur **Vue de diagramme**.  
  
 Pour créer une hiérarchie, cliquez avec le bouton droit sur une colonne que vous souhaitez désigner comme niveau parent, puis sélectionnez **Créer une hiérarchie**. Vous pouvez sélectionner plusieurs colonnes (dans une table unique) à inclure, ou vous pouvez ajouter des colonnes ultérieurement comme niveaux enfants en cliquant et en faisant glisser des colonnes vers le niveau parent. Lorsque plusieurs colonnes sont sélectionnées, elles sont automatiquement placées dans un ordre selon la cardinalité. Vous pouvez modifier cet ordre en cliquant et en faisant glisser une colonne (niveau) vers un ordre différent ou en utilisant les contrôles de navigation Haut et Bas dans le menu contextuel. Lors de l'ajout d'une colonne comme niveau enfant, vous pouvez utiliser la détection automatique en faisant glisser, puis en déplaçant la colonne sur le niveau parent.  
  
 Une colonne peut apparaître dans plusieurs hiérarchies. Les hiérarchies ne peuvent pas contenir des objets autres que des colonnes, tels que des mesures ou des indicateurs de performance clé. Une hiérarchie peut être basée uniquement sur des colonnes appartenant à une seule table. Si vous sélectionnez plusieurs mesures avec une ou plusieurs colonnes, ou si vous sélectionnez des colonnes à partir de plusieurs tables, la commande **Créer une hiérarchie** est désactivée dans le menu contextuel. Pour ajouter une colonne à partir d'une table différente, utilisez la fonction RELATED DAX pour ajouter une colonne calculée qui référence la colonne de la table liée. La fonction utilise la syntaxe suivante : `=RELATED(TableName[ColumnName])`. Pour plus d'informations, consultez la fonction RELATED.  
  
 Par défaut, les nouvelles hiérarchies sont nommées hierarchy1, hiérarchie 2, etc. Il est recommandé de modifier les noms de hiérarchie afin de refléter la nature de la relation parent-enfant, et ainsi les rendre plus identifiables aux utilisateurs.  
  
 Après avoir créé des hiérarchies, vous pouvez tester leur efficacité dans votre modèle à l'aide de la fonctionnalité Analyser dans Excel. Pour plus d’informations, consultez [analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md).  
  
##  <a name="bkmk_related_tasks"></a> Related tasks  
  
|Tâche| Description|  
|----------|-----------------|  
|[Créer et gérer des hiérarchies](../../analysis-services/tabular-models/create-and-manage-hierarchies-ssas-tabular.md)|Décrit comment créer et gérer des hiérarchies à l'aide du générateur de modèles dans la vue de diagramme.|  
  
## <a name="see-also"></a>Voir aussi  
 [Générateur de modèles tabulaires](../../analysis-services/tabular-models/tabular-model-designer-ssas.md)   
 [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)   
 [Roles](../../analysis-services/tabular-models/roles-ssas-tabular.md)  
  
  
