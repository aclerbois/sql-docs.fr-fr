---
title: Configurer l’option de configuration de serveur max degree of parallelism | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- parallel queries [SQL Server]
- processors [SQL Server], parallel queries
- number of processors for parallel queries
- max degree of parallelism option
- MaxDop
ms.assetid: 86b65bf1-a6a1-4670-afc0-cdfad1558032
caps.latest.revision: 33
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: edafaecb6bdc11622c7394e3f4a35410c9525ac4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-the-max-degree-of-parallelism-server-configuration-option"></a>Configurer l'option de configuration de serveur max degree of parallelism
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment configurer l’option de configuration de serveur  **max degree of parallelism (MAXDOP)** dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Lorsqu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sur un ordinateur doté de plusieurs microprocesseurs ou UC, elle détecte le degré de parallélisme optimal, qui correspond au nombre de processeurs employés pour exécuter une seule instruction, pour chaque exécution d'un plan parallèle. Vous pouvez utiliser l'option **max degree of parallelism** pour limiter le nombre de processeurs à utiliser lors de l'exécution des plans parallèles. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en compte les plans d’exécution parallèle pour les requêtes, les opérations du langage de définition de données (DDL) d’index, l’insertion parallèle, la modification de colonne en ligne, la collecte de statistiques parallèle et l’alimentation des curseurs statiques et de jeux de clés.

##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Si l'option affinity mask n'est pas définie sur la valeur par défaut, il est possible qu'elle limite le nombre de processeurs disponibles pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur les systèmes de multitraitement symétrique (SMP, symmetric multiprocessing).  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Pour permettre au serveur de déterminer le degré maximal de parallélisme, définissez cette option sur 0, c'est-à-dire la valeur par défaut. Attribuer la valeur 0 à l'option Degré maximal de parallélisme permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'utiliser tous les processeurs disponibles, dans la limite de 64. Pour supprimer la génération de plans parallèles, attribuez la valeur 1 à l'option **Degré maximal de parallélisme** . Affectez la valeur à un nombre compris entre 1 et 32 767 pour spécifier le nombre maximal de noyaux de processeurs qui peuvent être utilisés par une seule exécution. Si une valeur supérieure au nombre de processeurs disponibles est spécifiée, le nombre réel de processeurs disponibles est utilisé. Si votre ordinateur est équipé d'un seul processeur, la valeur de l'option **max degree of parallelism** est ignorée.  
  
-   Vous pouvez remplacer la valeur de l'option max degree of parallelism dans les requêtes en spécifiant l'indicateur de requête MAXDOP dans l'instruction de requête. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
-   Les opérations d'index destinées à créer ou à recréer un index, voire à supprimer un index cluster, peuvent nécessiter une quantité importante de ressources. Vous pouvez remplacer la valeur de l'option max degree of parallelism pour les opérations d'index en spécifiant l'option d'index MAXDOP dans l'instruction d'index. La valeur de MAXDOP est appliquée à l'instruction au moment de son exécution et n'est pas stockée dans les métadonnées de l'index. Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
-   En plus des requêtes et des opérations d'index, cette option gère également le parallélisme de DBCC CHECKTABLE, DBCC CHECKDB et DBCC CHECKFILEGROUP. Vous pouvez désactiver les plans d'exécution parallèle pour ces instructions en utilisant l'indicateur de trace 2528. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

###  <a name="Guidelines"></a> Instructions  
Utilisez les directives suivantes quand vous configurez la valeur de configuration de serveur **max degree of parallelism** :

||||
|----------------|-----------------|-----------------|
|Serveur avec un seul nœud NUMA|Moins de 8 processeurs logiques|Conserver MAXDOP à une valeur égale ou inférieure au nombre de processeurs logiques|
|Serveur avec un seul nœud NUMA|Supérieur à 8 processeurs logiques|Conservez MAXDOP à 8|
|Serveur avec plusieurs nœuds NUMA|Moins de 8 processeurs logiques par nœud NUMA|Conservez MAXDOP à une valeur égale ou inférieure au nombre de processeurs logiques par nœud NUMA|
|Serveur avec plusieurs nœuds NUMA|Supérieur à 8 processeurs logiques par nœud NUMA|Conservez MAXDOP à 8|
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>Pour configurer l'option max degree of parallelism  
  
1.  Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Avancé** .  
  
3.  Dans la zone **Degré maximal de parallélisme** , sélectionnez le nombre maximal de processeurs à utiliser au cours de l'exécution d'un plan parallèle.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-max-degree-of-parallelism-option"></a>Pour configurer l'option max degree of parallelism  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `max degree of parallelism` la valeur `8`.  
  
```sql  
USE AdventureWorks2012 ;  
GO   
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
EXEC sp_configure 'max degree of parallelism', 8;  
GO  
RECONFIGURE WITH OVERRIDE;  
GO  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="FollowUp"></a> Suivi : Après avoir configuré l'option max degree of parallelism  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a> Voir aussi  
 [affinity mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md) [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKFILEGROUP &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkfilegroup-transact-sql.md)   
 [Configurer des opérations d'index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md)   
 [Indicateurs de requête & #40 ; Transact-SQL & #41 ; ](../../t-sql/queries/hints-transact-sql-query.md) [Définir les options d’index](../../relational-databases/indexes/set-index-options.md)  
 [Recommandations et directives pour l’option de configuration « max degree of parallelism » dans SQL Server](http://support.microsoft.com/help/2806535)
  
  
