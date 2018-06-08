---
title: Fermeture des objets inutilisés | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ce8f9b35-c761-4b0c-9a46-985eef2c2e0b
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 644f3f22d544940e1f0f72cdfbfd5351fee6f519
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="closing-objects-when-not-in-use"></a>Fermeture d'objets inutilisés
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Lorsque vous travaillez avec des objets de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], en particulier le [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) ou un de l’instruction des objets tels que [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement ](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) ou [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md), vous devez les fermer explicitement à l’aide de leurs méthodes fermer lorsqu’ils ne sont plus nécessaires. Cela améliore les performances en libérant les ressources de pilote et de serveur le plus tôt possible, au lieu d'attendre que le garbage collector de la machine virtuelle Java ne le fasse pour vous.  
  
 La fermeture des objets est particulièrement importante pour conserver un bon accès simultané sur le serveur lorsque vous utilisez des arrêts de défilement. Les arrêts de défilement dans la dernière mémoire tampon d'extraction accédée sont maintenus jusqu'à la fermeture du jeu de résultats. De même, les handles préparés par instruction sont maintenus jusqu'à ce que l'instruction soit fermée. Si vous réutilisez une connexion pour plusieurs instructions, le fait de fermer les instructions avant de leur permettre de sortir de portée permettra au serveur de nettoyer plus tôt les handles préparés.  
  
## <a name="see-also"></a>Voir aussi  
 [Amélioration des performances et de la fiabilité avec le pilote JDBC](../../connect/jdbc/improving-performance-and-reliability-with-the-jdbc-driver.md)  
  
  
