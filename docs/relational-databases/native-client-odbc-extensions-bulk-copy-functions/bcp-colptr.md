---
title: bcp_colptr | Microsoft Docs
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: 
ms.component: native-client-odbc-extensions-bulk-copy-functions
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: reference
apiname:
- bcp_colptr
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_colptr function
ms.assetid: 02ece13e-1da3-4f9d-b860-3177e43d2471
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 48e3469b232033eb3da753e57dfb489f92f19f4d
ms.sourcegitcommit: a0aa5e611a0e6ebb74ac1e2f613e8916dc7a7617
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="bcpcolptr"></a>bcp_colptr
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Définit l'adresse de données de variable de programme pour la copie actuelle dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RETCODE bcp_colptr (  
        HDBC hdbc,  
        LPCBYTE pData,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Arguments  
 *pas*  
 Handle de connexion ODBC compatible avec la copie en bloc.  
  
 *pData*  
 Pointeur vers les données à copier. Si le type de données liées est de type de valeur élevée (tel que SQLTEXT ou SQLIMAGE), *pData* peut être NULL. Une valeur NULL *pData* indique les valeurs de données de type long seront envoyés à SQL Server par segments à l’aide de [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md).  
  
 Si *pData* a la valeur NULL et que la colonne correspondant au champ lié n’est pas un type de valeur élevée, **bcp_colptr** échoue.  
  
 Pour plus d’informations sur les types de valeur élevée, consultez [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)**.**  
  
 *idxServerCol*  
 Position ordinale de la colonne dans la table de base de données vers laquelle les données sont copiées. La première colonne d'une table est la colonne 1. La position ordinale d'une colonne est indiquée par [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="returns"></a>Valeur renvoyée  
 SUCCEED ou FAIL.  
  
## <a name="remarks"></a>Notes  
 Le **bcp_colptr** fonction vous permet de modifier l’adresse de source de données pour une colonne particulière lors de la copie des données vers SQL Server avec [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md).  
  
 Initialement, le pointeur vers les données utilisateur est défini par un appel à **bcp_bind**. Si l’adresse de données de variable de programme change entre les appels à **bcp_sendrow**, vous pouvez appeler **bcp_colptr** pour réinitialiser le pointeur vers les données. L’appel suivant à **bcp_sendrow** envoie les données traitées par l’appel à **bcp_colptr**.  
  
 Il doit y avoir un distinct **bcp_colptr** appel pour chaque colonne dans la table dont les données d’adresse que vous souhaitez modifier.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
