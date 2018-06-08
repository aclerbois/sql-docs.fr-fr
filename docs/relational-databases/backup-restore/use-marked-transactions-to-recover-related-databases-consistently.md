---
title: Utiliser les transactions marquées pour récupérer des bases de données associées uniformément | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transaction marks [SQL Server]
- marked transactions [SQL Server]
- database restores [SQL Server], inserting transaction marks for
- recovery [SQL Server], related databases
- restoring databases [SQL Server], related database recovery
- database restores [SQL Server], related databases
- marked transactions [SQL Server], creating
- BEGIN TRAN...WITH MARK statement
- two-phase commit
ms.assetid: 50a73574-1a69-448e-83dd-9abcc7cb7e1a
caps.latest.revision: 45
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: cccec56e7d84618fab4127d11f7360d4cc2bd022
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-marked-transactions-to-recover-related-databases-consistently"></a>Utiliser les transactions marquées pour récupérer des bases de données associées uniformément
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique s'applique uniquement aux bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] employant le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions.  
  
 Quand vous effectuez des mises à jour associées vers plusieurs bases de données ( *bases de données associées*), vous pouvez utiliser des marques de transaction pour les récupérer à un point cohérent d’un point de vue logique. Cependant, cette récupération perd les transactions validées après la marque utilisée comme point de récupération. Le marquage des transactions est adapté uniquement au test des bases de données associées ou si vous êtes prêt à risquer la perte des transactions validées récemment.  
  
 Le marquage régulier des transactions associées dans chaque base de données associée établit une série de points de récupération dans les bases de données. Les marques de transaction sont enregistrées dans le journal des transactions et incluses dans les sauvegardes du journal. En cas de sinistre, vous pouvez restaurer chaque base de données à la même marque de transaction pour les récupérer à un point cohérent.  
  
> [!NOTE]  
>  Les sauvegardes du journal sur les différentes bases de données peuvent être créées indépendamment et ne doivent pas être simultanées.  
  
 La récupération des bases de données associées dans les scénarios suivants nécessite de disposer déjà de transactions marquées dans chaque base de données associée :  
  
-   Un ou plusieurs journaux des transactions sont détruits. Vous devez restaurer l’ensemble des bases de données dans un état cohérent au moment de votre dernière sauvegarde de journal.  
  
-   Vous devez restaurer la totalité des bases de données dans un état mutuellement cohérent correspondant à un moment antérieur dans le temps.  
  
> [!IMPORTANT]  
>  Vous ne pouvez récupérer des bases de données associées qu'en fonction d'une transaction marquée, et non d'un moment précis.  
  
 Pour des informations sur la création des marquages de transactions, consultez « Création des transactions marquées » plus loin dans cette rubrique.  
  
## <a name="typical-scenario-for-using-marked-transactions"></a>Scénario classique d'utilisation des transactions marquées  
 Ce scénario inclut les procédures suivantes :  
  
1.  la création d'une sauvegarde complète ou différentielle de chaque base de données associée ;  
  
2.  le marquage d'un verrou de transaction dans toutes les bases de données ;  
  
3.  la sauvegarde du journal des transactions pour toutes les bases de données ;  
  
4.  la restauration des sauvegardes de base de données à l'aide de WITH NORECOVERY ;  
  
5.  la restauration des journaux à l'aide de WITH STOPATMARK.  
  
## <a name="considerations-for-using-marked-transactions"></a>Considérations relatives à l'utilisation des transactions marquées  
 Avant d'insérer des marques nommées dans le journal des transactions, prenez en compte les éléments suivants :  
  
-   les marques de transaction occupant de l'espace dans le journal, utilisez-les seulement pour les transactions ayant un rôle significatif dans la stratégie de récupération de la base de données ;  
  
-   Après la validation d'une transaction marquée, une ligne est insérée dans la table [logmarkhistory](../../relational-databases/system-tables/logmarkhistory-transact-sql.md) de la base de données **msdb**.  
  
-   si une transaction marquée s'étend à plusieurs bases de données sur le même serveur de bases de données ou sur différents serveurs, les marques doivent être enregistrées dans les journaux de toutes les bases de données concernées.  
  
## <a name="creating-the-marked-transactions"></a>Création des transactions marquées  
 Pour créer une transaction marquée, utilisez l’instruction [BEGIN TRANSACTION](../../t-sql/language-elements/begin-transaction-transact-sql.md) et la clause WITH MARK [*description*]. La *description* facultative est une description textuelle de la marque. Un nom de marque pour la transaction est requis. Un nom de marque peut être réutilisé. Le journal des transactions enregistre le nom de la marque, la description, la base de données, l'utilisateur, la date et l'heure, et le numéro séquentiel dans le journal (LSN, Log Sequence Number). Les informations de date et d'heure sont utilisées conjointement avec le nom de la marque pour identifier celle-ci de façon univoque.  
  
 **Pour créer des transactions marquées dans un jeu de bases de données :**  
  
1.  Nommez la transaction dans l'instruction BEGIN TRAN et utilisez la clause WITH MARK.  
  
     Vous pouvez imbriquer l’instruction BEGIN TRAN *nom_nouvelle_marque* WITH MARK au sein d’une transaction existante. La valeur de *nom_nouvelle_marque* correspond au nom de la marque de la transaction, même si la transaction a un nom de transaction.  
  
    > [!NOTE]  
    >  Si vous émettez une deuxième instruction BEGIN TRAN...WITH MARK, celle-ci est ignorée mais génère un message d'avertissement.  
  
