---
title: Définition de procédures stockées | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 54b14cd5ee54edab4508c06c55e5815bfeab934f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="defining-stored-procedures"></a>Définition de procédures stockées
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez utiliser des procédures stockées pour appeler des routines externes à partir de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Vous pouvez écrire les routines externes appelées par une procédure stockée dans n'importe quel langage CLR (Common Language Runtime), comme C, C++, C#, Visual Basic ou Visual Basic .NET. Une procédure stockée peut être créée une fois et être ensuite appelée à partir de nombreux contextes, par exemple par d'autres procédures stockées, par des mesures calculées ou par des applications clientes. Les procédures stockées simplifient le développement et l'implémentation des bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en permettant de développer une fois du code commun et de le stoker dans un emplacement unique. Les procédures stockées peuvent être utilisées pour ajouter à vos applications des fonctionnalités métier qui ne sont pas fournies par les fonctionnalités natives de MDX.  
  
 Cette section contient les informations nécessaires pour comprendre, concevoir et mettre en œuvre des procédures stockées.  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Conception de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/designing-stored-procedures.md)|Explique comment concevoir des assemblys pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[Création de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/creating-stored-procedures.md)|Explique comment créer des assemblys pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[Appel de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/calling-stored-procedures.md)|Fournit des informations sur l'utilisation des assemblys dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[Accès au contexte de requête dans les procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/accessing-query-context-in-stored-procedures.md)|Explique comment accéder aux informations d'étendue et de contexte avec des assemblys.|  
|[Définition de la sécurité des procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/setting-security-for-stored-procedures.md)|Explique comment configurer la sécurité des assemblys dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[Débogage de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/debugging-stored-procedures.md)|Explique comment déboguer les assemblys dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des assemblys de modèle multidimensionnel](../../analysis-services/multidimensional-models/multidimensional-model-assemblies-management.md)  
  
  
