---
title: Fonctions définies par l’utilisateur et les procédures stockées | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0a91e1e45be22ade9e7eeb7358bb83c4875f6b0b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="user-defined-functions-and-stored-procedures"></a>Fonctions définies par l'utilisateur et procédures stockées
  Avec des objets serveur ADOMD.NET, vous pouvez créer une fonction définie par l’utilisateur (UDF) ou des procédures stockées pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui interagissent avec les métadonnées et les données à partir du serveur. Ces méthodes in-process sont appelées au moyen d'instructions MDX (Multidimensional Expressions) ou DMX (Data Mining Extensions) pour fournir des fonctionnalités supplémentaires sans les temps d'attente inhérents aux communications réseau.  
  
## <a name="udf-examples"></a>Exemples de fonctions définies par l'utilisateur (UDF)  
 Une fonction définie par l'utilisateur est une méthode qui peut être appelée dans le contexte d'une instruction MDX ou DMX, qui peut prendre un nombre illimité de paramètres et qui peut retourner tout type de données.  
  
 Une fonction définie par l'utilisateur créée à l'aide de MDX est semblable à une fonction définie par l'utilisateur créée pour DMX. La principale différence réside dans le fait que certaines propriétés de l'objet <xref:Microsoft.AnalysisServices.AdomdServer.Context>, notamment <xref:Microsoft.AnalysisServices.AdomdServer.Context.CurrentCube%2A> et <xref:Microsoft.AnalysisServices.AdomdServer.Context.CurrentMiningModel%2A>, ne sont disponibles que pour l'un ou l'autre des langages de script.  
  
 Les exemples suivants indiquent comment utiliser une fonction définie par l'utilisateur pour retourner une description de nœud, filtrer des tuples et appliquer un filtre à un tuple.  
  
### <a name="returning-a-node-description"></a>Retour d'une description de nœud  
 L'exemple suivant crée une fonction définie par l'utilisateur chargée de retourner la description d'un nœud spécifié. La fonction définie par l'utilisateur utilise le contexte dans lequel elle est exécutée et emploie une clause DMX FROM pour récupérer le nœud à partir du modèle d'exploration de données actif.  
  
```  
public string GetNodeDescription(string nodeUniqueName)  
{  
   return Context.CurrentMiningModel.GetNodeFromUniqueName(nodeUniqueName).Description;  
}  
```  
  
 Une fois déployée, la fonction définie par l'utilisateur de l'exemple précédent peut être appelée à l'aide de l'expression DMX ci-dessous, qui récupère le nœud de prédiction le plus probable. La description contient des informations qui décrivent les conditions rattachées au nœud de prédiction.  
  
```  
select Cluster(), SampleAssembly.GetNodeDescription( PredictNodeId(Cluster()) ) FROM [Customer Clusters]  
```  
  
### <a name="returning-tuples"></a>Retour de tuples  
 En s'appuyant sur un ensemble et un nombre retourné, l'exemple suivant récupère aléatoirement des tuples de l'ensemble, retournant ainsi un sous-ensemble final :  
  
```  
public Set RandomSample(Set set, int returnCount)  
{  
   //Return the original set if there are fewer tuples  
   //in the set than the number requested.  
   if (set.Tuples.Count <= returnCount)  
      return set;  
  
   System.Random r = new System.Random();  
   SetBuilder returnSet = new SetBuilder();  
  
   //Retrieve random tuples until the return set is filled.  
   int i = set.Tuples.Count;  
   foreach (Tuple t in set.Tuples)  
   {  
      if (r.Next(i) < returnCount)  
      {  
         returnCount--;  
         returnSet.Add(t);  
      }  
      i--;  
      //Stop the loop if we have enough tuples.  
      if (returnCount == 0)  
         break;  
   }  
   return returnSet.ToSet();  
}  
```  
  
 L'exemple précédent est appelé dans l'exemple MDX suivant. Dans cet exemple MDX, cinq États ou provinces aléatoires sont récupérées à partir de la **Adventure Works** base de données.  
  
```  
SELECT SampleAssembly.RandomSample([Geography].[State-Province].Members, 5) on ROWS,   
[Date].[Calendar].[Calendar Year] on COLUMNS  
FROM [Adventure Works]  
WHERE [Measures].[Reseller Freight Cost]  
```  
  
### <a name="applying-a-filter-to-a-tuple"></a>Application d'un filtre à un tuple  
 Dans l'exemple suivant, la fonction définie par l'utilisateur prend un ensemble et applique un filtre à chaque tuple de l'ensemble en utilisant l'objet Expression. Les tuples qui sont conformes au filtre sont ajoutés à l'ensemble retourné.  
  
 [!code-cs[Adomd.NetServer#FilterSet](../../analysis-services/multidimensional-models-adomd-net-server/codesnippet/csharp/user-defined-functions-a_1.cs)]  
  
 L'exemple précédent est appelé dans l'exemple MDX ci-dessous, dont le filtre porte sur les noms de ville commençant par la lettre A.  
  
```  
Select Measures.Members on Rows,  
SampleAssembly.FilterSet([Customer].[Customer Geography].[City], "[Customer].[Customer Geography].[City].CurrentMember.Name < 'B'") on Columns  
From [Adventure Works]  
```  
  
## <a name="stored-procedure-example"></a>Exemple de procédure stockée  
 Dans l'exemple suivant, une procédure stockée MDX utilise AMO pour créer, si nécessaire, des partitions pour Internet Sales.  
  
 [!code-cs[Adomd.NetServer#CreateInternetSalesMeasureGroupPartitions](../../analysis-services/multidimensional-models-adomd-net-server/codesnippet/csharp/user-defined-functions-a_2.cs)]  
  
  
