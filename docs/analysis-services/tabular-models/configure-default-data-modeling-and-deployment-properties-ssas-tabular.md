---
title: Configurer les propriétés de déploiement et de modélisation de données par défaut | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 926cb59f263b5dfbfb4178d4d512b07511cdc0cc
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-default-data-modeling-and-deployment-properties"></a>Configurer les propriétés de déploiement et de modélisation de données par défaut 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Cet article décrit comment configurer le niveau de compatibilité par défaut, de déploiement et les paramètres de propriété espace de travail de base de données, qui peuvent être prédéfinis pour chaque nouveau modèle tabulaire de projet vous créent dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Après qu'un projet a été créé, ces propriétés peuvent être encore être modifiées selon vos besoins.  
  
#### <a name="to-configure-the-default-compatibility-level-property-setting-for-new-model-projects"></a>Pour configurer le paramètre de propriété Niveau de compatibilité par défaut pour les nouveaux projets de modèle  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur le menu **Outils** , puis sur **Options**.  
  
2.  Dans la boîte de dialogue **Options** , développez **Concepteurs tabulaires Analysis Services**, puis cliquez sur **Niveau de compatibilité**.  
  
3.  Configurez les paramètres de propriétés suivants :  
  
    |Propriété|Paramètre par défaut| Description|  
    |--------------|---------------------|-----------------|  
    |**Niveau de compatibilité par défaut pour les nouveaux projets**|SQL Server 2016 (1200)|Ce paramètre spécifie le niveau de compatibilité par défaut à utiliser pour créer un projet de modèle tabulaire. Vous pouvez choisir SQL Server 2012 (1100) si vous comptez effectuer le déploiement sur une instance Analysis Services sans avoir appliqué le SP1, ou SQL Server 2012 SP1 ou plus si le SP1 est appliqué sur votre instance de déploiement. Pour plus d’informations, consultez [Niveau de compatibilité pour les modèles tabulaires dans Analysis Services](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md).|  
    |**Options de niveau de compatibilité**|Toutes activées|Spécifie les options de niveau de compatibilité pour les nouveaux projets de modèles tabulaires et le déploiement vers une autre instance Analysis Services.|  
  
#### <a name="to-configure-the-default-deployment-server-property-setting-for-new-model-projects"></a>Pour configurer le paramètre de propriété du serveur de déploiement par défaut pour les nouveaux projets de modèle  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur le menu **Outils** , puis sur **Options**.  
  
2.  Dans la boîte de dialogue **Options** , développez **Concepteurs tabulaires Analysis Services**, puis cliquez sur **Déploiement**.  
  
3.  Configurez les paramètres de propriétés suivants :  
  
    |Propriété|Paramètre par défaut| Description|  
    |--------------|---------------------|-----------------|  
    |**Serveur de déploiement par défaut**|localhost|Ce paramètre spécifie le serveur par défaut à utiliser lors du déploiement d'un modèle. Vous pouvez cliquer sur la flèche du bas pour rechercher des serveurs Analysis Services sur le réseau local ou vous pouvez taper le nom d'un serveur distant.|  
  
    > [!NOTE]  
    >  Les modifications apportées au paramètre de propriété du serveur de déploiement par défaut n'affecteront pas les projets existants créés avant la modification.  
  
###  <a name="bkmk_conf_default"></a> Pour configurer les paramètres de propriété Base de données d'espace de travail par défaut pour les nouveaux projets de modèle  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur le menu **Outils** , puis sur **Options**.  
  
2.  Dans la boîte de dialogue **Options** , développez **Concepteurs tabulaires Analysis Services**, puis cliquez sur **Base de données d'espace de travail**.  
  
