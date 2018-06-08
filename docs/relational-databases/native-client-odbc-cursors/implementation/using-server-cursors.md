---
title: À l’aide de curseurs de serveur | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-cursors
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- ODBC applications, cursors
- cursors [ODBC], server cursors
- ODBC cursors, server cursors
- server cursors [SQL Server]
ms.assetid: 8a6d99b7-10b8-4474-8639-4914b25ba170
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 71c7354c0624d583d6389d3896a27e7548ebeb07
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-server-cursors"></a>Utilisation des curseurs côté serveur
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Si une application ODBC définit des attributs de curseur ODBC que les valeurs par défaut, le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client demande au serveur d’implémenter un curseur de serveur API du même type. L'utilisation de curseurs côté serveur d'API libère la mémoire sur le client et peut réduire considérablement le trafic réseau entre le client et le serveur.  
  
 Un inconvénient possible des curseurs côté serveur d'API est qu'ils ne prennent pas en charge toutes les instructions SQL. Les curseurs côté serveur d'API ne peuvent pas être utilisés pour exécuter :  
  
-   Les lots ou les procédures stockées qui retournent plusieurs ensembles de résultats.  
  
-   Les instructions SELECT contenant les clauses COMPUTE, COMPUTE BY, FOR BROWSE ou INTO.  
  
-   Une instruction EXECUTE faisant référence à une procédure stockée distante.  
  
 En cas de connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'exécution d'une instruction avec ces caractéristiques à l'aide d'un curseur côté serveur provoque la conversion du curseur en un jeu de résultats par défaut. En cas de connexion aux versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il s'ensuit une erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [Comment les curseurs sont implémentés.](../../../relational-databases/native-client-odbc-cursors/implementation/how-cursors-are-implemented.md)  
  
  
