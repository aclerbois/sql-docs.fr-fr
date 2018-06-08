---
title: Créer et gérer des rôles | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5d97cd04228b13d0f57d99b6f8808a955bba1bea
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-and-manage-roles"></a>Créer et gérer des rôles 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Les rôles, dans les modèles tabulaires, définissent des autorisations de membre pour un modèle. Les rôles sont définis pour un projet de modèle à l'aide de la boîte de dialogue Gestionnaire de rôles de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Lorsqu'un modèle est déployé, les administrateurs de base de données peuvent gérer les rôles à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Les tâches de cet article décrivent comment créer et gérer les rôles pendant la création d’un modèle à l’aide de la boîte de dialogue Gestionnaire de rôles dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Pour plus d’informations sur la gestion des rôles dans une base de données model déployée, consultez [des rôles de modèle tabulaire](../../analysis-services/tabular-models/tabular-model-roles-ssas-tabular.md).  
  
## <a name="tasks"></a>Tâches  
 Pour créer, modifier, copier et supprimer des rôles, utilisez la boîte de dialogue **Gestionnaire de rôles** . Pour consulter la boîte de dialogue **Gestionnaire de rôles** , dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], cliquez sur le menu **Modèle** , puis sur **Gestionnaire de rôles**.  
  
###  <a name="bkmk_new_role"></a> Pour créer un rôle  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], cliquez sur le menu **Modèle** , puis sur **Gestionnaire de rôles**.  
  
2.  Dans la boîte de dialogue **Gestionnaire de rôles** , cliquez sur **Nouveau**.  
  
     Un nouveau rôle en surbrillance est ajouté à la liste de rôles.  
  
3.  Dans la liste **Rôles** , dans le champ **Nom** , tapez un nom pour le rôle.  
  
     Par défaut, le nom du rôle par défaut est numéroté de manière incrémentielle pour chaque nouveau rôle. Il est recommandé de taper un nom qui identifie sans ambiguïté le type de membre, par exemple, Directeurs financiers ou Responsables des ressources humaines.  
  
4.  Dans le champ **Autorisations** , cliquez sur la flèche Bas, puis sélectionnez un des types d'autorisation suivants :  
  
    |Autorisation| Description|  
    |----------------|-----------------|  
    |**Aucun**|Les membres ne peuvent pas apporter de modifications au schéma de modèle et ne peuvent pas interroger les données.|  
    |**Lecture**|Les membres sont autorisés à interroger des données (selon les filtres de lignes) mais ne peuvent pas apporter de modifications au schéma de modèle.|  
    |**Lire et traiter**|Les membres sont autorisés à interroger des données (selon les filtres au niveau de la ligne) et à exécuter toutes les opérations Traiter et Traiter tout, mais ils ne peuvent pas apporter de modifications au schéma de modèle.|  
    |**Traiter**|Les membres peuvent exécuter les opérations Traiter et Traiter tout. Impossible de modifier le schéma de modèle et d'interroger les données.|  
    |**Administrateur**|Les membres peuvent apporter des modifications au schéma de modèle et peuvent interroger toutes les données.|  
  
5.  Pour entrer une description du rôle, cliquez sur le champ **Description** et tapez une description.  
  
6.  Si le rôle que vous créez dispose d'autorisations de lecture ou de lecture et traitement, vous pouvez ajouter des filtres de lignes à l'aide d'une formule DAX. Pour ajouter des filtres de lignes, cliquez sur l'onglet **Filtres de lignes** , sélectionnez une table, cliquez sur le champ **Filtre DAX** , puis tapez une formule DAX.  
  
7.  Pour ajouter des membres au rôle, cliquez sur l'onglet **Membres** , puis sur **Ajouter**.  
  
    > [!NOTE]  
    >  Les membres du rôle peuvent également être ajoutés à un modèle déployé à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [gérer les rôles à l’aide de SSMS](../../analysis-services/tabular-models/manage-roles-by-using-ssms-ssas-tabular.md).  
  
8.  Dans la boîte de dialogue **Sélectionner les utilisateurs ou les groupes** , entrez des objets utilisateur ou de groupe Windows comme membres.  
  
9. Cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md)   
 [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)   
 [Analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md)   
 [Nom d’utilisateur, fonction (DAX)](http://msdn.microsoft.com/en-us/22dddc4b-1648-4c89-8c93-f1151162b93f)   
 [Fonction CUSTOMDATA (DAX)](http://msdn.microsoft.com/en-us/58235ad8-226c-43cc-8a69-5a52ac19dd4e)  
  
  
