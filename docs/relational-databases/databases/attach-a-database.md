---
title: Attacher une base de données | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.attachdatabase.f1
helpviewer_keywords:
- database attaching [SQL Server]
- attaching databases [SQL Server]
ms.assetid: b4efb0ae-cfe6-4d81-a4b4-6e4916885caa
caps.latest.revision: 52
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 04097e3b88a7d15f13546eca3f9abffe09d15f19
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34581883"
---
# <a name="attach-a-database"></a>Attacher une base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment attacher une base de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous pouvez utiliser cette fonctionnalité pour copier, déplacer, ou mettre à niveau une base de données SQL Server.  
  
 
  
##  <a name="Prerequisites"></a> Conditions préalables  
  
-   La base de données doit d'abord être détachée. Toute tentative d'attachement d'une base de données qui n'a pas été détachée retourne une erreur. Pour plus d’informations, consultez [Détacher une base de données](../../relational-databases/databases/detach-a-database.md).  
  
-   Lorsque vous attachez une base de données, tous les fichiers de données (fichiers MDF et LDF) doivent être disponibles. Si un fichier de données possède un chemin différent de celui qui existait lorsque la base de données a été créée pour la première fois ou attachée pour la dernière fois, vous devez spécifier le chemin actuel du fichier.  
  
-   Au moment d’attacher une base de données, si les fichiers MDF et LDF se trouvent dans des répertoires différents et qu’un des chemins contient \\\\?\GlobalRoot, l’opération échoue.  
  
###  <a name="Recommendations"></a> La commande Attacher est-elle le meilleur choix ?  
 Nous vous recommandons de déplacer les bases de données à l’aide de la procédure de déplacement planifié ALTER DATABASE, plutôt qu’à l’aide des opérations de détachement et d’attachement, lorsque vous déplacez des fichiers de base de données dans la même instance. Pour plus d’informations, consultez [Déplacer des bases de données utilisateur](../../relational-databases/databases/move-user-databases.md). 
 
Nous ne recommandons pas l’utilisation des opérations de détachement et d’attachement pour la sauvegarde et la récupération. Il n’existe aucune sauvegarde du journal des transactions et il est possible de supprimer accidentellement des fichiers.
  
