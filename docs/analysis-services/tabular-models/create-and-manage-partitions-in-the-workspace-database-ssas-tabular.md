---
title: Créer et gérer des Partitions dans la base de données de l’espace de travail | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3912002c016508b36f200f2786e2d5f00e05c48b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-and-manage-partitions-in-the-workspace-database"></a>Créer et gérer des partitions dans la base de données de l’espace de travail 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les partitions divisent une table en sections logiques. Chaque partition peut ensuite être traitée (actualisée) indépendamment ou parallèlement à d'autres partitions. Les partitions peuvent améliorer l'évolutivité et la gestion de bases de données volumineuses. Par défaut, chaque table possède une partition qui inclut toutes les colonnes. Tâches de cette rubrique décrivent comment créer et gérer des partitions dans la base de données de modèle espace de travail à l’aide de la **le Gestionnaire de Partition** boîte de dialogue[!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]  
  
 Une fois que le modèle a été déployé sur une autre instance Analysis Services, les administrateurs de bases de données peuvent créer et gérer des partitions dans le modèle (déployé) à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [créer et gérer des Partitions de modèles tabulaires](../../analysis-services/tabular-models/create-and-manage-tabular-model-partitions-ssas-tabular.md).  
  
> [!NOTE]  
>  Vous ne pouvez pas fusionner des partitions dans la base de données model de l'espace de travail à l'aide de la boîte de dialogue Gestionnaire de partitions. Les partitions peuvent être fusionnées dans un modèle déployé uniquement à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="tasks"></a>Tâches  
 Pour créer et gérer des partitions, vous allez utiliser la boîte de dialogue **Gestionnaire de partition** . Pour consulter la boîte de dialogue **Gestionnaire de partition** , dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], cliquez sur le menu **Table** , puis sur **Partitions**.  
  
###  <a name="bkmk_create_new"></a> Pour créer une nouvelle partition  
  
1.  Dans le générateur de modèles, sélectionnez la table pour laquelle vous souhaitez définir une partition.  
  
2.  Cliquez sur le menu **Table** , puis sur **Partitions**.  
  
3.  Dans **Gestionnaire de partition**, dans la zone de liste **Table** , vérifiez ou sélectionnez la table que vous voulez partitionner, puis cliquez sur **Nouveau**.  
  
4.  Dans **Nom de la partition**, tapez un nom pour la partition. Par défaut, le nom de la partition par défaut est numéroté de manière incrémentielle pour chaque nouvelle partition.  
  
5.  Vous pouvez sélectionner les lignes et les colonnes à inclure dans la partition à l'aide du mode Aperçu de la table ou d'une requête SQL créée avec le mode Éditeur de requête.  
  
     Pour utiliser le mode Aperçu de la table (valeur par défaut), cliquez sur le bouton **Aperçu de la table** en haut à droite dans la fenêtre d’aperçu. Sélectionnez les colonnes à inclure dans la partition en activant la case à cocher en regard du nom de la colonne. Pour filtrer des lignes, cliquez avec le bouton droit sur une valeur de cellule, puis cliquez sur **Filtrer par valeur de la cellule sélectionnée**.  
  
     Pour utiliser une instruction SQL, cliquez sur le bouton **Éditeur de requête** en haut à droite dans la fenêtre d’aperçu, puis tapez ou collez une instruction de requête SQL dans la fenêtre de requête. Pour valider votre instruction, cliquez sur **Valider**. Pour utiliser le concepteur de requêtes, cliquez sur **Conception**.  
  
###  <a name="bkmk_copy"></a> Pour copier une partition  
  
1.  Dans **Gestionnaire de partition**, dans la zone de liste **Table** , vérifiez ou sélectionnez la table qui contient la partition que vous souhaitez copier.  
  
2.  Dans la liste **Partitions** , sélectionnez la partition à copier, puis cliquez sur **Copier**.  
  
3.  Dans **Nom de la partition**, tapez un nouveau nom pour la partition.  
  
###  <a name="bkmk_delete"></a> Pour supprimer une partition  
  
1.  Dans **Gestionnaire de partition**, dans la zone de liste **Table** , vérifiez ou sélectionnez la table qui contient la partition à supprimer.  
  
2.  Dans la liste **Partitions** , sélectionnez la partition à supprimer, puis cliquez sur **Supprimer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Partitions](../../analysis-services/tabular-models/partitions-ssas-tabular.md)   
 [Traiter les partitions dans la base de données d’espace de travail](../../analysis-services/tabular-models/process-partitions-in-the-workspace-databse-ssas-tabular.md)  
  
  
