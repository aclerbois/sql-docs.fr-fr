---
title: Utilisation des instructions et des jeux de résultats | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: cc917534-f5f8-4844-87c8-597c48b4e06d
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 14a6f86b503cf4c0f864a881479a37314c11d528
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-statements-and-result-sets"></a>Travail sur des instructions et des jeux de résultats
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Lorsque vous travaillez avec le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] et l’instruction et les objets de jeu de résultats qui l’accompagnent, il existe plusieurs techniques que vous pouvez utiliser pour améliorer les performances et la fiabilité de vos applications.  
  
## <a name="use-the-appropriate-statement-object"></a>Utilisez l'objet d'instruction approprié  
 Lorsque vous utilisez un des objets de l’instruction du pilote JDBC, telles que la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md), ou [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) objet, Assurez-vous que vous utilisez l’objet approprié pour le travail.  
  
-   Si vous n’avez pas les paramètres de sortie, il est inutile d’utiliser l’objet SQLServerCallableStatement. Au lieu de cela, utilisez la SQLServerStatement ou l’objet SQLServerPreparedStatement.  
  
-   Si vous n’envisagez pas d’exécuter l’instruction plusieurs fois, ou que vous n’avez pas IN ou les paramètres de sortie, il est inutile d’utiliser le SQLServerCallableStatement ou l’objet SQLServerPreparedStatement. Au lieu de cela, utilisez l’objet SQLServerStatement.  
  
## <a name="use-the-appropriate-concurrency-for-resultset-objects"></a>Utilisez l'accès simultané approprié aux objets ResultSet  
 Ne demandez pas d'accès simultané pouvant être mis à jour lorsque vous créez des instructions produisant des jeux de résultats, à moins que vous n'ayez réellement l'intention de mettre à jour les résultats. Le modèle de curseur en avant uniquement et en lecture seule est le plus rapide pour lire les petits jeux de résultats.  
  
## <a name="limit-the-size-of-your-result-sets"></a>Limitez la taille de vos jeux de résultats  
 Envisagez d’utiliser le [setMaxRows](../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md) (méthode) (ou la syntaxe SET ROWCOUNT ou SELECT TOP N SQL) pour limiter le nombre de lignes retournées à partir de jeux de résultats potentiellement volumineux. Si vous devez gérer des jeux de résultats volumineux, songez à utiliser une mise en mémoire tampon adaptative des réponses en définissant la propriété de chaîne de connexion responseBuffering=adaptive, qui est le mode par défaut. Cette approche permet à l'application de traiter des jeux de résultats volumineux sans nécessiter de curseurs côté serveur ; par ailleurs, elle réduit l'utilisation de la mémoire de l'application. Pour plus d’informations, consultez [à l’aide de mise en mémoire tampon adaptative](../../connect/jdbc/using-adaptive-buffering.md).  
  
## <a name="use-the-appropriate-fetch-size"></a>Utilisez la taille d'extraction appropriée  
 Pour les curseurs côté serveur en lecture seule, il est nécessaire de choisir entre de nombreux allers-retours vers le serveur ou l'utilisation d'une grande quantité de mémoire dans le pilote. Pour les curseurs de serveurs pouvant être mis à jour, la taille d'extraction influence également la sensibilité des modifications au jeu de résultats et à l'accès simultané du serveur. Mises à jour des lignes dans le tampon d’extraction actuel ne sont pas visibles tant qu’explicite [refreshRow](../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md) méthode est publiée ou jusqu'à ce que le curseur quitte le tampon d’extraction. Les performances des tampons d'extraction importants seront meilleures (moins de boucles de connexion avec le serveur) ; néanmoins, les tampons sont moins sensibles aux modifications et réduisent l'accès simultané au serveur si CONCUR_SS_SCROLL_LOCKS (1009) est utilisé. Pour une sensibilité maximale aux modifications, utilisez une taille d'extraction de 1. Cependant, notez que cela provoquera une boucle avec le serveur pour chaque ligne extraite.  
  
## <a name="use-streams-for-large-in-parameters"></a>Utilisez des flux pour des paramètres IN importants  
 Utilisez des flux ou des BLOB et des CLOB matérialisés de manière incrémentielle pour traiter la mise à jour d'importantes valeurs de colonne ou l'envoi d'importants paramètres IN. Le pilote JDBC envoie ces éléments au serveur par plusieurs boucles de connexion, ce qui vous permet de définir et de mettre à jour des valeurs plus importantes que ce que la mémoire peut contenir.  
  
## <a name="see-also"></a>Voir aussi  
 [Amélioration des performances et de la fiabilité avec le pilote JDBC](../../connect/jdbc/improving-performance-and-reliability-with-the-jdbc-driver.md)  
  
  
