---
title: ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/142018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- ALTER_DATABASE_SCOPED_CONFIGURATION
- ALTER_DATABASE_SCOPED_CONFIGURATION_TSQL
- DATABASE_SCOPED_CONFIGURATION_TSQL
- SCOPED_CONFIGURATION_TSQL
- SCOPED_TSQL
- ALTER_DATABASE_SCOPED_TSQL
- DATABASE_SCOPED_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CONFIGURATION statement
- configuration [SQL Server], ALTER DATABASE SCOPED CONFIGURATION statement
ms.assetid: 63373c2f-9a0b-431b-b9d2-6fa35641571a
caps.latest.revision: 32
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 1fc2b483ff3a3b4a60d02c281041bb403485aaa2
ms.sourcegitcommit: 6fd8a193728abc0a00075f3e4766a7e2e2859139
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="alter-database-scoped-configuration-transact-sql"></a>ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Cette instruction active plusieurs paramètres de configuration de base de données au niveau de **chaque base de données**. Cette instruction est disponible dans [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)] et dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. Ces paramètres sont les suivants :  
  
- Effacer le cache de procédures.  
- Affecter au paramètre MAXDOP une valeur arbitraire (1, 2, etc.) adaptée à la base de données primaire et affecter une autre valeur (par exemple, 0) à toutes les bases de données secondaires utilisées (par exemple, pour les requêtes de rapport).  
- Définir le modèle d’estimation de la cardinalité de l’optimiseur de requête indépendant de la base de données au niveau de compatibilité.  
- Activer ou désactiver la détection de paramètres au niveau de la base de données.
- Activer ou désactiver les correctifs d’optimisation des requêtes au niveau de la base de données.
- Activer ou désactiver le cache d’identité au niveau de la base de données
- Activer ou désactiver un stub de plan compilé à stocker dans le cache lorsqu’un lot est compilé pour la première fois  
- Activer ou désactiver la collecte de statistiques d’exécution pour les modules T-SQL compilés en mode natif.
- Activer ou désactiver les options par défaut « online » pour les instructions DDL qui prennent en charge la syntaxe ONLINE=.
- Activer ou désactiver les options par défaut « resumable » pour les instructions DDL qui prennent en charge la syntaxe RESUMABLE=. 

 ![Icône de lien](../../database-engine/configure-windows/media/topic-link.gif "Icône de lien") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ALTER DATABASE SCOPED CONFIGURATION  
{        
     {  [ FOR SECONDARY] SET <set_options>  }    
}  
| CLEAR PROCEDURE_CACHE  
| SET < set_options >
[;]    
  
