---
title: Close (méthode) (SQLServerResultSet) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLServerResultSet.close
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8f3adf5b-874e-4cf2-b4ef-672dda42d77a
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: deea90b13bb4bb99c504f5eaa344babdf4a1c680
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="close-method-sqlserverresultset"></a>Méthode close (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cela libère [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) l’objet de base de données et ressources JDBC immédiatement au lieu d’attendre pour que cela se produit lorsqu’il est fermé automatiquement.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void close()  
```  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode de fermeture est spécifiée par la méthode de fermeture dans l’interface java.sql.ResultSet.  
  
 Un objet SQLServerResultSet est fermé automatiquement par le [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet ayant généré lorsque cet objet SQLServerStatement est fermé, ré-exécution ou utilisé pour récupérer le résultat suivant à partir d’une séquence de plusieurs résultats . Un objet SQLServerResultSet est également automatiquement fermé lorsqu’il est par le garbage collecté.  
  
 Lors de l'exécution d'une instruction qui produit un ensemble unique et important de résultats avant uniquement, en lecture seule, un ensemble initial de lignes uniquement doit vous intéresser dans le jeu de résultats retourné. Dans ce cas, l’application peut appeler le [Annuler](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md) méthode de l’objet d’instruction associé avant de fermer le résultat de la valeur afin de réduire le temps de traitement nécessaire pour ignorer les lignes restantes inutiles. Nous vous recommandons de faire la part entre le temps de traitement gagné et le temps, ainsi que l'aller-retour jusqu'au serveur requis pour annuler l'exécution, lorsque la décision doit être prise d'utiliser cette technique ou pas.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
