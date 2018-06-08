---
title: Gérer des FileTables | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
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
- FileTables [SQL Server], security
- FileTables [SQL Server], managing access
ms.assetid: 93af982c-b4fe-4be0-8268-11f86dae27e1
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cf899960d8e3025c9c7218990260fcbcd1394c87
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-filetables"></a>Gérer des FileTables
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Décrit les tâches d'administration courantes permettant de gérer des FileTables.  
  
##  <a name="HowToEnumerate"></a> Procédure : obtenir une liste de FileTables et d'objets connexes  
 Pour obtenir une liste de FileTables, interrogez l'un des affichages catalogue suivants :  
  
-   [sys.filetables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filetables-transact-sql.md)  
  
-   [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) (vérifiez la valeur de la colonne **is_filetable**)  
  
```sql  
SELECT * FROM sys.filetables;  
GO  
  
SELECT * FROM sys.tables WHERE is_filetable = 1;  
GO  
```  
  
 Pour obtenir une liste des objets définis par le système créés avec les FileTables associés, interrogez l’affichage catalogue [sys.filetable_system_defined_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filetable-system-defined-objects-transact-sql.md).  
  
```sql  
SELECT object_id, OBJECT_NAME(object_id) AS 'Object Name'  
FROM sys.filetable_system_defined_objects;  
GO  
```  
  
##  <a name="BasicsDisabling"></a> Désactivation et réactivation de l'accès non transactionnel au niveau de la base de données  
 Pour acquérir l'accès exclusif qui est obligatoire pour certaines tâches d'administration, vous devrez peut-être désactiver temporairement l'accès non transactionnel.  
  
 **Comportement de l'instruction ALTER DATABASE lors de la modification du niveau d'accès non transactionnel**  
  
-   Lorsque vous affectez à l'accès non transactionnel la valeur READ_ONLY ou OFF, la commande ALTER DATABASE ne redonne pas le contrôle à l'utilisateur tant que des descripteurs de fichiers ouverts sont en conflit avec l'opération demandée. Les descripteurs de fichiers qui sont en conflit avec cette opération sont notamment les suivants :  
  
    -   Lorsque vous affectez à l'accès la valeur **NONE**, tous les descripteurs de fichiers ouverts.  
  
    -   Lorsque vous affectez à l’accès la valeur **READ_ONLY**, tous les descripteurs de fichiers ouverts pour l’accès en écriture.  
  
     Pour plus d'informations sur la manière de supprimer des descripteurs de fichiers ouverts, consultez [Suppression des descripteurs de fichiers ouverts associés à un FileTable](#BasicsKilling) dans cette rubrique.  
  
     Si la commande ALTER DATABASE est annulée ou se solde par une erreur de délai d'attente, le niveau d'accès transactionnel n'est pas modifié.  
  
-   Si vous appelez l’instruction ALTER DATABASE avec une clause WITH \<termination> (ROLLBACK AFTER integer [ SECONDS ] | ROLLBACK IMMEDIATE | NO_WAIT), tous les descripteurs de fichiers non transactionnels ouverts sont alors supprimés.  
  
> [!WARNING]  
>  La suppression des descripteurs de fichiers ouverts peut entraîner la perte de données utilisateur non enregistrées. Ce comportement est cohérent avec le comportement du système de fichiers lui-même.  
  
 **Effets de la désactivation de l'accès non transactionnel**  
  
 La modification du niveau de l'accès non transactionnel au niveau de la base de données a les effets suivants sur les répertoires FileTable sous le répertoire au niveau de la base de données :  
  
-   Lorsque vous affectez à l'accès la valeur **NONE**, tous les répertoires FileTable et leur contenu ne sont plus accessibles ni visibles.  
  
-   Lorsque vous affectez à l’accès la valeur **READ_ONLY**, tous les répertoires FileTable et leur contenu sont également en lecture seule.  
  
 La désactivation de FILESTREAM au niveau de l'instance a les effets suivants sur les répertoires au niveau de la base de données sur cette instance et sur les répertoires de FileTable sous ceux-ci :  
  
