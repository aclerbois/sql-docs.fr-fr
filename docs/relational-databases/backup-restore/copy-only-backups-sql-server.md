---
title: Sauvegardes de type copie seule (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- copy-only backups [SQL Server]
- COPY_ONLY option [BACKUP statement]
- backups [SQL Server], copy-only backups
ms.assetid: f82d6918-a5a7-4af8-868e-4247f5b00c52
caps.latest.revision: 48
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4384584a2dc1f278bfe3fc63baae2e1068cc9956
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="copy-only-backups-sql-server"></a>Sauvegardes de type copie seule (SQL Server)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]
  Une *sauvegarde de copie uniquement* est une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indépendante du mécanisme des sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conventionnelles. Normalement, une sauvegarde modifie la base de données et affecte la restauration des sauvegardes ultérieures. Parfois, cependant, il est utile d'effectuer une sauvegarde à une fin précise sans affecter les procédures globales de sauvegarde et de restauration de la base de données. Pour cela, on peut recourir à une sauvegarde de copie uniquement.  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]
  
 Les types de sauvegarde de copie uniquement sont les suivants :  
  
-   Sauvegardes complètes de copie uniquement (tous modes de récupération)  
  
     Une sauvegarde de copie uniquement ne peut pas servir de base différentielle ni de sauvegarde différentielle et n'a aucune incidence sur la base différentielle.  
  
     La restauration d'une sauvegarde complète de copie uniquement est identique à la restauration de toute autre sauvegarde complète.  
  
-   Sauvegardes de fichier journal de copie uniquement (en mode de récupération complète et en mode de récupération utilisant les journaux de transactions uniquement)  
  
     Une sauvegarde du journal de type copie seule préserve le point d'archive du journal existant et, donc, n'a pas d'incidence sur l'ordre des sauvegardes régulières des journaux. Les sauvegardes de journaux de type copie seule sont généralement superflues. En revanche, vous pouvez créer une nouvelle sauvegarde de routine des journaux (à l'aide de WITH NORECOVERY) et utiliser cette sauvegarde conjointement avec toute sauvegarde des journaux précédente nécessaire à la séquence de restauration. Toutefois, une sauvegarde de fichier journal de copie uniquement peut parfois être utile pour effectuer une restauration en ligne. Pour obtenir un exemple, consultez [Exemple : restauration en ligne d’un fichier en lecture/écriture &#40;Mode de récupération complète&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md).  
  
     Le journal des transactions n'est jamais tronqué après une sauvegarde de type copie seule.  
  
 Les sauvegardes de copie uniquement sont enregistrées dans la colonne **is_copy_only** de la table [backupset](../../relational-databases/system-tables/backupset-transact-sql.md) .  
  
## <a name="to-create-a-copy-only-backup"></a>Pour créer une sauvegarde de type copie uniquement  
 Vous pouvez créer une sauvegarde de copie uniquement à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]ou de PowerShell.  

### <a name="examples"></a>Exemples  
###  <a name="SSMSProcedure"></a> A.  Utilisation de SQL Server Management Studio  
Dans cet exemple, une sauvegarde de copie seule de la base de données `Sales` sauvegardée sur le disque à l’emplacement de sauvegarde par défaut.

1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur `Sales`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

3.  Sur la page **Général** de la section **Source** , cochez la case **Sauvegarde de copie seule** .

4.  Cliquez sur **OK**.

  
###  <a name="TsqlProcedure"></a>B.  Utilisation de Transact-SQL  
Cet exemple crée une sauvegarde de copie seule pour la base de données `Sales` utilisant le paramètre COPY_ONLY.  Une sauvegarde de copie seule du journal des transactions est également prise.

```sql
BACKUP DATABASE Sales
TO DISK = 'E:\BAK\Sales_Copy.bak'
WITH COPY_ONLY;

BACKUP LOG Sales
TO DISK = 'E:\BAK\Sales_LogCopy.trn'
WITH COPY_ONLY;
```
  
> [!NOTE]  
> COPY_ONLY n'a aucun effet lorsqu'il est spécifié avec l'option DIFFERENTIAL.  

  
###  <a name="PowerShellProcedure"></a>C.  Utilisation de PowerShell  
Cet exemple crée une sauvegarde de copie seule pour la base de données `Sales` utilisant le paramètre -CopyOnly.  
```powershell
Backup-SqlDatabase -ServerInstance 'SalesServer' -Database 'Sales' -BackupFile 'E:\BAK\Sales_Copy.bak' -CopyOnly
```  
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Pour créer une sauvegarde complète ou de fichier journal**  
  
-   [Créer une sauvegarde complète de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
 **Pour afficher des sauvegardes de copie uniquement**  
  
-   [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../relational-databases/scripting/sql-server-powershell-provider.md)  
  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
 [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
[BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
[Backup-SqlDatabase](https://technet.microsoft.com/library/mt683378.aspx)

  
