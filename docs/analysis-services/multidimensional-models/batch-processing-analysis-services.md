---
title: Traitement par lots (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6d2f72dd0c7b380391a7b500d494d5aac83c4c6f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="batch-processing-analysis-services"></a>Traitement par lots (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous pouvez utiliser la commande batch pour envoyer plusieurs commandes de traitement au serveur dans une demande unique. Le traitement par lots vous offre une méthode pour contrôler les objets qui doivent être traités, et dans quel ordre. De plus, un traitement par lots peut s'exécuter en tant que série de travaux autonomes ou en tant que transaction dans laquelle l'échec d'un processus entraîne une annulation de l'ensemble du traitement par lots.  
  
 Le traitement par lots optimise la disponibilité des données en consolidant et en réduisant la durée nécessaire à la validation des modifications. Lorsque vous traitez entièrement une dimension, toute partition qui utilise cette dimension est marquée comme non traitée. En conséquence, les cubes qui contiennent les partitions non traitées sont indisponibles pour l'exploration. Vous pouvez résoudre ce problème à l'aide d'un travail de traitement par lots en traitant les dimensions avec les partitions affectées. L'exécution du travail de traitement par lots en tant que transaction permet de s'assurer que tous les objets inclus dans la transaction demeurent disponibles pour les requêtes jusqu'à ce que tout le traitement soit terminé. Lorsque la transaction valide les modifications, des verrous sont placés sur les objets affectés, ce qui les rend temporairement indisponibles, mais globalement, la durée nécessaire pour valider les modifications est moindre que si vous traitiez des objets individuellement.  
  
 Les procédures de cette rubrique décrivent les étapes de traitement complet de dimensions et de partitions. Le traitement par lots peut également inclure d'autres options de traitement, telles que le traitement incrémentiel. Pour que ces procédures fonctionnent correctement, vous devez utiliser une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante qui contient au moins deux dimensions et une partition.  
  
 Cette rubrique comprend les sections suivantes :  
  
 [Traitement par lots dans SQL Server Data Tools](#bkmk_ssdt)  
  
 [Traitement par lots à l'aide de XMLA dans Management Studio](#bkmk_xmla)  
  
##  <a name="bkmk_ssdt"></a> Traitement par lots dans SQL Server Data Tools  
 Pour que des objets puissent être traités dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], le projet contenant les objets doit être déployé. Pour plus d’informations, consultez [Déployer des projets Analysis Services &#40;SSDT&#41;](../../analysis-services/multidimensional-models/deploy-analysis-services-projects-ssdt.md).  
  
1.  Ouvrez [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
2.  Ouvrez un projet qui a été déployé.  
  
3.  Dans l'Explorateur de solutions, sous le projet déployé, développez le dossier **Dimensions** .  
  
4.  En maintenant la touche Ctrl enfoncée, cliquez sur chaque dimension répertoriée dans le dossier **Dimensions** .  
  
5.  Cliquez avec le bouton droit sur les dimensions sélectionnées, puis cliquez sur **Traiter**.  
  
6.  En maintenant la touche Ctrl enfoncée, cliquez sur chaque dimension répertoriée dans **Liste d'objets**.  
  
7.  Cliquez avec le bouton droit sur les dimensions sélectionnées et cliquez sur **Traiter entièrement**.  
  
8.  Pour personnaliser le travail de traitement par lots, cliquez sur **Modifier les paramètres**.  
  
9. Sous **Options de traitement**, marquez les paramètres suivants :  
  
    -   **Ordre de traitement** a la valeur **Séquentiel**et **Mode de transaction** a la valeur **Une seule transaction**.  
  
    -   **Option de la table d'écriture différée** a la valeur **Utiliser l'existante**.  
  
    -   Sous **Objets affectés**, activez la case à cocher **Traiter les objets affectés** .  
  
10. Cliquez sur l'onglet **Erreurs de clé de dimension** . Vérifiez que l'option **Utiliser la configuration d'erreur par défaut** est sélectionnée.  
  
11. Cliquez sur **OK** pour fermer l'écran **Modifier les paramètres** .  
  
12. Cliquez sur **Exécuter** sur l'écran **Traiter les objets** pour démarrer le travail de traitement.  
  
13. Lorsque la zone **État** indique **Traitement réussi**, cliquez sur **Fermer**.  
  
14. Cliquez sur **Fermer** sur l'écran **Traiter les objets** .  
  
##  <a name="bkmk_xmla"></a> Traitement par lots à l'aide de XMLA dans Management Studio  
 Vous pouvez créer un script XMLA qui exécute le traitement par lots. Commencez en générant un script XMLA dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour chaque objet, puis associez-les dans une seule requête XMLA que vous exécutez de façon interactive ou dans une tâche planifiée.  
  
 Pour obtenir des dansstructions étape par étape, consultez l’ **exemple 2** dans [Schedule SSAS Admdansistrative Tasks with SQL Server Agent](../../analysis-services/instances/schedule-ssas-administrative-tasks-with-sql-server-agent.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)  
  
  