###  <a name="Security"></a> Sécurité  
 Les autorisations d'accès au fichier sont définies au cours de plusieurs opérations de base de données, notamment le détachement ou l'attachement d'une base de données. Pour plus d’informations sur les autorisations de fichier définies lors du détachement et de l’attachement d’une base de données, consultez [Sécurisation des fichiers de données et des fichiers journaux](http://technet.microsoft.com/library/ms189128.aspx) dans la documentation en ligne de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] (une lecture utile) 
  
 Nous vous recommandons de ne pas attacher ni restaurer de bases de données provenant de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez aussi le code, notamment les procédures stockées ou le code défini par l’utilisateur, de la base de données. Pour plus d’informations sur l’attachement de bases de données et sur les modifications apportées aux métadonnées à cette occasion, consultez [Attacher et détacher une base de données (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md).  
  
####  <a name="Permissions"></a> Permissions  
 L'autorisation CREATE DATABASE, CREATE ANY DATABASE ou ALTER ANY DATABASE est obligatoire.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-attach-a-database"></a>Pour attacher une base de données  
  
1.  Dans l’Explorateur d’objets [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], puis cliquez pour développer cette vue d’instance dans SSMS.  
  
2.  Cliquez avec le bouton droit sur **Bases de données** , puis cliquez sur **Attacher**.  
  
3.  Dans la boîte de dialogue **Attacher des bases de données** , pour spécifier la base de données à attacher, cliquez sur **Ajouter**, dans la boîte de dialogue **Rechercher les fichiers de base de données** , sélectionnez l'unité de disque contenant la base de données et développez l'arborescence pour rechercher et sélectionner le fichier .mdf de la base de données. Par exemple :  
  
     `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\AdventureWorks2012_Data.mdf`  
  
    > [!IMPORTANT]  
    >  Une tentative de sélection d'une base de données déjà attachée génère une erreur.  
  
     **Bases de données à attacher**  
     Affiche des informations sur les bases de données sélectionnées.  
  
     \<aucun en-tête de colonne>  
     Affiche une icône indiquant l'état de l'opération d'attachement. Les icônes possibles sont décrites dans la section **État** ci-dessous.  
  
     **Emplacement du fichier MDF**  
     Affiche le chemin d'accès et le nom du fichier MDF sélectionné.  
  
     **Database Name**  
     Affiche le nom de la base de données.  
  
     **Attacher en tant que**  
     Permet de spécifier éventuellement un autre nom sous lequel la base de données doit être attachée.  
  
     **Propriétaire**  
     Fournit une liste déroulante des propriétaires de bases de données possibles dans laquelle vous pouvez sélectionner un autre propriétaire.  
  
     **État**  
     Affiche l'état de la base de données conformément au tableau ci-après.  
  
    |Icône|Texte d'état|Description|  
    |----------|-----------------|-----------------|  
    |(Aucune icône)|(Aucun texte)|L'opération d'attachement n'a pas démarré ou est peut-être en attente pour cet objet. Il s'agit de la valeur par défaut lorsque la boîte de dialogue est ouverte.|  
    |Triangle vert dirigé vers la droite|En cours|L'opération d'attachement a démarré, mais n'est pas terminée.|  
    |Coche verte|Réussi|L'attachement de l'objet a réussi.|  
    |Cercle rouge contenant une croix blanche|Error|L'opération d'attachement a rencontré une erreur et ne s'est pas terminée correctement.|  
    |Cercle contenant deux quartiers noirs (à gauche et à droite) et deux quartiers blancs (en haut et en bas)|Arrêté|L'opération d'attachement n'a pas réussi, car l'utilisateur l'a interrompue.|  
    |Cercle contenant une flèche courbe pointant dans le sens inverse des aiguilles d'une montre|Restauré|L'opération d'attachement a réussi, mais a été restaurée en raison d'une erreur lors de l'attachement d'un autre objet.|  
  
     **Message**  
     Affiche un message vierge ou un lien hypertexte « Fichier introuvable ».  
  
     **Ajouter**  
     Permet de rechercher les principaux fichiers de base de données nécessaires. Lorsque l'utilisateur sélectionne un fichier .mdf, les informations applicables sont automatiquement remplies dans les champs respectifs de la grille **Bases de données à attacher** .  
  
     **Supprimer**  
     Supprime le fichier sélectionné de la grille **Bases de données à attacher** .  
  
     **"** *<database_name>* **»détails de la base de données**  
     Affiche le nom des fichiers à attacher. Pour vérifier ou modifier le nom du chemin d’accès d’un fichier, cliquez sur le bouton **Parcourir** (**…**).  
  
    > [!NOTE]  
    >  Si un fichier n'existe pas, la colonne **Message** affiche « Introuvable ». Si un fichier journal est introuvable, cela signifie qu'il se trouve dans un autre répertoire ou qu'il a été supprimé. Vous devez mettre à jour le chemin d'accès du fichier dans la grille **Détails de la base de données** pour désigner l'emplacement correct ou supprimer le fichier journal de la grille. Si un fichier de données .ndf est introuvable, vous devez mettre à jour son chemin d'accès dans la grille pour désigner l'emplacement correct.  
  
     **Nom du fichier d'origine**  
     Affiche le nom du fichier attaché appartenant à la base de données.  
  
     **Type de fichier**  
     Indique le type du fichier, **Données** ou **Journal**.  
  
     **Chemin d'accès au fichier actuel**  
     Affiche le chemin d'accès au fichier de base de données sélectionné. Le chemin d'accès peut être modifié manuellement.  
  
     **Message**  
     Affiche un message vierge ou un lien hypertexte «**Fichier introuvable**».  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-attach-a-database"></a>Pour attacher une base de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Utilisez l'instruction [CREATE DATABASE](../../t-sql/statements/create-database-sql-server-transact-sql.md) avec la clause FOR ATTACH.  
  
     Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple attache les fichiers de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et renomme la base de données `MyAdventureWorks`.  
  
    ```  
    CREATE DATABASE MyAdventureWorks   
        ON (FILENAME = 'C:\MySQLServer\AdventureWorks_Data.mdf'),   
        (FILENAME = 'C:\MySQLServer\AdventureWorks_Log.ldf')   
        FOR ATTACH;  
  
    ```  
  
    > [!NOTE]  
    >  Vous pouvez aussi utiliser la procédure stockée [sp_attach_db](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md) ou [sp_attach_single_file_db](../../relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql.md) . Toutefois, ces procédures seront supprimées dans une future version de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Il est conseillé d'utiliser plutôt CREATE DATABASE … FOR ATTACH.  
  
##  <a name="FollowUp"></a> Suivi : Après la mise à niveau d'une base de données SQL Server  
 Après avoir mis à niveau une base de données à l'aide de la méthode par attachement, cette base de données est immédiatement disponible et est automatiquement mise à niveau. Si la base de données comprend des index de recherche en texte intégral, la mise à niveau les importe, les réinitialise ou les reconstruit, selon le paramètre de la propriété de serveur **Option de mise à niveau du catalogue de texte intégral** . Si l’option de mise à niveau est définie sur **Importer** ou **Reconstruire**, les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez aussi que lorsque l’option de mise à niveau est définie sur **Importer**, si aucun catalogue de texte intégral n’est disponible, les index de recherche en texte intégral associés sont reconstruits.  
  
 Si le niveau de compatibilité d'une base de données utilisateur est à 100 ou supérieur avant la mise à niveau, il reste le même après la mise à niveau. Si le niveau de compatibilité était à 90 avant la mise à niveau, dans la base de données mise à niveau, le niveau de compatibilité est défini à 100, ce qui correspond au niveau de compatibilité le plus bas pris en charge dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
  > [!NOTE]
  > Si vous attachez une base de données située sur une instance qui exécute SQL Server 2014 ou antérieur, et sur laquelle la capture de données modifiées est activée, vous devez également exécuter la commande suivante pour mettre à niveau les métadonnées de la capture de données modifiées.
  ```
  USE <database name>
  EXEC sys.sp_cdc_vupgrade  
  ``` 
  
## <a name="see-also"></a> Voir aussi  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md) 
 <br>[Gérer les métadonnées lors de la mise à disposition d’une base de données sur un autre serveur](manage-metadata-when-making-a-database-available-on-another-server.md)  
 [Détacher une base de données](../../relational-databases/databases/detach-a-database.md)  
  
  
