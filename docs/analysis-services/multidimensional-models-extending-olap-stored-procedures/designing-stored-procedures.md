---
title: Création des procédures stockées | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a617df9d88bde17c08fb4d2235ad751a5609e2d2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="designing-stored-procedures"></a>Conception des procédures stockées
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le modèle objet administratif AMO (Analysis Management Objects) et le modèle objet orienté client ([!INCLUDE[msCoName](../../includes/msconame-md.md)] ADO (ActiveX® Data Objects) MD (Multidimensional)) sont disponibles dans les procédures stockées.  
  
 Les procédures stockées doivent être dans la portée (le serveur ou la base de données) afin d'être visibles au niveau MDX (Multidimensional Expressions) pour être appelées. Toutefois, lorsqu'une procédure stockée est appelée, sa portée ne se limite pas aux actions effectuées sous son parent. Une procédure stockée peut effectuer des modifications n'importe où sur le serveur ; elle est juste astreinte à respecter les limites de sécurité du processus utilisateur qui l'appelle ou les limites de la transaction dans laquelle elle fonctionne.  
  
 Les procédures de portée du serveur sont disponibles dans tous les contextes du serveur. Les procédures stockées de portée de base de données sont uniquement visibles dans le contexte de la base de données dans laquelle elles sont définies.  
  
 Comme pour toute fonction MDX, une procédure stockée doit être résolue pour qu'une session MDX puisse continuer ; les procédures stockées bloquent les sessions MDX pendant leur exécution. Sauf si une raison spécifique justifie l'arrêt d'une session MDX dans l'attente d'une intervention de l'utilisateur, il est fortement recommandé d'éviter toute interaction de ce type (telle qu'une boîte de dialogue par exemple).  
  
## <a name="dependent-assemblies"></a>Assemblys dépendants  
 Tous les assemblys dépendants doivent être chargés dans une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que le Common Language Runtime (CLR) doit trouver. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] stocke les assemblys dépendants dans le même dossier que l'assembly principal, afin que le CLR résolve automatiquement toutes les références de fonction à des fonctions de ces assemblys.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des assemblys de modèle multidimensionnel](../../analysis-services/multidimensional-models/multidimensional-model-assemblies-management.md)   
 [Définition de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/defining-stored-procedures.md)  
  
  
