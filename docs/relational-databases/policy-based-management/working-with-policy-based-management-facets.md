---
title: Utiliser les facettes de la gestion basée sur des stratégies | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing Policy-Based Management facets
- facets [Policy-Based Management], copying
- facets [Policy-Based Management], viewing
- copying Policy-Based Management facets
ms.assetid: 88d025c4-07c2-4e4d-8634-204249a8c82c
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 8add0751a1c299ff927f11e836a4865e57eb86dd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-policy-based-management-facets"></a>Utiliser les facettes de la gestion basée sur des stratégies
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une facette de la gestion basée sur des stratégies est un ensemble de propriétés logiques liées à une zone d'intérêt de gestion. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut plusieurs facettes prédéfinies. Par exemple, la facette Configuration de la surface d'exposition définit en tant que propriétés les fonctionnalités qui sont désactivées par défaut.  
  
 Lorsque vous gérez de nombreux environnements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] similaires, vous pouvez configurer une facette dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], copier l’état de cette facette dans un fichier, puis importer ce fichier dans une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en tant que stratégie. Une fois l'état converti en stratégie, cette stratégie peut être appliquée à d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à des objets d'instance, à des bases de données ou à des objets de base de données.  
  
 Cette rubrique décrit comment copier l'état d'une facette dans un fichier XML.  
  
##  <a name="BeforeYouBegin"></a> Autorisations  
 Les procédures de cette rubrique nécessite l’appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
## <a name="viewing-and-copying-facet-states"></a>Affichage et copie d'états de facette  
 [Afficher les facettes de gestion basée sur des stratégies sur un objet SQL Server](../../relational-databases/policy-based-management/view-the-policy-based-management-facets-on-a-sql-server-object.md)  
  
 [Copier un état de facette de gestion basée sur des stratégies dans un fichier XML](../../relational-databases/policy-based-management/copy-a-policy-based-management-facet-state-to-an-xml-file.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
  
  
