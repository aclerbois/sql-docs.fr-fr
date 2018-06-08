---
title: Éviter les conflits avec les opérations de base de données dans les applications FILESTREAM | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: blob
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-blob
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], Win32 and Transact-SQL Conflicts
ms.assetid: 8b1ee196-69af-4f9b-9bf5-63d8ac2bc39b
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: de11c4124d10bf11557169577c20688d13855907
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="avoid-conflicts-with-database-operations-in-filestream-applications"></a>Éviter les conflits avec les opérations de base de données dans les applications FILESTREAM
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les applications qui utilisent SqlOpenFilestream() pour ouvrir des descripteurs de fichiers Win32 afin de lire ou d’écrire des données BLOB FILESTREAM peuvent rencontrer des erreurs de conflit avec les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] gérées dans une transaction commune. Cela inclut [!INCLUDE[tsql](../../includes/tsql-md.md)] ou les requêtes MARS dont l'exécution dure longtemps. Les applications doivent être conçues avec soin afin de mieux éviter ces types de conflits.  
  
 Lorsque [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ou des applications essaient d’ouvrir des FILESTREAM BLOB, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie le contexte de transaction associé. [!INCLUDE[ssDE](../../includes/ssde-md.md)] autorise ou refuse la demande selon que l’opération en cours fonctionne avec des instructions DDL, des instructions DML, la récupération de données ou la gestion de transactions. Le tableau suivant indique comment le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine si une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] sera autorisée ou refusée selon les types de fichiers qui sont ouverts pendant la transaction.  
  
|instructions Transact-SQL|Ouvert pour la lecture|Ouvert pour l'écriture|  
|------------------------------|---------------------|----------------------|  
|Instructions DDL qui fonctionnent avec des métadonnées de base de données, telles que CREATE TABLE, CREATE INDEX, DROP TABLE et ALTER TABLE.|Autorisé|Sont bloquées et échouent à la suite d'un délai d'expiration.|  
|Instructions DML qui fonctionnent avec les données qui sont stockées dans la base de données, telles qu'UPDATE, DELETE et INSERT.|Autorisé|Refusées|  
|SELECT|Autorisé|Autorisé|  
|COMMIT TRANSACTION|Refusées*|Refusées*|  
|SAVE TRANSACTION|Refusées*|Refusées*|  
|ROLLBACK|Autorisées*|Autorisées*|  
  
 \* La transaction est annulée, et les descripteurs ouverts pour le contexte de transaction sont invalidés. L'application doit fermer tous les descripteurs ouverts.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent comment les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et l’accès FILESTREAM Win32 peut provoquer des conflits.  
  
### <a name="a-opening-a-filestream-blob-for-write-access"></a>A. Ouverture d'un BLOB FILESTREAM pour un accès en écriture  
 L'exemple suivant montre l'effet d'ouverture d'un fichier pour un accès uniquement en écriture.  
  
```  
dstHandle =  OpenSqlFilestream(dstFilePath, Write, 0,  
    transactionToken, cbTransactionToken, 0);  
  
//Write some date to the FILESTREAM BLOB.  
WriteFile(dstHandle, updateData, …);  
  
//DDL statements will be denied.  
//DML statements will be denied.  
//SELECT statements will be allowed. The FILESTREAM BLOB is  
//returned without the modifications that are made by  
//WriteFile(dstHandle, updateData, …).  
CloseHandle(dstHandle);  
  
//DDL statements will be allowed.  
//DML statements will be allowed.  
//SELECT statements will be allowed. The FILESTREAM BLOB  
//is returned with the updateData applied.  
```  
  
### <a name="b-opening-a-filestream-blob-for-read-access"></a>B. Ouverture d'un BLOB FILESTREAM pour un accès en lecture  
 L'exemple suivant montre l'effet d'ouverture d'un fichier pour un accès uniquement en lecture.  
  
```  
dstHandle =  OpenSqlFilestream(dstFilePath, Read, 0,  
    transactionToken, cbTransactionToken, 0);  
//DDL statements will be denied.  
//DML statements will be allowed. Any changes that are  
//made to the FILESTREAM BLOB will not be returned until  
//the dstHandle is closed.  
//SELECT statements will be allowed.  
CloseHandle(dstHandle);  
  
//DDL statements will be allowed.  
//DML statements will be allowed.  
//SELECT statements will be allowed.  
```  
  
### <a name="c-opening-and-closing-multiple-filestream-blob-files"></a>C. Ouverture et fermeture de plusieurs fichiers BLOB FILESTREAM  
 Si plusieurs fichiers sont ouverts, c'est la règle la plus restrictive qui est utilisée. L'exemple suivant ouvre deux fichiers. Le premier fichier est ouvert pour la lecture et le second pour l'écriture. Les instructions DML seront refusées jusqu'à ce que le deuxième fichier soit ouvert.  
  
```  
dstHandle =  OpenSqlFilestream(dstFilePath, Read, 0,  
    transactionToken, cbTransactionToken, 0);  
//DDL statements will be denied.  
//DML statements will be allowed.  
//SELECT statements will be allowed.  
  
dstHandle1 =  OpenSqlFilestream(dstFilePath1, Write, 0,  
    transactionToken, cbTransactionToken, 0);  
  
//DDL statements will be denied.  
//DML statements will be denied.  
//SELECT statements will be allowed.  
  
//Close the read handle. The write handle is still open.  
CloseHandle(dstHandle);  
//DML statements are still denied because the write handle is open.  
  
//DDL statements will be denied.  
//DML statements will be denied.  
//SELECT statements will be allowed.  
  
CloseHandle(dstHandle1);  
//DDL statements will be allowed.  
//DML statements will be allowed.  
//SELECT statements will be allowed.  
```  
  
### <a name="d-failing-to-close-a-cursor"></a>D. Échec de fermeture d'un curseur  
 L'exemple suivant montre comment un curseur d'instruction qui n'est pas fermé peut empêcher `OpenSqlFilestream()` d'ouvrir le BLOB pour l'accès en écriture.  
  
```  
TCHAR *sqlDBQuery =  
TEXT("SELECT GET_FILESTREAM_TRANSACTION_CONTEXT(),")  
TEXT("Chart.PathName() FROM Archive.dbo.Records");  
  
//Execute a long-running Transact-SQL statement. Do not allow  
//the statement to complete before trying to  
//open the file.  
  
SQLExecDirect(hstmt, sqlDBQuery, SQL_NTS);  
  
//Before you call OpenSqlFilestream() any open files  
//that the Cursor the Transact-SQL statement is using  
// must be closed. In this example,  
//SQLCloseCursor(hstmt) is not called so that  
//the transaction will indicate that there is a file  
//open for reading. This will cause the call to  
//OpenSqlFilestream() to fail because the file is  
//still open.  
  
HANDLE srcHandle =  OpenSqlFilestream(srcFilePath,  
     Write, 0,  transactionToken,  cbTransactionToken,  0);  
  
//srcHandle will == INVALID_HANDLE_VALUE because the  
//cursor is still open.  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Accéder à des données FILESTREAM avec OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)   
 [Utilisation de MARS &#40;Multiple Active Result Sets&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)  
  
  
