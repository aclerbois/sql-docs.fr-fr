---
title: Modifier ou supprimer une base de données Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 44952f477cf16a169ad96f2bfe881b16ee9738da
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="modify-or-delete-an-analysis-services-database"></a>Modifier ou supprimer une base de données Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez modifier le nom et la description d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] avant son déploiement dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et après son déploiement dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez également régler des paramètres supplémentaires sur une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , en fonction de l'environnement.  
  
> [!NOTE]  
>  Vous ne pouvez pas modifier les propriétés de base de données à l'aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode en ligne.  
  
## <a name="modifying-databases-using-sql-server-management-studio"></a>Modification des bases de données à l'aide de SQL Server Management Studio  
 Une fois qu'une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] a été déployée, vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour modifier le mode d'emprunt d'identité utilisé par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] lors de la connexion à des sources de données incluses dans la base de données. Le mode d'emprunt d'identité vous permet de spécifier le contexte de sécurité utilisé par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] lors d'une tentative de connexion à une source de données pour des tâches de traitement, de navigation ou d'extraction.  
  
## <a name="modifying-databases-using-sql-server-data-tools"></a>Modification des bases de données à l'aide des outils de données SQL Server  
 Vous pouvez utiliser [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode projet pour modifier les traductions de la légende et de la description d’un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilisé pour définir une base de données. Pour plus d’informations sur l’utilisation de traductions dans une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , consultez [Scénarios de globalisation pour Analysis Services](../../analysis-services/globalization-scenarios-for-analysis-services.md).  
  
 Vous pouvez également définir les alias et les fonctions d'agrégation associés aux types de comptes utilisés par les attributs de compte dans les dimensions incluses dans la base de données. Les alias vous permettent de sélectionner la terminologie d'entreprise utilisée par votre organisation pour les types de comptes dans un plan comptable. Les types de comptes sont utilisés par les membres d'un attribut de compte pour indiquer comment agréger les mesures sur chaque membre en utilisant les fonctions d'agrégation spécifiées pour chaque type de compte inclus dans la base de données. Pour plus d’informations sur les attributs de compte, consultez [Attributs et hiérarchies d’attributs](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md).  
  
## <a name="deleting-databases"></a>suppression de bases de données  
 La suppression d'une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante entraîne la suppression de la base de données et de tous les cubes, dimensions et modèles d'exploration de données inclus dans la base de données. Vous pouvez supprimer uniquement une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] existante dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-delete-an-analysis-services-database"></a>Pour supprimer une base de données Analysis Services  
  
1.  Connectez-vous à une instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
2.  Dans **l’Explorateur d’objets**, développez le nœud de l’instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] connectée et vérifiez que l’objet à supprimer est visible.  
  
3.  Cliquez avec le bouton droit sur l’objet à supprimer et sélectionnez **Supprimer**.  
  
4.  Dans la boîte de dialogue **Supprimer l'objet** , cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Document and Script d’une base de données Analysis Services](../../analysis-services/multidimensional-models/document-and-script-an-analysis-services-database.md)  
  
  