-   Aucun des répertoires au niveau de la base de données sur l'instance n'est visible si FILESTREAM est désactivé au niveau de l'instance.  
  
###  <a name="HowToDisable"></a> Procédure : désactiver et réactiver l'accès non transactionnel au niveau de la base de données  
 Pour plus d’informations, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 **Pour désactiver l'accès non transactionnel complet**  
 Appelez l’instruction **ALTER DATABASE** et définissez (avec SET) la valeur de **NON_TRANSACTED_ACCESS** sur **READ_ONLY** ou **OFF**.  
  
```sql  
-- Disable write access.  
ALTER DATABASE database_name  
SET FILESTREAM ( NON_TRANSACTED_ACCESS = READ_ONLY );  
GO  
  
-- Disable non-transactional access.  
ALTER DATABASE database_name  
SET FILESTREAM ( NON_TRANSACTED_ACCESS = OFF );  
GO  
```  
  
 **Pour réactiver l'accès non transactionnel complet**  
 Appelez l’instruction **ALTER DATABASE** et définissez (avec SET) la valeur de **NON_TRANSACTED_ACCESS** sur **FULL**.  
  
```sql  
ALTER DATABASE database_name  
SET FILESTREAM ( NON_TRANSACTED_ACCESS = FULL );  
GO  
```  
  
###  <a name="visible"></a> Procédure : assurer la visibilité des FileTables dans une base de données  
 Un répertoire au niveau de la base de données et les répertoires FileTable sous celui-ci sont visibles lorsque toutes les conditions suivantes sont remplies :  
  
1.  FILESTREAM est activé au niveau de l'instance.  
  
2.  L'accès non transactionnel est activé au niveau de la base de données.  
  
3.  Un répertoire valide a été spécifié au niveau de la base de données.  
  
##  <a name="BasicsEnabling"></a> Désactivation et réactivation de l'espace de noms FileTable au niveau de la table  
 La désactivation de l'espace de noms FileTable désactive tous les déclencheurs et contraintes définis par le système qui ont été créés avec le FileTable. Cela s'avère utile dans les cas où vous devez réorganiser un FileTable à grande échelle à l'aide d'opérations [!INCLUDE[tsql](../../includes/tsql-md.md)] sans avoir à appliquer la sémantique du FileTable. Toutefois, ces opérations peuvent laisser le FileTable dans un état cohérent, et de ce fait empêcher la réactivation de l'espace de noms FileTable.  
  
 La désactivation d'un espace de noms FileTable produit les résultats suivants :  
  
-   Les colonnes et les données FileTable ne sont pas supprimées physiquement de la table.  
  
-   Le répertoire FileTable et les fichiers et répertoires qu'il contient disparaissent du système de fichiers et ne sont pas disponibles pour l'accès aux E/S de fichier.  
  
-   Les colonnes FileTable définies par le système ne peuvent pas être supprimées et recréées ; toutefois, elles se comportent comme des colonnes ordinaires pour des opérations DML.  
  
-   Les descripteurs de fichiers ouverts empêchent la désactivation des contraintes FileTable, cette opération requérant un verrou de schéma sur la table.  
  
-   L'application de l'ensemble de la sémantique de FileTable, y compris les contraintes et les déclencheurs définies par le système, s'arrête une fois que l'espace de noms FileTable est désactivé.  
  
 La réactivation d'un espace de noms FileTable produit les résultats suivants :  
  
-   La cohérence du FileTable est vérifiée. Si des incohérences sont détectées, une erreur est générée et le FileTable reste désactivé ; sinon, le FileTable est réactivé.  
  
-   L'application de la sémantique FileTable, y compris les contraintes et les déclencheurs définies par le système, est restaurée.  
  
