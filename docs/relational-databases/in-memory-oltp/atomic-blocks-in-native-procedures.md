---
title: Blocs atomiques | Microsoft Docs
ms.custom: ''
ms.date: 10/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 40e0e749-260c-4cfc-a848-444d30c09d85
caps.latest.revision: 13
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e26743a1020bc71e20e17d64aad7c14ccd29c325
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="atomic-blocks-in-native-procedures"></a>Blocs atomiques dans des procédures en mode natif
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  **BEGIN ATOMIC** fait partie de la norme SQL ANSI. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les blocs atomiques au niveau supérieur des procédures stockées compilées en mode natif, ainsi que des fonctions scalaires définies par l’utilisateur compilées en mode natif. Pour plus d’informations sur ces fonctions, consultez [Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
-   Chaque procédure stockée compilée en mode natif contient précisément un bloc d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Il s'agit d'un bloc ATOMIC.  
  
-   Les procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] interprétées en mode non natif et les lots ad hoc ne prennent pas en charge les blocs Atomic.  
  
 Les blocs Atomic sont exécutés (atomiquement) dans la transaction. Toutes les instructions du bloc réussissent ou l'intégralité du bloc est restauré au point de sauvegarde créé au démarrage du bloc. En outre, les paramètres de session sont fixes pour le bloc Atomic. Le fait d'exécuter le même bloc Atomic dans des sessions avec des paramètres différents génère le même comportement, indépendamment des paramètres de la session active.  
  
