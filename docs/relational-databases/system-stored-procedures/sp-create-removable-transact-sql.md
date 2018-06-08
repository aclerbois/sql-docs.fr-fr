---
title: sp_create_removable (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_create_removable
- sp_create_removable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_removable
ms.assetid: 06e36ae5-f70d-4a26-9a7f-ee4b9360b355
caps.latest.revision: 31
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7ac999e58a6c88d8a121d7708b6fc9e954cf7419
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcreateremovable-transact-sql"></a>sp_create_removable (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Cette procédure crée une base de données sur un support amovible. Elle crée trois fichiers minimum (un pour les tables de catalogue système, un pour le journal des transactions et un ou plus pour les tables de données) et place la base de données sur ces fichiers.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Nous vous recommandons d’utiliser [créer la base de données](../../t-sql/statements/create-database-sql-server-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_create_removable   
   [ @dbname = ] 'dbname',   
   [ @syslogical= ] 'syslogical',   
   [ @sysphysical = ] 'sysphysical',   
   [ @syssize = ] syssize,   
   [ @loglogical = ] 'loglogical',   
   [ @logphysical = ] 'logphysical',   
   [ @logsize = ] logsize,   
   [ @datalogical1 = ] 'datalogical1',   
   [ @dataphysical1 = ] 'dataphysical1',   
   [ @datasize1 = ] datasize1 ,   
   [ @datalogical16 = ] 'datalogical16',   
   [ @dataphysical16 = ] 'dataphysical16',   
   [ @datasize16 = ] datasize16 ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@dbname=** ] **'***dbname***'**  
 Nom de la base de données à créer pour une utilisation sur un support amovible. *dbname* est **sysname**.  
  
 [  **@syslogical=** ] **'***syslogical***'**  
 Nom logique du fichier qui contient les tables du catalogue système. *syslogical* est **sysname**.  
  
 [  **@sysphysical=** ] **'***sysphysical***'**  
 Nom physique. Il comprend un chemin d'accès complet du fichier contenant les tables du catalogue système. *sysphysical* est **nvarchar (260)**.  
  
 [  **@syssize=** ] *syssize*  
 Taille, en Mo, du fichier contenant les tables du catalogue système. *syssize* est **int**. La valeur minimale *syssize* est 1.  
  
 [  **@loglogical=** ] **'***loglogical***'**  
 Nom logique du fichier contenant le journal des transactions. *loglogical* est **sysname**.  
  
 [  **@logphysical=** ] **'***logphysical***'**  
 Nom physique. Il comprend un chemin d'accès complet du fichier contenant le journal des transactions. *logphysical* est **nvarchar (260)**.  
  
 [  **@logsize=** ] *logsize*  
 Taille, en Mo, du fichier contenant le journal des transactions. *LogSize* est **int**. La valeur minimale *logsize* est 1.  
  
 [  **@datalogical1=** ] **'***datalogique***'**  
 Nom logique d'un fichier contenant les tables de données. *datalogique* est **sysname**.  
  
 Il doit exister entre 1 et 16 fichiers de données. Habituellement, plusieurs fichiers de données sont créés lorsqu'il est prévu que la base de données soit volumineuse et qu'elle doive être distribuée sur plusieurs disques.  
  
 [  **@dataphysical1=** ] **'***dataphysique***'**  
 Nom physique. Il comprend un chemin d'accès complet du fichier contenant les tables de données. *dataphysique* est **nvarchar (260)**.  
  
 [  **@datasize1=** ] **'***datasize***'**  
 Taille, en Mo, du fichier contenant les tables de données. *DataSize* est **int**. La valeur minimale *datasize* est 1.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Si vous souhaitez faire une copie de votre base de données sur un support amovible (par exemple un CD-ROM) pour la distribuer à d'autres utilisateurs, utilisez cette procédure stockée.  
  
## <a name="permissions"></a>Autorisations  
 L'autorisation CREATE DATABASE, CREATE ANY DATABASE ou ALTER ANY DATABASE est obligatoire.  
  
 Pour garder le contrôle de l'utilisation du disque sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'autorisation de création de bases de données est généralement limitée à quelques comptes de connexion.  
  
### <a name="permissions-on-data-and-log-files"></a>Autorisations sur les données et les journaux  
 Chaque fois que certaines opérations sont effectuées sur une base de données, les autorisations correspondantes sont définies sur les données et les fichiers journaux. Les autorisations empêchent les fichiers d'être accidentellement falsifiés s'ils résident dans un répertoire doté d'autorisations d'ouverture.  
  
|Opération sur la base de données|Autorisations d'accès aux fichiers définies|  
|---------------------------|------------------------------|  
|Modifiée pour ajouter un nouveau fichier|Créé le|  
|Sauvegardée|Attachée|  
|Restaurée|Détachée|  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne définit pas d'autorisations sur les fichiers de données et les journaux.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée la base de données `inventory` comme base de données amovible.  
  
```  
EXEC sp_create_removable 'inventory',   
   'invsys',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invsys.mdf'  
, 2,   
   'invlog',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invlog.ldf', 4,  
   'invdata',  
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\invdata.ndf',   
10;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_certify_removable &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-certify-removable-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_dbremove &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbremove-transact-sql.md)   
 [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_helpfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
