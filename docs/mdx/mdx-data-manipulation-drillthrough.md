---
title: L’instruction DRILLTHROUGH (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4f5b56c03ec6e575b647ed7eecaf26d35bfae047
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580061"
---
# <a name="mdx-data-manipulation---drillthrough"></a>Manipulation de données MDX - d’extraction
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Extrait les lignes de la table sous-jacente qui ont été utilisées pour créer une cellule spécifiée dans un cube.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DRILLTHROUGH[MAXROWSUnsigned_Integer]   
      <MDX SELECT statement>   
      [RETURNSet_of_Attributes_and_Measures   
            [,Set_of_Attributes_and_Measures ...]  
      ]  
```  
  
## <a name="arguments"></a>Arguments  
 *Unsigned_Integer*  
 Valeur d'entier positif.  
  
 *Instruction MDX SELECT*  
 N'importe quelle instruction SELECT MDX (Multidimensional Expressions) valide.  
  
 *Set_of_Attributes_and_Measures*  
 Liste séparée par des virgules d'attributs de dimension et de mesures.  
  
## <a name="remarks"></a>Notes  
 L'extraction est une opération au cours de laquelle un utilisateur final sélectionne une cellule unique dans un cube et extrait un ensemble de résultats des données source de cette cellule pour se procurer des informations plus détaillées. Par défaut, un ensemble de résultats obtenus par extraction provient des lignes de la table qui ont été évaluées afin de calculer la valeur de la cellule sélectionnée dans le cube. Pour être en mesure de procéder à une extraction, les utilisateurs finaux doivent disposer de cette fonctionnalité sur leurs applications clientes. Dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], les résultats sont récupérés directement à partir de stockage MOLAP, sauf si les partitions ROLAP ou dimensions sont interrogées.  
  
> [!IMPORTANT]  
>  La sécurité de l'extraction est basée sur les options de sécurité générales définies dans le cube. Si un utilisateur ne parvient pas à se procurer des données via MDX, l'extraction limite aussi l'utilisateur exactement de la même manière.  
  
 Une instruction MDX spécifie la cellule concernée. La valeur spécifiée par la **MAXROWS** argument indique le nombre maximal de lignes qui doivent être retournées par l’ensemble de lignes.  
  
 Par défaut, le nombre maximal de lignes qui sont retournées est de 10 000 lignes. Cela signifie que si vous laissez **MAXROWS** n’est pas spécifié, vous obtiendrez 10 000 lignes au maximum. Si cette valeur est trop faible pour votre scénario, vous pouvez définir **MAXROWS** sur un nombre plus élevé, tel que `MAXROWS 20000`. Si elle est trop faible globalement, vous pouvez augmenter la valeur par défaut en modifiant le **OLAP\Query\DefaultDrillthroughMaxRows** propriété de serveur. Pour plus d’informations sur la modification de cette propriété, consultez [propriétés du serveur dans Analysis Services](../analysis-services/server-properties/server-properties-in-analysis-services.md).  
  
 Sauf indication contraire, les colonnes retournées incluent tous les attributs de granularité de toutes les dimensions (autres que les dimensions de type plusieurs à plusieurs) associées au groupe de mesures. Les dimensions du cube sont précédées du signe $ afin de distinguer les dimensions et les groupes de mesures. Le **retourner** clause est utilisée pour spécifier les colonnes retournées par la requête d’extraction. Les fonctions suivantes peuvent être appliquées à un seul attribut ou mesure par le **retourner** clause.  
  
 Name(attribute_name)  
 Retourne le nom du membre d'attribut spécifié.  
  
 UniqueName(attribute_name)  
 Retourne le nom unique du membre d'attribut spécifié.  
  
 Key(attribute_name[, N])  
 Retourne la clé du membre d'attribut spécifié, où N spécifie la colonne dans la clé composite (le cas échéant). La valeur par défaut pour N est 1.  
  
 Caption(attribute_name)  
 Retourne la légende du membre d'attribut spécifié.  
  
 MemberValue(attribute_name)  
 Retourne la valeur de membre du membre d'attribut spécifié.  
  
 CustomRollup(attribute_name)  
 Retourne l'expression de cumul personnalisé du membre d'attribut spécifié.  
  
 CustomRollupProperties(attribute_name)  
 Retourne les propriétés de cumul personnalisé du membre d'attribut spécifié.  
  
 UnaryOperator(attribute_name)  
 Retourne l'opérateur unaire du membre d'attribut spécifié.  
  
## <a name="example"></a>Exemple  
 L'exemple suivant spécifie la cellule du mois de juillet 2007 pour la mesure (mesure par défaut) Reseller Sales Amount (montant des ventes du revendeur) pour l'Australie. La clause RETURN précise que les valeurs sous-jacentes de cette cellule (valeurs concernant la date de chaque vente, le nom du modèle de produit, le nom de l'employé, le montant des ventes, le montant des taxes et le coût du produit) sont à retourner.  
  
```  
DRILLTHROUGH  
SELECT  
   ([Date].[Calendar].[Month].[July 2007])  
ON 0   
FROM [Adventure Works]  
WHERE [Geography].[Country].[Australia]  
RETURN   
  [$Date].[Date]  
  ,KEY([$Product].[Model Name])  
  ,NAME([$Employee].[Employee])  
  ,[Reseller Sales].[Reseller Sales Amount]  
  ,[Reseller Sales].[Reseller Tax Amount]  
  ,[Reseller Sales].[Reseller Standard Product Cost]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Les instructions de Manipulation de données MDX &#40;MDX&#41;](../mdx/mdx-data-manipulation-statements-mdx.md)  
  
  
