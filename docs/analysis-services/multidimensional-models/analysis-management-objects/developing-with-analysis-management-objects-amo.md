---
title: Développement avec Analysis Management Objects (AMO) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6f47c7fe9abc3b7a23c0eea2dd78a536fb9e0ad2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="developing-with-analysis-management-objects-amo"></a>Développement avec AMO (Analysis Management Objects)
Objets AMO (Analysis Management) est la bibliothèque d’objets accessibles par programme complète qui permet à une application gérer une instance en cours d’exécution de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].

Cette section explique les concepts AMO en mettant l'accent sur les objets principaux. Elle explique comment et quand les utiliser et la façon dont ils sont liés les uns aux autres. Pour plus d’informations sur les classes ou des objets spécifiques, consultez :

- [Microsoft.AnalysisServices Namespace](http://msdn.microsoft.com/library/microsoft.analysisservices.aspx), pour la documentation de référence
- [Analysis Services Management Objects (AMO)](http://www.bing.com/search?q=Analysis+Services+Management+Objects+%28AMO%29), comme une recherche générale Bing.com.


 **Nouveautés de SQL Server 2016**

Dans SQL Server 2016, AMO est refactorisé en plusieurs assemblys. Classes génériques, telles que Server, base de données et des rôles sont dans le **Microsoft.AnalysisServices.Core** Namespace. API spécifiques multidimensionnel restent dans [Microsoft.AnalysisServices Namespace](https://msdn.microsoft.com/library/ms146720.aspx).

Des scripts personnalisés et les applications écrites sur des versions antérieures de AMO continue de fonctionner sans modification. Toutefois, si vous avez script ou des applications qui ciblent SQL Server 2016 en particulier, ou si vous avez besoin de reconstruire une solution personnalisée, veillez à ajouter le nouvel assembly et l’espace de noms à votre projet.

## <a name="see-also"></a>Voir aussi
[Développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md)
[développement avec XMLA dans Analysis Services](../../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)
