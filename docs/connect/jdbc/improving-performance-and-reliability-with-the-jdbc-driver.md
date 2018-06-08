---
title: Amélioration des performances et fiabilité avec le pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e1592499-b87b-45ee-bab8-beaba8fde841
caps.latest.revision: 25
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 58521cbe6c21e7ef58fc97b52e1ef6b27209e4fe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="improving-performance-and-reliability-with-the-jdbc-driver"></a>Amélioration des performances et de la fiabilité avec le pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Un aspect du développement d'applications commun à toutes les applications est le besoin constant d'amélioration des performances et de la fiabilité. Il existe un certain nombre de techniques pour y parvenir avec le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].  
  
 Les rubriques de cette section décrivent plusieurs techniques d'amélioration des performances et de la fiabilité des applications lors de l'utilisation du pilote JDBC.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Fermeture des objets non utilisés](../../connect/jdbc/closing-objects-when-not-in-use.md)|Décrit l'importance de la fermeture des objets du pilote JDBC lorsqu'ils ne sont plus nécessaires.|  
|[Gestion de la taille des transactions](../../connect/jdbc/managing-transaction-size.md)|Décrit les techniques d'amélioration des performances des transactions.|  
|[Utilisation des instructions et des jeux de résultats](../../connect/jdbc/working-with-statements-and-result-sets.md)|Décrit les techniques d’amélioration des performances lorsque vous utilisez les objets de l’instruction ou le jeu de résultats.|  
|[Utilisation de la mise en mémoire tampon adaptative](../../connect/jdbc/using-adaptive-buffering.md)|Décrit une fonctionnalité de mise en mémoire tampon adaptative, conçue pour récupérer tout type de données de grande valeur sans la charge liée au temps de traitement des curseurs côté serveur.|  
|[Colonnes éparses](../../connect/jdbc/sparse-columns.md)|Décrit la prise en charge du pilote JDBC pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] les colonnes éparses.|  
|[Mise en cache des métadonnées d’instruction préparée pour le pilote JDBC](../../connect/jdbc/prepared-statement-metadata-caching-for-the-jdbc-driver.md)|Présente les techniques d’amélioration des performances des requêtes de l’instruction préparée.|
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  