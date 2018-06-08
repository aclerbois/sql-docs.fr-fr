---
title: 'Leçon 14 : Déployer | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 533b6197c72d03876b928f4024fc5eb4fb0f2fc0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-13-deploy"></a>Leçon 13 : déployer
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../includes/ssas-appliesto-sql2016-later-aas.md)]

Dans cette leçon, vous allez configurer les propriétés de déploiement ; spécification d’un site local ou instance de serveur Windows Azure et un nom pour le modèle. Vous allez ensuite déployer le modèle à cette instance. Une fois votre modèle est déployé, les utilisateurs peuvent se connecter à ce dernier à l’aide d’une application cliente de création de rapports. Pour en savoir plus sur le déploiement, consultez [déploiement de solutions de modèle tabulaire](../analysis-services/tabular-models/tabular-model-solution-deployment-ssas-tabular.md) et [déployer vers Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Durée estimée pour effectuer cette leçon : **5 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  
Cette rubrique fait partie d'un didacticiel de modélisation tabulaire, qui doit être suivi dans l'ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 12 : analyser dans Excel](../analysis-services/lesson-12-analyze-in-excel.md).  
  
## <a name="deploy-the-model"></a>Déployer le modèle  
  
#### <a name="to-configure-deployment-properties"></a>Pour configurer les propriétés de déploiement  
  
1.  Dans **l’Explorateur de solutions**, avec le bouton droit le **AW Internet Sales** de projet, puis cliquez sur **propriétés**.  
  
2.  Dans le **Pages de propriétés de AW Internet Sales** boîte de dialogue **serveur de déploiement**, dans le **serveur** , tapez le nom d’un serveur Azure Analysis Services ou d’un serveur local de l’instance en cours d’exécution en mode tabulaire. Il s’agit de l’instance de serveur que à votre modèle sera déployé.  

    ![agents-déployer-déploiement-server-propriété](../analysis-services/media/aas-deploy-deployment-server-property.png)
 
    > [!IMPORTANT]  
    > Vous devez disposer des autorisations d’administrateur sur le distant Analysis Services instance afin de déployer sur celle-ci.  
  
3.  Dans le **base de données** , tapez **Internet Sales Adventure Works**.  
  
4.  Dans le **Nom_modèle** , tapez **Adventure Works Internet Sales Model**.  
  
5.  Vérifiez vos sélections, puis cliquez sur **OK**.  
  
#### <a name="to-deploy-the-adventure-works-internet-sales-tabular-model"></a>Pour déployer le modèle tabulaire Internet Sales Adventure Works  
  
1.  Dans **l’Explorateur de solutions**, avec le bouton droit le **AW Internet Sales** projet > **Build**.  

2.  Cliquez sur le **AW Internet Sales** projet > **déployer**.

    Lors du déploiement vers Azure Analysis Services, vous allez probablement être invité à entrer votre compte. Permet d’entrer votre compte professionnel et un mot de passe, par exemple nancy@adventureworks.com. Ce compte doit être dans les administrateurs sur l’instance de serveur.
  
    La boîte de dialogue Déployer apparaît et affiche l'état de déploiement des métadonnées ainsi que de chaque table incluse dans le modèle.  
    
    ![état déployer des agents](../analysis-services/media/aas-deploy-status.png)
  
3. Une fois le déploiement correctement effectué, cliquez sur **Fermer**.  
  
## <a name="conclusion"></a>Conclusion  
Félicitations ! Vous avez terminé de créer et déployer votre premier modèle tabulaire Analysis Services. Ce didacticiel vous a guidés dans les tâches courantes pour créer un modèle tabulaire. Maintenant que votre modèle Internet Sales Adventure Works est déployé, vous pouvez utiliser SQL Server Management Studio pour gérer le modèle, créer des scripts de processus et un plan de sauvegarde. Les utilisateurs peuvent également maintenant vous connecter à l’aide d’une application cliente de création de rapports tels que Microsoft Excel ou Power BI.  

![en tant que-tabulaire-lesson13-ssms](../analysis-services/media/as-tabular-lesson13-ssms.png)
  
  
## <a name="see-also"></a>Voir aussi  
[Mode DirectQuery](../analysis-services/tabular-models/directquery-mode-ssas-tabular.md)  
[Configurer les propriétés par défaut de la modélisation et du déploiement des données](../analysis-services/tabular-models/configure-default-data-modeling-and-deployment-properties-ssas-tabular.md)  
[Bases de données Model tabulaire](../analysis-services/tabular-models/tabular-model-databases-ssas-tabular.md)  
  
  
  ## <a name="whats-next"></a>Quelle est l’étape suivante ?
*  [Leçon supplémentaire - implémenter la sécurité dynamique à l’aide de filtres de lignes](../analysis-services/supplemental-lesson-implement-dynamic-security-by-using-row-filters.md).

*  [Supplémentaires leçon - configurer les propriétés de création de rapports pour les rapports Power View](../analysis-services/supplemental-lesson-configure-reporting-properties-for-power-view-reports.md).