< set_options > ::=    
{  
    MAXDOP = { <value> | PRIMARY}    
    | LEGACY_CARDINALITY_ESTIMATION = { ON | OFF | PRIMARY}    
    | PARAMETER_SNIFFING = { ON | OFF | PRIMARY}    
    | QUERY_OPTIMIZER_HOTFIXES = { ON | OFF | PRIMARY}
    | IDENTITY_CACHE = { ON | OFF }
    | OPTIMIZE_FOR_AD_HOC_WORKLOADS = { ON | OFF }
    | XTP_PROCEDURE_EXECUTION_STATISTICS = { ON | OFF } 
    | XTP_QUERY_EXECUTION_STATISTICS = { ON | OFF }    
    | ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED } 
    | ELEVATE_RESUMABLE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }  
}  
```  
  
## <a name="arguments"></a>Arguments  
 
FOR SECONDARY  
 
Spécifie les paramètres des bases de données secondaires (toutes les bases de données secondaires doivent avoir des valeurs identiques).  
  
MAXDOP **=** {\<value> | PRIMARY }  
**\<value>**  
  
Spécifiez le paramètre MAXDOP par défaut qui doit être utilisé pour les instructions. 0 est la valeur par défaut, et indique que la configuration du serveur doit être utilisée. Le paramètre MAXDOP défini au niveau de la base de données remplace (sauf si sa valeur est 0) le paramètre **max degree of parallelism** défini au niveau du serveur par sp_configure. Les indicateurs de requête peuvent tout de même remplacer le paramètre MAXDOP défini au niveau de la base de données afin de configurer les requêtes qui nécessitent un paramétrage différent. Tous ces paramètres sont limités par le paramètre MAXDOP défini pour le groupe de charge de travail.   

Vous pouvez utiliser l'option max degree of parallelism pour limiter le nombre de processeurs à utiliser lors de l'exécution des plans parallèles. SQL Server prend en compte les plans d’exécution parallèle pour les requêtes, les opérations DDL d’index, l’insertion parallèle, la modification de colonne en ligne, la collecte de statistiques parallèle et l’alimentation des curseurs statiques et de jeu de clés.
 
Pour définir cette option au niveau de l’instance, consultez [Configurer l’option de configuration du serveur max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). 

> [!TIP] 
> Pour définir cette option au niveau de la requête, ajoutez [l’indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**.  
  
PRIMARY  
  
Peut uniquement être défini pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que la configuration est celle définie pour la base de données primaire. Si la configuration de la base de données primaire est modifiée, la valeur des bases de données secondaires est modifiée en conséquence, sans que vous ayez à la définir explicitement. **PRIMARY** est le paramètre par défaut des bases de données secondaires.  
  
LEGACY_CARDINALITY_ESTIMATION **=** { ON | **OFF** | PRIMARY }  

Permet de définir le modèle d’estimation de la cardinalité de l’optimiseur de requête sur SQL Server 2012 ou antérieur selon le niveau de compatibilité de la base de données. La valeur par défaut est **OFF**, ce qui définit le modèle d’estimation de la cardinalité de l’optimiseur de requête en fonction du niveau de compatibilité de la base de données. L’utilisation de la valeur **ON** équivaut à activer [l’indicateur de trace 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). 

> [!TIP] 
> Pour définir cette option au niveau de la requête, ajoutez [l’indicateur de requête](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**. Avec [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 et versions ultérieures, pour effectuer cette opération au niveau de la requête, ajoutez [l’indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) **USE HINT** au lieu de l’indicateur de trace. 
  
PRIMARY  
  
Cette valeur est valide uniquement pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que le paramètre du modèle d’estimation de la cardinalité de l’optimiseur de requête de toutes les bases de données secondaires est défini sur la valeur de la base de données primaire. Si la configuration du modèle d’estimation de la cardinalité de l’optimiseur de requête qui est définie dans la base de données primaire est modifiée, la valeur des bases de données secondaires est modifiée en conséquence. **PRIMARY** est le paramètre par défaut des bases de données secondaires.  
  
PARAMETER_SNIFFING **=** { **ON** | OFF | PRIMARY}  

Active ou désactive la [détection de paramètres](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing). La valeur par défaut est ON. Cette propriété est équivalente à l’ [indicateur de trace 4136](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).   

> [!TIP] 
> Pour définir cette option au niveau de la requête, utilisez [l’indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) **OPTIMIZE FOR UNKNOWN**. Avec [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 et versions ultérieures, pour effectuer cette opération au niveau de la requête, vous pouvez également utiliser [l’indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) **USE HINT**. 
  
PRIMARY  
  
Cette valeur est valide uniquement pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que, sur toutes les bases de données secondaires, ce paramètre est défini sur la valeur de la base de données primaire. Si la configuration de la [détection de paramètres](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) est modifiée sur la base de données primaire, la valeur des bases de données secondaires est modifiée en conséquence, sans que vous ayez à la définir explicitement. Il s’agit du paramètre par défaut pour les bases de données secondaires.  
  
QUERY_OPTIMIZER_HOTFIXES **=** { ON | **OFF** | PRIMARY }  

Active ou désactive les correctifs logiciels d’optimisation de requête, quel que soit le niveau de compatibilité de la base de données. La valeur par défaut est **OFF**, laquelle désactive les correctifs logiciels d’optimisation des requêtes qui ont été publiés après l’arrivée du plus haut niveau de compatibilité d’une version donnée (post-RTM). L’utilisation de la valeur **ON** équivaut à activer [l’indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).   

> [!TIP] 
> Pour définir cette option au niveau de la requête, ajoutez [l’indicateur de requête](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**. Avec [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 et versions ultérieures, pour effectuer cette opération au niveau de la requête, ajoutez [l’indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md) USE HINT au lieu de l’indicateur de trace.  
  
PRIMARY  
  
Cette valeur est valide uniquement pour les bases de données secondaires lorsque la base de données est définie sur PRIMARY, et indique que, sur toutes les bases de données secondaires, ce paramètre est défini sur la valeur de la base de données primaire. Si la configuration de la base de données primaire est modifiée, la valeur des bases de données secondaires est modifiée en conséquence, sans que vous ayez à la définir explicitement. Il s’agit du paramètre par défaut pour les bases de données secondaires.  
  
CLEAR PROCEDURE_CACHE  

Efface le contenu du cache de procédures (ou cache du plan) pour la base de données. Vous pouvez exécuter cette commande à la fois sur la base de données primaire et sur les bases de données secondaires.  

IDENTITY_CACHE **=** { **ON** | OFF }  

**S’applique à :** [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Active ou désactive le cache d’identité au niveau de la base de données. La valeur par défaut est **ON**. La mise en cache d’identité est utilisée pour améliorer les performances INSERT sur les tables comprenant des colonnes d’identité. Pour éviter les écarts dans les valeurs des colonnes d’identité si un serveur redémarre de façon inattendue ou bascule vers un serveur secondaire, désactivez l’option IDENTITY_CACHE. Cette option est similaire à [l’indicateur de trace 272](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) existant, sauf qu’elle peut être définie au niveau de la base de données et non uniquement au niveau du serveur.   

> [!NOTE] 
> Cette option peut uniquement être définie sur la valeur PRIMARY. Pour plus d’informations, consultez [Colonnes d’identité](create-table-transact-sql-identity-property.md).  

OPTIMIZE_FOR_AD_HOC_WORKLOADS **=** { ON | **OFF** }  

**S’applique à** : [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)] 

Active ou désactive un stub de plan compilé à stocker dans le cache lorsqu’un lot est compilé pour la première fois. La valeur par défaut est OFF. Une fois que la configuration étendue à la base de données OPTIMIZE_FOR_AD_HOC_WORKLOADS est activée pour une base de données, un stub de plan compilé est stocké dans le cache lorsqu’un lot est compilé pour la première fois. Les stubs de plan ont un encombrement mémoire moins important que celui des plans compilés complets.  Si un lot est compilé ou réexécuté, le stub de plan compilé est supprimé et remplacé par un plan compilé complet.

XTP_PROCEDURE_EXECUTION_STATISTICS  **=** { ON | **OFF** }  

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] 

Active ou désactive la collecte de statistiques d’exécution au niveau du module pour les modules T-SQL compilés en mode natif dans la base de données actuelle. La valeur par défaut est OFF. Les statistiques d’exécution sont disponibles dans [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md).

Les statistiques d’exécution au niveau du module pour les modules T-SQL compilés en mode natif sont collectées si cette option est activée (ON) ou si la collecte des statistiques est activée avec [sp_xtp_control_proc_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).

XTP_QUERY_EXECUTION_STATISTICS  **=** { ON | **OFF** }  

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]

Active ou désactive la collecte de statistiques d’exécution au niveau de l’instruction pour les modules T-SQL compilés en mode natif dans la base de données actuelle. La valeur par défaut est OFF. Les statistiques d’exécution sont disponibles dans [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) et dans le [magasin des requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

Les statistiques d’exécution au niveau de l’instruction pour les modules T-SQL compilés en mode natif sont collectées si cette option est activée (ON) ou si la collecte des statistiques est activée avec [sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

Pour plus d’informations sur la surveillance des performances des modules T-SQL compilés en mode natif, consultez [Surveillance des performances des procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/monitoring-performance-of-natively-compiled-stored-procedures.md).

ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Permet de sélectionner les options destinées à forcer le moteur à élever automatiquement les opérations prises en charge pour une exécution en ligne. La valeur par défaut est OFF, ce qui signifie que les opérations ne sont pas élevées pour une exécution en ligne, sauf si cela est spécifié dans l’instruction. [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) reflète la valeur actuelle de ELEVATE_ONLINE. Ces options s’appliquent uniquement aux opérations généralement prises en charge pour une exécution en ligne.  

FAIL_UNSUPPORTED

Cette valeur élève toutes les opérations DDL prises en charge pour une exécution en ligne (option ONLINE). Les opérations qui ne prennent pas en charge l’exécution en ligne échouent et génèrent un avertissement.

WHEN_SUPPORTED  

Cette valeur élève les opérations qui prennent en charge l’option ONLINE. Les opérations qui ne prennent pas en charge une exécution en ligne sont exécutées en mode hors connexion.

> [!NOTE]
> Vous pouvez remplacer le paramètre par défaut en envoyant une instruction avec l’option ONLINE spécifiée. 
 
ELEVATE_RESUMABLE= { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Permet de sélectionner des options pour forcer le moteur à élever automatiquement les opérations prises en charge pour une exécution pouvant être reprise. La valeur par défaut est OFF, ce qui signifie que les opérations ne sont pas élevées pour une exécution pouvant être reprise, sauf si cela est spécifié dans l’instruction. [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) reflète la valeur actuelle de ELEVATE_RESUMABLE. Ces options s’appliquent uniquement aux opérations qui sont généralement prises en charge pour une exécution pouvant être reprise. 

FAIL_UNSUPPORTED

Cette valeur élève toutes les opérations DDL prises en charge pour une exécution pouvant être reprise (option RESUMABLE). Les opérations qui ne prennent pas en charge l’exécution pouvant être reprise échouent et génèrent un avertissement.

WHEN_SUPPORTED  

Cette valeur élève les opérations qui prennent en charge l’option RESUMABLE. Les opérations qui ne prennent pas en charge l’exécution pouvant être reprise sont exécutées en tant que telles.  

> [!NOTE]
> Vous pouvez remplacer le paramètre par défaut en envoyant une instruction avec l’option RESUMABLE spécifiée. 

##  <a name="Permissions"></a> Permissions  
 Nécessite ALTER ANY DATABASE SCOPE CONFIGURATION   
dans la base de données. Cette autorisation peut être accordée par un utilisateur disposant de l’autorisation CONTROL pour une base de données.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Même si vous pouvez configurer des bases de données secondaires avec des paramètres différents de ceux de la base de données primaire, toutes les bases de données secondaires doivent utiliser la même configuration. Vous ne pouvez pas configurer des paramètres différents pour chaque base de données secondaire.  
  
 L’exécution de cette instruction efface le contenu du cache de procédures de la base de données actuelle, ce qui signifie que toutes les requêtes doivent être recompilées.  
  
 Pour les requêtes de noms en trois parties, les paramètres de connexion de la base de données actuelle sont honorés (autres que ceux des modules SQL tels que les procédures, les fonctions et les déclencheurs, qui sont compilés dans le contexte de base de données actuel). Les options de la base de données dans laquelle ils résident sont donc utilisées.  
  
 L’événement ALTER_DATABASE_SCOPED_CONFIGURATION est ajouté comme un événement DDL qui peut être utilisé pour déclencher un déclencheur DDL. Il s’agit d’un enfant du groupe de déclencheurs ALTER_DATABASE_EVENTS.  
 
 Les paramètres configurés au niveau de la base de données sont reportés avec la base de données. Cela signifie que lorsqu’une base de données est restaurée ou jointe, les paramètres de configuration existants sont conservés.
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
**MAXDOP**  
  
 Les paramètres granulaires peuvent remplacer les paramètres globaux, et le gouverneur de ressources peut limiter tous les autres paramètres MAXDOP.  La logique du paramètre MAXDOP est la suivante :  
  
-   L’indicateur de requête remplace sp_configure et le paramètre défini au niveau de la base de données. Si le groupe de ressources MAXDOP est défini pour le groupe de charge de travail :  
  
    -   Si l’indicateur de requête est défini sur 0, il est remplacé par le paramètre Resource Governor.  
  
    -   Si l’indicateur de requête n’est pas défini sur 0, il est limité par le paramètre Resource Governor.  
  
-   Le paramètre défini au niveau de la base de données (sauf s’il est défini sur 0) remplace le paramètre sp_configure, sauf s’il existe un indicateur de requête et sauf s’il est limité par le paramètre Resource Governor.  
  
-   Le paramètre sp_configure est remplacé par le paramètre Resource Governor.  
  
**QUERY_OPTIMIZER_HOTFIXES**  
  
 Lorsque l’indicateur QUERYTRACEON est utilisé pour activer l’optimiseur de requête hérité ou les correctifs logiciels de l’optimiseur de requête, une condition OR lie l’indicateur de requête et le paramètre défini au niveau de la base de données, ce qui signifie que si l’un d’eux est activé, les options s’appliquent.  
  
**GeoDR**  
  
 Les bases de données secondaires accessibles en lecture (par exemple, les groupes de disponibilité AlwaysOn et la géoréplication) utilisent la valeur de la base de données secondaire en vérifiant l’état de la base de données. Même si la recompilation ne se produit pas lors du basculement et même si, techniquement, la nouvelle base de données primaire comprend des requêtes qui utilisent les paramètres des bases de données secondaires, l’idée est que le paramètre entre les bases de données primaires et secondaires varient uniquement lorsque la charge de travail est différente, et donc, que les requêtes mises en cache utilisent les paramètres optimaux, tandis que les nouvelles requêtes choisissent les nouveaux paramètres qui leur conviennent.  
  
**DacFx**  
  
 Étant donné qu’ALTER DATABASE SCOPED CONFIGURATION est une nouvelle fonctionnalité d’Azure SQL Database et de SQL Server 2016 qui affecte le schéma de base de données, les exportations du schéma (avec ou sans données) ne peuvent pas être importées dans une version antérieure de SQL Server, comme [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou [!INCLUDE[ssSQLv14](../../includes/sssqlv14-md.md)]. Par exemple, une exportation vers un [DACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_3) ou un [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) à partir d’une base de données [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] ayant utilisé cette nouvelle fonctionnalité ne peut pas être importée dans un serveur de niveau inférieur.  

**ELEVATE_ONLINE** 

Cette option s’applique uniquement aux instructions DDL qui prennent en charge la syntaxe WITH(ONLINE=). Les index XML ne sont pas affectés. 

**ELEVATE_RESUMABLE**

Cette option s’applique uniquement aux instructions DDL qui prennent en charge la syntaxe WITH(ONLINE=). Les index XML ne sont pas affectés. 

  
## <a name="metadata"></a>Métadonnées  

La vue système [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) fournit des informations sur les configurations étendues à une base de données. Les options de configuration définies au niveau de la base de données s’affichent dans sys.database_scoped_configurations parce qu’elles remplacent les paramètres par défaut définis au niveau du serveur. La vue système [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) affiche uniquement les paramètres définis au niveau du serveur.  
  
## <a name="examples"></a>Exemples  
Ces exemples illustrent l’utilisation de ALTER DATABASE SCOPED CONFIGURATION.  
  
### <a name="a-grant-permission"></a>A. Accorder une autorisation  

Cet exemple accorde l’autorisation nécessaire à l’exécution d’ALTER DATABASE SCOPED CONFIGURATION     
à l’utilisateur [Joe].  
  
```sql  
GRANT ALTER ANY DATABASE SCOPED CONFIGURATION to [Joe] ;  
```  
  
### <a name="b-set-maxdop"></a>B. Définir MAXDOP  

Cet exemple définit MAXDOP = 1 pour une base de données primaire et MAXDOP = 4 pour la base de données secondaire dans un scénario de géoréplication.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 1 ;  
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP=4 ;  
```  
  
