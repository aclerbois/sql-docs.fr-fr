---
title: Activer et désactiver la capture de données modifiées (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: track-changes
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], enabling tables
- change data capture [SQL Server], enabling databases
- change data capture [SQL Server], disabling databases
- change data capture [SQL Server], disabling tables
ms.assetid: b741894f-d267-4b10-adfe-cbc14aa6caeb
caps.latest.revision: 13
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 9fd8c6016a58de25e0495d8195340e8d0f2f0732
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enable-and-disable-change-data-capture-sql-server"></a>Activer et désactiver la capture de données modifiées (SQL Server)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit l'activation et la désactivation de la capture de données modifiées pour une base de données et une table.  
  
## <a name="enable-change-data-capture-for-a-database"></a>Activer la capture des données modifiées pour une base de données  
 Avant qu’une instance de capture puisse être créée pour des tables individuelles, un membre du rôle serveur fixe **sysadmin** doit d’abord activer la base de données pour la capture des données modifiées. Cela se fait en exécutant la procédure stockée [sys.sp_cdc_enable_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md) dans le contexte de la base de données. Pour déterminer si une base de données est déjà activée, interrogez la colonne **is_cdc_enabled** dans l’affichage catalogue **sys.databases**.  
  
 Quand une base de données est activée pour la capture des données modifiées, le schéma **cdc** , l’utilisateur **cdc** , les tables de métadonnées et d’autres objets système sont créés pour la base de données. Le schéma **cdc** contient les tables de métadonnées de capture des données modifiées et, une fois que les tables sources sont activées pour la capture des données modifiées, les tables de modifications individuelles servent de référentiel pour les données modifiées. Le schéma **cdc** contient également les fonctions système associées utilisées pour rechercher les données modifiées.  
  
 La capture des données modifiées requiert une utilisation exclusive du schéma **cdc** et de l’utilisateur **cdc** . Si un schéma ou un utilisateur de base de données nommé *cdc* existe actuellement dans une base de données, celle-ci ne peut pas être activée pour la capture des données modifiées tant que le schéma et/ou l'utilisateur n'a pas été supprimé ou renommé.  
  
 Pour un exemple d'activation de base de données, consultez le modèle Activer la base de données pour la capture de données modifiées.  
  
> [!IMPORTANT]  
>  Pour repérer les modèles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], allez dans **Afficher**, cliquez sur **Explorateur de modèles**, puis sélectionnez **Modèles SQL Server**. **Capture de données modifiées** est un sous-dossier. Sous ce dossier, vous trouverez tous les modèles auxquels il est fait référence dans cette rubrique. On trouve également une icône **Explorateur de modèles** dans la barre d'outils [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
```sql  
-- ====  
-- Enable Database for CDC template   
-- ====  
USE MyDB  
GO  
EXEC sys.sp_cdc_enable_db  
GO  
```  
  
## <a name="disable-change-data-capture-for-a-database"></a>Désactiver la capture des données modifiées pour une base de données  
 Un membre du rôle serveur fixe **sysadmin** peut exécuter la procédure stockée [sys.sp_cdc_disable_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) dans le contexte de la base de données pour désactiver la capture des données modifiées pour une base de données. Il n'est pas nécessaire de désactiver individuellement les tables avant de désactiver la base de données. La désactivation de la base de données supprime toutes les métadonnées de capture de données modifiées associées, y compris l’utilisateur et le schéma **cdc** , ainsi que les travaux de capture des données modifiées. Toutefois, tous les rôles de régulation créés par capture de données modifiées ne seront pas supprimés automatiquement et doivent être supprimés explicitement. Pour déterminer si une base de données est activée, interrogez la colonne **is_cdc_enabled** dans l’affichage catalogue sys.databases.  
  
 Si une base de données sur laquelle la capture de données modifiées est activée est supprimée, les travaux de capture de données modifiées sont également et automatiquement supprimés.  
  
 Pour un exemple de désactivation d'une base de données, consultez le modèle Désactiver la base de données pour la capture de données modifiées.  
  
> [!IMPORTANT]  
>  Pour repérer les modèles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], allez dans **Afficher**, cliquez sur **Explorateur de modèles**, puis sur **Modèles SQL Server**. **Capture de données modifiées** est un sous-dossier où vous trouverez tous les modèles qui sont référencés dans cette rubrique. On trouve également une icône **Explorateur de modèles** dans la barre d'outils [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
```sql  
-- =======  
-- Disable Database for Change Data Capture template   
-- =======  
USE MyDB  
GO  
EXEC sys.sp_cdc_disable_db  
GO  
```  
  
## <a name="enable-change-data-capture-for-a-table"></a>Activer la capture des données modifiées pour une table  
 Une fois qu’une base de données a été activée pour la capture des données modifiées, les membres du rôle de base de données fixe **db_owner** peuvent créer une instance de capture pour les tables sources individuelles à l’aide de la procédure stockée **sys.sp_cdc_enable_table**. Pour déterminer si une table source a été déjà activée pour la capture des données modifiées, examinez la colonne is_tracked_by_cdc dans l’affichage catalogue **sys.tables** .  
  
 Les options suivantes peuvent être spécifiées lorsque vous créez une instance de capture :  
  
 **Les colonnes de la table source à capturer**.  
  
 Par défaut, toutes les colonnes de la table source sont identifiées comme colonnes capturées. Si un suivi concerne uniquement un sous-ensemble de colonnes, par exemple pour des raisons de confidentialité ou de performance, utilisez le paramètre *@captured_column_list* pour spécifier ce sous-ensemble.  
  
 **Groupe de fichiers devant contenir la table de modifications.**  
  
 Par défaut, la table de modifications se situe dans le groupe de fichiers par défaut de la base de données. Les propriétaires de base de données qui souhaitent contrôler le placement des tables de modifications individuelles peuvent utiliser le paramètre *@filegroup_name* pour spécifier un groupe de fichiers particulier pour la table de modifications associée à l’instance de capture. Le groupe de fichiers nommé doit déjà exister. En règle générale, il est recommandé de placer des tables de modifications dans un groupe de fichiers séparé des tables sources. Consultez le modèle **Activer une table en spécifiant l’option Filegroup** pour un exemple montrant l’utilisation du paramètre *@filegroup_name* .  
  