## <a name="transactions-and-error-handling"></a>Transactions et gestion des erreurs  
 Si une transaction existe déjà dans une session (car un lot a exécuté une instruction **BEGIN TRANSACTION** et la transaction reste active), le démarrage d’un bloc Atomic crée un point de sauvegarde dans la transaction. Si le bloc se termine sans exception, le point de sauvegarde créé pour le bloc est validé, mais la transaction ne sera pas validée avant la validation de la transaction au niveau de la session. Si le bloc lève une exception, les effets du bloc sont restaurés, mais la transaction au niveau de la session se poursuit, à moins que l'exception ne condamne la transaction. Par exemple, un conflit d'écriture condamne la transaction, mais pas une erreur de conversion de type.  
  
 S'il n'y a pas de transactions actives dans une session, **BEGIN ATOMIC** démarre une nouvelle transaction. Si aucune exception n'est levée en dehors de l'étendue du bloc, la transaction est validée à la fin du bloc. Si le bloc lève une exception (autrement dit, l'exception n'est pas interceptée et n'est pas gérée dans le bloc), la transaction est restaurée. Pour les transactions couvrant un bloc Atomic (une procédure stockée compilée en mode natif), vous n’avez pas besoin d’écrire d’instructions **BEGIN TRANSACTION** anetd **COMMIT** ou **ROLLBACK** explicites.  
  
 Les procédures stockées compilées en mode natif prennent en charge les constructions **TRY**, **CATCH**et **THROW** pour la gestion des erreurs. La fonction**RAISERROR** n'est pas prise en charge.  
  
 L'exemple suivant illustre le comportement de gestion des erreurs avec les blocs Atomic et les procédures stockées compilées en mode natif :  
  
```sql  
-- sample table  
CREATE TABLE dbo.t1 (  
  c1 int not null primary key nonclustered  
)  
WITH (MEMORY_OPTIMIZED=ON)  
GO  
  
-- sample proc that inserts 2 rows  
CREATE PROCEDURE dbo.usp_t1 @v1 bigint not null, @v2 bigint not null  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS  
BEGIN ATOMIC  
WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english', DELAYED_DURABILITY = ON)  
  
  INSERT dbo.t1 VALUES (@v1)  
  INSERT dbo.t1 VALUES (@v2)  
  
END  
GO  
  
-- insert two rows  
EXEC dbo.usp_t1 1, 2  
GO  
  
-- verify we have no active transaction  
SELECT @@TRANCOUNT  
GO  
  
-- verify the rows 1 and 2 were committed  
SELECT c1 FROM dbo.t1  
GO  
  
-- execute proc with arithmetic overflow  
EXEC dbo.usp_t1 3, 4444444444444  
GO  
-- expected error message:  
-- Msg 8115, Level 16, State 0, Procedure usp_t1  
-- Arithmetic overflow error converting bigint to data type int.  
  
-- verify we have no active transaction  
SELECT @@TRANCOUNT  
GO  
  
-- verify rows 3 was not committed; usp_t1 has been rolled back  
SELECT c1 FROM dbo.t1  
GO  
  
-- start a new transaction  
BEGIN TRANSACTION  
  -- insert rows 3 and 4  
  EXEC dbo.usp_t1 3, 4  
  
  -- verify there is still an active transaction  
  SELECT @@TRANCOUNT  
  
  -- verify the rows 3 and 4 were inserted  
  SELECT c1 FROM dbo.t1 WITH (SNAPSHOT)   
  ORDER BY c1  
  
  -- catch the arithmetic overflow error  
  BEGIN TRY  
    EXEC dbo.usp_t1 5, 4444444444444  
  END TRY  
  BEGIN CATCH  
    PRINT N'Error occurred: ' + error_message()  
  END CATCH  
  
  -- verify there is still an active transaction  
  SELECT @@TRANCOUNT  
  
  -- verify rows 3 and 4 are still in the table, and row 5 has not been inserted  
  SELECT c1 FROM dbo.t1 WITH (SNAPSHOT)   
  ORDER BY c1  
  
COMMIT  
GO  
  
-- verify we have no active transaction  
SELECT @@TRANCOUNT  
GO  
  
-- verify rows 3 and 4 has been committed  
SELECT c1 FROM dbo.t1  
ORDER BY c1  
GO  
```  
  
 Les messages d'erreur suivants propres aux tables optimisées en mémoire condamnent les transactions. S’ils apparaissent dans l’étendue d’un bloc Atomic, ils entraînent l’abandon de la transaction : 10772, 41301, 41302, 41305, 41325, 41332, 41333 et 41839.  
  
## <a name="session-settings"></a>Paramètres de session  
 Les paramètres de session dans les blocs Atomic sont fixes lorsque la procédure stockée est compilée. Certains paramètres peuvent être spécifiés avec **BEGIN ATOMIC** tandis que les autres paramètres sont toujours fixes avec la même valeur.  
  
 Les options suivantes sont requises avec **BEGIN ATOMIC**:  
  
|Paramètre obligatoire|Description|  
|----------------------|-----------------|  
|**TRANSACTION ISOLATION LEVEL**|Les valeurs prises en charge sont **SNAPSHOT**, **REPEATABLEREAD**et **SERIALIZABLE**.|  
|**LANGUAGE**|Détermine les formats de date et d'heure, et les messages système. Tous les langages et les alias dans [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) sont pris en charge.|  
  
 Les paramètres suivants sont facultatifs :  
  
|Paramètre facultatif|Description|  
|----------------------|-----------------|  
|**DATEFORMAT**|Tous les formats de date [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont pris en charge. Lorsqu'il est spécifié, **DATEFORMAT** remplace le format de date par défaut associé à **LANGUAGE**.|  
|**DATEFIRST**|Lorsqu'il est spécifié, **DATEFIRST** remplace la valeur par défaut associée à **LANGUAGE**.|  
|**DELAYED_DURABILITY**|Les valeurs prises en charge sont **OFF** et **ON**.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Les validations de transactions peuvent avoir une durabilité complète, la durabilité par défaut ou une durabilité retardée. Pour plus d’informations, consultez [Contrôler la durabilité d’une transaction](../../relational-databases/logs/control-transaction-durability.md).|  
  
 Les options SET suivantes ont la même valeur système par défaut pour tous les blocs Atomic de toutes les procédures stockées compilées en mode natif :  
  
|Option SET|Valeur système par défaut pour les blocs Atomic|  
|----------------|--------------------------------------|  
|ANSI_NULLS|ON|  
|ANSI_PADDING|ON|  
|ANSI_WARNING|ON|  
|ARITHABORT|ON|  
|ARITHIGNORE|OFF|  
|CONCAT_NULL_YIELDS_NULL|ON|  
|IDENTITY_INSERT|OFF|  
|NOCOUNT|ON|  
|NUMERIC_ROUNDABORT|OFF|  
|QUOTED_IDENTIFIER|ON|  
|ROWCOUNT|0|  
|TEXTSIZE|0|  
|XACT_ABORT|OFF<br /><br /> Les exceptions qui ne sont pas interceptées entraînent la restauration des blocs Atomic, mais pas l'abandon de la transaction, sauf si l'erreur condamne la transaction.|  
  
## <a name="see-also"></a> Voir aussi  
 [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)  
  
  
