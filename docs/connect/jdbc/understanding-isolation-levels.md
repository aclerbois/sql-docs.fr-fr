---
title: Présentation des niveaux d’Isolation | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2c41e23a-da6c-4650-b5fc-b5fe53ba65c3
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a7c09de18ede2c5230179f4ac4df68686d9d256c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-isolation-levels"></a>Fonctionnement des niveaux d'isolement
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Les transactions spécifient un niveau d'isolement. Ce niveau définit le degré d'isolement d'une transaction par rapport aux modifications de ressource ou de données apportées par d'autres transactions. Les niveaux d'isolement déterminent les effets secondaires de la concurrence (lectures erronées, lectures fantômes) qui sont autorisés.  
  
 Les niveaux d'isolement des transactions contrôlent les éléments suivants :  
  
-   l'acquisition de verrous lors de la lecture de données, et le type de verrous nécessaires ;  
  
-   la durée de vie des verrous de lecture ;  
  
-   la réaction d'une opération de lecture qui fait référence à des lignes modifiées par une autre transaction :  
  
    -   Blocage jusqu'à ce que le verrou exclusif sur la ligne soit levé  
  
    -   Récupération de la version validée de la ligne telle qu'elle était au début de l'instruction ou de la transaction  
  
    -   Lecture de la modification des données non validées  
  
 Le choix d'un niveau d'isolement n'a aucune influence sur les verrous acquis pour protéger les modifications de données. Une transaction acquiert toujours un verrou exclusif sur les données qu'elle modifie et garde celui-ci jusqu'à ce qu'elle ait terminé son travail, indépendamment du niveau d'isolement défini pour elle. Dans le cas des opérations de lecture, le niveau d'isolation d'une transaction définit principalement son niveau de protection contre les effets des modifications apportées par les autres transactions.  
  
 Plus le niveau d'isolement est faible, plus le nombre de personnes susceptibles d'accéder aux données en même temps est élevé, et plus les effets secondaires de la concurrence (lectures erronées, mises à jour perdues) sont nombreux. Inversement, plus le niveau d'isolement est élevé, plus le nombre de types d'effets secondaires de la concurrence qu'un utilisateur est susceptible de rencontrer est réduit. Cependant, la quantité de ressources système nécessaires et la probabilité d'un blocage mutuel de transactions sont plus élevées. Le choix du niveau d'isolation adéquat dépend d'une mise en équilibre de l'espace réservé et des exigences en matière d'intégrité des données de l'application. Le niveau le plus élevé, sérialisable, garantit qu'une transaction récupère exactement les mêmes données à chaque fois qu'elle répète une opération de lecture, mais en utilisant un niveau de verrouillage susceptible de gêner les autres utilisateurs dans les systèmes multi-utilisateurs. Le niveau le plus bas, lecture non validée, permet la récupération de données qui ont été modifiées mais non validées par d'autres transactions. Ce niveau permet l'apparition de tous les effets secondaires de la concurrence, mais la charge du système est réduite puisqu'il n'y a ni verrouillage de lecture, ni versioning de ligne.  
  
## <a name="remarks"></a>Notes  
 Le tableau suivant répertorie les effets secondaires de la concurrence provoqués par les différents niveaux d'isolement.  
  
|Niveau d'isolement|Lecture erronée|Lecture non reproductible|Fantôme|  
|---------------------|----------------|-------------------------|-------------|  
|Lecture non validée|Oui|Oui|Oui|  
|Lecture validée|non|Oui|Oui|  
|Lecture renouvelable|non|Non|Oui|  
|Snapshot|non|Non|non|  
|Sérialisable|non|Non|non|  
  
 Les transactions doivent être exécutées à un niveau d'isolement au moins égal à lecture renouvelée afin d'empêcher les pertes de mises à jour qui peuvent se produire lorsque deux transactions extraient la même ligne, puis mettent ultérieurement à jour la ligne en fonction des valeurs extraites initialement. Si les deux transactions mettent à jour des lignes à l'aide d'une instruction UPDATE unique et qu'elles ne basent pas la mise à jour sur les valeurs extraites précédemment, aucune perte de mise à jour ne peut se produire au niveau d'isolement par défaut de lecture validée.  
  
 Pour définir le niveau d’isolement pour une transaction, vous pouvez utiliser la [setTransactionIsolation](../../connect/jdbc/reference/settransactionisolation-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe. Cette méthode accepte un **int** valeur comme argument, qui est basé sur l’une des constantes de connexion comme suit :  
  
```  
con.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);  
```  
  
 Pour utiliser le nouveau niveau d’isolation de capture instantanée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez utiliser une des constantes SQLServerConnection comme suit :  
  
```  
con.setTransactionIsolation(SQLServerConnection.TRANSACTION_SNAPSHOT);  
```  
  
 ou vous pouvez utiliser :  
  
```  
con.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED + 4094);  
```  
  
 Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] niveaux d’isolation, consultez « niveaux d’Isolation de la [!INCLUDE[ssDE](../../includes/ssde_md.md)]» dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="see-also"></a>Voir aussi  
 [Réalisation de transactions avec le pilote JDBC](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  
  
  