```sql  
-- =========  
-- Enable a Table Specifying Filegroup Option Template  
-- =========  
USE MyDB  
GO  
  
EXEC sys.sp_cdc_enable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@role_name     = N'MyRole',  
@filegroup_name = N'MyDB_CT',  
@supports_net_changes = 1  
GO  
```  
  
 **Rôle pour contrôler l'accès à une table de modifications.**  
  
 L'objectif du rôle nommé consiste à contrôler l'accès aux données modifiées. Le rôle spécifié peut être un rôle serveur fixe existant ou un rôle de base de données. Si le rôle spécifié n'existe pas déjà, un rôle de base de données portant ce nom est automatiquement créé. Les membres du rôle **sysadmin** ou **db_owner** disposent d’un accès complet aux données des tables de modification. Tous les autres utilisateurs doivent disposer de l'autorisation SELECT pour toutes les colonnes capturées de la table source. De plus, quand un rôle est spécifié, les utilisateurs qui ne sont pas membres du rôle **sysadmin** ou **db_owner** doivent également être membres du rôle spécifié.  
  
 Si vous ne souhaitez pas utiliser de rôle de régulation, affectez explicitement la valeur NULL au paramètre *@role_name* . Consultez le modèle **Activer une table sans utiliser un rôle de régulation** pour obtenir un exemple d’activation d’une table sans un rôle de régulation.  
  
```sql  
-- =========  
-- Enable a Table Without Using a Gating Role template   
-- =========  
USE MyDB  
GO  
EXEC sys.sp_cdc_enable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@role_name     = NULL,  
@supports_net_changes = 1  
GO  
  
```  
  
 **Fonction pour interroger les modifications nettes.**  
  
 Une instance de capture inclura toujours une fonction table pour retourner toutes les entrées de table de modifications qui ont eu lieu au cours d'un intervalle défini. On nomme cette fonction en ajoutant le nom de l'instance de capture à "cdc.fn_cdc_get_all_changes_". Pour plus d’informations, consultez [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md).  
  
 Si le paramètre *@supports_net_changes* a la valeur 1, une fonction de suivi des modifications nettes est également générée pour l’instance de la capture. Cette fonction retourne une seule modification pour chaque ligne distincte modifiée dans l'intervalle spécifié dans l'appel. Pour plus d’informations, consultez [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md).  
  
 Pour prendre en charge les requêtes de modifications nettes, la table source doit disposer d'une clé primaire ou d'un index unique permettant d'identifier sans ambiguïté les lignes. Si un index unique est utilisé, le nom de l’index doit être spécifié à l’aide du paramètre *@index_name* . Les colonnes définies dans la clé primaire ou l'index unique doivent être incluses dans la liste des colonnes sources à capturer.  
  
 Consultez le modèle **Activer une table pour tous et pour les requêtes de modifications nettes** pour un exemple montrant la création d’une instance de capture avec les deux fonctions de requête.  
  
```sql  
-- =============  
-- Enable a Table for All and Net Changes Queries template   
-- =============  
USE MyDB  
GO  
EXEC sys.sp_cdc_enable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@role_name     = N'MyRole',  
@supports_net_changes = 1  
GO  
```  
  
> [!NOTE]  
>  Si la capture de données modifiées est activée sur une table avec une clé primaire existante, et que le paramètre *@index_name* n’est pas utilisé pour identifier un autre index unique, la fonctionnalité de capture de données modifiées utilisera la clé primaire. Les modifications ultérieures à la clé primaire ne seront pas autorisées sans désactivation préalable de la capture de données modifiées pour la table. Cela est vrai, que la prise en charge des requêtes de modifications nettes ait été demandée ou non lors de la configuration de la capture de données. Si une table ne contient pas de clé primaire au moment de son activation pour la capture de données modifiées, tout ajout ultérieur de clé primaire sera ignoré par la capture des données modifiées. Étant donné que la capture de données modifiées n'utilisera pas de clé primaire créée une fois la table activée, la clé et les colonnes clés peuvent être supprimées sans restrictions.  
  
## <a name="disable-change-data-capture-for-a-table"></a>Désactiver la capture des données modifiées pour une table  
 Les membres du rôle de base de données fixe **db_owner** peuvent supprimer une instance de capture pour les tables sources individuelles à l’aide de la procédure stockée **sys.sp_cdc_disable_table**. Pour déterminer si une table source est actuellement activée pour la capture des données modifiées, examinez la colonne **is_tracked_by_cdc** dans l’affichage catalogue **sys.tables** . S'il n'y a pas de tables activées pour la base de données après la désactivation, les travaux de capture de données modifiées sont également supprimés.  
  
 Si une table pour laquelle la capture de données modifiées est activée est supprimée, les métadonnées de capture de données modifiées associées à la table sont automatiquement supprimées.  
  
 Pour un exemple de désactivation de table, consultez le modèle Désactiver une instance de capture pour une table.  
  
```sql  
-- =====  
-- Disable a Capture Instance for a Table template   
-- =====  
USE MyDB  
GO  
EXEC sys.sp_cdc_disable_table  
@source_schema = N'dbo',  
@source_name   = N'MyTable',  
@capture_instance = N'dbo_MyTable'  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Utiliser les données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Administrer et surveiller la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