-   Le répertoire FileTable et les fichiers et répertoires qu'il contient sont visibles dans le système de fichiers et sont disponibles pour l'accès aux E/S de fichier.  
  
###  <a name="HowToEnableNS"></a> Procédure : désactiver et réactiver l'espace de noms FileTable au niveau de la table  
 Appelez l’instruction ALTER TABLE avec l’option **{ENABLE | DISABLE} FILETABLE_NAMESPACE** .  
  
 **Pour désactiver l'espace de noms FileTable**  
 ```sql  
ALTER TABLE filetable_name  
DISABLE FILETABLE_NAMESPACE;  
GO  
```  
  
 **Pour réactiver l'espace de noms FileTable**  
 ```sql  
ALTER TABLE filetable_name  
ENABLE FILETABLE_NAMESPACE;  
GO  
```  
  
##  <a name="BasicsKilling"></a> Suppression des descripteurs de fichiers ouverts associés à un FileTable  
 Les descripteurs ouverts dans les fichiers stockés dans un FileTable peuvent empêcher l'accès exclusif qui est obligatoire pour certaines tâches d'administration. Vous devrez peut-être supprimer les descripteurs de fichiers ouverts associés à un ou à plusieurs FileTables pour activer des tâches urgentes.  
  
> [!WARNING]  
>  La suppression des descripteurs de fichiers ouverts peut entraîner la perte de données utilisateur non enregistrées. Ce comportement est cohérent avec le comportement du système de fichiers lui-même.  
  
###  <a name="HowToListOpen"></a> Procédure : obtenir une liste des descripteurs de fichiers ouverts associés à un FileTable  
 Interrogez l’affichage catalogue [sys.dm_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md).  
  
```sql  
SELECT * FROM sys.dm_filestream_non_transacted_handles;  
GO  
```  
  
###  <a name="HowToKill"></a> Procédure : supprimer les descripteurs de fichiers ouverts associés à un FileTable  
 Appelez la procédure stockée [sp_kill_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md) avec les arguments appropriés pour supprimer tous les descripteurs de fichiers ouverts dans la base de données ou dans le FileTable, ou pour supprimer un descripteur spécifique.  
  
```sql  
USE database_name;  
  
-- Kill all open handles in all the filetables in the database.  
EXEC sp_kill_filestream_non_transacted_handles;  
GO  
  
-- Kill all open handles in a single filetable.  
EXEC sp_kill_filestream_non_transacted_handles @table_name = 'filetable_name';  
GO  
  
-- Kill a single handle.  
EXEC sp_kill_filestream_non_transacted_handles @handle_id = integer_handle_id;  
GO  
```  
  
###  <a name="HowToIdentifyLocks"></a> Procédure : identifier les verrous maintenus par les FileTables  
 La plupart des verrous acceptés par les FileTables correspondent aux fichiers ouverts par des applications.  
  
 **Pour identifier les fichiers ouverts et les verrous associés**  
 Joignez le champ **request_owner_id** dans la vue de gestion dynamique [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md) au champ **fcb_id** dans [sys.dm_filestream_non_transacted_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md). Dans certains cas, le verrou ne correspond pas à un descripteur de fichier ouvert unique.  
  
```sql  
SELECT opened_file_name  
FROM sys.dm_filestream_non_transacted_handles  
WHERE fcb_id IN  
    ( SELECT request_owner_id FROM sys.dm_tran_locks );  
GO  
```  
  
