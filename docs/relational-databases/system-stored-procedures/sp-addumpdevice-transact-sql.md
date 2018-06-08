---
title: sp_addumpdevice (Transact-SQL) | Documents Microsoft
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
- sp_addumpdevice_TSQL
- sp_addumpdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], defining
- sp_addumpdevice
ms.assetid: c2d2ae49-0808-46d8-8444-db69a69d0ec3
caps.latest.revision: 49
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: cbf23913e95b53e490d55099cde44b5ab60d3141
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddumpdevice-transact-sql"></a>sp_addumpdevice (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  
**S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).  

Ajoute une unité de sauvegarde à une instance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addumpdevice [ @devtype = ] 'device_type'   
    , [ @logicalname = ] 'logical_name'   
    , [ @physicalname = ] 'physical_name'  
      [ , { [ @cntrltype = ] controller_type |  
          [ @devstatus = ] 'device_status' }  
      ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@devtype=** ] **'***device_type***'**  
 Type de périphérique de sauvegarde. *device_type* est **varchar (20)**, sans valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Disque**|Fichier de disque dur comme unité de sauvegarde.|  
|**Bande**|Tout périphérique à bandes géré par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> Remarque : la prise en charge des unités de sauvegarde sur bande sera supprimée dans une prochaine version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.|  
  
 [  **@logicalname =** ] **'***nom_logique***'**  
 Nom logique de l'unité de sauvegarde utilisée dans les instructions BACKUP et RESTORE. *nom_logique* est **sysname**, sans valeur par défaut, et ne peut pas être NULL.  
  
 [  **@physicalname =** ] **'***physical_name***'**  
 Nom physique de l'unité de sauvegarde. Les noms physiques doivent respecter les règles en vigueur pour les noms de fichiers du système d'exploitation ou les conventions d'affectation des noms pour les unités réseau, et doivent comprendre un chemin d'accès complet. *physical_name* est **nvarchar (260)**, sans valeur par défaut de valeur et ne peut pas être NULL.  
  
 Lorsque vous créez une unité de sauvegarde sur un site de réseau distant, assurez-vous que le nom sous lequel le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a été démarré est capable d'assurer les opérations d'écriture sur l'ordinateur distant.  
  
 Si vous ajoutez un périphérique à bandes, ce paramètre doit être le nom physique affecté sur le périphérique à bandes local par Windows ; par exemple,  **\\ \\. \TAPE0** pour la première unité de bande sur l’ordinateur. Ce périphérique à bandes doit être relié à l'ordinateur serveur, il ne peut être utilisé à distance. Insérez les noms comportant des caractères non alphanumériques entre guillemets.  
  
> [!NOTE]  
>  Cette procédure entre le nom physique spécifié dans le catalogue, mais elle ne tente pas de créer l'unité ou d'y accéder.  
  
 [  **@cntrltype =** ] **'***type_de_contrôleur***'**  
 Obsolète. S'il est spécifié, ce paramètre est ignoré. Il est conservé uniquement pour des raisons de compatibilité descendante. Les nouvelles utilisations de **sp_addumpdevice** doit omettre ce paramètre.  
  
 [  **@devstatus =** ] **'***état_unité***'**  
 Obsolète. S'il est spécifié, ce paramètre est ignoré. Il est conservé uniquement pour des raisons de compatibilité descendante. Les nouvelles utilisations de **sp_addumpdevice** doit omettre ce paramètre.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_addumpdevice** ajoute une unité de sauvegarde à la **sys.backup_devices** affichage catalogue. Vous pouvez ensuite faire référence à cette unité de manière logique dans les instructions BACKUP et RESTORE. **sp_addumpdevice** n’effectue pas l’accès à l’unité physique. L'accès à l'unité spécifié survient uniquement lorsqu'une instruction BACKUP ou RESTORE est exécutée. La création d'une unité de sauvegarde logique peut simplifier les instructions BACKUP et RESTORE, car la définition du nom de l'unité est une solution via l'utilisation d'une clause « TAPE = » ou « DISK = » pour spécifier le chemin d'accès de l'unité.  
  
 Des problèmes de propriété et de permissions sont susceptibles de perturber l'utilisation des unités de sauvegarde sur disque ou sur fichiers. Assurez-vous que le compte Windows sous lequel le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a été démarré bénéficie des autorisations de fichiers adéquates.  
  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] assure les sauvegardes sur des périphériques à bandes pris en charge par Windows. Pour plus d'informations sur les périphériques à bandes pris en charge par Windows, consultez la liste de compatibilité du matériel de Windows. Pour afficher les périphériques à bandes disponibles sur l'ordinateur, utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 N'utilisez dans le lecteur de bande que les bandes recommandées par le fabricant du lecteur. Si vous utilisez des lecteurs DAT, utilisez des bandes DAT conçues pour fonctionner avec un ordinateur (Digital Data Storage-DDS).  
  
 **sp_addumpdevice** ne peut pas être exécutée dans une transaction.  
  
 Pour supprimer un périphérique, utilisez [sp_dropdevice](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md) ou[SQL Server Management Studio](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **diskadmin** .  
  
 Requiert l'autorisation d'écrire sur le disque.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-disk-dump-device"></a>A. Ajout d'une unité de sauvegarde sur disque  
 L'exemple suivant ajoute une unité de sauvegarde sur disque appelée `mydiskdump`, dont le nom physique est `c:\dump\dump1.bak`.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak';  
```  
  
### <a name="b-adding-a-network-disk-backup-device"></a>B. Ajout d'une unité de sauvegarde sur disque du réseau  
 L'exemple suivant ajoute une unité de sauvegarde sur disque distant appelée `networkdevice`. Le nom sous lequel le [!INCLUDE[ssDE](../../includes/ssde-md.md)] a été démarré doit disposer des autorisations pour ce fichier distant (`\\<servername>\<sharename>\<path>\<filename>.bak`).  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'networkdevice',  
    '\\<servername>\<sharename>\<path>\<filename>.bak';  
```  
  
### <a name="c-adding-a-tape-backup-device"></a>C. Ajout d'une unité de sauvegarde sur bande  
 L'exemple suivant ajoute le périphérique à bandes `tapedump1` dont le nom physique est `\\.\tape0`.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'tape', 'tapedump1', '\\.\tape0';  
```  
  
### <a name="d-backing-up-to-a-logical-backup-device"></a>D. Sauvegarde sur une unité de sauvegarde logique  
 L'exemple suivant crée une unité de sauvegarde logique, `AdvWorksData`, pour un fichier de sauvegarde sur disque. Il sauvegarde ensuite la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur cette unité de sauvegarde logique.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\AdvWorksData.bak';  
GO  
BACKUP DATABASE AdventureWorks2012   
 TO AdvWorksData  
   WITH FORMAT;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Définir une unité de sauvegarde logique pour un fichier de disque &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Définir une unité de sauvegarde logique pour un lecteur de bande &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
