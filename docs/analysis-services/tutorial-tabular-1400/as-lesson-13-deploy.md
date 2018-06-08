---
title: 'Leçon du didacticiel Analysis Services 13 : déployer | Documents Microsoft'
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8ed155c0d3621c59e844e30d10dc7117e587a8f9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deploy"></a>Déployer

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

Dans cette leçon, vous configurez les propriétés de déploiement ; spécification de déployer sur un serveur et un nom pour le modèle. Vous déployez ensuite le modèle sur le serveur. Une fois votre modèle est déployé, les utilisateurs peuvent se connecter à ce dernier à l’aide d’une application cliente de création de rapports. Pour plus d’informations, consultez [déployer vers Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy) et [déploiement de solutions de modèle tabulaire](../tabular-models/tabular-model-solution-deployment-ssas-tabular.md).  
  
Durée estimée pour effectuer cette leçon : **5 minutes**  
  
## <a name="prerequisites"></a>Configuration requise  

Cet article fait partie d’un didacticiel de modélisation tabulaire, qui doit être effectué dans l’ordre. Avant d’effectuer les tâches de cette leçon, vous devez avoir terminé la leçon précédente : [leçon 12 : analyser dans Excel](../tutorial-tabular-1400/as-lesson-12-analyze-in-excel.md).  

> [!IMPORTANT]  
> Si le déploiement sur Azure Analysis Services, vous devez disposer [des autorisations d’administrateur](https://docs.microsoft.com/azure/analysis-services/analysis-services-server-admins) sur la serever.  

> [!IMPORTANT]  
> Si vous avez installé la base de données exemple AdventureWorksDW sur un ordinateur local SQL Server, et que vous déployez votre modèle à un serveur Azure Analysis Services, un [passerelle de données locale](https://docs.microsoft.com/azure/analysis-services/analysis-services-gateway) est requis.
  
## <a name="deploy-the-model"></a>Déployer le modèle  
  
#### <a name="to-configure-deployment-properties"></a>Pour configurer les propriétés de déploiement  

  
1.  Dans **l’Explorateur de solutions**, avec le bouton droit le **AW Internet Sales** de projet, puis cliquez sur **propriétés**.  
  
2.  Dans le **Pages de propriétés de AW Internet Sales** boîte de dialogue **serveur de déploiement**, dans le **Server** propriété, entrez le nom du serveur complet. Si vous vous connectez à Azure Analysis Services, le nom du serveur doit incluent l’URL complète.

    ![en tant que-lesson13-déployer-propriété](../tutorial-tabular-1400/media/as-lesson13-deploy-property.png)
  
3.  Dans le **base de données** , tapez **Internet Sales Adventure Works**.  
  
4.  Dans le **Nom_modèle** , tapez **Adventure Works Internet Sales Model**.  
  
5.  Vérifiez vos sélections, puis cliquez sur **OK**.  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a>Pour déployer les ventes Internet Adventure Works
  
1.  Dans **l’Explorateur de solutions**, avec le bouton droit le **AW Internet Sales** projet > **Build**.  

2.  Cliquez sur le **AW Internet Sales** projet > **déployer**.

    Lors du déploiement vers Azure Analysis Services, vous pouvez être invité à entrer votre compte. Permet d’entrer votre compte professionnel et un mot de passe, par exemple nancy@adventureworks.com. Ce compte doit être dans les administrateurs sur le serveur.
  
    La boîte de dialogue déployer apparaît et affiche l’état du déploiement des métadonnées et chaque table incluse dans le modèle.  
    
    ![en tant que-lesson13-déployer-status](../tutorial-tabular-1400/media/as-lesson13-deploy-status.png)
  
3. Une fois le déploiement correctement effectué, cliquez sur **Fermer**.  
  

Cette leçon décrit la méthode plus courante et la plus simple pour déployer un modèle tabulaire à partir de SSDT. Options de déploiement avancées telles que l’Assistant de déploiement ou l’automatisation avec XMLA et AMO fournissent une plus grande souplesse, la cohérence et les déploiements planifiés. Pour plus d’informations, consultez [déploiement de solutions de modèle tabulaire](../tabular-models/tabular-model-solution-deployment-ssas-tabular.md).

## <a name="conclusion"></a>Conclusion  
Félicitations ! Vous avez terminé de créer et déployer votre premier modèle tabulaire Analysis Services. Ce didacticiel vous a guidés dans les tâches courantes pour créer un modèle tabulaire. Maintenant que votre modèle Internet Sales Adventure Works est déployé, vous pouvez utiliser SQL Server Management Studio pour gérer le modèle, créer des scripts de processus et un plan de sauvegarde. Les utilisateurs peuvent également maintenant vous connecter à l’aide d’une application cliente de création de rapports tels que Microsoft Excel ou Power BI.  

![en tant que ssms de lesson13](../tutorial-tabular-1400/media/as-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Quelle est l’étape suivante ?
[Se connecter avec Power BI Desktop](https://docs.microsoft.com/azure/analysis-services/analysis-services-connect-pbi)   
[Leçon supplémentaire - sécurité dynamique](../tutorial-tabular-1400/as-supplemental-lesson-dynamic-security.md)   
[Leçon supplémentaire - les lignes de détails](../tutorial-tabular-1400/as-supplemental-lesson-detail-rows.md)   
[Leçon supplémentaire - hiérarchies irrégulières](../tutorial-tabular-1400/as-supplemental-lesson-ragged-hierarchies.md)   