Cet exemple définit MAXDOP de sorte que sa valeur soit la même pour la base de données primaire et pour la base de données secondaire dans un scénario de géoréplication.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP=PRIMARY ;
```  
  
### <a name="c-set-legacycardinalityestimation"></a>C. Définir LEGACY_CARDINALITY_ESTIMATION  

Cet exemple définit LEGACY_CARDINALITY_ESTIMATION sur ON pour une base de données secondaire dans un scénario de géoréplication.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION=ON ;  
```  
  
Cet exemple définit LEGACY_CARDINALITY_ESTIMATION de la même manière pour la base de données primaire et pour une base de données secondaire dans un scénario de géoréplication.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION=PRIMARY ;  
```  
  
### <a name="d-set-parametersniffing"></a>D. Définir PARAMETER_SNIFFING  

Cet exemple définit PARAMETER_SNIFFING sur OFF pour une base de données primaire dans un scénario de géoréplication.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION SET PARAMETER_SNIFFING =OFF ;  
```  
  
Cet exemple définit PARAMETER_SNIFFING sur OFF pour une base de données primaire dans un scénario de géoréplication.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING=OFF ;  
```  
  
Cet exemple définit PARAMETER_SNIFFING de la même manière pour la base de données primaire et pour une base de données secondaire dans un scénario de géoréplication.  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING=PRIMARY ;  
```  
  
