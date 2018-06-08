---
title: Fonctionnalités serveur ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 003e50efa8ed21720410830a3d02134df15c3c97
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="adomdnet-server-functionality"></a>Fonctionnalités serveur ADOMD.NET
  Tous les objets serveur ADOMD.NET fournissent un accès en lecture seule aux données et aux métadonnées présentes sur le serveur. Pour récupérer les données et les métadonnées, vous devez utiliser le modèle d'objet serveur ADOMD.NET, car le modèle d'objet serveur ne prend pas en charge les ensembles de lignes de schéma.  
  
 Avec les objets serveur ADOMD.NET, vous pouvez créer une fonction définie par l’utilisateur (UDF) ou une procédure stockée pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Ces méthodes in-process sont appelées par l'intermédiaire d'instructions de requête créées dans des langages tels que MDX (Multidimensional Expressions), DMX (Data Mining Extensions) ou SQL. Ces méthodes in-process fournissent également des fonctionnalités supplémentaires sans les temps d'attente inhérents aux communications réseau.  
  
> [!NOTE]  
>  L'objet <xref:Microsoft.AnalysisServices.AdomdServer.AdomdCommand> ne prend en charge que le langage DMX.  
  
## <a name="what-is-a-udf"></a>Qu'est qu'une fonction définie par l'utilisateur ?  
 A *UDF* est une méthode qui présente les caractéristiques suivantes :  
  
-   Vous pouvez appeler une fonction définie par l'utilisteur dans le contexte d'une requête.  
  
-   Une fonction définie par l'utilisateur peut prendre un nombre illimité de paramètres.  
  
-   Une fonction définie par l'utilisateur peut retourner divers types de données.  
  
 L'exemple suivant utilise la fonction définie par l'utilisateur fictive `FinalSalesNumber` :  
  
```  
SELECT SalesPerson.Name ON ROWS,  
       FinalSalesNumber() ON COLUMNS  
FROM SalesModel  
```  
  
## <a name="what-is-a-stored-procedure"></a>Qu'est-ce qu'une procédure stockée ?  
 A *procédure stockée* est une méthode qui présente les caractéristiques suivantes :  
  
-   Vous appelez une procédure stockée sur son propre avec l’expression MDX [appeler](../../mdx/mdx-data-manipulation-call.md) instruction.  
  
-   Une procédure stockée peut prendre un nombre illimité de paramètres.  
  
-   Une procédure stockée peut retourner un jeu de données, un **IDataReader**, ou un résultat vide.  
  
 L'exemple suivant utilise la procédure stockée fictive `FinalSalesNumbers` :  
  
```  
CALL FinalSalesNumbers()  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation du serveur ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-server/adomd-net-server-programming.md)  
  
  
