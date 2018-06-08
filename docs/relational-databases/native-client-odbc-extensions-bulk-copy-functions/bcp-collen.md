---
title: bcp_collen | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-extensions-bulk-copy-functions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- bcp_collen
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_collen function
ms.assetid: faaf1f7a-81f2-4852-a178-56602c33673a
caps.latest.revision: 30
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b3e5fc28fe03efe74e74fbe6fc33cb3cb3b4f787
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bcpcollen"></a>bcp_collen
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Définit la longueur des données dans la variable de programme pour la copie en bloc actuelle dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RETCODE bcp_collen (  
        HDBC hdbc,  
        DBINT cbData,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Arguments  
 *pas*  
 Handle de connexion ODBC compatible avec la copie en bloc.  
  
 *cbData*  
 Longueur des données dans la variable de programme, à l'exclusion de la longueur de tout indicateur de longueur ou terminateur. La définition de *cbData* avec la valeur SQL_NULL_DATA indique que toutes les lignes copiées sur le serveur contiennent une valeur NULL pour la colonne. La définition de cbData avec la valeur SQL_VARLEN_DATA indique qu'un terminateur de chaîne ou une autre méthode est utilisé pour déterminer la longueur des données copiées. S'il existe à la fois un indicateur de longueur et un terminateur, le système utilise celui qui entraîne la copie du moins grand nombre de données.  
  
 *idxServerCol*  
 Position ordinale de la colonne dans la table vers laquelle les données sont copiées. La première colonne est 1. La position ordinale d'une colonne est indiquée par [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="returns"></a>Valeur renvoyée  
 SUCCEED ou FAIL.  
  
## <a name="remarks"></a>Notes  
 La fonction **bcp_collen** vous permet de modifier la longueur de données dans la variable de programme pour une colonne particulière lors de la copie de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md).  
  
 Initialement, la longueur de données est déterminée quand [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) est appelé. Si la longueur de données change entre des appels à **bcp_sendrow** et qu'aucun préfixe de longueur ou terminateur n'est en cours d'utilisation, vous pouvez appeler **bcp_collen** pour réinitialiser la longueur. L'appel suivant à **bcp_sendrow** utilise la longueur définie par l'appel à **bcp_collen**.  
  
 Vous devez appeler **bcp_collen** une fois pour chaque colonne de la table dont vous voulez modifier la longueur de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
