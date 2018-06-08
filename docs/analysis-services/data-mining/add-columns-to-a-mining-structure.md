---
title: Ajouter des colonnes à une Structure d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4c09d4b263dc4e4274888f6cbd8bf1f27103dd8b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="add-columns-to-a-mining-structure"></a>ajouter des colonnes à une structure d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Utilisez le Concepteur d'exploration de données dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour ajouter des colonnes à une structure d'exploration de données après l'avoir définie dans l'Assistant Exploration de données. Vous pouvez ajouter n'importe quelle colonne qui existe dans la vue de source de données utilisée pour définir la structure d'exploration de données.  
  
> [!NOTE]  
>  Vous pouvez ajouter plusieurs copies de colonnes à une structure d'exploration de données ; toutefois, vous devez éviter d'utiliser plusieurs instances de la colonne dans le même modèle, afin d'éviter de fausses corrélations entre la source et la colonne dérivée.  
  
### <a name="to-add-a-column-to-a-mining-structure"></a>Pour ajouter une colonne à une structure d'exploration de données  
  
1.  Sélectionnez l’onglet **Structure d’exploration de données** dans le Concepteur d’exploration de données.  
  
2.  Cliquez avec le bouton droit de la souris et sélectionnez **Ajouter une colonne**.  
  
     La boîte de dialogue **Sélectionner une colonne** s’ouvre.  
  
3.  Sous **Table source**, sélectionnez la table de la vue de source de données qui contient la colonne.  
  
4.  Sous **Colonne source**, sélectionnez la colonne à ajouter à la structure d’exploration de données.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
> [!NOTE]  
>  Si vous ajoutez une colonne existante, une copie est incluse dans la structure et « 1 » est ajouté au nom. Vous pouvez modifier le nom de la colonne copiée pour qu’elle soit plus descriptive en tapant un nouveau nom dans la propriété **Name** de la colonne de structure d’exploration de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches de la Structure d’exploration de données et procédures](../../analysis-services/data-mining/mining-structure-tasks-and-how-tos.md)  
  
  
