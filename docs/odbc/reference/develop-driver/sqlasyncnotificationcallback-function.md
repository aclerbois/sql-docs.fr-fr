---
title: Fonction de SQLAsyncNotificationCallback | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c56aedc9-f7f7-4641-b605-f0f98ed4400c
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3c785f8547730817c0b1723da38f7ee021aa4f00
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlasyncnotificationcallback-function"></a>SQLAsyncNotificationCallback (fonction)
**Mise en conformité**  
 Version introduite : ODBC 3.8  
  
 La conformité aux normes : aucun  
  
 **Résumé**  
 **SQLAsyncNotificationCallback** permet à un pilote appeler le Gestionnaire de pilotes lorsqu’il existe des progrès de l’opération asynchrone actuelle une fois que le pilote retourne SQL_STILL_EXECUTING. **SQLAsyncNotificationCallback** peut uniquement être appelé par le pilote.  
  
 Pilotes n’appellent pas **SQLAsyncNotificationCallback** avec le nom de la fonction **SQLAsyncNotificationCallback**. Au lieu de cela, le Gestionnaire de pilotes passe un pointeur de fonction à un pilote en tant que la valeur de l’attribut SQL_ATTR_ASYNC_DBC_NOTIFICATION_CALLBACK ou SQL_ATTR_ASYNC_STMT_NOTIFICATION_CALLBACK le handle de connexion correspondante ou un handle d’instruction, respectivement. Les valeurs de pointeur de fonction différentes peuvent être affectées à plusieurs poignées. Le type du pointeur de fonction est défini en tant que SQL_ASYNC_NOTIFICATION_CALLBACK.  
  
 **SQLAsyncNotificationCallback** est thread-safe. Un pilote peut choisir d’utiliser plusieurs threads appelant **SQLAsyncNotificationCallback** sur différents gère simultanément.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
typedef SQLRETURN (SQL_API *SQL_ASYNC_NOTIFICATION_CALLBACK)(  
   SQLPOINTER pContex,   
   BOOL fLast);  
```  
  
## <a name="arguments"></a>Arguments  
 *pContex*  
 Pointeur vers une structure de données définie par le Gestionnaire de pilote. La valeur est passée au pilote via SQLSetConnectAttr(SQL_ATTR_ASYNC_DBC_NOTIFICATION_CONTEXT) ou SQLSetStmtAttr(SQL_ATTR_ASYNC_STMT_NOTIFICATION_CONTEXT).  Le pilote n’a pas accès à la valeur.  
  
 *fLast*  
 Utilisé par un pilote pour indique que cet appel de fonction de rappel est le dernier pour l’opération asynchrone actuelle. Le pilote retourne un code de retour différent de SQL_STILL_EXECUTING lorsque le Gestionnaire de pilotes appelle la fonction à nouveau. Le Gestionnaire de pilote peut utiliser ces informations, par exemple, pour informer l’application à l’avance que l’opération asynchrone se termine.  
  
 Si *gérer* n’est pas un handle valide du type spécifié par *HandleType*, **SQLCancelHandle** retourne SQL_INVALID_HANDLE.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS ou SQL_ERROR.  
  
## <a name="diagnostics"></a>Diagnostics  
 **SQLAsyncNotificationCallback** peut retourner SQL_ERROR pour deux situations suivantes (celles-ci indiquent un problème d’implémentation dans le pilote ou le Gestionnaire de pilotes.  
  
|Erreur| Description|  
|-----------|-----------------|  
|Connexion ou une instruction n’a pas demandé de notification.||  
|Non valide *gérer*|Le pilote est passé dans un handle non valide, qui ont échoué les tests de validation interne du Gestionnaire de pilotes.|  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution asynchrone (méthode d’interrogation)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