##  <a name="BasicsSecurity"></a> Sécurité d'un FileTable  
 La sécurité des fichiers et des répertoires stockés dans des FileTables est uniquement assurée par la sécurité SQL Server. La sécurité basée sur les tables et les colonnes est appliquée pour l'accès au système de fichiers et l'accès [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les API de sécurité du système de fichiers Windows et les paramètres ACL ne sont pas pris en charge.  
  
 Les autorisations d'accès et de sécurité applicables aux conteneurs et aux groupes de fichiers FILESTREAM s'appliquent également aux FileTables, puisque les données des fichiers sont stockées en tant que colonne FILESTREAM dans le FileTable.  
  
 **Sécurité FileTable et accès Transact-SQL**  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] L’accès aux données dans les FileTables est sécurisé de la même façon que dans toute autre table. Des contrôles de sécurité appropriés au niveau de la table et de la colonne sont effectués pour chaque opération qui accède aux données ou les modifie.  
  
 **Sécurité FileTable et accès au système de fichiers**  
 Les API du système de fichiers nécessitent des autorisations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appropriées sur la ligne entière dans le FileTable (c'est-à-dire au niveau de la table) pour ouvrir un descripteur à un fichier ou répertoire stocké dans le FileTable. Si l'utilisateur ne dispose pas de l'autorisation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appropriée sur une colonne du FileTable, l'accès au système de fichiers est alors refusé.  
  
##  <a name="OtherBackup"></a> Sauvegarde et FileTables  
 Lorsque vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour sauvegarder un FileTable, les données FILESTREAM sont sauvegardées avec les données structurées dans la base de données. Si vous ne souhaitez pas sauvegarder les données FILESTREAM avec les données relationnelles, vous pouvez utiliser une sauvegarde partielle pour exclure les groupes de fichiers FILESTREAM.  
  
 **Cohérence transactionnelle des sauvegardes de FileTable**  
  
 De nombreux outils et opérations d'administration (notamment la sauvegarde, la sauvegarde de fichier journal et la réplication transactionnelle) lisent les données cohérentes au niveau transactionnel en lisant les journaux des transactions. À ce stade, ils lisent toutes les données FILESTREAM mises à jour dans le cadre d'une transaction. Si l'accès non transactionnel n'est pas activé au niveau de la base de données, ces outils et opérations fonctionnent avec une cohérence transactionnelle complète.  
  
 Toutefois, lorsque l'accès non transactionnel complet est activé, un FileTable pourrait contenir des données mises à jour plus récemment (via une mise à jour non transactionnelle) que la transaction que l'outil ou le processus lit à partir du journal des transactions. Cela signifie qu'une opération de restauration à un point précis dans le temps dans une transaction spécifique peut contenir des données FILESTREAM plus récentes que cette transaction. Il s'agit du comportement attendu lorsque des mises à jour non transactionnelles sont autorisées sur les FileTables.  
  
##  <a name="Monitor"></a> SQL Server Profiler et FileTables  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler peut capturer les opérations Windows File Open et File Close dans le résultat de trace pour les fichiers stockés dans un FileTable.  
  
##  <a name="OtherAuditing"></a> Audit et FileTables  
 Le FileTable peut faire l'objet d'un audit  comme n'importe quelle autre table. Cependant, les modèles d’accès Win32 ne sont pas des opérations basées sur des ensembles. Une action unique dans le système de fichiers se traduit par plusieurs opérations DML Transact-SQL. Par exemple, l'ouverture d'un fichier dans Microsoft Word se traduit par plusieurs opérations d'ouverture/de fermeture/de création/d'attribution d'un nouveau nom/de suppression et des activités DML Transact-SQL correspondantes. Cela provoque des enregistrements d'audit détaillés dans lesquels il est difficile de mettre en corrélation des enregistrements entre les actions de système de fichiers et les enregistrements d'audit de DML Transact-SQL correspondants.  
  
##  <a name="OtherDBCC"></a> DBCC et FileTables  
 Vous pouvez utiliser DBCC CHECKCONSTRAINTS pour valider les contraintes sur un FileTable qui inclut des contraintes définies par le système.  
  
## <a name="see-also"></a> Voir aussi  
 [Compatibilité de FileTable avec d'autres fonctionnalités SQL Server](../../relational-databases/blob/filetable-compatibility-with-other-sql-server-features.md)   
 [DDL, fonctions, procédures stockées et vues FileTable](../../relational-databases/blob/filetable-ddl-functions-stored-procedures-and-views.md)  
