---
title: Durée de vie des transactions | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- lifetimes [SQL Server]
- Transact-SQL vs. managed code
ms.assetid: cb076fda-6488-4959-a6a4-7adaccf3f25c
caps.latest.revision: 10
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 2c78887d28fdc202da63f58167b4fb414e905f58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transaction-lifetimes"></a>Durées de vie des transactions
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Il existe une différence importante entre des transactions démarrées dans des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] et celles démarrées dans du code managé : le code CLR (Common Language Runtime) ne peut pas déséquilibrer l'état des transactions lors de l'entrée ou de la sortie d'un appel au CLR. Soyez conscient des implications d'une telle différence :  
  
-   Une transaction démarrée à l'intérieur d'une trame CLR doit être validée ou annulée ; dans le cas contraire, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur à la sortie de la trame.  
  
-   Une transaction externe ne peut pas être validée ni annulée au sein du code CLR.  
  
-   Une tentative de validation d'une transaction non démarrée dans la même procédure provoque une erreur d'exécution.  
  
-   Une tentative d'annulation d'une transaction non démarrée dans la même procédure provoque le blocage de la transaction (ce qui empêche toute opération collatérale de se produire). La transaction cesse jusqu'à ce que le code CLR soit hors de portée. Notez que cela peut être utile lorsque vous détectez une erreur à l'intérieur de votre procédure et souhaitez vous assurer que la transaction entière se termine.  
  
## <a name="see-also"></a>Voir aussi  
 [Transactions et l’intégration du CLR](../../relational-databases/clr-integration-data-access-transactions/clr-integration-and-transactions.md)  
  
  
