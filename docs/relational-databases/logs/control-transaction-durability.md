---
title: Contrôler la durabilité d’une transaction | Microsoft Docs
ms.custom: ''
ms.date: 09/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- delayed durability
- Lazy Commit
ms.assetid: 3ac93b28-cac7-483e-a8ab-ac44e1cc1c76
caps.latest.revision: 27
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f19aea56e2cf892c363f057045914f661185513f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="control-transaction-durability"></a>Contrôler la durabilité d'une transaction
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Les validations de transactions[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent avoir une durabilité complète, la durabilité par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une durabilité retardée (également appelée Validation différée).    
    
 Les validations de transactions à durabilité complète sont synchrones. Elles signalent la réussite de la validation (COMMIT) et restituent le contrôle au client uniquement lorsque les enregistrements de journal de transactions ont été écrits sur le disque. Les validations de transactions à durabilité retardée sont asynchrones. Elles signalent la réussite de la validation (COMMIT) avant que les enregistrements de journal de transactions ne soient écrits sur le disque. L'écriture des entrées du journal des transactions sur le disque est nécessaire pour qu'une transaction soit durable. Les transactions à durabilité retardée deviennent durables lorsque les enregistrements de journal de transactions sont vidés sur le disque.    
    
 Cette rubrique décrit en détail les transactions à durabilité retardée.    
    
## <a name="full-vs-delayed-transaction-durability"></a>Durabilité complète/ retardée des transactions    
 La durabilité complète et la durabilité retardée des transactions présentent chacune des avantages et des inconvénients. Une application peut comporter à la fois des transactions à durabilité complète et à durabilité retardée. Vous devez évaluer soigneusement vos besoins professionnels et déterminer de quelle manière chaque méthode peut y répondre.    
    
### <a name="full-transaction-durability"></a>Durabilité complète des transactions    
 Les transactions à durabilité complète écrivent la sécurisation du journal des transactions sur le disque avant de restituer le contrôle au client. Vous devez utiliser des transactions à durabilité complète dans les cas suivants :    
    
-   Le système ne tolère aucune perte de données.     
    Consultez la section [Quand puis-je perdre des données ?](../../relational-databases/logs/control-transaction-durability.md#bkmk_DataLoss) pour obtenir des informations sur la perte de certaines données.    
    
-   Le goulot d'étranglement n'est pas attribuable à une latence d'écriture du journal des transactions.    
    
 La durabilité retardée des transactions réduit la latence attribuable aux E/S du journal, car elle permet de conserver les enregistrements du journal de transactions en mémoire et d'écrire ces enregistrements par lots, ce qui nécessite moins d'opérations d'E/S. La durabilité différée des transactions réduit potentiellement les contentions d'E/S du journal, et diminue de fait les temps d'attente dans le système.    
    
 **Garanties offertes par la durabilité complète des transactions**    
    
-   Une fois qu'une transaction a été validée, les modifications apportées par la transaction sont visibles par les autres transactions du système. Pour plus d’informations sur les niveaux d’isolation des transactions, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md) ou [Transactions avec des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md).    
    
-   La durabilité est garantie lors de la validation. Les enregistrements de journal correspondants sont conservés sur le disque jusqu'à ce que la transaction soit validée et que le contrôle soit restitué au client.    
    
### <a name="delayed-transaction-durability"></a>Durabilité retardée des transactions    
 La durabilité retardée des transactions est obtenue par des écritures asynchrones d'enregistrements de journal sur le disque. Les enregistrements du journal des transactions sont conservés dans une mémoire tampon et écrits sur le disque lorsque la mémoire tampon se remplit ou qu'un événement de vidage de la mémoire tampon a lieu. La durabilité retardée des transactions réduit à la fois la latence et les contentions dans le système pour les raisons suivantes :    
    
-   Les validations de transactions sont effectuées sans attendre la fin des E/S du journal et la restitution du contrôle au client.    
    
-   Les transactions concomitantes sont moins susceptibles d'entrer en conflit pour les E/S du journal ; à la place, le tampon journal peut être vidé sur le disque sous forme de blocs plus volumineux, ce qui réduit les risques de contention et augmente le débit.    
    
    > [!NOTE]    
    >  Vous pouvez toujours assister à une contention d'E/S du journal s'il existe un niveau élevé d'accès concurrentiel, en particulier si vous remplissez le tampon journal plus vite que vous ne le videz.    
    
 #### <a name="when-to-use-delayed-transaction-durability"></a>Circonstances dans lesquelles utiliser la durabilité retardée des transactions    
    
 Voici quelques-uns des cas dans lesquels vous pouvez tirer parti de l'utilisation de la durabilité retardée des transactions :    
    
 **Vous tolérez la perte de certaines données.**    
 Si vous tolérez la perte de certaines données, par exemple des enregistrements qui ne sont pas indispensables tant que vous disposez de la majeure partie des données, il peut être intéressant d'utiliser la durabilité retardée. Si, en revanche, vous ne tolérez aucune perte de données, n'utilisez pas la durabilité retardée des transactions.    
    
 **Vous rencontrez un goulot d’étranglement au niveau des écritures du journal de transactions.**    
 Si les problèmes de performances sont attribuables à une latence dans les écritures du journal de transactions, votre application bénéficiera probablement de l'utilisation de la durabilité retardée des transactions.    
    
 **Vos charges de travail présentent un niveau de contention élevé.**    
 Si votre système a des charges de travail présentant un haut degré de contention, beaucoup de temps est perdu à attendre la libération de verrous. La durabilité retardée des transactions réduit la durée de validation. De ce fait, les verrous sont libérés plus rapidement, ce qui se traduit par un débit supérieur.    
    
 ### <a name="delayed-transaction-durability-guarantees"></a>Garanties offertes par la durabilité retardée des transactions   
    
-   Une fois qu'une transaction a été validée, les modifications apportées par la transaction sont visibles par les autres transactions du système.    
    
-   La durabilité des transactions est garantie uniquement après un vidage du journal des transactions en mémoire sur le disque. Le journal des transactions en mémoire est vidé sur le disque dans les circonstances suivantes :    
    
    -   Une transaction à durabilité complète dans la même base de données apporte une modification dans la base de données, puis est validée.   
    
    -   L'utilisateur exécute la procédure stockée système `sp_flush_log` avec succès.     
    
        Si une transaction à durabilité complète ou un sp_flush_log est validé avec succès, toutes les transactions à durabilité retardée déjà validées sont assurées d’être rendues durables.
        
    - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de vider le journal sur le disque en se basant sur la génération du journal et sur le minutage, même si toutes les transactions sont à durabilité différée. Cette opération réussit généralement si le périphérique d’E/S suit le rythme. Cependant, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne fournit pas de garantie solide de durabilité autre que les transactions durables et sp_flush_log.      
    
  
    
## <a name="how-to-control-transaction-durability"></a>Procédure pour contrôler la durabilité d'une transaction    
    
###  <a name="bkmk_DbControl"></a> Contrôle au niveau de la base de données    
 En tant qu'administrateur de base de données, vous pouvez contrôler si les utilisateurs peuvent utiliser la durabilité retardée des transactions sur une base de données avec l'instruction suivante. Vous devez définir le paramètre de durabilité retardée avec ALTER DATABASE.    
    
```sql    
ALTER DATABASE … SET DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }    
```    
    
 **DISABLED**    
 [Valeur par défaut] Avec ce paramètre, toutes les transactions qui sont validées sur la base de données ont une durabilité complète, quel que soit le paramètre de niveau de validation ([DELAYED_DURABILITY= ON | OFF]). Aucune modification ni recompilation de procédure stockée n'est nécessaire. Cela vous permet de vous assurer que les données ne sont jamais mises en danger par la durabilité retardée.    
    
 **ALLOWED**    
 Avec ce paramètre, la durabilité de chaque transaction est déterminée au niveau de la transaction – DELAYED_DURABILITY = { *OFF* | ON }. Pour plus d’informations, consultez [Contrôle au niveau du bloc atomique – Procédures stockées compilées en mode natif](../../relational-databases/logs/control-transaction-durability.md#CompiledProcControl) et [Contrôle au niveau de la validation – Transact-SQL](../../relational-databases/logs/control-transaction-durability.md#bkmk_T-SQLControl) .    
    
 **FORCED**    
 Avec ce paramètre, chaque transaction qui est validée sur la base de données a une durabilité retardée, même si la transaction spécifie une durabilité complète (DELAYED_DURABILITY = OFF) ou omette toute indication de durabilité. Ce paramètre est utile lorsque la durabilité retardée des transactions a un intérêt pour une base de données, mais que vous ne souhaitez pas modifier le code de l'application.    
    
###  <a name="CompiledProcControl"></a> Contrôle au niveau du bloc atomique – Procédures stockées compilées en mode natif    
 Le code suivant s'insère à l'intérieur du bloc atomique.    
    
```sql    
DELAYED_DURABILITY = { OFF | ON }    
```    
    
 **OFF**    
 [Valeur par défaut] La transaction a une durabilité complète, sauf si l'option de base de données DELAYED_DURABLITY = FORCED est activée, auquel cas la validation COMMIT est asynchrone et donc à durabilité retardée. Pour plus d’informations, consultez [Contrôle au niveau de la base de données](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) .    
    
 **ON**    
 La transaction a une durabilité retardée, sauf si l'option de base de données DELAYED_DURABLITY = DISABLED est activée, auquel cas la validation COMMIT est synchrone et donc à durabilité complète.  Pour plus d’informations, consultez [Contrôle au niveau de la base de données](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) .    
    
 **Exemple de code :**    
    
```sql    
CREATE PROCEDURE <procedureName> …    
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER    
AS BEGIN ATOMIC WITH     
(    
    DELAYED_DURABILITY = ON,    
    TRANSACTION ISOLATION LEVEL = SNAPSHOT,    
    LANGUAGE = N'English'    
    …    
)    
END    
```    
    
### <a name="table-1-durability-in-atomic-blocks"></a>Tableau 1 : durabilité dans les blocs atomiques    
    
|Option de durabilité de bloc atomique|Aucune transaction existante|Transaction en cours (à durabilité complète ou retardée)|    
|------------------------------------|-----------------------------|---------------------------------------------------------|    
|**DELAYED_DURABILITY = OFF**|Le bloc atomique démarre une nouvelle transaction à durabilité complète.|Le bloc atomique crée un point d'enregistrement dans la transaction existante, puis démarre la nouvelle transaction.|    
|**DELAYED_DURABILITY = ON**|Le bloc atomique démarre une nouvelle transaction à durabilité retardée.|Le bloc atomique crée un point d'enregistrement dans la transaction existante, puis démarre la nouvelle transaction.|    
    
###  <a name="bkmk_T-SQLControl"></a> Contrôle au niveau de la validation (COMMIT) –[!INCLUDE[tsql](../../includes/tsql-md.md)]    
 La syntaxe de l'option COMMIT est étendue pour vous permettre de forcer la durabilité retardée des transactions. Si DELAYED_DURABILITY a la valeur DISABLED ou FORCED au niveau de la base de données (voir ci-dessus), cette option COMMIT est ignorée.    
    
```sql    
COMMIT [ { TRAN | TRANSACTION } ] [ transaction_name | @tran_name_variable ] ] [ WITH ( DELAYED_DURABILITY = { OFF | ON } ) ]    
    
```    
    
 **OFF**    
 [Valeur par défaut] La validation COMMIT de la transaction a une durabilité complète, sauf si l'option de base de données DELAYED_DURABLITY = FORCED est activée, auquel cas la validation COMMIT est asynchrone et donc à durabilité retardée. Pour plus d’informations, consultez [Contrôle au niveau de la base de données](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) .    
    
 **ON**    
 La validation COMMIT de la transaction a une durabilité retardée, sauf si l'option de base de données DELAYED_DURABLITY = DISABLED est activée, auquel cas la validation COMMIT est synchrone et donc à durabilité complète. Pour plus d’informations, consultez [Contrôle au niveau de la base de données](../../relational-databases/logs/control-transaction-durability.md#bkmk_DbControl) .    
    
### <a name="summary-of-options-and-their-interactions"></a>Récapitulatif des options et de leurs interactions    
 Ce tableau récapitule les interactions entre les paramètres de durabilité retardée au niveau de la base de données et les paramètres au niveau de la validation. Les paramètres au niveau de la base de données ont toujours priorité sur les paramètres au niveau de la validation.    
    
|Paramètre de validation (COMMIT)/Paramètre de base de données|DELAYED_DURABILITY = DISABLED|DELAYED_DURABILITY = ALLOWED|DELAYED_DURABILITY = FORCED|    
|--------------------------------------|-------------------------------------|------------------------------------|-----------------------------------|    
|**DELAYED_DURABILITY = OFF** Transactions au niveau de la base de données.|La transaction a une durabilité complète.|La transaction a une durabilité complète.|La transaction a une durabilité retardée.|    
|**DELAYED_DURABILITY = ON** Transactions au niveau de la base de données.|La transaction a une durabilité complète.|La transaction a une durabilité retardée.|La transaction a une durabilité retardée.|    
|**DELAYED_DURABILITY = OFF** Transaction distribuée ou de bases de données croisées.|La transaction a une durabilité complète.|La transaction a une durabilité complète.|La transaction a une durabilité complète.|    
|**DELAYED_DURABILITY = ON** Transaction distribuée ou de bases de données croisées.|La transaction a une durabilité complète.|La transaction a une durabilité complète.|La transaction a une durabilité complète.|    
    
## <a name="how-to-force-a-transaction-log-flush"></a>Procédure pour forcer le vidage du journal de transactions    
 Il existe deux moyens pour forcer le vidage du journal de transactions sur le disque.    
    
-   Exécutez chaque transaction à durabilité complète qui modifie la même base de données. De cette façon, vous forcez le vidage sur le disque des enregistrements de journal de toutes les transactions à durabilité retardée déjà validées.    
    
-   Exécutez la procédure stockée système `sp_flush_log`. Cette procédure force le vidage sur le disque des enregistrements de journal de toutes les transactions à durabilité retardée déjà validées. Pour plus d’informations, consultez [sys.sp_flush_log &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-flush-log-transact-sql.md).    
    
##  <a name="bkmk_OtherSQLFeatures"></a> Durabilité différée et autres fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]    
 **Suivi des modifications et capture de données modifiées**    
 Toutes les transactions avec suivi des modifications ont une durabilité complète. Une transaction présente la propriété de suivi des modifications si elle effectue des opérations d’écriture dans des tables qui sont activées pour le suivi des modifications. L’utilisation de la durabilité différée n’est pas prise en charge pour les bases de données qui utilisent la capture de données modifiées (CDC).    
    
 **Récupération sur incident**    
 La cohérence est garantie, mais certaines modifications des transactions à durabilité retardée qui ont été validées peuvent être perdues.    
    
 **Bases de données croisées et DTC**    
 Les transactions distribuées ou de bases de données croisées ont une durabilité complète, quel que soit le paramètre de validation des bases de données ou des transactions.    
    
 **Groupes de disponibilité Always On et mise en miroir**    
 Les transactions à durabilité retardée ne garantissent aucune durabilité sur le réplica principal ni sur les réplicas secondaires. En outre, elles ne garantissent aucune information sur la transaction au niveau du réplica secondaire. Après une validation (COMMIT), le contrôle est restitué au client avant qu'un accusé de réception (ACK) ne soit reçu d'un réplica synchrone secondaire. La réplication sur les réplicas secondaires se poursuit lors du vidage sur disque sur le réplica principal.   
    
 **Clustering de basculement**    
 Certaines écritures de transactions à durabilité retardée peuvent être perdues.    
    
 **Réplication de transaction**    
 Les transactions durables différées ne sont pas prises en charge avec la réplication transactionnelle.    
    
 **Copie des journaux de transaction**    
 Seules les transactions rendues durables sont incluses dans le journal qui est copié.    
    
 **Sauvegarde de journal**    
 Seules les transactions rendues durables sont incluses dans la sauvegarde.    
    
##  <a name="bkmk_DataLoss"></a> Quand puis-je perdre des données ?    
 Si vous implémentez la durabilité retardée sur l'une de vos tables, certaines circonstances peuvent entraîner une perte de données. Si vous ne pouvez pas vous permettre de perdre des données, n'utilisez pas la durabilité retardée sur les tables.    
    
### <a name="catastrophic-events"></a>Événements graves    
 Dans le cas d'événements graves, par exemple une défaillance du serveur, vous pouvez perdre les données de toutes les transactions validées qui n'ont pas été enregistrées sur le disque. Les transactions durables retardées sont enregistrées sur le disque quand une transaction à durabilité complète est exécutée sur une table (durable optimisé en mémoire ou sur disque) dans la base de données, ou quand `sp_flush_log` est appelé. Si vous utilisez des transactions durables retardées, vous pouvez créer une petite table dans la base de données afin de mettre à jour ou d'appeler périodiquement `sp_flush_log` pour enregistrer toutes les transactions validées en attente. Le journal des transactions est également vidé quand il est plein, mais cela est difficile à prévoir et impossible à contrôler.    
    
### <a name="includessnoversionincludesssnoversion-mdmd-shutdown-and-restart"></a>Arrêt et redémarrage de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]     
 Pour une durabilité retardée, il n'y a pas de différence entre un arrêt inattendu et un arrêt/redémarrage attendu de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Comme pour les événements graves, vous devez prévoir une perte de données. Dans un arrêt/redémarrage planifié, certaines transactions qui n'ont pas été écrites sur disque peuvent tout d'abord l'être, mais vous ne pouvez pas planifier cela. La planification comme pour un arrêt/redémarrage, planifiée ou non, entraîne la perte de données comme pour un événement grave.    
    
## <a name="see-also"></a> Voir aussi    
 [Transactions avec tables optimisées en mémoire](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)    
    
  
