---
title: Gestion des tailles de lot de copie en bloc | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-bulk-copy-operations
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- ODBC, bulk copy operations
- batches [ODBC]
- bulk copy [ODBC], batch sizes
ms.assetid: 4b24139f-788b-45a6-86dc-ae835435d737
caps.latest.revision: 32
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 816f5ec577e65b84e23ba538008d06542348f02c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="managing-bulk-copy-batch-sizes"></a>Gestion des tailles de lot de copie en bloc
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  L'objectif principal d'un lot dans des opérations de copie en bloc est de définir l'étendue d'une transaction. Si aucune taille de lot n'est définie, les fonctions de copie en bloc considèrent une copie en bloc entière comme une transaction. Si une taille de lot est définie, chaque lot constitue alors une transaction validée à la fin de l'exécution de ce lot.  
  
 Si une copie en bloc est effectuée sans taille de lot et qu'une erreur est détectée, la copie en bloc entière est restaurée. La récupération d'une copie en bloc de longue durée peut prendre un certain temps. Lorsqu'une taille de lot est définie, la copie en bloc considère chaque lot comme une transaction et valide chaque lot. Si une erreur est détectée, seul le dernier lot en attente doit être restauré.  
  
 La taille de lot peut également affecter la surcharge de verrouillage. Lorsque vous effectuez une copie en bloc contre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’indicateur TABLOCK peut être spécifié à l’aide de [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) pour acquérir un verrou de table au lieu de verrous de ligne. Le verrou de table unique peut être maintenu avec une charge minimale pour une opération de copie en bloc entière. Si TABLOCK n'est pas spécifié, les verrous sont maintenus sur des lignes individuelles et la charge liée à la maintenance de tous les verrous pour la durée de la copie en bloc peut ralentir les performances. Les verrous étant uniquement maintenus pour la durée d'une transaction, la spécification d'une taille de lot résout ce problème en générant périodiquement une validation qui libère les verrous actuellement maintenus.  
  
 Le nombre de lignes constituant un lot peut avoir des effets significatifs sur les performances lors de la copie en bloc d'un grand nombre de lignes. Les recommandations pour la taille de lot varient selon le type de copie en bloc effectuée.  
  
-   Lorsque vous effectuez une copie en bloc vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], spécifiez l'indicateur de copie en bloc TABLOCK et définissez une taille de lot importante.  
  
-   Lorsque TABLOCK n'est pas spécifié, limitez les tailles de lot à 1 000 lignes.  
  
 Lors de la copie en bloc dans un fichier de données, la taille de lot est spécifiée en appelant **bcp_control** avec l’option BCPBATCH avant d’appeler [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md). Lorsque la copie en bloc à partir de variables de programme à l’aide de [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) et [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md), la taille de lot est contrôlée en appelant [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md) après avoir appelé [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) *x* fois, où *x* est le nombre de lignes dans un lot.  
  
 Outre la spécification de la taille d'une transaction, les lots affectent également le moment où les lignes sont envoyées au serveur via le réseau. Les fonctions de copie en bloc mettent normalement en cache les lignes à partir de **bcp_sendrow** jusqu'à ce qu’un paquet réseau est remplie et ensuite envoyer le paquet complet au serveur. Lorsqu’une application appelle **bcp_batch**, toutefois, le paquet actuel est envoyé au serveur indépendamment de si elle a été remplie. Le fait d'utiliser une taille de lot très réduite peut ralentir les performances si cela aboutit à l'envoi de nombreux paquets partiellement remplis au serveur. Par exemple, l’appel **bcp_batch** après chaque **bcp_sendrow** signifie que chaque ligne à envoyer dans un paquet séparé et, à moins que les lignes sont très volumineux, gaspille de l’espace dans chaque paquet. La taille par défaut des paquets réseau pour SQL Server est de 4 Ko, bien qu’une application peut modifier la taille en appelant [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) spécifiant l’attribut SQL_ATTR_PACKET_SIZE.  
  
 Un autre effet secondaire de lots est que chaque lot est considéré comme un résultat en suspens jusqu'à ce que celle-ci se termine **bcp_batch**. Si toutes les autres opérations sont tentées sur un handle de connexion alors qu’un lot est en attente, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client génère une erreur avec SQLState = « HY000 » et une chaîne de message d’erreur :  
  
```  
"[Microsoft][SQL Server Native Client] Connection is busy with  
results for another hstmt."  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations de copie en bloc &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)   
 [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  
