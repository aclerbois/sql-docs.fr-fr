---
title: Supprimer une Source de données dans l’Explorateur de solutions (SSAS multidimensionnel) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 51510891524b77ee0a2edaa33f024c538dcd4b81
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="delete-a-data-source-in-solution-explorer-ssas-multidimensional"></a>Supprimer une source de données dans l'Explorateur de solutions (SSAS Multidimensionnel)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez supprimer un objet de source de données afin de l'effacer définitivement d'un projet de modèle multidimensionnel Analysis Services.  
  
 Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], les sources de données constituent la base sur laquelle les vues de sources de données sont construites et les vues de sources de données servent à leur tour à définir des dimensions, des cubes et des structures d'exploration de données dans un projet ou une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Par conséquent, la suppression d'une source de données peut rendre d'autres objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] non valides. Pensez à toujours vérifier la liste des objets dépendants qui est fournie avant de supprimer l'objet.  
  
> [!IMPORTANT]  
>  Les sources de données dont dépendent d’autres objets ne peuvent pas être supprimées d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ouverte par [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode en ligne. Vous devez supprimer tous les objets de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui dépendent de cette source de données avant de supprimer la source de données. Pour plus d’informations sur le mode en ligne, consultez [Connexion en mode en ligne à une base de données Analysis Services](../../analysis-services/multidimensional-models/connect-in-online-mode-to-an-analysis-services-database.md).  
  
### <a name="to-delete-a-data-source"></a>Pour supprimer une source de données  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet ou connectez-vous à la base de données dans laquelle vous souhaitez supprimer une source de données.  
  
2.  Dans **l’Explorateur de solutions**, développez le dossier **Sources de données** .  
  
3.  Cliquez avec le bouton droit sur la source de données, puis sélectionnez **Supprimer**. La boîte de dialogue **Supprimer les objets**  qui s’affiche indique les objets qui seront rendus non valides si vous supprimez la source de données. Examinez soigneusement cette liste avant de cliquer sur **OK** pour supprimer la source de données.  
  
4.  Enregistrez le projet.  
  
     Après avoir supprimé une source de données dans un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous devez enregistrer le projet modifié. Dans le cas contraire, vous recevrez un message d’erreur la prochaine fois que vous ouvrirez le projet, car le fichier XML sous-jacent de la source de données supprimée sera manquant lorsque le projet tentera de charger la source de données supprimée.  
  
## <a name="see-also"></a>Voir aussi  
 [Sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-sources-in-multidimensional-models.md)   
 [Sources de données prises en charge &#40;SSAS – Multidimensionnel&#41;](../../analysis-services/multidimensional-models/supported-data-sources-ssas-multidimensional.md)  
  
  
