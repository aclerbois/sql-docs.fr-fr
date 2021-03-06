---
title: "Analyser un modèle tabulaire dans Excel | Documents Microsoft"
ms.custom: 
ms.date: 02/21/2018
ms.prod: analysis-services
ms.prod_service: analysis-services, azure-analysis-services
ms.service: 
ms.component: data-mining
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords:
- sql13.asvs.bidtoolset.chooseperspect.f1
ms.assetid: 47fa45fc-60ab-41a1-bde3-5781c8462889
caps.latest.revision: 
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: Inactive
ms.openlocfilehash: 6b9248c15ba18811781fe24ae3f432e61b7df540
ms.sourcegitcommit: d8ab09ad99e9ec30875076acee2ed303d61049b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="analyze-a-tabular-model-in-excel"></a>Analyser un modèle tabulaire dans Excel  
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
La fonctionnalité Analyser dans Excel de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ouvre l’application Microsoft Excel, crée une connexion de source de données à la base de données model de l’espace de travail, puis ajoute un tableau croisé dynamique à la feuille de calcul. Les objets de modèle (tables, colonnes, mesures, hiérarchies et KPI) sont inclus en tant que champs dans la liste de champs du tableau croisé dynamique.  
  
> [!NOTE]  
>  Pour utiliser la fonctionnalité Analyser dans Excel, vous devez disposer de Microsoft Office 2003 ou d’une version ultérieure installée sur le même ordinateur que [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Si Office n'est pas installé sur le même ordinateur, vous pouvez utiliser Excel sur un autre ordinateur et vous connecter à la base de données model de l'espace de travail comme source de données. Vous pouvez ensuite ajouter manuellement un tableau croisé dynamique à la feuille de calcul. Les objets de modèle (tables, colonnes, mesures et KPI) sont inclus en tant que champs dans la liste de champs du tableau croisé dynamique.  
  
## <a name="tasks"></a>Tâches  
  
#### <a name="to-analyze-a-tabular-model-project-by-using-the-analyze-in-excel-feature"></a>Pour analyser un projet de modèle tabulaire à l'aide de la fonctionnalité Analyser dans Excel  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], cliquez sur le menu **Modèle** , puis sur **Analyser dans Excel**.  
  
2.  Dans la boîte de dialogue **Choisir les informations d’identification et la perspective** , sélectionnez l’une des options relatives aux informations d’identification suivantes pour vous connecter à la source de données de l’espace de travail du modèle :  
  
    -   Pour utiliser le compte d’utilisateur actuel, sélectionnez **Utilisateur Windows actuel**.  
  
    -   Pour utiliser un compte d’utilisateur différent, sélectionnez **Autre utilisateur Windows**.  
  
         En général, ce compte d'utilisateur est membre d'un rôle. Aucun mot de passe n'est requis. Le compte ne peut être utilisé que dans le contexte d'une connexion Excel à la base de données d'espace de travail.  
  
    -   Pour utiliser un rôle de sécurité, sélectionnez **Rôle**puis, dans la zone de liste, sélectionnez un ou plusieurs rôles.  
  
         Les rôles de sécurité doivent être définis à l'aide du gestionnaire de rôles. Pour plus d’informations, consultez [créer et gérer les rôles](../../analysis-services/tabular-models/create-and-manage-roles-ssas-tabular.md).  
  
3.  Pour utiliser une perspective, dans la zone de liste **Perspective**, sélectionnez une perspective.  
  
     Les perspectives (autres que celles par défaut) doivent être définies à l'aide de la boîte de dialogue Perspectives. Pour plus d’informations, consultez [créer et gérer des Perspectives](../../analysis-services/tabular-models/create-and-manage-perspectives-ssas-tabular.md).  
  
> [!NOTE]  
>  La liste de champs du tableau croisé dynamique dans Excel n'est pas actualisée automatiquement lorsque vous apportez des modifications au projet de modèle dans le générateur de modèles. Pour actualiser la liste de champs du tableau croisé dynamique, dans Excel, dans le ruban **Options** , cliquez sur **Actualiser**.  
  
## <a name="see-also"></a>Voir aussi  
 [Analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md)  
  
  