2.  Exécutez une mise à jour sur toutes les bases de données dans le jeu.  
  
     La marque d'une transaction spécifique est insérée dans les journaux de transaction uniquement sur l'instance du serveur où est exécutée l'instruction BEGIN TRAN...WITH MARK. La marque de transaction est placée dans le journal de transaction de chaque base de données mise à jour par la transaction marquée sur cette instance du serveur. Si les bases de données résident sur différentes instances du serveur, des marques identiques doivent être créées sur chaque instance du serveur.  
  
### <a name="examples"></a>Exemples  
 L'exemple suivant restaure le journal des transactions jusqu'à la marque dans la transaction marquée nommée `ListPriceUpdate`.  
  
```sql  
USE AdventureWorks  
GO  
BEGIN TRANSACTION ListPriceUpdate  
   WITH MARK 'UPDATE Product list prices';  
GO  
  
UPDATE Production.Product  
   SET ListPrice = ListPrice * 1.10  
   WHERE ProductNumber LIKE 'BK-%';  
GO  
  
COMMIT TRANSACTION ListPriceUpdate;  
GO  
  
-- Time passes. Regular database   
-- and log backups are taken.  
-- An error occurs in the database.  
USE master  
GO  
  
RESTORE DATABASE AdventureWorks  
FROM AdventureWorksBackups  
WITH FILE = 3, NORECOVERY;  
GO  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups   
   WITH FILE = 4,  
   RECOVERY,   
   STOPATMARK = 'ListPriceUpdate';  
```  
  
## <a name="forcing-a-mark-to-spread-to-other-servers"></a>Distribution forcée d'une marque à d'autres serveurs  
 Le nom d'une marque de transaction n’est pas automatiquement distribué à un autre serveur au moment où la transaction atteint ce dernier. Pour forcer la distribution de la marque aux autres serveurs, vous devez écrire une procédure stockée contenant une instruction BEGIN TRAN *nom* WITH MARK. Cette procédure stockée doit ensuite être exécutée sur le serveur distant en fonction de la portée de la transaction dans le serveur d'origine.  
  
 Supposons, par exemple, qu'une base de données partitionnée existe sur plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sur chaque instance se trouve une base de données nommée `coyote`. Créez d’abord une procédure stockée, par exemple `sp_SetMark`, dans chaque base de données.  
  
```sql  
CREATE PROCEDURE sp_SetMark  
@name nvarchar (128)  
AS  
BEGIN TRANSACTION @name WITH MARK  
UPDATE coyote.dbo.Marks SET one = 1  
COMMIT TRANSACTION;  
GO  
```  
  
 Ensuite, créez une procédure stockée `sp_MarkAll` contenant une transaction qui place une marque dans chaque base de données. `sp_MarkAll` peut être exécutée à partir de n’importe quelle instance.  
  
```sql  
CREATE PROCEDURE sp_MarkAll  
@name nvarchar (128)  
AS  
BEGIN TRANSACTION  
EXEC instance0.coyote.dbo.sp_SetMark @name  
EXEC instance1.coyote.dbo.sp_SetMark @name  
EXEC instance2.coyote.dbo.sp_SetMark @name  
COMMIT TRANSACTION;  
GO  
```  
  
### <a name="two-phase-commit"></a>validation en deux temps  
 La validation d'une transaction distribuée s'effectue en deux étapes : préparation et validation. Lorsqu’une transaction marquée est validée, l’enregistrement du journal de validation pour chaque base de données dans la transaction marquée est placé dans le journal à un point où il n’existe aucun doute concernant les transactions dans les journaux. À partir de ce point, il est garanti qu’il n’existe aucune transaction qui apparaît comme validée dans un journal, mais pas dans un autre.  
  
 Les étapes suivantes accomplissent cela lors de la validation d’une transaction marquée :  
  
1.  La phase de préparation d’une transaction marquée bloque toutes les nouvelles préparations et validations.  
  
2.  Seules les validations de transactions déjà préparées sont autorisées à continuer.  
  
3.  Le marquage des transactions attend ensuite toutes les transactions préparées à traiter (avec un délai d’attente).  
  
4.  La transaction marquée est préparée puis validée.  
  
5.  Le blocage des nouvelles préparations et validations est supprimé.  
  
 Ces blocages générés par des transactions marquées qui s’étendent à plusieurs bases de données peuvent atténuer les performances de traitement de transactions sur le serveur.  
  
 Il est recommandé de ne pas exécuter simultanément des transactions marquées. Il est rare mais possible qu’une transaction marquée distribuée en cours de validation se bloque avec d’autres transactions analogues également en cours de validation. Dans ce cas, la transaction marquée est choisie comme victime du blocage et est annulée. Lorsque cette erreur se produit, l’application peut tester à nouveau la transaction marquée. Lorsque plusieurs transactions marquées tentent une validation simultanée, l’éventualité d’un verrouillage est plus élevée.  
  
## <a name="recovering-to-a-marked-transaction"></a>Récupération jusqu'à une transaction marquée  
 Pour plus d’informations sur la manière de récupérer une base de données qui contient des transactions marquées jusqu’à une marque particulière ou juste avant, consultez [Récupération de bases de données associées contenant une transaction marquée](../../relational-databases/backup-restore/recovery-of-related-databases-that-contain-marked-transaction.md).  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Récupération de bases de données associées contenant une transaction marquée](../../relational-databases/backup-restore/recovery-of-related-databases-that-contain-marked-transaction.md)  
  
  
