---
title: Propriétés de Structure d’exploration de données et les colonnes de Structure | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 72f89879ac7b50f35af283e13b25fb8c8b439b2d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="properties-for-mining-structure-and-structure-columns"></a>Propriétés des colonnes de structure et des structure d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez définir ou modifier les propriétés pour une structure d’exploration de données ainsi que pour ses colonnes et colonnes imbriquées associées en utilisant l’onglet **Structure d’exploration de données** du Concepteur d’exploration de données. Les propriétés que vous définissez sous cet onglet sont propagées à chaque modèle d'exploration de données associé à la structure.  
  
> [!NOTE]  
>  Si vous modifiez la valeur d'une propriété dans la structure d'exploration de données, y compris des métadonnées telles qu'un nom ou une description, la structure d'exploration de données et ses modèles doivent être retraités pour que vous puissiez afficher ou interroger le modèle.  
  
## <a name="properties-of-mining-structures-and-mining-structure-columns"></a>Propriétés des structures d'exploration de données et des colonnes de structure d'exploration de données  
 Le tableau suivant décrit les propriétés de la structure d’exploration de données et des colonnes de structure d’exploration de données que vous pouvez afficher ou configurer sous l’onglet **Structure d’exploration de données** . Pour afficher ou configurer ces propriétés, cliquez avec le bouton droit sur un élément dans l’arborescence, puis cliquez sur **Propriétés**.  
  
-   Cliquez sur l'en-tête de la structure d'exploration de données pour afficher ses propriétés.  
  
-   Pour afficher les propriétés d'une colonne ou d'une table imbriquée, cliquez sur le nom de la colonne.  
  
### <a name="properties-of-the-mining-structure"></a>Propriétés de la structure d'exploration de données  
  
|Propriété| Description|  
|--------------|-----------------|  
|**CacheMode**|Spécifie si les cas utilisés dans le cadre de l'apprentissage doivent être mis en cache ou supprimés à la fin de la formation. **Note :**  Cette propriété doit avoir la valeur **KeepTrainingCases** pour activer l’extraction et les données d’exclusion.|  
|**Classement**|Spécifie le classement par défaut de la colonne. Si aucun n'est spécifié, le classement du serveur est utilisé.|  
|**Description**|Décrit la structure d'exploration de données. Il est recommandé que la description indique la fonction et la composition des données dans la structure.|  
|**ErrorConfiguration (par défaut)**|Spécifie les options relatives à la gestion spéciale des erreurs, le cas échéant.|  
|**HoldoutMaxCases**|Spécifie le nombre maximal de cas de structure qui peuvent être réservés en tant que jeu de données de test.  Si des valeurs sont spécifiées pour **HoldoutMaxCases** et **HoldoutPercent**, les conditions sont combinées. **Note :**  Pour définir cette propriété, <xref:Microsoft.AnalysisServices.MiningStructure.CacheMode%2A> doit être défini sur **KeepTrainingCases**.|  
|**HoldoutPercent**|Spécifie le pourcentage de cas de structure à réserver comme jeu de données de test. Si des valeurs sont spécifiées pour **HoldoutMaxCases** et **HoldoutPercent**, les conditions sont combinées. **Note :**  Pour définir cette propriété, <xref:Microsoft.AnalysisServices.MiningStructure.CacheMode%2A> doit être défini sur **KeepTrainingCases**.|  
|**HoldoutSeed**|Spécifie une valeur de départ pour initialiser le partitionnement du jeu de test d'exclusion, afin de garantir que le jeu de données de test pourra être recréé. **Note :**  Pour définir cette propriété, <xref:Microsoft.AnalysisServices.MiningStructure.CacheMode%2A> doit être défini sur **KeepTrainingCases**.|  
|**ID**|Affiche l'identificateur unique de la structure d'exploration de données.<br /><br /> Le nom que vous avez attribué à la structure d'exploration de données lorsque vous l'avez créée est utilisé comme ID. Si vous changez ultérieurement le nom en tapant une nouvelle valeur pour la propriété **Nom** , le nouveau nom est utilisé seulement comme alias ; l’ID ne change pas.|  
|**Langage**|Spécifie la langue des légendes dans la structure d'exploration de données.|  
|**Nom**|Spécifie le nom ou l'alias de la structure d'exploration de données.<br /><br /> Si vous modifiez la valeur de la propriété Name, le nouveau nom est utilisé comme légende ou alias uniquement ; l'identificateur de la structure d'exploration de données ne change pas.|  
|**Source**|Affiche le nom et le type de la source de données.|  
  
### <a name="properties-of-the-mining-structure-columns"></a>Oropriétés des colonnes de la structure d'exploration de données  
  
|Propriété| Description|  
|--------------|-----------------|  
|**ClassifiedColumns**|Identifie la colonne qu'une colonne classifiée décrit.|  
|**Contenu**|Type de contenu de la colonne.|  
|**Description**|Décrit la colonne. Il est recommandé que la description de la colonne fournisse des informations sur la manière dont les données dans la colonne ont été dérivées ou modifiées pour l'exploration de données.|  
|**DiscretizationBucketCount**|Affiche le nombre de compartiments dans la colonne discrétisée.<br /><br /> Activé uniquement si le type de contenu a la valeur **Discrétisé**.<br /><br /> Cette propriété est en lecture seule.|  
|**DiscretizationMethod**|Affiche la méthode utilisée pour discrétiser la colonne.<br /><br /> Activé uniquement si le type de contenu a la valeur **Discrétisé**.<br /><br /> Cette propriété est en lecture seule.|  
|**Distribution**|Spécifie la distribution de contenu dans la colonne.|  
|**ID**|Affiche l'identificateur de la colonne.<br /><br /> Modifier la valeur de la propriété Name de la colonne n'affecte pas la valeur de la propriété ID.|  
|**IsKey**|Indique si la colonne est une colonne clé.|  
|**KeyColumns**|Contient la définition d'une colonne qui représente la clé ou une partie de la clé d'un attribut.|  
|**ModelingFlags**|Spécifie des paramètres supplémentaires rendus disponibles par l'algorithme.|  
|**Nom**|Nom de la colonne.|  
|**NameColumn**|Identifie la colonne qui fournit le nom de l'élément parent.|  
|**Source**|Affiche la source de la colonne.<br /><br /> Pour les sources de données relationnelles, la valeur est toujours **(aucun)**.<br /><br /> Pour les structures basées sur un cube OLAP, la valeur est l'instruction MDX qui définit la coupe utilisée comme source pour la table imbriquée.|  
|**SourceMeasureGroup**|Affiche la source du groupe de mesures.<br /><br /> Pour les sources de données relationnelles, la valeur est toujours **(aucun)**.<br /><br /> Pour les structures basées sur un cube OLAP, la valeur est l'instruction MDX qui définit la coupe utilisée comme source pour la table imbriquée.|  
|**Type**|Type de données pour le contenu de la colonne.|  
  
 Pour plus d’informations sur la définition ou la modification des propriétés, consultez [Tâches de la structure d’exploration de données et procédures](../../analysis-services/data-mining/mining-structure-tasks-and-how-tos.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une structure d'exploration de données relationnelle](../../analysis-services/data-mining/create-a-relational-mining-structure.md)   
 [Colonnes de structure d'exploration de données](../../analysis-services/data-mining/mining-structure-columns.md)  
  
  
