---
title: Mettre à niveau une base de données avec Detach et Attach (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- database attaching [SQL Server]
- upgrading databases
- database upgrades [SQL Server]
- database detaching [SQL Server]
- detaching databases [SQL Server]
- attaching databases [SQL Server]
ms.assetid: 99f66ed9-3a75-4e38-ad7d-6c27cc3529a9
caps.latest.revision: 73
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 28f1aefd634978deeac6d6e9f14a1a102a8e8fbb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="upgrade-a-database-using-detach-and-attach-transact-sql"></a>Mettre à niveau une base de données avec Detach et Attach (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment utiliser les opérations d'attachement et de détachement pour mettre à niveau une base de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Une fois attachée à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], la base de données est immédiatement disponible et est automatiquement mise à niveau.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
-   **Pour mettre à niveau une base de données SQL Server :**  
  
     [Utilisation des opérations de détachement et d'attachement](#SSMSProcedure)  
  
-   **Suivi :**  [Après la mise à niveau d'une base de données SQL Server](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Les bases de données système ne peuvent pas être attachées.  
  
-   Attach et Detach désactivent le chaînage des propriétés des bases de données croisées pour la base de données en affectant la valeur 0 à l’option **cross db ownership chaining** . Pour plus d’informations sur l’activation du chaînage, consultez [Chaînage des propriétés des bases de données croisées (option de configuration de serveur)](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md).  
  
-   Si vous attachez une base de données répliquée qui a été copiée et non pas détachée :  
  
    -   Si vous attachez la base de données à une version mise à niveau de la même instance de serveur, vous devez exécuter **sp_vupgrade_replication** pour mettre à niveau la réplication au terme de l’opération d’attachement. Pour plus d’informations, consultez [sp_vupgrade_replication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md).  
  
    -   Si vous attachez la base de données à une instance de serveur différente (quelle que soit sa version), vous devez exécuter **sp_removedbreplication** pour supprimer la réplication au terme de l’opération d’attachement. Pour plus d’informations, consultez [sp_removedbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
###  <a name="Recommendations"></a> Recommandations  
 Nous vous recommandons de ne pas attacher ni restaurer de bases de données provenant de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez également le code, notamment les procédures stockées ou le code défini par l’utilisateur, de la base de données.  
  
##  <a name="SSMSProcedure"></a> Pour mettre à niveau une base de données à l'aide des opérations de détachement et d'attachement  
  
1.  Détachez la base de données. Pour plus d’informations, consultez [Détacher une base de données](../../relational-databases/databases/detach-a-database.md).  
  
2.  Éventuellement, déplacez le ou les fichiers de base de données détachés et le ou les fichiers journaux.  
  
     Vous devez déplacer les fichiers journaux ainsi que les fichiers de données, même si vous souhaitez créer de nouveaux fichiers journaux. Dans certains cas, le rattachement d'une base de données nécessite ses fichiers journaux existants. Par conséquent, conservez toujours tous les fichiers journaux détachés jusqu'à ce que la base de données ait été attachée avec succès sans eux.  
  
    > [!NOTE]  
    >  Si vous tentez d'attacher la base de données sans spécifier le fichier journal, l'opération attach recherche le fichier journal à son emplacement d'origine. Si la copie d'origine du journal figure toujours dans cet emplacement, la copie est attachée. Pour éviter d'utiliser le fichier journal d'origine, spécifiez le chemin d'accès au nouveau fichier journal ou supprimez la copie d'origine du fichier journal (après l'avoir copiée au nouvel emplacement).  
  
3.  Attachez les fichiers copiés à l'instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d’informations, consultez [Attach a Database](../../relational-databases/databases/attach-a-database.md).  
  
## <a name="example"></a> Exemple  
 L'exemple suivant met à niveau une copie d'une base de données à partir d'une version antérieure de SQL Server. Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont exécutées dans une fenêtre d'éditeur de requêtes connectée à l'instance de serveur concernée par l'attachement.  
  
1.  Détachez la base de données en exécutant les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
    ```  
    USE master;  
    GO  
    EXEC sp_detach_db @dbname = N'MyDatabase';  
    GO  
    ```  
  
2.  À l'aide de la méthode de votre choix, copiez les fichiers de données et les fichiers journaux au nouvel emplacement.  
  
    > [!IMPORTANT]  
    >  Dans le cas d'une base de données de production, placez la base de données et le journal des transactions sur des disques distincts.  
  
     Pour copier des fichiers via le réseau sur le disque d'un ordinateur distant, utilisez le nom UNC (Universal Naming Convention) de l'emplacement distant. Un nom UNC se présente sous la forme **\\\\***nom_serveur***\\***nom_partage***\\***chemin***\\***nom_fichier*. Comme lors de l'écriture de fichiers sur le disque dur local, les autorisations appropriées nécessaires à la lecture et à l'écriture d'un fichier sur le disque distant doivent être accordées au compte d'utilisateur utilisé par l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Attachez la base de données déplacée, puis, si vous le souhaitez, son journal, en exécutant l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```  
    USE master;  
    GO  
    CREATE DATABASE MyDatabase   
        ON (FILENAME = 'C:\MySQLServer\MyDatabase.mdf'),  
        (FILENAME = 'C:\MySQLServer\Database.ldf')  
        FOR ATTACH;  
    GO  
    ```  
  
     Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], une base de données nouvellement attachée n'est pas immédiatement visible dans l'Explorateur d'objets. Pour visualiser la base de données, dans l'Explorateur d'objets, cliquez sur **Affichage** puis sur **Actualiser**. Si le nœud **Bases de données** est développé dans l'Explorateur d'objets, la base de données récemment attachée apparaît dans la liste des bases de données.  
  
##  <a name="FollowUp"></a> Suivi : Après la mise à niveau d'une base de données SQL Server  
 Si la base de données comprend des index de recherche en texte intégral, la mise à niveau les importe, les réinitialise ou les reconstruit, selon le paramètre de la propriété de serveur **upgrade_option** . Si l’option de mise à niveau a la valeur Importer (**upgrade_option** = 2) ou Reconstruire (**upgrade_option** = 0), les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que lorsque l'option de mise à niveau est Importer, les index de recherche en texte intégral associés sont reconstruits si aucun catalogue de texte intégral n'est disponible. Pour modifier le paramètre de la propriété de serveur **upgrade_option** , utilisez [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).  
  
### <a name="database-compatibility-level-after-upgrade"></a>Niveau de compatibilité des bases de données après une mise à niveau  
 Si le niveau de compatibilité d'une base de données utilisateur est à 100 ou supérieur avant la mise à niveau, il reste le même après la mise à niveau. Si le niveau de compatibilité était à 90 avant la mise à niveau dans la base de données mise à niveau, le niveau de compatibilité est défini à 100, ce qui correspond au niveau de compatibilité le plus bas pris en charge dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
### <a name="managing-metadata-on-the-upgraded-server-instance"></a>Gestion des métadonnées sur l'instance de serveur mise à niveau  
 Lorsque vous attachez une base de données à une autre instance de serveur et si vous souhaitez offrir une expérience cohérente aux utilisateurs et aux applications, il est possible que vous deviez recréer une partie ou l'ensemble des métadonnées de la base de données, telles que les connexions, les travaux, et les autorisations sur cette autre instance de serveur. Pour plus d’informations, consultez [Gérer les métadonnées durant la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
### <a name="service-master-key-and-database-master-key-encryption-changes-from-3des-to-aes"></a>Modifications du chiffrement de la clé principale du service et de la clé principale de la base de données de 3DES à AES  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures utilise l’algorithme de chiffrement AES pour protéger la clé principale du service (SMK) et la clé principale de base de données (DMK). AES est un algorithme de chiffrement plus récent que 3DES, qui était utilisé dans les versions antérieures. Lorsqu'une base de données est attachée ou restaurée pour la première fois à une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une copie de la clé principale de la base de données (chiffrée par la clé principale du service) n'est pas encore stockée sur le serveur. Vous devez utiliser l’instruction **OPEN MASTER KEY** pour déchiffrer la clé principale de la base de données. Une fois la clé principale de la base de données déchiffrée, vous avez la possibilité d’activer le déchiffrement automatique dans le futur en exécutant l’instruction **ALTER MASTER KEY REGENERATE** pour fournir au serveur une copie de la clé principale de la base de données chiffrée avec la clé principale du service. Lorsqu'une base de données a été mise à niveau à partir d'une version antérieure, la clé DMK doit être régénérée de façon à utiliser le nouvel algorithme AES. Pour plus d’informations sur la régénération de la clé DMK, consultez [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md). La durée nécessaire pour régénérer la clé DMK à mettre à niveau vers AES dépend du nombre d'objets protégés par la clé DMK. La régénération de la clé DMK à mettre à niveau vers AES est nécessaire une seule fois et n'a aucune incidence sur les régénérations ultérieures effectuées dans le cadre d'une stratégie de rotation de clés.  
  
  