### <a name="e-set-queryoptimizerhotfixes"></a>E. Définir QUERY_OPTIMIZER_HOTFIXES  

Cet exemple définit QUERY_OPTIMIZER_HOTFIXES sur ON pour la base de données primaire dans un scénario de géoréplication.  

```sql  
ALTER DATABASE SCOPED CONFIGURATION SET QUERY_OPTIMIZER_HOTFIXES=ON ;  
```  
  
### <a name="f-clear-procedure-cache"></a>F. Effacer le contenu du cache de procédures  

Cet exemple efface le contenu du cache de procédures (possible uniquement pour la base de données primaire).  
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE ;  
```  

### <a name="g-set-identitycache"></a>G. Définir IDENTITY_CACHE

**S’applique à** : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)] (fonctionnalité en préversion publique) 

Cet exemple désactive le cache d’identité.

```sql 
ALTER DATABASE SCOPED CONFIGURATION SET IDENTITY_CACHE=OFF ; 
```

### <a name="h-set-optimizeforadhocworkloads"></a>H. Définir OPTIMIZE_FOR_AD_HOC_WORKLOADS

**S’applique à** : [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 

Cet exemple permet à un stub de plan compilé d’être stocké dans le cache lorsqu’un lot est compilé pour la première fois.

```sql 
ALTER DATABASE SCOPED CONFIGURATION SET OPTIMIZE_FOR_AD_HOC_WORKLOADS = ON;
```

### <a name="i--set-elevateonline"></a>I.  Définir ELEVATE_ONLINE 

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)
 
Cet exemple définit ELEVATE_ONLINE sur FAIL_UNSUPPORTED.  tsqlCopy 

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_ONLINE=FAIL_UNSUPPORTED ;
```  

