---
title: Préparation des métadonnées d’instruction mise en cache pour le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: 1
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a291cfb9497cee4fea87db915ca088069c1ec3cf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prepared-statement-metadata-caching-for-the-jdbc-driver"></a>Métadonnées d’instruction préparée mise en cache pour le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cet article fournit des informations sur les deux modifications sont implémentées pour améliorer les performances du pilote.

## <a name="batching-of-unprepare-for-prepared-statements"></a>Supprimer le traitement par lot de la préparation pour les instructions préparées
Étant donné que la version 6.1.6-preview, une amélioration des performances a été implémentée en limitant boucles de serveur à SQL Server. Auparavant, pour chaque requête prepareStatement, un appel à l’annulation de préparation a été également envoyé. À présent, le pilote est le traitement par lot est requêtes jusqu’au seuil « ServerPreparedStatementDiscardThreshold », qui a une valeur par défaut de 10.

> [!NOTE]  
>  Les utilisateurs peuvent modifier la valeur par défaut avec la méthode suivante : setServerPreparedStatementDiscardThreshold (la valeur int)

Une modification plus introduite à partir de 6.1.6-preview est qu’avant cette version, pilote serait toujours appeler sp_prepexec. Maintenant, pour la première exécution d’une instruction préparée, pilote appelle sp_executesql et pour le reste, il exécute sp_prepexec et lui assigne un handle. Plus de détails se trouvent [ici](https://github.com/Microsoft/mssql-jdbc/wiki/PreparedStatement-metadata-caching).

> [!NOTE]  
>  Les utilisateurs peuvent modifier le comportement par défaut pour les versions précédentes de toujours appeler sp_prepexec en définissant l’enablePrepareOnFirstPreparedStatementCall à **true** à l’aide de la méthode suivante : setEnablePrepareOnFirstPreparedStatementCall (valeur booléenne)

### <a name="list-of-the-new-apis-introduced-with-this-change-for-batching-of-unprepare-for-prepared-statements"></a>Supprimer la liste des nouvelles API introduites avec cette modification, pour le traitement par lot de préparation pour les instructions préparées

 **SQLServerConnection**
 
|Nouvelle méthode| Description|  
|-----------|-----------------|  
|int getDiscardedServerPreparedStatementCount()|Retourne le nombre d’en suspens préparée instruction ces actions.|
|void closeUnreferencedPreparedStatementHandles()|Force les demandes unprepare pour toute instruction préparée rejetées en suspens doit être exécutée.|
|getEnablePrepareOnFirstPreparedStatementCall() booléenne|Retourne le comportement pour une instance de connexion spécifique. Si false la première exécution appelle sp_executesql n’a pas préparer une instruction, une fois que la deuxième exécution se produit qu’elle appelle sp_prepexec et réellement le programme d’installation un descripteur d’instruction préparée. Sp_execute d’appels exécutions suivantes. Cela évite la nécessité de sp_unprepare sur instruction préparée fermer si l’instruction est exécutée uniquement si une seule fois. La valeur par défaut de cette option peut être modifiée en appelant setDefaultEnablePrepareOnFirstPreparedStatementCall().|
|void setEnablePrepareOnFirstPreparedStatementCall(boolean value)|Spécifie le comportement d’une instance de connexion spécifique. Si la valeur est false la première exécution appelle sp_executesql n’a pas préparer une instruction, une fois que la deuxième exécution se produit qu’elle appelle sp_prepexec et réellement le programme d’installation un descripteur d’instruction préparée. Sp_execute d’appels exécutions suivantes. Cela évite la nécessité de sp_unprepare sur instruction préparée fermer si l’instruction est exécutée uniquement si une seule fois.|
|int getServerPreparedStatementDiscardThreshold()|Retourne le comportement pour une instance de connexion spécifique. Ce paramètre contrôle préparé en suspens combien rejet instruction actions (sp_unprepare) peuvent être en attente par connexion avant l’exécution d’un appel à nettoyer les handles en attente sur le serveur. Si le paramètre est < = 1, ces actions sont exécutées immédiatement sur une instruction préparée fermer. S’il est défini {@literal >} 1, ces appels sont regroupées afin d’éviter la surcharge de l’appel sp_unprepare trop souvent. La valeur par défaut de cette option peut être modifiée en appelant getDefaultServerPreparedStatementDiscardThreshold().|
|void setServerPreparedStatementDiscardThreshold(int value)|Spécifie le comportement d’une instance de connexion spécifique. Ce paramètre contrôle préparé en suspens combien rejet instruction actions (sp_unprepare) peuvent être en attente par connexion avant l’exécution d’un appel à nettoyer les handles en attente sur le serveur. Si le paramètre est < = 1 ces actions sont exécutées immédiatement sur une instruction préparée fermer. Si elle est définie sur 1 > ces appels sont regroupées afin d’éviter le traitement de l’appel sp_unprepare trop souvent.|

 **SQLServerDataSource**
 
|Nouvelle méthode| Description|  
|-----------|-----------------|  
|void setEnablePrepareOnFirstPreparedStatementCall(boolean enablePrepareOnFirstPreparedStatementCall)|Si cette configuration a la valeur false la première exécution d’une instruction préparée appelle sp_executesql n’a pas préparer une instruction, une fois que la deuxième exécution se produit qu’elle appelle sp_prepexec et réellement le programme d’installation un descripteur d’instruction préparée. Sp_execute d’appels exécutions suivantes. Cela évite la nécessité de sp_unprepare sur instruction préparée fermer si l’instruction est exécutée uniquement si une seule fois.|
|getEnablePrepareOnFirstPreparedStatementCall() booléenne|Si cette configuration retourne la valeur false la première exécution d’une instruction préparée appelle sp_executesql et n’a pas préparer une instruction, une fois que la deuxième exécution se produit, il appelle sp_prepexec et réellement le programme d’installation un descripteur d’instruction préparée. Sp_execute d’appels exécutions suivantes. Cela évite la nécessité de sp_unprepare sur instruction préparée fermer si l’instruction est exécutée uniquement si une seule fois.|
|void setServerPreparedStatementDiscardThreshold(int serverPreparedStatementDiscardThreshold)|Ce paramètre contrôle préparé en suspens combien rejet instruction actions (sp_unprepare) peuvent être en attente par connexion avant l’exécution d’un appel à nettoyer les handles en attente sur le serveur. Si le paramètre est < = 1 ces actions sont exécutées immédiatement sur une instruction préparée fermer. S’il est défini {@literal >} 1 ces appels sont regroupées afin d’éviter le traitement de l’appel sp_unprepare trop souvent.|
|int getServerPreparedStatementDiscardThreshold()|Ce paramètre contrôle préparé en suspens combien rejet instruction actions (sp_unprepare) peuvent être en attente par connexion avant l’exécution d’un appel à nettoyer les handles en attente sur le serveur. Si le paramètre est < = 1 ces actions sont exécutées immédiatement sur une instruction préparée fermer. S’il est défini {@literal >} 1 ces appels sont regroupées afin d’éviter le traitement de l’appel sp_unprepare trop souvent.|

## <a name="prepared-statement-metatada-caching"></a>Mise en cache de métadonnées de l’instruction préparée
À compter de la version de 6.3.0-preview, Microsoft JDBC driver pour SQL Server prend en charge la mise en cache d’instruction préparée. Avant v6.3.0-version préliminaire, si un exécute une requête qui a été déjà préparée et stockée dans le cache, l’appel à nouveau la même requête ne provoque pas de sa préparation. À présent, le pilote recherche la requête dans le cache et le handle et exécutez-le avec sp_execute.
Mise en cache de métadonnées d’instruction préparée est **désactivé** par défaut. Pour pouvoir l’activer, vous devez appeler la méthode suivante sur l’objet de connexion :

`setStatementPoolingCacheSize(int value)   //value is the desired cache size (any value bigger than 0)`
`setDisableStatementPooling(boolean value) //false allows the caching to take place`

Par exemple : `connection.setStatementPoolingCacheSize(10)`
`connection.setDisableStatementPooling(false)`

### <a name="list-of-the-new-apis-introduced-with-this-change-for-prepared-statement-metadata-caching"></a>Liste des nouvelles API introduites avec cette modification, pour l’instruction préparée la mise en cache des métadonnées

 **SQLServerConnection**
 
|Nouvelle méthode| Description|  
|-----------|-----------------|  
|void setDisableStatementPooling(boolean value)|Définit le regroupement d’instructions à true ou false.|
|getDisableStatementPooling() booléenne|Retourne la valeur true si le regroupement d’instructions est désactivé.|
|void setStatementPoolingCacheSize(int value)|Spécifie la taille du cache d’instruction préparée pour cette connexion. Une valeur inférieure à 1 ne signifie aucun cache.|
|int getStatementPoolingCacheSize()|Retourne la taille du cache d’instruction préparée pour cette connexion. Une valeur inférieure à 1 ne signifie aucun cache.|
|int getStatementHandleCacheEntryCount()|Retourne le nombre actuel de handles d’instructions préparées mis en pool.|
|isPreparedStatementCachingEnabled() booléenne|Si le regroupement d’instructions est activé ou non pour cette connexion.|

 **SQLServerDataSource**
 
|Nouvelle méthode| Description|  
|-----------|-----------------|  
|void setDisableStatementPooling(boolean disableStatementPooling)|Définit l’instruction de mise en pool à true ou false|
|getDisableStatementPooling() booléenne|Retourne la valeur true si le regroupement d’instructions est désactivé.|
|void setStatementPoolingCacheSize(int statementPoolingCacheSize)|Spécifie la taille du cache d’instruction préparée pour cette connexion. Une valeur inférieure à 1 ne signifie aucun cache.|
|int getStatementPoolingCacheSize()|Retourne la taille du cache d’instruction préparée pour cette connexion. Une valeur inférieure à 1 ne signifie aucun cache.|

## <a name="see-also"></a>Voir aussi  
 [Amélioration des performances et de la fiabilité avec le pilote JDBC](../../connect/jdbc/improving-performance-and-reliability-with-the-jdbc-driver.md)  
  
  
