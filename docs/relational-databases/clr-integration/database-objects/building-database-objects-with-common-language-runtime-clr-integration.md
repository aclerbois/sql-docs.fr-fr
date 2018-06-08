---
title: Création d’objets de base de données avec l’intégration du Common Language Runtime (CLR) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- routines [CLR integration]
- database objects [CLR integration], building
- common language runtime [SQL Server], building database objects
- managed code [SQL Server], database objects
- building database objects [CLR integration]
- .NET Framework routines [SQL Server]
ms.assetid: ce34132c-bfa3-447b-9131-b6e17c672efe
caps.latest.revision: 48
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 597645c52f37034ba4f294784cbde2a7489516d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="building-database-objects-with-common-language-runtime-clr-integration"></a>Création d'objets de base de données avec intégration du Common Language Runtime (CLR)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez générer des objets de base de données à l'aide de l'intégration de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le CLR (Common Language Runtime) .NET Framework. Code managé qui s’exécute à l’intérieur de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est appelé « routine CLR ». Ces routines incluent :  
  
-   Fonctions scalaires définies par l'utilisateur (fonctions UDF scalaires)  
  
-   Fonctions table définies par l'utilisateur (TVF)  
  
-   Procédures définies par l'utilisateur (UDP)  
  
-   Déclencheurs définis par l’utilisateur  
  
 Les routines CLR ont la même structure en code managé. Elles sont mappées à des méthodes statiques publiques (partagées dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic .NET) d'une classe. Outre les routines, les types définis par l'utilisateur (UDT) et les fonctions d'agrégation définies par l'utilisateur peuvent également être définies à l'aide du .NET Framework. Les types UDT et les agrégats définis par l'utilisateur sont mappés à des classes .NET Framework entières.  
  
 Chaque type de routine .NET Framework possède une déclaration [!INCLUDE[tsql](../../../includes/tsql-md.md)] et peut être utilisé partout dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] où l'équivalent [!INCLUDE[tsql](../../../includes/tsql-md.md)] peut être utilisé. Par exemple, les fonctions UDF scalaires peuvent être utilisées dans toute expression scalaire. Une fonction TVF peut être utilisée dans toute clause FROM. Une procédure peut être appelée dans une instruction EXEC ou à partir d'une application cliente.  
  
> [!NOTE]  
>  L'exécution d'un objet CLR (fonction définie par l'utilisateur, type défini par l'utilisateur ou déclencheur) sur le Common Language Runtime peut concerner plusieurs threads (plan parallèle), si l'optimiseur de requêtes décide que cela est nécessaire. Toutefois, si une fonction définie par l'utilisateur accède aux données, l'exécution se fera sur un plan en série. En cas d'exécution sur une version serveur antérieure à [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], si une fonction définie par l'utilisateur contient des paramètres LOB ou des valeurs de retour, l'exécution doit également se faire sur un plan en série.  
  
 Le tableau ci-dessous répertorie les rubriques traitées dans cette section.  
  
 [Prise en main de l’intégration du CLR](../../../relational-databases/clr-integration/database-objects/getting-started-with-clr-integration.md)  
 Fournit une brève vue d'ensemble des bibliothèques et des espaces de noms requis pour compiler l'objet en utilisant l'intégration du CLR avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Inclut un exemple de procédure stockée CLR "Hello World".  
  
 [Bibliothèques .NET Framework prises en charge](../../../relational-databases/clr-integration/database-objects/supported-net-framework-libraries.md)  
 Fournit des informations sur les bibliothèques .NET Framework prises en charge par l'intégration du CLR.  
  
 [Restrictions du modèle de programmation de l’intégration du CLR](../../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md)  
 Fournit des informations sur les restrictions du modèle de programmation de l'intégration du CLR.  
  
 [Types de données SQL Server dans le .NET Framework](../../../relational-databases/clr-integration-database-objects-types-net-framework/sql-server-data-types-in-the-net-framework.md)  
 Vue d'ensemble de types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et de leurs équivalents .NET Framework.  
  
 [Vue d’ensemble des attributs personnalisés de l’intégration de CLR](http://msdn.microsoft.com/library/ecf5c097-0972-48e2-a9c0-b695b7dd2820)  
 Fournit des informations sur les attributs personnalisés de l'intégration du CLR.  
  
 [Fonctions CLR définies par l’utilisateur](../../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md)  
 Explique comment implémenter et utiliser les différents types de fonctions CLR : fonctions table, scalaires et d'agrégation définies par l'utilisateur.  
  
 [Types CLR définis par l’utilisateur](../../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)  
 Explique comment implémenter et utiliser les types CLR définis par l'utilisateur.  
  
 [Procédures stockées du CLR](http://msdn.microsoft.com/library/bbdd51b2-a9b4-4916-ba6f-7957ac6c3f33)  
 Explique comment implémenter et utiliser les procédures stockées CLR.  
  
 [Déclencheurs CLR](http://msdn.microsoft.com/library/302a4e4a-3172-42b6-9cc0-4a971ab49c1c)  
 Explique comment implémenter et utiliser les déclencheurs CLR.  
  
## <a name="see-also"></a>Voir aussi  
 [Common Language Runtime & #40 ; CLR & #41 ; Vue d’ensemble](../../../relational-databases/clr-integration/common-language-runtime-integration-overview.md)  
  
  
