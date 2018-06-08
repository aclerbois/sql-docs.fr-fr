---
title: Créer une sauvegarde complète de base de données (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- backing up databases [SQL Server], full backups
- backing up databases [SQL Server], SQL Server Management Studio
- backups [SQL Server], creating
- database backups [SQL Server], SQL Server Management Studio
ms.assetid: 586561fc-dfbb-4842-84f8-204a9100a534
caps.latest.revision: 63
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 0cdafe9c854ccce0dd554d52afb850b39c97a7d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-full-database-backup-sql-server"></a>Créer une sauvegarde complète de base de données (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
 > Pour SQL Server 2014, accédez à [Créer une sauvegarde complète de base de données (SQL Server)](https://msdn.microsoft.com/en-US/library/ms187510(SQL.120).aspx).

  Cette rubrique explique comment créer une sauvegarde de base de données complète dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou de PowerShell.  
  
>  Pour plus d’informations sur la sauvegarde SQL Server dans le service Stockage Blob Azure, consultez [Sauvegarde et restauration SQL Server avec le service Stockage Blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) et [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer 
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   L'instruction BACKUP n'est pas autorisée dans une transaction explicite ou implicite.  
  
-   Les sauvegardes créées avec une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Pour obtenir une vue d’ensemble et approfondir vos connaissances des concepts de sauvegarde et des tâches, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md) avant de continuer.  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   À mesure que la taille d’une base de données augmente, les sauvegardes complètes de base de données nécessitent davantage de temps et d’espace de stockage. Pour les bases de données volumineuses, songez à compléter les sauvegardes complètes avec une série de [sauvegardes différentielles de base de données](../../relational-databases/backup-restore/differential-backups-sql-server.md). Pour plus d’informations, consultez [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
-   Vous pouvez estimer la taille d’une sauvegarde complète de base de données en utilisant la procédure stockée système [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) .  
  
-   Par défaut, chaque opération de sauvegarde réussie ajoute une entrée au journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et au journal des événements système. Si vous effectuez régulièrement une sauvegarde, ces messages de réussite s’accumuleront rapidement et vos journaux d’erreurs deviendront énormes. Cela peut rendre la recherche d’autres messages difficile. Dans ces cas-là, vous pouvez supprimer ces entrées de journaux de sauvegarde en utilisant l’indicateur de trace 3226 si aucun de vos scripts ne dépend de ces entrées. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  
  
###  <a name="Security"></a> Sécurité  
 TRUSTWORTHY a la valeur OFF pour une sauvegarde de base de données. Pour plus d’informations sur la façon d’affecter la valeur ON à TRUSTWORTHY, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , les options **PASSWORD** et **MEDIAPASSWORD** sont suspendues pour la création de sauvegardes. Vous pouvez toujours restaurer les sauvegardes créées avec des mots de passe.  
  
####  <a name="Permissions"></a> Permissions  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .  
  
 Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit pouvoir lire et écrire sur l’unité. Le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute **doit** avoir des autorisations d’écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. De tels problèmes pour le fichier physique de l'unité de sauvegarde peuvent n'apparaître que lorsque la ressource physique est sollicitée au moment de la sauvegarde ou de la restauration.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
>  Si vous spécifiez une tâche de sauvegarde à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez générer le script [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) correspondant en cliquant sur le bouton **Script** et en sélectionnant une destination de script.  
  
### <a name="back-up-a-database"></a>Sauvegarder une base de données  
  
1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)][!INCLUDE[msCoName](../../includes/msconame-md.md)], dans **l’Explorateur d’objets**, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données**, puis sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**. La boîte de dialogue **Sauvegarder la base de données** s'affiche.  

  #### <a name="general-page"></a>**Page Général**
  
4.  Dans la liste déroulante **Base de données** , vérifiez le nom de la base de données. Vous avez la possibilité de sélectionner une autre base de données dans la liste.  
  
5.  La zone de texte **Mode de récupération** est fournie à titre de référence uniquement.  Vous pouvez effectuer une sauvegarde de base de données pour tout mode de récupération (**FULL**, **BULK_LOGGED**ou **SIMPLE**).  
  
6.  Dans la liste déroulante **Type de sauvegarde** , sélectionnez **Complète**.  
  
     Notez qu’après la création d’une sauvegarde de base de données complète, vous pouvez créer une sauvegarde de base de données différentielle. Pour plus d’informations, consultez [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md).  
  
7.  Vous avez la possibilité de sélectionner la case à cocher **Sauvegarde de copie uniquement** pour créer une sauvegarde de copie uniquement. Une *sauvegarde de copie uniquement* est une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indépendante du mécanisme des sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conventionnelles. Pour plus d’informations, consultez [Sauvegardes de copie uniquement &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).  Une sauvegarde de copie uniquement n’est pas disponible pour le type de sauvegarde **Différentielle**.  

8.  Pour **Composant de sauvegarde**, sélectionnez la case d’option **Base de données** .  
  
9. Dans la section **Destination** , utilisez la liste déroulante **Sauvegarde sur :** pour sélectionner la destination de sauvegarde. Cliquez sur **Ajouter** pour ajouter des objets ou des destinations de sauvegarde supplémentaires.
  
     Pour supprimer une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Supprimer**. Pour afficher le contenu d’une destination de sauvegarde existante, sélectionnez-la, puis cliquez sur **Contenu**.  

  #### <a name="media-options-page"></a>**Page Options de support**  
10. Pour afficher ou sélectionner les options de support, cliquez sur **Options de support** dans le volet **Sélectionner une page** .   
    
11. Sélectionnez une option **Remplacer le support** en cliquant sur un des éléments suivants : 

    > [!IMPORTANT]  
    >  L’option **Remplacer le support** est désactivée si vous avez sélectionné **URL** comme destination de sauvegarde dans la page **Général**. Pour plus d’informations, consultez [Sauvegarder la base de données &#40;page Options de support&#41;](../../relational-databases/backup-restore/back-up-database-media-options-page.md)  


  -   **Sauvegarder sur le support de sauvegarde existant**  
  
      > [!IMPORTANT]  
      >  Si vous envisagez d'utiliser le chiffrement, ne sélectionnez pas cette option. Si vous sélectionnez cette option, les options de chiffrement dans la page **Options de sauvegarde** seront désactivées. Le chiffrement n'est pas pris en charge lors de l'ajout à un jeu de sauvegarde existant.  
  
         Pour cette option, cliquez sur **Ajouter au jeu de sauvegarde existant** ou sur **Remplacer tous les jeux de sauvegarde existants**. Pour plus d’informations, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
         Vous pouvez aussi activer la case à cocher **Vérifier le nom du support de sauvegarde et la date d'expiration du jeu de sauvegarde** pour forcer l'opération de sauvegarde à vérifier la date et l'heure de l'expiration du jeu de supports ou du jeu de sauvegarde.  
  
         Vous pouvez éventuellement entrer un nom dans la zone de texte **Nom du support de sauvegarde** . Si aucun nom n'est spécifié, un support de sauvegarde avec un nom vide est créé. Si vous spécifiez un nom pour le support de sauvegarde, ce support (bande ou disque) est vérifié pour voir si le nom réel correspond bien au nom que vous entrez ici.  
  
-   **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants**  
  
    Pour cette option, entrez un nom dans la zone de texte **Nouveau nom du support de sauvegarde** et décrivez éventuellement le jeu de supports dans la zone de texte **Description du nouveau support de sauvegarde** .  
  
14. Dans la section **Fiabilité** , vous pouvez activer les cases à cocher :  
  
    -   **Vérifier la sauvegarde en fin d'opération**;  
  
    -   **Effectuer une somme de contrôle avant d'écrire sur le support**.  Pour plus d’informations sur les sommes de contrôle, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
    
    -   **Continuer lors d’erreurs**. 

15. La section **Journal des transactions** est inactive, sauf si vous sauvegardez un journal des transactions (spécifié dans la section **Type de sauvegarde** de la page **Général** ).  
      
16. Dans la section **Lecteur de bande** **Décharger la bande après la sauvegarde** est active si vous effectuez la sauvegarde sur un lecteur de bande (spécifié dans la section **Destination** de la page **Général** ). Vous pouvez cliquer sur cette option pour activer l'option **Rembobiner la bande avant de décharger** .   

  #### <a name="backup-options-page"></a>**Page Options de sauvegarde**  

17. Pour afficher ou sélectionner les options de sauvegarde, cliquez sur **Options de sauvegarde** dans le volet **Sélectionner une page** .  
  
18. Dans la zone de texte **Nom** acceptez le nom par défaut du jeu de sauvegarde ou attribuez-lui un autre nom.  
  
19. Dans la zone de texte **Description** , vous avez la possibilité de saisir une description du jeu de sauvegarde.  
  
20. Spécifiez le moment où le jeu de sauvegarde va expirer et pourra être remplacé sans ignorer explicitement la vérification des données d'expiration :  
  
    -   Pour que le jeu de sauvegarde expire au bout d’un nombre de jours spécifique, cliquez sur **Après** (option par défaut) et entrez le nombre de jours souhaité pour l’expiration du jeu après sa création. Cette valeur doit être comprise entre 0 et 99999 jours ; une valeur de 0 jour signifie que le jeu de sauvegarde n'expirera jamais.  
  
         La valeur par défaut est définie dans l’option **Délai de rétention par défaut du support de sauvegarde (jours)** de la boîte de dialogue **Propriétés du serveur** (page Paramètres de base de données). Pour y accéder, cliquez avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets et sélectionnez les propriétés. Ensuite, sélectionnez la page **Paramètres de base de données** .  
  
    -   Pour que le jeu de sauvegarde expire à une date spécifique, cliquez sur **Le**et entrez la date d'expiration souhaitée.  
  
         Pour plus d’informations sur les dates d’expiration des sauvegardes, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
21. Dans la section **Compression** , utilisez la liste déroulante **Définir la compression de sauvegarde** pour sélectionner le niveau de compression souhaité.  [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et versions ultérieures prennent en charge la [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md). Par défaut, la compression d’une sauvegarde dépend de la valeur de l’option de configuration de serveur **Compression par défaut des sauvegardes** . Toutefois, quelle que soit la valeur par défaut actuelle au niveau du serveur, vous pouvez compresser une sauvegarde en activant **Compresser la sauvegarde**, et vous pouvez empêcher la compression en activant **Ne pas compresser la sauvegarde**.  
  
     Pour plus d’informations sur les paramètres de compression de sauvegarde, consultez [Afficher ou configurer l’option de configuration du serveur valeur par défaut de compression de la sauvegarde](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).  
  
22. Dans la section **Chiffrement** , sélectionnez la case à cocher **Chiffrer la sauvegarde** si vous souhaitez utiliser un chiffrement pour la sauvegarde. Utilisez la liste déroulante **Algorithme** pour sélectionner un algorithme de chiffrement.  Utilisez la liste déroulante **ertificat ou clé asymétrique** pour sélectionner un certificat ou une clé asymétrique existants. Le chiffrement est pris en charge dans SQL Server 2014 ou les versions ultérieures. Pour plus d’informations sur les options de chiffrement, consultez [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md).  
  
  
Vous pouvez utiliser l’ [Assistant Plan de maintenance](../maintenance-plans/use-the-maintenance-plan-wizard.md) pour créer des sauvegardes de bases de données. 

### <a name="examples"></a>Exemples  
#### <a name="a--full-back-up-to-disk-to-default-location"></a>**A.  Sauvegarde complète sur disque à l’emplacement par défaut**
Dans cet exemple, la base de données `Sales` est sauvegardée sur disque à l’emplacement de sauvegarde par défaut.  La base de données `Sales` n’a jamais été sauvegardée.
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur `Sales`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

3.  Cliquez sur **OK**.

#### <a name="b--full-back-up-to-disk-to-non-default-location"></a>**B.  Sauvegarde complète sur disque à un emplacement autre que celui par défaut**
Dans cet exemple, la base de données `Sales` est sauvegardée sur disque à l’emplacement `E:\MSSQL\BAK`.  La base de données `Sales` a déjà été sauvegardée plusieurs fois.
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur `Sales`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

3.  Dans la page **Général** de la section **Destination** , sélectionnez **Disque** dans la liste déroulante **Sauvegarde sur :** .

4.  Cliquez sur **Supprimer** jusqu’à ce que tous les fichiers de sauvegarde existants aient été supprimés.

5.  Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** .

6.  Entrez `E:\MSSQL\BAK\Sales_20160801.bak` dans la zone de texte **nom de fichier** .

7.  Cliquez sur **OK**.

8.  Cliquez sur **OK**.

#### <a name="c--create-an-encrypted-backup"></a>**C.  Créer une sauvegarde chiffrée**
Dans cet exemple, la base de données `Sales` est sauvegardée avec chiffrement à l’emplacement de sauvegarde par défaut.  Une  [**clé principale de base de données**](../../relational-databases/security/encryption/create-a-database-master-key.md) a déjà été créée.  Un  [**certificat**](../../t-sql/statements/create-certificate-transact-sql.md) appelé `MyCertificate`, a déjà été créé. Un exemple T-SQL de création d’une **clé principale de base de données** et d’un **certificat** est donné dans l’article [Créer une sauvegarde chiffrée](../../relational-databases/backup-restore/create-an-encrypted-backup.md).  
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur `Sales`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

3.  Dans la page **Options de support** de la section **Remplacer le support** , sélectionnez **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants**.

4.  Dans la page **Options de sauvegarde** de la section **Chiffrement** sélectionnez la case à cocher **Chiffrer la sauvegarde** .

5.  Dans la liste déroulante **Algorithme** , sélectionnez **AES 256**.

6.  Dans la liste déroulante **Certificat ou clé asymétrique** , sélectionnez `MyCertificate`.

7.  Cliquez sur **OK**.

#### <a name="d--back-up-to-the-azure-blob-storage-service"></a>**D.  Sauvegarder sur le service Stockage Blob Azure**
#### <a name="common-steps"></a>**Étapes courantes**  
Les trois exemples suivants effectuent une sauvegarde complète de la base de données `Sales` vers le service de stockage d’objets blob Microsoft Azure.  Le nom du compte de stockage est `mystorageaccount`.  Le conteneur se nomme `myfirstcontainer`.  Par souci de concision, les quatre premières étapes ne sont répertoriées ici qu’une seule fois et tous les exemples commencent à l’ **Étape 5**.
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur `Sales`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

3.  Dans la page **Général** de la section **Destination** , sélectionnez **URL** dans la liste déroulante **Sauvegarde sur** .

4.  Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** .

    **D1.  Sauvegarde distribuée vers une URL quand il existe déjà des informations d’identification SQL Server**  
Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture et liste.  Les informations d’identification SQL Server, `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`, ont été créées à l’aide d’une signature d’accès partagé associée à la stratégie d’accès stockée.  
*
    5.  Sélectionnez `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` dans la zone de texte **Conteneur de stockage Windows Azure** .

    6.  Dans la zone de texte **Fichier de sauvegarde** , entrez `Sales_stripe1of2_20160601.bak`.

    7.  Cliquez sur **OK**.

    8.  Répétez les étapes **4** et **5**.

    9.  Dans la zone de texte **Fichier de sauvegarde** , entrez `Sales_stripe2of2_20160601.bak`.

    10.  Cliquez sur **OK**.

    11.   Cliquez sur **OK**.

    **D2.  Il existe une signature d’accès partagé mais pas d’informations d’identification SQL Server**
  5.    Entrez `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` dans la zone de texte **Conteneur de stockage Windows Azure** .
  
  6.    Entrez la signature d’accès partagé dans la zone de texte **Stratégie d’accès partagé** .
  
  7.    Cliquez sur **OK**.
  
  8.    Cliquez sur **OK**.

    **D3.  Il n’existe aucune signature d’accès partagé**
  5.    Cliquez sur le bouton **Nouveau conteneur** pour ouvrir la boîte de dialogue **Se connecter à un abonnement Microsoft** .  
  
  6.    Terminez la boîte de dialogue **Se connecter à un abonnement Microsoft** et cliquez sur **OK** pour revenir à la boîte de dialogue **Sélectionner la destination de la sauvegarde** .  Pour plus d’informations, consultez [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md) .
  
  7.    Cliquez sur **OK** dans la boîte de dialogue **Sélectionner la destination de la sauvegarde** .
  
  8.    Cliquez sur **OK**.

  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
### <a name="create-a-full-database-backup"></a>Créer une sauvegarde de base de données complète  
  
1.  Exécutez l'instruction BACKUP DATABASE en spécifiant les éléments suivants :  
  
    -   le nom de la base de données à sauvegarder ;  
  
    -   l'unité de sauvegarde où est écrite la sauvegarde complète de la base de données.  
  
     La syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] de base nécessaire pour une sauvegarde de base de données complète est la suivante :  
  
     BACKUP DATABASE *database*  
  
     TO *unité_sauvegarde* [ **,**...*n* ]  
  
     [ WITH *options_with* [ **,**...*o* ] ] ;  
  
    |Option|Description|  
    |------------|-----------------|  
    |*database*|Base de données à sauvegarder|  
    |*unité_sauvegarde* [ **,**...*n* ]|Spécifie une liste de 1 à 64 unités de sauvegarde à utiliser pour l'opération de sauvegarde. Vous pouvez spécifier une unité de sauvegarde physique ou une unité de sauvegarde logique correspondante, si celle-ci est déjà définie. Pour spécifier une unité de sauvegarde physique, utilisez l'option DISK ou TAPE :<br /><br /> { DISK &#124; TAPE } **=***physical_backup_device_name*<br /><br /> Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).|  
    |WITH *options_with* [ **,**...*o* ]|Spécifie éventuellement une ou plusieurs options supplémentaires, *o*. Pour obtenir des informations de base sur les options, consultez l'étape 2.|  
  
2.  Spécifiez éventuellement une ou plusieurs options WITH. Quelques options WITH de base sont décrites ici. Pour plus d’informations sur toutes les options WITH, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
    -   Options WITH de base relatives au jeu de sauvegarde :  
  
         { COMPRESSION | NO_COMPRESSION }  
         Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et versions ultérieures uniquement, spécifie si la [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md) est effectuée sur cette sauvegarde, remplaçant la valeur par défaut au niveau du serveur.  
  
         ENCRYPTION (ALGORITHM,  SERVER CERTIFICATE |ASYMMETRIC KEY)  
         Dans SQL Server 2014 ou les versions ultérieures, spécifiez l'algorithme de chiffrement à utiliser, ainsi que le certificat ou la clé asymétrique pour sécuriser le chiffrement.  
  
         DESCRIPTION **=** { **'***text***'** | **@***text_variable* }  
         Spécifie le texte au format libre servant à décrire le jeu de sauvegarde. La chaîne peut compter jusqu'à 255 caractères.  
  
         NAME **=** { *backup_set_name* | **@***backup_set_name_var* }  
         Spécifie le nom du jeu de sauvegarde. Les noms peuvent contenir jusqu'à 128 caractères. Si l'option NAME n'est pas spécifiée, le nom reste vide.  
  
    -   Options WITH de base relatives au jeu de sauvegarde :  
  
         Par défaut, l'option BACKUP ajoute la sauvegarde à un support de sauvegarde existant, préservant les jeux de sauvegarde existants. Pour spécifier explicitement ceci, utilisez l'option NOINIT. Pour plus d’informations sur l’ajout à des jeux de sauvegarde existants, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
         Une autre méthode pour formater le support de sauvegarde consiste à utiliser l'option FORMAT :  
  
         FORMAT [ **,** MEDIANAME**=** { *nom_support* | **@***variable_nom_support* } ] [ **,** MEDIADESCRIPTION **=** { *text* | **@***variable_texte* } ]  
         Utilisez la clause FORMAT si vous utilisez le support pour la première fois ou si vous souhaitez écraser toutes les données existantes. Assignez éventuellement un nom et une description au nouveau support.  
  
        > [!IMPORTANT]  
        >  Soyez extrêmement vigilant lorsque vous utilisez la clause FORMAT de l'instruction BACKUP, car elle entraîne la destruction de toutes les sauvegardes préalablement stockées sur le support de sauvegarde.  
  
###  <a name="TsqlExample"></a> Exemples (Transact-SQL)  
  
#### <a name="a-back-up-to-a-disk-device"></a>**A. Sauvegarder sur une unité de disque**  
 L'exemple suivant sauvegarde entièrement la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] sur disque, à l'aide de `FORMAT` , pour créer une nouveau jeu de supports.  
  
```sql  
USE AdventureWorks2012;  
GO  
BACKUP DATABASE AdventureWorks2012  
TO DISK = 'Z:\SQLServerBackups\AdventureWorks2012.Bak'  
   WITH FORMAT,  
      MEDIANAME = 'Z_SQLServerBackups',  
      NAME = 'Full Backup of AdventureWorks2012';  
GO  
```  
  
#### <a name="b-back-up-to-a-tape-device"></a>**B. Sauvegarder sur un périphérique à bandes**  
 L’exemple suivant sauvegarde la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] complète sur bande, en ajoutant la sauvegarde aux sauvegardes précédentes.  
  
```sql  
USE AdventureWorks2012;  
GO  
BACKUP DATABASE AdventureWorks2012  
   TO TAPE = '\\.\Tape0'  
   WITH NOINIT,  
      NAME = 'Full Backup of AdventureWorks2012';  
GO  
```  
  
#### <a name="c-back-up-to-a-logical-tape-device"></a>**C. Sauvegarder sur un périphérique à bandes logique**  
 L'exemple suivant crée une unité de sauvegarde logique pour un périphérique à bandes. Il sauvegarde ensuite la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] complète sur ce périphérique.  
  
```sql  
-- Create a logical backup device,   
-- AdventureWorks2012_Bak_Tape, for tape device \\.\tape0.  
USE master;  
GO  
EXEC sp_addumpdevice 'tape', 'AdventureWorks2012_Bak_Tape', '\\.\tape0'; USE AdventureWorks2012;  
GO  
BACKUP DATABASE AdventureWorks2012  
   TO AdventureWorks2012_Bak_Tape  
   WITH FORMAT,  
      MEDIANAME = 'AdventureWorks2012_Bak_Tape',  
      MEDIADESCRIPTION = '\\.\tape0',   
      NAME = 'Full Backup of AdventureWorks2012';  
GO  
```  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
Utilisez l’applet de commande **Backup-SqlDatabase** . Pour indiquer explicitement qu’il s’agit d’une sauvegarde complète de la base de données, spécifiez le paramètre **-BackupAction**  avec sa valeur par défaut, **Database**. Ce paramètre est facultatif pour les sauvegardes complètes de base de données.  

### <a name="examples"></a>Exemples
#### <a name="a--full-local-backup"></a>**A.  Sauvegarde locale complète**  
L'exemple suivant crée une sauvegarde complète de la base de données `MyDB` à l'emplacement de sauvegarde par défaut de l'instance de serveur `Computer\Instance`. Cet exemple spécifie, de manière facultative, **-BackupAction Database**.  
```powershell 
Backup-SqlDatabase -ServerInstance Computer\Instance -Database MyDB -BackupAction Database  
```
 
#### <a name="b--full-backup-to-microsoft-azure"></a>**B.  Sauvegarde complète sur Microsoft Azure**  
L’exemple suivant crée une sauvegarde complète de la base de données `Sales` sur l’instance `MyServer` pour le service Stockage Blob Microsoft Azure.  Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture et liste.  Les informations d’identification SQL Server, `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`, ont été créées à l’aide d’une signature d’accès partagé associée à la stratégie d’accès stockée.  La commande PowerShell utilise le paramètre **BackupFile** pour spécifier l’emplacement (URL) et le nom du fichier de sauvegarde.

```powershell  
import-module sqlps;
$container = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer';
$FileName = 'Sales.bak';
$database = 'Sales';
$BackupFile = $container + '/' + $FileName ;
  
Backup-SqlDatabase -ServerInstance "MyServer" –Database $database -BackupFile $BackupFile;
```
 
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../relational-databases/scripting/sql-server-powershell-provider.md)  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Sauvegarder une base de données (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Restaurer une sauvegarde de base de données en mode de récupération simple &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)  
  
-   [Restaurer une base de données jusqu’au point d’échec en mode de récupération complète &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
-   [Utiliser l'Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
## <a name="see-also"></a>Voir aussi  
**[Dépannage des opérations de sauvegarde et de restauration SQL Server](https://support.microsoft.com/en-us/kb/224071)**          
[Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Sauvegarder la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)   
 [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)   
 [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)  
  
  
