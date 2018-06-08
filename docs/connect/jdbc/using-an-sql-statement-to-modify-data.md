---
title: À l’aide d’une instruction SQL pour modifier les données | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4704199b-c0ae-4c77-8a2e-6963715b4ffb
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0d02d5187e869eb626cfddde9e12bcf55feed51a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-an-sql-statement-to-modify-data"></a>Utilisation d'une instruction SQL pour modifier des données
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Pour modifier les données contenues dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données en utilisant une instruction SQL, vous pouvez utiliser la [executeUpdate](../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe. La méthode executeUpdate transmet l’instruction SQL à la base de données pour le traitement et puis retourne une valeur qui indique le nombre de lignes qui ont été affectés.  
  
 Pour ce faire, vous devez d’abord créer un objet SQLServerStatement à l’aide de la [createStatement](../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe.  
  
 Dans l’exemple suivant, une connexion ouverte à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données est transmise à la fonction, une instruction SQL est générée qui ajoute de nouvelles données à la table, puis l’instruction est exécutée et la valeur de retour s’affiche.  
  
 [!code[JDBC#UsingSQLToModifyData1](../../connect/jdbc/codesnippet/Java/using-an-sql-statement-t_1_1.java)]  
  
> [!NOTE]  
>  Si vous devez utiliser une instruction SQL qui contient des paramètres pour modifier les données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données, vous devez utiliser le [executeUpdate](../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md) méthode de la [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe.  
>   
>  Si la colonne dans laquelle vous tentez d'insérer des données contient des caractères spéciaux, tels que des espaces, vous devez fournir les valeurs à insérer, même s'il s'agit de valeurs par défaut. Si vous ne le faites pas, l'insertion échoue.  
>   
>  Si vous souhaitez que le pilote JDBC retourne tous les nombres de mises à jour, y compris les nombres de mises à jour retournées par des déclencheurs qui ont pu se déclencher, définissez la propriété de chaîne de connexion lastUpdateCount sur « false ». Pour plus d’informations sur la propriété lastUpdateCount, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’instructions avec SQL](../../connect/jdbc/using-statements-with-sql.md)  
  
  
