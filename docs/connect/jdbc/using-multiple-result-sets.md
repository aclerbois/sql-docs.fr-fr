---
title: À l’aide de plusieurs jeux de résultats | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ab6a3cfa-073b-44e9-afca-a8675cfe5fd1
caps.latest.revision: 33
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b4f3c8b75d18b598bfe43a48969f098022893c8b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-multiple-result-sets"></a>Utilisation de plusieurs jeux de résultats
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Lorsque vous travaillez avec SQL inline ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] procédures stockées qui retournent plusieurs jeux de résultats, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit le [getResultSet](../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md) méthode dans le [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe pour la récupération de chaque jeu de données retourné. En outre, lors de l’exécution d’une instruction qui retourne plusieurs jeux de résultats, vous pouvez utiliser la [exécuter](../../connect/jdbc/reference/execute-method-sqlserverstatement.md) la méthode de la SQLServerStatement classe, car elle retourne un **booléenne** valeur qui indique si le valeur retournée est un jeu de résultats ou un nombre de mises à jour.  
  
 Si la méthode execute retourne **true**, l’instruction qui a été exécutée a retourné un ou plusieurs jeux de résultats. Vous pouvez accéder au premier jeu de résultats en appelant la méthode getResultSet. Pour déterminer si les jeux de résultats supplémentaires est disponibles, vous pouvez appeler la [getMoreResults](../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md) (méthode), qui retourne un **booléenne** valeur **true** si les jeux de résultats supplémentaires est disponibles. Si plusieurs jeux de résultats est disponibles, vous pouvez appeler la méthode getResultSet pour y accéder, tout en continuant le processus jusqu'à ce que tous les jeux de résultats ont été traités. Si la méthode getMoreResults retourne **false**, il en existe aucun plusieurs jeux de résultats au processus.  
  
 Si la méthode execute retourne **false**, l’instruction qui a été exécutée a retourné une valeur de nombre de mises à jour, que vous pouvez extraire en appelant le [getUpdateCount](../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md) (méthode).  
  
> [!NOTE]  
>  Pour plus d’informations sur le nombre de mises à jour, consultez [à l’aide d’une procédure stockée avec un nombre de mettre à jour](../../connect/jdbc/using-a-stored-procedure-with-an-update-count.md).  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction, et une instruction SQL est générée qui, lorsqu’il est exécuté, retourne deux jeux de résultats :  
  
 [!code[JDBC#UsingMultipleResultSets1](../../connect/jdbc/codesnippet/Java/using-multiple-result-sets_1.java)]  
  
 Dans ce cas, le nombre de jeux de résultats retourné est deux. Cependant, le code est écrit de telle façon que si un nombre inconnu de jeux de résultats est retourné, comme lors de l'appel d'une procédure stockée, tous les jeux de résultats sont traités. Pour voir un exemple d’appel de procédure stockée qui retourne plusieurs jeux de résultats avec des valeurs de mise à jour, consultez [instructions complexes de gestion des](../../connect/jdbc/handling-complex-statements.md).  
  
> [!NOTE]  
>  Lorsque vous effectuez l’appel à la méthode getMoreResults de la classe SQLServerStatement, le jeu de résultats précédemment retourné est implicitement fermé.  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec le pilote JDBC](../../connect/jdbc/using-statements-with-the-jdbc-driver.md)  
  
  
