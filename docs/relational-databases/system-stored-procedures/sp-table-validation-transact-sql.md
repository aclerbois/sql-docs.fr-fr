---
title: sp_table_validation (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_table_validation_TSQL
- sp_table_validation
helpviewer_keywords:
- sp_table_validation
ms.assetid: 31b25f9b-9b62-496e-a97e-441d5fd6e767
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d82517f09ad18c7cc0b2e8d49acfdae6ab200ee9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sptablevalidation-transact-sql"></a>sp_table_validation (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur le nombre de lignes ou sur la somme de contrôle d'une table ou d'une vue indexée, ou bien compare ces informations avec la table ou la vue indexée spécifiée. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication et sur la base de données d'abonnement de l'Abonné. *Non pris en charge pour les serveurs de publication Oracle*.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_table_validation [ @table = ] 'table'  
    [ , [ @expected_rowcount = ] type_of_check_requested OUTPUT]  
    [ , [ @expected_checksum = ] expected_checksum OUTPUT]  
    [ , [ @rowcount_only = ] rowcount_only ]  
    [ , [ @owner = ] 'owner' ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @table_name = ] table_name ]  
    [ , [ @column_list = ] 'column_list' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table=**] **'***table***'**  
 Est le nom de la table. *table* est **sysname**, sans valeur par défaut.  
  
 [  **@expected_rowcount=**] *expected_rowcount*sortie  
 Spécifie s'il faut retourner le nombre de lignes attendu pour la table. *expected_rowcount* est **int**, avec NULL comme valeur par défaut. Si la valeur est NULL, le nombre de lignes réel est retourné en tant que paramètre de sortie. Si une valeur est fournie, celle-ci est confrontée au nombre réel de lignes en vue d'une identification des éventuelles différences.  
  
 [  **@expected_checksum=**] *expected_checksum*sortie  
 Spécifie s'il faut retourner la somme de contrôle attendue pour la table. *expected_checksum* est **numérique**, avec NULL comme valeur par défaut. Si la valeur est NULL, la somme de contrôle réelle est retournée en tant que paramètre de sortie. Si une valeur est fournie, celle-ci est confrontée à la somme de contrôle réelle en vue d'une identification des éventuelles différences.  
  
 [  **@rowcount_only=**] *type_of_check_requested*  
 Spécifie le type de la somme de contrôle ou du nombre de lignes à effectuer. *type_of_check_requested* est **smallint**, avec une valeur par défaut **1**.  
  
 Si **0**, effectuer un décompte de lignes et un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] somme de contrôle compatible 7.0.  
  
 Si **1**, effectuer une vérification du nombre de lignes uniquement.  
  
 Si **2**, effectuer une somme de contrôle du nombre de lignes et binaires.  
  
 [  **@owner=**] **'***propriétaire***'**  
 Est le nom du propriétaire de la table. *propriétaire* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@full_or_fast=**] *full_or_fast*  
 Méthode utilisée pour calculer le nombre de lignes. *full_or_fast* est **tinyint**, avec une valeur par défaut **2**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Effectue un comptage total à l'aide de COUNT(*).|  
|**1**|Effectue un comptage de rapide **sysindexes.rows**. Le décompte de lignes **sysindexes** est beaucoup plus rapide que le décompte de lignes dans la table. Toutefois, étant donné que **sysindexes** est immédiatement mis à jour, le nombre de lignes peut être inexact.|  
|**2** (par défaut)|Exécute un décompte rapide conditionnel en essayant d'abord la méthode rapide. Si la méthode rapide affiche des différences, revient à la méthode totale. Si *expected_rowcount* a la valeur NULL et la procédure stockée est en cours d’utilisation pour obtenir la valeur, un Count (\*) complète est toujours utilisée.|  
  
 [  **@shutdown_agent=**] *shutdown_agent*  
 Si l’Agent de Distribution est en cours d’exécution **sp_table_validation**, spécifie si l’Agent de Distribution doit être fermé immédiatement après l’achèvement de la validation. *shutdown_agent* est **bits**, avec une valeur par défaut **0**. Si **0**, l’agent de réplication ne s’arrête pas. Si **1**, l’erreur 20578 est déclenchée et l’agent de réplication est signalé pour l’arrêter. Ce paramètre est ignoré lorsque **sp_table_validation** exécuté directement par un utilisateur.  
  
 [  **@table_name =**] *nom_table*  
 Nom de la table qui contient la vue utilisée pour les messages de sortie. *nom_table* est **sysname**, avec une valeur par défaut **@table**.  
  
 [ **@column_list**=] **'***column_list***'**  
 Liste de colonnes à utiliser dans la fonction de somme de contrôle. *column_list* est **nvarchar (4000)**, avec NULL comme valeur par défaut. Active la validation d'articles de fusion pour spécifier une liste de colonnes excluant les colonnes calculées et les colonnes timestamp.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Si l’exécution d’une validation de somme de contrôle et la somme de contrôle attendue est égal à la somme de contrôle dans la table, **sp_table_validation** renvoie un message que le tableau passé la validation de somme de contrôle. Sinon, elle retourne un message indiquant que la table peut ne plus être synchronisée et indique la différence entre le nombre de lignes attendu et le nombre réel.  
  
 Si une validation du nombre de lignes et le nombre de lignes attendu est égal le numéro de la table, **sp_table_validation** renvoie un message que le tableau passé la validation du nombre de lignes. Sinon, elle retourne un message indiquant que la table peut ne plus être synchronisée et indique la différence entre le nombre de lignes attendu et le nombre réel.  
  
## <a name="remarks"></a>Notes  
 **sp_table_validation** est utilisée dans tous les types de réplication. **sp_table_validation** n’est pas pris en charge pour les serveurs de publication Oracle.  
  
 La somme de contrôle calcule une vérification de redondance cyclique de 32 bits (CRC) sur l'image de la ligne entière de la page. La somme de contrôle ne vérifie pas les colonnes de manière sélective et ne peut pas s'exécuter sur une vue ou une partition verticale de la table. En outre, la somme de contrôle saute les contenus des **texte** et **image** colonnes (par conception).  
  
 Lors d'une somme de contrôle, la structure de la table doit être identique sur les deux serveurs ; les tables doivent posséder les mêmes colonnes, dans le même ordre, les mêmes types et longueurs de données et les mêmes conditions NULL/NOT NULL. Par exemple, si le serveur de publication a exécuté une instruction CREATE TABLE, puis une instruction ALTER TABLE pour ajouter des colonnes, mais que le script appliqué au niveau de l'Abonné est une simple CREATE TABLE, la structure n'est PAS la même. Si vous n’êtes pas certain que la structure des deux tables est identique, examinez [syscolumns](../../relational-databases/system-compatibility-views/sys-syscolumns-transact-sql.md) et confirmez que le décalage de chaque table est le même.  
  
 Valeurs à virgule flottante sont susceptibles de générer des différences de somme de contrôle si le mode caractère **bcp** a été utilisé, ce qui est le cas si la publication a non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés. Cela est dû à des erreurs mineures et inévitables de précision lors de la conversion vers le mode caractère et à partir de ce mode.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter **sp_table_validation**, vous devez disposer des autorisations SELECT sur la table en cours de validation.  
  
## <a name="see-also"></a>Voir aussi  
 [Somme de contrôle &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)   
 [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md)   
 [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
