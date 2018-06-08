---
title: Programmation des procédures stockées étendues | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: extended-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- gateway applications [SQL Server]
- extended stored procedures [SQL Server], about extended stored procedures
- Open Data Services [SQL Server]
- ODS [SQL Server]
ms.assetid: 561305cd-c803-48af-9eec-2c19f4d311ce
caps.latest.revision: 42
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: c80abd97dc5a371f6df37c3df1a474998b080493
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-engine-extended-stored-procedures---programming"></a>Moteur de base de données des procédures stockées étendues - programmation
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Par le passé, les services ODS (Open Data Services) permettaient d'écrire des applications serveur, telles que des passerelles à des environnements de base de données non-SQL Server. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les éléments obsolètes de l’API Open Data Services. Les seuls éléments de l'API ODS d'origine encore pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont les fonctions de procédure stockée étendue ; c'est pourquoi l'API a été renommée « API de procédure stockée étendue ».  
  
 Avec l'apparition de nouvelles technologies plus puissantes telles que les requêtes distribuées et l'intégration du CLR, les besoins en termes d'applications de l'API de procédure stockée étendue ont été en grande partie éliminés.  
  
> [!NOTE]  
>  Si vous possédez des applications de passerelle existantes, vous ne pouvez pas utiliser le fichier opends60.dll fourni avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter les applications. Les applications de passerelle ne sont plus prises en charge.  
  
## <a name="extended-stored-procedures-vs-clr-integration"></a>Procédures stockées étendues et Intégration du CLR  
 Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les procédures stockées étendues constituaient pour les développeurs d'applications de base de données le seul mécanisme leur permettant d'écrire une logique côté serveur difficile à exprimer voire impossible à écrire en [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'intégration du CLR offre une alternative plus robuste à l'écriture de telles procédures stockées. Qui plus est, la logique, auparavant écrite sous forme de procédures stockées, est souvent mieux exprimée sous forme de fonctions table avec l'intégration du CLR. Ces dernières vous permettent d'interroger les résultats construits par la fonction dans des instructions SELECT en les incorporant dans la clause FROM.  
  
## <a name="see-also"></a>Voir aussi  
 [Common Language Runtime &#40;CLR&#41; vue d’ensemble](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md)   
 [Fonctions table CLR](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-table-valued-functions.md)  
  
  