3.  Configurez les paramètres de propriétés suivants :  
  
    |Propriété|Paramètre par défaut| Description|  
    |--------------|---------------------|-----------------|  
    |**Serveur d'espace de travail par défaut**|**localhost**|Cette propriété indique le serveur par défaut qui sera utilisé pour héberger la base de données de l'espace de travail tandis que le modèle est créé dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Toutes les instances disponibles d'Analysis Services qui fonctionnent sur l'ordinateur local sont incluses dans la zone de liste.<br /><br /> <br /><br /> Remarque : il est recommandé de toujours spécifier un serveur Analysis Services local en tant que serveur d’espace de travail. Pour les bases de données d’espace de travail sur serveur distant, l’importation de données depuis [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’est pas prise en charge ; les données ne peuvent pas être sauvegardées localement et l’interface utilisateur peut rencontrer une latence pendant les requêtes.|  
    |**Rétention de la base de données d'espace de travail une fois le modèle fermé**|**Garder des bases de données d'espace de travail sur le disque, mais décharger de la mémoire**|Spécifie comment une base de données d'espace de travail est conservée une fois que le modèle a été fermé. Une base de données d'espace de travail inclut les métadonnées du modèle, les données importées dans un modèle et les informations d'identification d'emprunt d'identité (chiffrées). Dans certains cas, la base de données d'espace de travail peut être très volumineuse et consommer une quantité significative de mémoire. Par défaut, les bases de données d'espace de travail sont supprimées de la mémoire. Lors de la modification de ce paramètre, il est important de considérer vos ressources mémoire disponibles ainsi que la fréquence à laquelle vous projetez de travailler sur le modèle. Ce paramètre de propriété a les options suivantes :<br /><br /> **Conserver les espaces de travail en mémoire** : indique de conserver les espaces de travail en mémoire une fois le modèle fermé. Cette option consommera plus de mémoire ; toutefois, lors de l'ouverture d'un modèle dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], moins de ressources sont consommées et l'espace de travail se charge plus vite.<br /><br /> **Conserver les bases de données d’espace de travail sur le disque, mais les décharger de la mémoire** : indique de garder la base de données d’espace de travail sur le disque, mais pas dans la mémoire, une fois le modèle fermé. Cette option consommera moins de mémoire ; toutefois, lors de l'ouverture d'un modèle dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], des ressources supplémentaires seront consommées et le modèle se chargera plus lentement que si la base de données de l'espace de travail est conservée en mémoire. Utilisez cette option lorsque les ressources en mémoire sont limitées ou lorsque vous travaillez sur une base de données d'espace de travail distante.<br /><br /> **Supprimer l’espace de travail** : indique de supprimer la base de données d’espace de travail de la mémoire et de ne pas la garder sur le disque une fois le modèle fermé. Cette option consommera moins de mémoire et d’espace de stockage. Toutefois, lors de l’ouverture d’un modèle dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], des ressources supplémentaires sont consommées et le modèle se charge plus lentement que si la base de données de l’espace de travail est conservée en mémoire ou sur le disque. Utilisez cette option uniquement si vous travaillez occasionnellement sur les modèles.|  
    |**Sauvegarde de données**|**Conserver la sauvegarde des données sur le disque**|Spécifie si une sauvegarde des données du modèle est conservée dans un fichier de sauvegarde. Ce paramètre de propriété a les options suivantes :<br /><br /> **Conserver la sauvegarde des données sur le disque** : indique de garder une sauvegarde des données du modèle sur le disque. Lorsque le modèle est enregistré, les données sont également enregistrées dans le fichier de sauvegarde (ABF). La sélection de cette option peut ralentir l'enregistrement du modèle et le temps de chargement.<br /><br /> **Ne pas conserver de sauvegarde des données sur le disque** : indique de ne pas garder de sauvegarde des données du modèle sur le disque. Cette option réduit le temps d'enregistrement et de chargement du modèle.|  
  
> [!NOTE]  
>  Les modifications apportées aux propriétés de modèle par défaut n'affecteront pas les propriétés des modèles existants créés avant la modification.  
  
## <a name="see-also"></a>Voir aussi  
 [Propriétés du projet](../../analysis-services/tabular-models/project-properties-ssas-tabular.md)   
 [Propriétés de modèle](../../analysis-services/tabular-models/model-properties-ssas-tabular.md)   
 [Niveau de compatibilité](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)  
  
  
