---
title: Définir l’écriture différée de Partition | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 844aad81d49f16718cb795f443c3d8101e2ff771
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="set-partition-writeback"></a>Définir l'écriture différée de partition
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Si vous activez en écriture un groupe de mesures, les utilisateurs finaux peuvent modifier les données du cube lorsqu'ils le parcourent, et les modifications sont stockées dans une table séparée appelée « table d'écriture différée », et non dans les données du cube ou les données sources. Les utilisateurs finaux qui explorent une partition activée en écriture peuvent voir le résultat de toutes les modifications dans la table d'écriture différée de la partition.  
  
 Vous pouvez parcourir ou supprimer les données en écriture différée. Vous pouvez aussi convertir en partition les données en écriture différée. Sur une partition activée en écriture, vous pouvez utiliser des rôles de cube pour accorder l'accès en lecture/écriture aux utilisateurs et aux groupes d'utilisateurs et pour limiter l'accès à des cellules ou des groupes de cellules spécifiques de la partition.  
  
 Pour obtenir une présentation visuelle courte de l’écriture différée, consultez [Excel 2010 Writeback to Analysis Services](http://go.microsoft.com/fwlink/p/?LinkId=394951)(Écriture différée Excel 2010 vers Analysis Services). Pour obtenir une description plus détaillée de cette fonctionnalité, consultez la série de publications de blog [Building a Writeback Application with Analysis Services (blog)](http://go.microsoft.com/fwlink/?LinkId=394977)(Création d’une application d’écriture différée avec Analysis Services).  
  
> [!NOTE]  
>  L’écriture différée est prise en charge pour les mini-Data Warehouse et les bases de données relationnelles SQL Server, et uniquement pour les modèles multidimensionnels [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
## <a name="how-to-write-enable-a-partition"></a>Comment activer une partition en écriture  
 Pour activer en écriture les groupes de mesures d'une partition, activez en écriture la partition elle-même dans le Concepteur de cube dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   Dans le Concepteur de cube, sous l’onglet Partitions, cliquez avec le bouton droit sur une partition et choisissez **Paramètres d’écriture différée**.  
  
-   Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], développez la base de données | le cube | le groupe de mesures, puis cliquez avec le bouton droit sur **Écriture différée** et choisissez **Activer l’écriture différée**.  
  
 L'écriture différée est prise en charge uniquement pour les mesures qui utilisent l'agrégation SUM. Dans l'exemple de base de données AdventureWorks, vous pouvez utiliser le groupe de mesures Sales Targets pour tester les comportements de l'écriture différée.  
  
 Lorsque vous activez en écriture une partition, vous spécifiez un nom de table et une source de données pour le stockage de la table d'écriture différée. Toutes les modifications ultérieures apportées au groupe de mesures sont enregistrées dans cette table.  
  
## <a name="browse-writeback-data-in-a-partition"></a>Parcourir les données en écriture différée dans une partition  
 Vous pouvez explorer le contenu de la table d’écriture différée d’un cube dans la boîte de dialogue **Parcourir les données** , accessible en cliquant avec le bouton droit sur une partition activée en écriture sous l’onglet **Partitions** dans le Concepteur de cube.  
  
## <a name="delete-writeback-data-or-disable-writeback"></a>Supprimer les données en écriture différée ou désactiver l'écriture différée  
 La suppression des données d'écriture différée vide le cache d'écriture différée ; dès que ces données sont supprimées, les tâches d'écriture différée supplémentaires sont effectuées dans un état propre. La désactivation de l'écriture différée pour une partition de cube désactive simplement l'écriture différée pour cette partition.  
  
## <a name="convert-writeback-data-to-a-partition"></a>Convertir les données en écriture différée en partition  
 Vous pouvez convertir en partition les données d'une table d'écriture différée d'une partition. À la suite de cette procédure, la table d'écriture différée devient la nouvelle table de faits de la partition.  
  
> [!CAUTION]  
>  Une mauvaise utilisation des partitions peut aboutir à des données de cube incorrectes. Pour plus d’informations, consultez [Créer et gérer une partition locale &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/create-and-manage-a-local-partition-analysis-services.md).  
  
 La conversion en partition de la table de données d'écriture différée désactive également la partition en écriture. Toutes les stratégies de lecture/écriture illimitée et les autorisations de lecture/écriture pour les cellules de la partition sont désactivées, et les utilisateurs finaux ne pourront pas changer les données de cube affichées. (Les utilisateurs finaux dont la stratégie de lecture/écriture illimitée ou les autorisations de lecture/écriture sont désactivées pourront encore explorer le cube.) Les autorisations Lire et Lire le contingent ne sont pas affectées.  
  
 Pour convertir en partition des données d’écriture différée, utilisez la boîte de dialogue **Convertir en partition**, accessible en cliquant avec le bouton droit sur la table d’écriture différée pour une partition activée en écriture dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous spécifiez un nom pour la partition et indiquez s'il convient de concevoir l'agrégation pour la partition ultérieurement ou au même moment que vous la créez. Pour créer l'agrégation au même moment ou vous choisissez la partition, vous devez choisir de copier la conception d'agrégation à partir d'une partition existante. Il s'agit normalement, mais pas obligatoirement, de la partition d'écriture différée actuelle. Vous pouvez également choisir de traiter la partition au même moment que vous la créez.  
  
## <a name="see-also"></a>Voir aussi  
 [Partitions activées en écriture](../../analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-write-enabled-partitions.md)   
 [Activation de l’écriture différée sur un Cube OLAP au niveau des cellules dans Excel 2010](http://go.microsoft.com/fwlink/p/?LinkId=394952)   
 [Activation et sécurisation de l’entrée de données avec écriture différée d’Analysis Services](http://go.microsoft.com/fwlink/p/?LinkId=394953)  
  
  
