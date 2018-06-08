---
title: Traiter un modèle d’exploration de données | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2bfc8d22ff87f467fa89d178d46b422918aa4dd6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="process-a-mining-model"></a>Traiter un modèle d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans l'onglet Modèles d'exploration de données du Concepteur d'exploration de données de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous pouvez traiter un modèle d'exploration de données associé à une structure d'exploration de données, ou traiter tous les modèles associés à la structure.  
  
 Vous pouvez traiter un modèle d'exploration de données à l'aide des outils suivants :  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
 Vous pouvez également utiliser une commande de traitement XMLA. Pour plus d’informations, consultez [Outils et approches de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/tools-and-approaches-for-processing-analysis-services.md).  
  
### <a name="process-a-single-mining-model-using-sql-server-data-tools"></a>Traiter un seul modèle d'exploration de données à l'aide des outils de données SQL Server  
  
1.  Sous l'onglet **Modèles d'exploration de données** du Concepteur d'exploration de données, sélectionnez un modèle d'exploration de données dans une ou plusieurs colonnes des modèles de la grille.  
  
2.  Dans le menu **Modèle d'exploration de données** , sélectionnez **Traiter le modèle**.  
  
     Si vous avez modifié la structure d'exploration de données, un message vous demande de la redéployer avant de traiter le modèle. Cliquez sur **Oui**.  
  
3.  Dans le **traiter le modèle d’exploration de données - \<modèle >** boîte de dialogue, cliquez sur **exécuter**.  
  
     La boîte de dialogue **État d'avancement du traitement** s'ouvre avec les informations de traitement du modèle.  
  
4.  Une fois le modèle traité, cliquez sur **Fermer** dans la boîte de dialogue **État d'avancement du traitement** .  
  
5.  Cliquez sur **fermer** dans les **traiter le modèle d’exploration de données - \<modèle >** boîte de dialogue.  
  
 Seuls la structure d'exploration de données et le modèle d'exploration de données sélectionné ont été traités.  
  
### <a name="process-all-mining-models-that-are-associated-with-a-mining-structure"></a>Traiter tous les modèles d'exploration de données associés à une structure d'exploration de données  
  
1.  Dans l'onglet **Modèles d'exploration de données** du Concepteur d'exploration de données, sélectionnez **Traiter l'exploration de données et tous les modèles** dans le menu **Modèle d'exploration de données** .  
  
2.  Si vous avez modifié la structure d'exploration de données, un message vous demande de la redéployer avant de traiter les modèles. Cliquez sur **Oui**.  
  
3.  Dans le **traiter la Structure d’exploration de données - \<structure >** boîte de dialogue, cliquez sur **exécuter**.  
  
4.  La boîte de dialogue **État d'avancement du traitement** s'ouvre avec les informations de traitement du modèle.  
  
5.  Une fois les modèles traités, cliquez sur **Fermer** dans la boîte de dialogue **État d'avancement du traitement** .  
  
6.  Cliquez sur **fermer** dans les **traitement \<modèle >** boîte de dialogue.  
  
 La structure d'exploration de données et tous les modèles d'exploration de données associés ont été traités.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches liées aux modèles d’exploration de données et procédures](../../analysis-services/data-mining/mining-model-tasks-and-how-tos.md)  
  
  
