---
title: Instructions SET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SET
- SET_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ISO SET statements
- queries [SQL Server], executing
- dates [SQL Server], SET statements
- time [SQL Server], SET statements
- SET statement, about SET statement
- SET statement
- statistical information [SQL Server], SET statements
- locking [SQL Server], SET statements
ms.assetid: f7e107f8-0fcf-408b-b30f-da2323eeb714
caps.latest.revision: 38
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: edd068ae8675f5af4130c0a802d65b2550d967e8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-statements-transact-sql"></a>Instructions SET (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le langage de programmation [!INCLUDE[tsql](../../includes/tsql-md.md)] fournit plusieurs instructions SET qui modifient la façon dont la session en cours gère des informations spécifiques. Les instructions SET sont regroupées selon les catégories indiquées dans le tableau suivant :  
  
 Pour plus d’informations sur la définition de variables locales à l’aide de l’instruction SET, consultez [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
|Catégorie|Instructions|  
|--------------|----------------|  
|Instructions de date et heure|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)<br /><br /> [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|  
|Instructions de verrouillage|[SET DEADLOCK_PRIORITY](../../t-sql/statements/set-deadlock-priority-transact-sql.md)<br /><br /> [SET LOCK_TIMEOUT](../../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|Instructions diverses|[SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)<br /><br /> [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)<br /><br /> [SET FIPS_FLAGGER](../../t-sql/statements/set-fips-flagger-transact-sql.md)<br /><br /> [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md)<br /><br /> [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)<br /><br /> [SET OFFSETS](../../t-sql/statements/set-offsets-transact-sql.md)<br /><br /> [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)|  
|Instructions d'exécution de requêtes|[SET ARITHABORT](../../t-sql/statements/set-arithabort-transact-sql.md)<br /><br /> [SET ARITHIGNORE](../../t-sql/statements/set-arithignore-transact-sql.md)<br /><br /> [SET FMTONLY](../../t-sql/statements/set-fmtonly-transact-sql.md)<br /><br /> Remarque : [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]<br /><br /> [SET NOCOUNT](../../t-sql/statements/set-nocount-transact-sql.md)<br /><br /> [SET NOEXEC](../../t-sql/statements/set-noexec-transact-sql.md)<br /><br /> [SET NUMERIC_ROUNDABORT](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)<br /><br /> [SET PARSEONLY](../../t-sql/statements/set-parseonly-transact-sql.md)<br /><br /> [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)<br /><br /> [SET ROWCOUNT](../../t-sql/statements/set-rowcount-transact-sql.md)<br /><br /> [SET TEXTSIZE](../../t-sql/statements/set-textsize-transact-sql.md)|  
|Instructions se rapportant aux paramètres ISO|[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_OFF](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)<br /><br /> [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)<br /><br /> [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)<br /><br /> [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)|  
|Instructions se rapportant aux statistiques|[SET FORCEPLAN](../../t-sql/statements/set-forceplan-transact-sql.md)<br /><br /> [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md)<br /><br /> [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md)<br /><br /> [SET SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md)<br /><br /> [SET STATISTICS IO](../../t-sql/statements/set-statistics-io-transact-sql.md)<br /><br /> [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md)<br /><br /> [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)<br /><br /> [SET STATISTICS TIME](../../t-sql/statements/set-statistics-time-transact-sql.md)|  
|Instructions se rapportant aux transactions|[SET IMPLICIT_TRANSACTIONS](../../t-sql/statements/set-implicit-transactions-transact-sql.md)<br /><br /> [SET REMOTE_PROC_TRANSACTIONS](../../t-sql/statements/set-remote-proc-transactions-transact-sql.md)<br /><br /> [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)<br /><br /> [SET XACT_ABORT](../../t-sql/statements/set-xact-abort-transact-sql.md)|  
  
## <a name="considerations-when-you-use-the-set-statements"></a>Remarques sur l'utilisation des instructions SET  
  
-   Toutes les instructions SET sont appliquées lors de l'exécution, à l'exception des instructions SET FIPS_FLAGGER, SET OFFSETS, SET PARSEONLY et SET QUOTED_IDENTIFIER. Ces instructions sont implémentées au moment de l'analyse.  
  
-   Si une instruction SET est exécutée dans une procédure stockée ou un déclencheur, la valeur de l'option SET est rétablie une fois que le contrôle est renvoyé par la procédure stockée ou le déclencheur. De même, si une instruction SET est spécifiée dans une chaîne SQL dynamique exécutée à l’aide de **sp_executesql** ou de EXECUTE, la valeur de l’option SET est rétablie une fois que le contrôle est renvoyé du traitement spécifié dans la chaîne SQL dynamique.  
  
-   Les procédures stockées sont exécutées avec les valeurs SET spécifiées au moment de l'exécution, sauf dans le cas de SET ANSI_NULLS et SET QUOTED_IDENTIFIER. Les procédures stockées spécifiant SET ANSI_NULLS ou SET QUOTED_IDENTIFIER utilisent les valeurs spécifiées au moment de la création de ces procédures stockées. Toute option SET utilisée dans une procédure stockée est ignorée.  
  
-   Le paramètre **user options** de la procédure **sp_configure** tient compte des paramètres définis à l’échelle du serveur et agit sur plusieurs bases de données à la fois. Son fonctionnement est identique à celui d'une instruction SET explicite, mais il prend effet lors de la connexion.  
  
-   Les paramètres de base de données (définis à l'aide de ALTER DATABASE) sont valides uniquement au niveau de la base de données et prennent effet seulement s'ils sont explicitement définis. Les paramètres de base de données ont priorité sur les valeurs des options de l’instance définies à l’aide de **sp_configure**.  
  
-   Pour toutes les instructions SET ayant des valeurs ON ou OFF, vous pouvez spécifier l'une ou l'autre de ces valeurs pour plusieurs options SET en même temps.  
  
    > [!NOTE]  
    >  Ceci ne s'applique pas aux statistiques liées aux options SET.  
  
     Par exemple, `SET QUOTED_IDENTIFIER, ANSI_NULLS ON` affecte la valeur ON à QUOTED_IDENTIFIER et à ANSI_NULLS.  
  
-   Les paramètres de l'instruction SET sont prioritaires sur les paramètres des options de base de données équivalents et définis à l'aide de ALTER DATABASE. Ainsi, la valeur spécifiée dans une instruction SET ANSI_NULLS se substitue à celle du paramètre de base de données pour ANSI_NULLs. De plus, certains paramètres de connexion sont activés (ON) automatiquement quand un utilisateur se connecte à une base de données, sur la base des valeurs activées au préalable à l’aide de la procédure **sp_configure user options** ou sur la base des valeurs applicables à toutes les connexions ODBC et OLE DB.  
  
-   Les instructions ALTER, CREATE et DROP DATABASE ne tiennent pas compte du paramètre SET LOCK_TIMEOUT.  
  
-   Lorsqu'une instruction SET globale ou contextuelle (par exemple, SET ANSI_DEFAULTS) définit plusieurs options, l'émission de l'instruction SET contextuelle rétablit les anciennes valeurs de toutes les options affectées par cette instruction SET contextuelle. Si une option SET particulière affectée par une instruction SET contextuelle est explicitement définie après l'émission de l'instruction SET contextuelle, ses paramètres remplacent les paramètres correspondants de l'instruction contextuelle.  
  
-   Lorsque vous utilisez des traitements d'instructions, le contexte de la base de données est déterminé par le traitement établi à l'aide de l'instruction USE. Les requêtes appropriées et toutes les instructions qui sont exécutées en dehors d'une procédure stockée et qui sont regroupées dans des traitements héritent des valeurs des options de la base de données et de la connexion établie à l'aide de l'instruction USE.  
  
-   Les requêtes MARS (Multiple Active Result Set) partagent un état global contenant les paramètres des options SET de la session la plus récente. Chaque requête qui s'exécute peut modifier les options SET. Ces modifications sont spécifiques au contexte de la requête dans lequel elles sont définies et n'ont pas d'incidence sur les autres requêtes MARS simultanées. Une fois l'exécution de la requête terminée, les nouvelles options SET sont toutefois copiées dans l'état de session global. Ainsi, les nouvelles requêtes qui s'exécutent dans la même session après ces modifications utilisent les nouvelles valeurs des options SET.  
  
-   Lorsqu'une procédure stockée est exécutée à partir d'un traitement ou d'une autre procédure stockée, elle est exécutée en fonction des valeurs des options actuellement en vigueur dans la base de données contenant la procédure stockée. Par exemple, si la procédure stockée **db1.dbo.sp1** appelle la procédure stockée **db2.dbo.sp2**, la procédure **sp1** est exécutée d’après le niveau de compatibilité actuellement défini dans la base de données **db1**, tandis que la procédure stockée **sp2** est exécutée d’après le niveau de compatibilité actuellement en vigueur dans la base de données **db2**.  
  
-   Quand une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] fait référence à des objets résidant dans plusieurs bases de données, le contexte de la base de données active et celui de la connexion actuelle s’appliquent à cette instruction. Dans ce cas, si l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] fait partie d'un traitement, le contexte de la connexion actuelle est la base de données définie par l'instruction USE ; si l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est dans une procédure stockée, le contexte de la connexion est la base de données contenant cette procédure stockée.  
  
-   Lorsque vous créez et que vous modifiez des index sur des colonnes calculées ou des vues indexées, les options SET ARITHABORT, CONCAT_NULL_YIELDS_NULL, QUOTED_IDENTIFIER, ANSI_NULLS, ANSI_PADDING et ANSI_WARNINGS doivent être activées (valeur ON). L'option NUMERIC_ROUNDABORT doit être désactivée (OFF).  
  
     Si l'une de ces options n'est pas définie avec les valeurs requises, les actions INSERT, UPDATE, DELETE, DBCC CHECKDB et DBCC CHECKTABLE sur les vues indexées ou les tables comportant des index sur des colonnes calculées échouent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déclenche une erreur et affiche la liste des options dont les valeurs sont incorrectes. Par ailleurs, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite les instructions SELECT sur ces tables ou ces vues indexées comme si les index sur les colonnes calculées ou sur les vues n'existaient pas.  
  
  