### <a name="j-set-elevateresumable"></a>J. Définir ELEVATE_RESUMABLE 

**S’applique à** : [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] (fonctionnalité en préversion publique)

Cet exemple définit ELEVEATE_RESUMABLE sur WHEN_SUPPORTED.  tsqlCopy 

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_RESUMABLE=WHEN_SUPPORTED ;  
``` 

## <a name="additional-resources"></a>Ressources supplémentaires

### <a name="maxdop-resources"></a>Ressources MAXDOP 
* [Degré de parallélisme](../../relational-databases/query-processing-architecture-guide.md#DOP)
* [Recommandations et directives pour l’option de configuration « max degree of parallelism » dans SQL Server](https://support.microsoft.com/en-us/kb/2806535) 

### <a name="legacycardinalityestimation-resources"></a>Ressources LEGACY_CARDINALITY_ESTIMATION    
* [Estimation de la cardinalité (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md)
* [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) (Optimisation de vos plans de requête avec l’estimateur de cardinalité SQL Server 2014)

### <a name="parametersniffing-resources"></a>Ressources PARAMETER_SNIFFING    
* [Détection de paramètres](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing)
* [I smell a parameter!](https://blogs.msdn.microsoft.com/queryoptteam/2006/03/31/i-smell-a-parameter/)

### <a name="queryoptimizerhotfixes-resources"></a>Ressources QUERY_OPTIMIZER_HOTFIXES    
* [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
* [Modèle de service de l’indicateur de trace 4199 pour les correctifs de l’optimiseur de requête SQL Server](https://support.microsoft.com/en-us/kb/974006)

### <a name="elevateonline-resources"></a>Ressources ELEVATE_ONLINE 

- [Instructions pour les opérations d’index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md) 

### <a name="elevateresumable-resources"></a>Ressources ELEVATE_RESUMABLE 

- [Instructions pour les opérations d’index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md) 
 
## <a name="more-information"></a>Informations complémentaires  
 [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)   
 [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [Affichages catalogue de bases de données et de fichiers](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Options de configuration du serveur](../../database-engine/configure-windows/server-configuration-options-sql-server.md) [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)  
 
