---
title: Fonction de SQLCleanupConnectionPoolID | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQLCleanupConnectionPoolID function [ODBC]
ms.assetid: 1fc61908-e003-4587-b91a-32f40569fb99
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b417ddc7b2aedcf2c23e2f607ceb53ee7ffb4c6b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcleanupconnectionpoolid-function"></a>SQLCleanupConnectionPoolID (fonction)
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 3,81 ODBC : ODBC  
  
 **Résumé**  
 **SQLCleanupConnectionPoolID** informe un pilote un ID du pool a été dépassé. Un pool d’ID peut le délai d’attente chaque fois que toutes les connexions dans un pool associé à cet ID de pool ont été a expiré. Consultez [le regroupement dans Microsoft Data Access Components](http://msdn.microsoft.com/library/ms810829.aspx) pour plus d’informations sur le délai de connexion.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SQLRETURN  SQLCleanupConnectionPoolID (  
                SQLHENV    EnvironmentHandle  
                SQLPOOLID  PoolID );  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 [Entrée] Le handle d’environnement du pool.  
  
 *PoolID*  
 [Entrée] Le pool associé à l’ID du pool qui a été dépassé.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Le Gestionnaire de pilotes ne traitera pas retournées à partir des informations de diagnostic **SQLCleanupConnectionPoolID**.  
  
 Une application ne peut pas recevoir le message d’erreur retourné par le pilote.  
  
## <a name="remarks"></a>Notes  
 **SQLCleanupConnectionPoolID** peut être appelée à tout moment, mais le Gestionnaire de pilotes garantit qu’aucun autre thread n’appelle simultanément **SQLGetPoolID** et aucun autre thread n’appelle simultanément  **SQLRateConnection** et **SQLPoolConnect** avec un jeton d’informations de connexion affecté à cet ID de pool. Par conséquent, le pilote doit Assurez-vous que cette fonction est thread-safe.  
  
 Un pilote puisse nettoyer les ressources associées à l’ID du pool.  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Inclure sqlspi.h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Le regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
