---
title: Implémentation de la fonctionnalité MERGE dans une procédure stockée compilée en mode natif | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d4bcdc36-3302-4abc-9b35-64ec2b920986
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 261700580577c008bacce1059e50747e094c2a63
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="implementing-merge-functionality-in-a-natively-compiled-stored-procedure"></a>Implémentation de la fonctionnalité MERGE dans une procédure stockée compilée en mode natif
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  
L’exemple de code Transact-SQL dans cette section montre comment vous pouvez simuler l’instruction T-SQL MERGE dans un module compilé en mode natif. Cet exemple utilise une variable de table avec une colonne d’identité, passe en revue les lignes de la variable de table et, pour chaque ligne, effectue la mise à jour si la condition est remplie, sinon une insertion.
  
Voici l’instruction T-SQL MERGE à théoriquement prendre en charge dans une procédure native et que l’exemple de code simule.  
  
  
  
  
    MERGE INTO dbo.Table1 t  
        USING @tvp v  
        ON t.Column1 = v.c1  
        WHEN MATCHED THEN   
            UPDATE SET Column2 = v.c2  
        WHEN NOT MATCHED THEN  
            INSERT (Column1, Column2) VALUES (v.c1, v.c2);  
  
  
  
  
Voici le code T-SQL qui permet de simuler MERGE.  
  
  
  
  
    DROP PROCEDURE IF EXISTS dbo.usp_merge1;  
    go  
    DROP TYPE IF EXISTS dbo.Type1;  
    go  
    DROP TABLE IF EXISTS dbo.Table1;  
    go  
    -----------------------------  
    -- target table and table type used for the workaround
    -----------------------------  
  
    CREATE TABLE dbo.Table1  
    (  
        Column1  INT  NOT NULL  PRIMARY KEY NONCLUSTERED,  
        Column2  INT  NOT NULL  
    )   
        WITH (MEMORY_OPTIMIZED = ON);  
    go  
  
    CREATE TYPE dbo.Type1 AS TABLE  
    (  
        c1  INT  NOT NULL,  
        c2  INT  NOT NULL,  
  
        RowID    INT  NOT NULL  IDENTITY(1,1),  
        INDEX ix_RowID HASH (RowID) WITH (BUCKET_COUNT=1024)  
    )   
        WITH (MEMORY_OPTIMIZED = ON);  
    go  
    -----------------------------  
    -- stored procedure implementing the workaround
    -----------------------------  
  
    CREATE PROCEDURE dbo.usp_merge1   
        @tvp1 dbo.Type1 READONLY  
        WITH  
        NATIVE_COMPILATION, SCHEMABINDING  
    AS   
    BEGIN ATOMIC  
        WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
              LANGUAGE = N'us_english')  

        DECLARE  @i INT = 1,  @c1 INT,  @c2 INT;  
    
        WHILE @i > 0  
        BEGIN  
            SELECT @c1 = c1, @c2 = c2  
                FROM @tvp1  
                WHERE RowID = @i;  
    
            --test whether the row exists in the TVP; if not, we end the loop
            IF @@ROWCOUNT=0  
                SET @i = 0
            ELSE
            BEGIN
                -- try the update
                UPDATE dbo.Table1  
                    SET   Column2 = @c2  
                    WHERE Column1 = @c1;  
    
                -- if there was no row to update, we insert
                IF @@ROWCOUNT=0  
                    INSERT INTO dbo.Table1 (Column1, Column2)  
                        VALUES (@c1, @c2);  
    
                SET @i += 1
            END
        END  
    END  
    go  
    -----------------------------  
    -- test to validate the functionality
    -----------------------------  
  
    INSERT dbo.Table1 VALUES (1,2);  
    go  
  
    SELECT N'Before-MERGE' AS [Before-MERGE], Column1, Column2  
        FROM dbo.Table1;  
    go  
  
    DECLARE @tvp1 dbo.Type1;  
  
    INSERT @tvp1 (c1, c2) VALUES (1,33), (2,4);  
    EXECUTE dbo.usp_merge1 @tvp1;  
    go  
  
    SELECT N'After--MERGE' AS [After--MERGE], Column1, Column2  
        FROM dbo.Table1;  
    go  
    -----------------------------  

  
    /****  Actual output:  
  
    Before-MERGE   Column1   Column2  
    Before-MERGE      1         2  
  
    After--MERGE   Column1   Column2  
    After--MERGE      1        33  
    After--MERGE      2         4  
    ****/  
  
  
## <a name="see-also"></a> Voir aussi  
 [Problèmes de migration pour les procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/migration-issues-for-natively-compiled-stored-procedures.md)   
 [Les constructions Transact-SQL ne sont pas prises en charge par l’OLTP en mémoire](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  
  
