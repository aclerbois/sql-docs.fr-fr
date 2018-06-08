---
title: À l’aide de procédures stockées (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ed9232137bcfed47852086ab1b6b76bc2bc2cb69
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34582241"
---
# <a name="using-stored-procedures-mdx"></a>Utilisation de procédures stockées (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Vous pouvez étendre les fonctionnalités de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] et MDX (Multidimensional Expressions) en écrivant des fonctions définies par l'utilisateur ou des procédures stockées .NET. Pour plus d’informations, consultez [de programmation serveur ADOMD.NET](../analysis-services/multidimensional-models-adomd-net-server/adomd-net-server-programming.md)  
  
 Lorsque vous référencez ou appelez une procédure stockée, vous spécifiez le nom de la fonction, suivi de parenthèses. Dans les parenthèses, vous pouvez spécifier des expressions appelées arguments qui fournissent les données à transmettre aux paramètres. Lorsque vous appelez une fonction, vous devez fournir des valeurs d'arguments pour tous les paramètres, en respectant l'ordre dans lequel les paramètres sont définis dans la fonction définie par l'utilisateur.  
  
 L'exemple de requête suivant suppose que vous avez un assembly nommé SampleAssembly inscrit sur votre serveur [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] :  
  
```  
SELECT SampleAssembly.RandomSample([Geography].[State-Province].Members, 5) on ROWS,   
[Date].[Calendar].[Calendar Year] on COLUMNS  
FROM [Adventure Works]  
WHERE [Measures].[Reseller Freight Cost]  
```  
  
> [!NOTE]  
>  *Procédure stockée* est la terminologie employée dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour ces types de fonctions. Les versions antérieures de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] appelée ces types de fonctions en tant que *les fonctions définies par l’utilisateur*.  
  
## <a name="types-of-stored-procedures"></a>Types de procédures stockées  
 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] prend en charge à la fois les assemblys COM et CLR. Les assemblys CLR sont recommandés en raison de la sécurité renforcée dont ils disposent. Si Microsoft Office Excel est installé sur le serveur, les fonctions Excel sont également disponibles.  
  
> [!NOTE]  
>  Les assemblys COM de Microsoft Visual Basic pour Applications (VBA) sont enregistrés automatiquement.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;syntaxe MDX&#41;](../mdx/functions-mdx-syntax.md)  
  
  
