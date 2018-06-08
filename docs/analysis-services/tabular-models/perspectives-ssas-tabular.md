---
title: Perspectives | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3e687116fa1f388da3b59884c29526cb9e7dd629
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="perspectives"></a>Perspectives
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Dans les modèles tabulaires, les perspectives définissent des sous-ensembles visualisables d'un modèle et des points de vue du modèle focalisés sur un domaine d'activité ou sur une application.  
  
##  <a name="bkmk_understanding"></a> Avantages  
 L'exploration des modèles tabulaires peut s'avérer très complexe pour les utilisateurs. Un modèle unique peut représenter le contenu d'un entrepôt de données complet avec de nombreuses tables, mesures et dimensions. Une telle complexité peut dérouter les utilisateurs qui n'ont besoin d'interagir qu'avec une petite partie du modèle afin de répondre à leurs besoins en termes de décisionnel et de rapports.  
  
 Dans une perspective, les tables, les colonnes et les mesures (dont les KPI) sont définis en tant qu'objets de champ. Vous pouvez sélectionner les champs visibles pour chaque perspective. Par exemple, un modèle peut contenir des données relatives au produit et aux ventes, ainsi que des données financières, des données géographiques et des données relatives aux employés. Si le service commercial a sans doute besoin des données relatives aux produits, aux ventes et aux promotions, ainsi que des données géographiques, il est peut probable qu'il s'intéresse aux données des employés et aux données financières. De même, le service des ressources humaines ne s'intéresse pas aux données relatives aux ventes et aux promotions, ni aux données géographiques.  
  
 Lorsqu'un utilisateur se connecte à un modèle (comme une source de données) avec des perspectives définies, il peut sélectionner la perspective qu'il souhaite utiliser. Par exemple, lors d'une connexion à une source de données de modèle dans Excel, les utilisateurs RH peuvent sélectionner la perspective Ressources Humaines sur la page Sélectionner des tables et des vues de l'Assistant Connexion de données. Seuls les champs (tables, colonnes et mesures) définis pour la perspective Ressources Humaines seront visibles dans la liste de champs du tableau croisé dynamique.  
  
 Les perspectives ne sont pas censées servir de mécanisme de sécurité, mais elles constituent plutôt un outil qui optimise l'expérience de l'utilisateur. L'intégralité de la sécurité d'une perspective donnée est héritée du modèle sous-jacent. Les perspectives ne peuvent pas donner accès à des objets de modèle auxquels un utilisateur n'a pas déjà accès. La sécurité de la base de données model doit d'abord être résolue, avant que l'utilisateur puisse accéder aux objets du modèle par le biais d'une perspective. Les rôles de sécurité peuvent permettre de sécuriser les métadonnées et les données de modèle. Pour plus d’informations, consultez [rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md).  
  
##  <a name="bkmk_testpersp"></a> Testing perspectives  
 Lors de la création d'un modèle, vous pouvez utiliser la fonctionnalité Analyser dans Excel dans le générateur de modèles afin de tester l'efficacité des perspectives que vous avez définies. Dans le menu **Modèle** du générateur de modèles, lorsque vous cliquez sur **Analyser dans Excel**, avant qu'Excel ne s'ouvre, la boîte de dialogue **Choisir les informations d'identification et la perspective** s'affiche. Dans cette boîte de dialogue, vous pouvez spécifier le nom d'utilisateur actuel, un utilisateur différent, un rôle et une perspective que vous utiliserez pour vous connecter à la base de données model de l'espace de travail comme source de données et afficher les données.  
  
##  <a name="bkmk_related_tasks"></a> Related tasks  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Créer et gérer des perspectives](../../analysis-services/tabular-models/create-and-manage-perspectives-ssas-tabular.md)|Décrit comment créer et gérer des perspectives à l'aide de la boîte de dialogue Perspectives dans le générateur de modèles.|  
  
## <a name="see-also"></a>Voir aussi  
 [Rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md)   
 [Hiérarchies](../../analysis-services/tabular-models/hierarchies-ssas-tabular.md)  
  
  
