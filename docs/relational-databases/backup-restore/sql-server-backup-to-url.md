---
title: Sauvegarde SQL Server vers une URL | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 11be89e9-ff2a-4a94-ab5d-27d8edf9167d
caps.latest.revision: 44
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 9d688171b49697b785f571f7e08fee0bfe339858
ms.sourcegitcommit: 38f8824abb6760a9dc6953f10a6c91f97fa48432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="sql-server-backup-to-url"></a>Sauvegarde SQL Server vers une URL
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Cette rubrique présente les concepts, les conditions et les composants nécessaires pour utiliser le service de stockage d’objets blob Microsoft Azure comme destination de sauvegarde. Les fonctionnalités de sauvegarde et de restauration sont identiques ou similaires à l'utilisation de l'option DISK ou TAPE, à quelques différences près. Ces différences et quelques exemples de code sont inclus dans cette rubrique.  
  
## <a name="requirements-components-and-concepts"></a>Configuration requise, composants et concepts  
 **Dans cette section :**  
  
-   [Sécurité](#security)  
  
-   [Présentation des principaux éléments et concepts](#intorkeyconcepts)  
  
-   [Service de stockage d’objets blob Microsoft Azure](#Blob)  
  
-   [Composants SQL Server](#sqlserver)  
  
-   [Limitations](#limitations)  
  
-   [Prise en charge des instructions de sauvegarde/restauration](#Support)  
  
-   [Utilisation de la tâche de sauvegarde dans SQL Server Management Studio](../../relational-databases/backup-restore/sql-server-backup-to-url.md#BackupTaskSSMS)  
  
-   [Sauvegarde SQL Server vers une URL à l'aide de l'Assistant Plan de maintenance](../../relational-databases/backup-restore/sql-server-backup-to-url.md#MaintenanceWiz)  
  
-   [Restauration à partir du stockage Windows Azure à l'aide de SQL Server Management Studio](../../relational-databases/backup-restore/sql-server-backup-to-url.md#RestoreSSMS)  
  
###  <a name="security"></a> Sécurité  
 Vous trouverez ci-dessous les considérations de sécurité et conditions requises pour la sauvegarde et la restauration à l’aide du service de stockage d’objets blob Microsoft Azure.  
  
-   Lorsque vous créez un conteneur pour le service de stockage d’objets blob Microsoft Azure, nous vous recommandons de définir l’accès sur **Privé**. La définition d'un accès privé limite l'accès aux seuls utilisateurs ou comptes capables de fournir les informations nécessaires pour s'authentifier auprès du compte Windows Azure.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert que, soit un nom de compte Microsoft Azure et une authentification par clé d’accès, soit une signature d’accès partagé (SAP) et un jeton d’accès soient stockés dans les informations d’identification de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces informations sont utilisées pour l’authentification auprès du compte Microsoft Azure lors de l’exécution d’opérations de sauvegarde ou de restauration.  
  
-   Le compte d’utilisateur utilisé pour émettre les commandes BACKUP (sauvegarder) ou RESTORE (restaurer) doit figurer dans le rôle de base de données **db_backup operator** avec les autorisations **Modifier des informations d’identification** .  
  
###  <a name="intorkeyconcepts"></a> Présentation des principaux éléments et concepts  
 Les deux sections suivantes présentent le service de stockage d’objets blob Microsoft Azure, ainsi que les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisés pour la sauvegarde et la restauration à l’aide de ce service. Il est important de comprendre les composants et leur interaction pour effectuer une sauvegarde ou une restauration à l’aide du service de stockage d’objets blob Microsoft Azure.  
  
 La création d’un compte de stockage Microsoft Azure dans votre abonnement Azure est la première étape de ce processus. Ce compte de stockage est un compte d’administrateur disposant des autorisations administratives complètes sur tous les conteneurs et objets créés avec le compte de stockage. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut soit utiliser le nom du compte de stockage Microsoft Azure et la valeur de sa clé d’accès pour s’authentifier, ainsi qu’écrire et lire des objets blob dans le service de stockage d’objets blob Microsoft Azure, soit utiliser un jeton de signature d’accès partagé généré sur des conteneurs spécifiques lui octroyant des droits de lecture et d’écriture. Pour plus d’informations sur les comptes Azure Storage, voir [À propos des comptes de stockage Azure](http://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/) et, pour plus d’informations sur les signatures d’accès partagé, voir [Signatures d’accès partagé, partie 1 : présentation du modèle SAP](http://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/). Les informations d'identification de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stockent ces informations et sont utilisées lors des opérations de sauvegarde ou de restauration.  
  
###  <a name="Blob"></a> Service de stockage d’objets blob Microsoft Azure  
 **Compte de stockage :** le compte de stockage constitue le point de départ de tous les services de stockage. Pour accéder au service de stockage d’objets blob Microsoft Azure, commencez par créer un compte de stockage Microsoft Azure. Pour plus d’informations, voir [Créez un compte de stockage](http://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/).  
  
 **Conteneur :** un conteneur permet de regrouper un ensemble d’objets blob, et peut stocker un nombre illimité d’objets blob. Pour pouvoir écrire une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le service de stockage d’objets blob Microsoft Azure, vous devez avoir créé au moins le conteneur racine. Vous pouvez générer un jeton de signature d’accès partagé sur un conteneur, et accorder l’accès aux objets uniquement sur un conteneur spécifique.  
  
 **Objet blob :** fichier de tout type et de toute taille. Deux types d’objets blob peuvent être stockés dans le service de stockage d’objets blob Microsoft Azure : les objets blob de blocs et les objets blob de pages. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La sauvegarde peut utiliser les deux types d’objets blob en fonction de la syntaxe Transact-SQL utilisée. Les objets blob sont adressables en utilisant le format d’URL suivant : https://\<compte de stockage>.blob.core.windows.net/\<conteneur>/\<objet blob>. Pour plus d’informations sur le service de stockage d’objets blob Microsoft Azure, voir [Prise en main du stockage d’objets blob Azure à l’aide de .NET](http://www.windowsazure.com/develop/net/how-to-guides/blob-storage/). Pour plus d’informations sur les objets blob de pages et de blocs, voir [Présentation des objets blob de blocs, des objets blob d’ajout et des objets blob de pages](http://msdn.microsoft.com/library/windowsazure/ee691964.aspx).  
  
 ![Stockage Blob Azure](../../relational-databases/backup-restore/media/backuptocloud-blobarchitecture.gif "Stockage Blob Azure")  
  
 **Capture instantanée Azure :** capture instantanée d’objet blob Azure effectuée à un point dans le temps. Pour plus d’informations, consultez [Création d’un instantané d’objet blob](https://msdn.microsoft.com/library/azure/hh488361.aspx). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend désormais en charge les sauvegardes de captures instantanées Azure de fichiers de base de données stockés dans le service de stockage d’objets blob Microsoft Azure. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
###  <a name="sqlserver"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Components  
 **URL :** une URL spécifie un URI (Uniform Resource Identifier) vers un fichier de sauvegarde unique. L'URL est utilisée pour indiquer l'emplacement et le nom du fichier de sauvegarde de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L’URL doit pointer vers un objet blob réel et pas un simple conteneur. Si l’objet blob n’existe pas, il est créé. Si un objet blob existant est spécifié, la sauvegarde échoue, sauf si l’option « WITH FORMAT » est spécifiée pour remplacer le fichier de sauvegarde existant dans l’objet blob.  
  
 Voici un exemple de valeur d’URL : http[s]://NOMCOMPTE.Blob.core.windows.net/\<CONTENEUR>/\<NOMFICHIER.bak>. HTTPS n'est pas obligatoire, mais est recommandé.  
  
 **Informations d'identification :** les informations d'identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont des objets utilisés pour stocker les informations d'authentification requises pour la connexion à une ressource en dehors de SQL Server. Ici, les processus de sauvegarde et de restauration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent des informations d’identification pour s’authentifier auprès du service de stockage d’objets blob Microsoft Azure ainsi que de son conteneur et de ses objets blob. Les informations d’identification contiennent soit le nom du compte de stockage et les valeurs de **clé d’accès** de celui-ci, soit l’URL du conteneur et son jeton de signature d’accès partagé. Une fois les informations d’identification créées, la syntaxe des instructions BACKUP/RESTORE détermine le type d’objet blob et les informations d’identification requises.  
  
 Pour savoir comment créer une signature d’accès partagé, voir les exemples [Créer une signature d’accès partagé](../../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS) plus loin dans cette rubrique. De même, pour savoir comment créer des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , voir les exemples [Créer des informations d'identification](../../relational-databases/backup-restore/sql-server-backup-to-url.md#credential) plus loin dans cette rubrique.  
  
 Pour plus d’informations sur les informations d’identification en général, consultez [Informations d’identification](http://msdn.microsoft.com/library/ms161950.aspx).  
  
 Pour des informations sur d’autres exemples d’utilisation des informations d’identification, voir [Créer un proxy de SQL Server Agent](http://msdn.microsoft.com/library/ms175834.aspx).  
  
###  <a name="limitations"></a> Limitations  
  
-   La sauvegarde dans le stockage Premium n’est pas prise en charge.  
  
-   SQL Server limite la taille de sauvegarde maximale prise en charge en utilisant un objet blob de pages à 1 To. La taille de sauvegarde maximale prise en charge en utilisant des objets blob de blocs est limitée à environ 200 Mo (50 000 blocs * 4 Mo de MAXTRANSFERSIZE). Les objets blob de blocs prennent en charge l’entrelacement pour prendre en charge des tailles de sauvegarde sensiblement plus importantes.  
  
-   Vous pouvez émettre des instructions de sauvegarde ou de restauration à l’aide de TSQL, SMO, des applets de commande PowerShell, ou de l’Assistant Restauration ou Sauvegarde de SQL Server Management Studio.   
  
-   La création d'un nom d'unité logique n'est pas prise en charge. Par conséquent, l'ajout d'une URL comme unité de sauvegarde à l'aide de sp_dumpdevice ou de SQL Server Management Studio n'est pas pris en charge.  
  
-   L'ajout d'objets blob de sauvegarde existants n'est pas pris en charge. Des sauvegardes vers un objet blob existant peuvent être remplacées uniquement à l’aide de l’option **WITH FORMAT** . Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument **WITH FORMAT** n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde file-snapshot d’origine.  
  
-   La sauvegarde vers plusieurs objets blob en une seule opération est prise en charge uniquement en utilisant un jeton de signature d’accès partagé (SAP) au lieu de la clé du compte de stockage pour les informations d’identification SQL.  
  
-   La spécification de **BLOCKSIZE** n’est pas prise en charge pour les objets blob de pages. 
  
-   La spécification de **MAXTRANSFERSIZE** n’est pas prise en charge pour les objets blob de pages. 
  
-   La spécification des options du jeu de sauvegarde **RETAINDAYS** et **EXPIREDATE** n’est pas prise en charge.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est soumis à une limite de 259 caractères pour le nom d'une unité de sauvegarde. BACKUP TO URL utilise 36 caractères pour les éléments requis utilisés pour spécifier l’URL « https://.blob.core.windows.net//.bak », ce qui laisse 223 caractères pour les noms de compte, de conteneur et d’objet blob réunis.  
  
###  <a name="Support"></a> Prise en charge des instructions de sauvegarde/restauration  
  
|Instruction de sauvegarde/restauration|Pris en charge|Exceptions|Commentaires|
|-|-|-|-|
|BACKUP|O|BLOCKSIZE et MAXTRANSFERSIZE sont pris en charge pour les objets blob de blocs. Ils ne sont pas pris en charge pour les objets blob de pages. | BACKUP sur un objet blob de blocs nécessite une Signature d’accès partagé enregistrée dans des informations d’identification SQL Server. BACKUP sur un objet blob de pages nécessite la clé de compte de stockage enregistrée dans des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et nécessite de spécifier l’argument WITH CREDENTIAL.|  
|RESTORE|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE FILELISTONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE HEADERONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE LABELONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE VERIFYONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE REWINDONLY|−|||  
  
 Pour plus d’informations sur la syntaxe et les instructions de sauvegarde, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
 Pour plus d’informations sur la syntaxe et les instructions de restauration, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
### <a name="support-for-backup-arguments"></a>Prise en charge des arguments de sauvegarde  

|Argument|Pris en charge|Exception|Commentaires|  
|-|-|-|-|  
|DATABASE|O|||  
|LOG|O|||  
||  
|TO (URL)|O|Contrairement à DISK et TAPE, URL ne prend pas en charge la spécification ou la création d'un nom logique.|Cet argument est utilisé pour spécifier le chemin d'accès de l'URL du fichier de sauvegarde.|  
|MIRROR TO|O|||  
|**WITH OPTIONS :**||||  
|CREDENTIAL|O||L’argument WITH CREDENTIAL est pris en charge uniquement en cas d’utilisation de l’option BACKUP TO URL pour sauvegarder vers le service de stockage d’objets blob Microsoft Azure, et uniquement si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|FILE_SNAPSHOT|O|||  
|ENCRYPTION|O||Quand l’argument **WITH ENCRYPTION** est spécifié, la sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie que la base de données entière a été chiffrée par TDE (Transparent Data Encryption) avant d’effectuer la sauvegarde et, dans ce cas, chiffre le fichier de sauvegarde de capture instantanée de fichier en utilisant l’algorithme spécifié pour TDE sur la base de données. Si des données de la base de données ne sont pas chiffrées (par exemple, si le processus de chiffrement n’est pas encore terminé), la sauvegarde échoue.|  
|DIFFERENTIAL|O|||  
|COPY_ONLY|O|||  
|COMPRESSION&#124;NO_COMPRESSION|O|Non prise en charge pour une sauvegarde de capture instantanée de fichier.||  
|DESCRIPTION|O|||  
|NAME|O|||  
|EXPIREDATE &#124; RETAINDAYS|−|||  
|NOINIT &#124; INIT|−||L'ajout aux objets blob n'est pas possible. Pour remplacer une sauvegarde, utilisez l’argument **WITH FORMAT** . Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument **WITH FORMAT** n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde d’origine.|  
|NOSKIP &#124; SKIP|−|||  
|NOFORMAT &#124; FORMAT|O||Une sauvegarde effectuée vers un objet blob existant échoue à moins que l’option **WITH FORMAT** soit spécifiée. Quand l’option **WITH FORMAT** est spécifiée, l’objet blob existant est remplacé. Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument FORMAT n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde file-snapshot d’origine. Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument **WITH FORMAT** n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde d’origine.|  
|MEDIADESCRIPTION|O|||  
|MEDIANAME|O|||  
|BLOCKSIZE|O|Non pris en charge pour l’objet blob de pages. Prise en charge pour l’objet blob de blocs.| Nous recommandons BLOCKSIZE=65536 pour optimiser l’utilisation des 50 000 blocs autorisés dans un objet blob de blocs. |  
|BUFFERCOUNT|O|||  
|MAXTRANSFERSIZE|O|Non pris en charge pour l’objet blob de pages. Prise en charge pour l’objet blob de blocs.| La valeur par défaut est 1 048 576. La valeur peut aller jusqu’à 4 Mo, par incréments de 65 536 octets.</br> Nous recommandons MAXTRANSFERSIZE=4194304 pour optimiser l’utilisation des 50 000 blocs autorisés dans un objet blob de blocs. |  
|NO_CHECKSUM &#124; CHECKSUM|O|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|O|||  
|STATS|O|||  
|REWIND &#124; NOREWIND|−|||  
|UNLOAD &#124; NOUNLOAD|−|||  
|NORECOVERY &#124; STANDBY|O|||  
|NO_TRUNCATE|O|||  
  
 Pour plus d’informations sur les arguments de Backup, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
### <a name="support-for-restore-arguments"></a>Prise en charge des arguments de restauration  
  
|Argument|Pris en charge|Exceptions|Commentaires|  
|-|-|-|-|  
|DATABASE|O|||  
|LOG|O|||  
|FROM (URL)|O||L'argument FROM URL est utilisé pour spécifier le chemin d'accès de l'URL du fichier de sauvegarde.|  
|**WITH Options:**||||  
|CREDENTIAL|O||L’argument WITH CREDENTIAL est pris en charge uniquement en cas d’utilisation de l’option RESTORE FROM URL pour restaurer à partir du service de stockage d’objets blob Microsoft Azure.|  
|PARTIAL|O|||  
|RECOVERY &#124; NORECOVERY &#124; STANDBY|O|||  
|LOADHISTORY|O|||  
|MOVE|O|||  
|REPLACE|O|||  
|RESTART|O|||  
|RESTRICTED_USER|O|||  
|FILE|−|||  
|PASSWORD|O|||  
|MEDIANAME|O|||  
|MEDIAPASSWORD|O|||  
|BLOCKSIZE|O|||  
|BUFFERCOUNT|−|||  
|MAXTRANSFERSIZE|−|||  
|CHECKSUM &#124; NO_CHECKSUM|O|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|O|||  
|FILESTREAM|O|Non prise en charge pour la sauvegarde de capture instantanée.||  
|STATS|O|||  
|REWIND &#124; NOREWIND|−|||  
|UNLOAD &#124; NOUNLOAD|−|||  
|KEEP_REPLICATION|O|||  
|KEEP_CDC|O|||  
|ENABLE_BROKER &#124; ERROR_BROKER_CONVERSATIONS &#124; NEW_BROKER|O|||  
|STOPAT &#124; STOPATMARK &#124; STOPBEFOREMARK|O|||  
  
 Pour plus d’informations sur les arguments de Restore, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
##  <a name="BackupTaskSSMS"></a> Utilisation de la tâche de sauvegarde dans SQL Server Management Studio  
Vous pouvez sauvegarder une base de données vers une URL par le biais de la tâche de sauvegarde dans SQL Server Management Studio à l’aide des informations d’identification SQL Server.  
  
> [!NOTE]  
>  Pour créer une sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ou pour remplacer un support de sauvegarde existant, vous devez utiliser Transact-SQL, Powershell ou C# plutôt que la tâche de sauvegarde dans SQL Server Management Studio.  
  
 Les étapes suivantes décrivent les modifications apportées à la tâche Sauvegarder la base de données dans SQL Server Management Studio pour autoriser la sauvegarde vers le stockage Microsoft Azure :  
  
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur la base de données souhaitée, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.
  
3.  Dans la page **Général** de la section **Destination** , l’option **URL** est disponible dans la liste déroulante **Sauvegarde sur** .  L’option **URL** sert à créer une sauvegarde dans le stockage Microsoft Azure. Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** .
   
    1.  **Conteneur de stockage Azure :** nom du conteneur de stockage Microsoft Azure pour stocker les fichiers de sauvegarde.  Sélectionnez un conteneur existant dans la liste déroulante ou entrez manuellement le conteneur. 
  
    2.  **Stratégie d’accès partagé** : entrez la signature d’accès partagé pour un conteneur entré manuellement.  Ce champ n’est pas disponible si vous avez choisi un conteneur existant. 
  
    3.  **Fichier de sauvegarde** : nom du fichier de sauvegarde.
    
    4.  **Nouveau conteneur** : permet d’enregistrer un conteneur existant pour lequel vous n’avez pas de signature d’accès partagé.  Consultez [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).
  
> [!NOTE] 
>  **Ajouter** prend en charge plusieurs fichiers de sauvegarde et conteneurs de stockage pour un seul support de sauvegarde.
  
 Quand vous sélectionnez **URL** comme destination, certaines options de la page **Options de support** sont désactivées.  Les rubriques suivantes contiennent d'autres informations sur la boîte de dialogue Sauvegarder la base de données :  
  
 [Sauvegarder la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)  
  
 [Sauvegarder la base de données &#40;page Options de support&#41;](../../relational-databases/backup-restore/back-up-database-media-options-page.md)  
  
 [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)  
  
 [Créer des informations d'identification - Authentification dans le stockage Azure](../../relational-databases/backup-restore/create-credential-authenticate-to-azure-storage.md)  
  
##  <a name="MaintenanceWiz"></a> Sauvegarde SQL Server vers une URL à l'aide de l'Assistant Plan de maintenance  
 Comme pour la tâche de sauvegarde décrite précédemment, l’Assistant Plan de maintenance dans SQL Server Management Studio inclut l’option **URL** parmi les options de destination, ainsi que d’autres objets de prise en charge requis pour sauvegarder vers un stockage Microsoft Azure, telles les informations d’identification SQL. Pour plus d’informations, voir la section **Définir les tâches de sauvegarde** dans [Using Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure).  
  
> [!NOTE]  
>  Pour créer un jeu de sauvegarde distribué, une sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou des informations d’identification SQL utilisant un jeton d’accès partagé, vous devez utiliser Transact-SQL, Powershell ou C# plutôt que la tâche Sauvegarder dans l’Assistant Plan de Maintenance.  
  
##  <a name="RestoreSSMS"></a> Restauration à partir du stockage Microsoft Azure à l’aide de SQL Server Management Studio  
La tâche Restaurer la base de données propose **URL** comme unité à partir de laquelle effectuer la restauration.  Les étapes suivantes décrivent l’utilisation de la tâche Restaurer pour restaurer à partir du service de stockage d’objets blob Microsoft Azure : 
  
1.  Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**. 
  
2.  Dans la page **Général** , sélectionnez **Unité** dans la section **Source** .
  
3.  Cliquez sur le bouton Parcourir (...) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . 

4.  Sélectionnez **URL** dans la liste déroulante **Type de support de sauvegarde** .  Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner un emplacement de fichier de sauvegarde** .

    1.  **Conteneur de stockage Azure :** nom qualifié complet du conteneur de stockage Microsoft Azure qui contient les fichiers de sauvegarde.  Sélectionnez un conteneur existant dans la liste déroulante ou entrez manuellement le nom qualifié complet du conteneur.
      
    2.  **Signature d’accès partagé :**  permet d’entrer la signature d’accès partagé pour le conteneur spécifié.
      
    3.  **Ajouter**  : permet d’enregistrer un conteneur existant pour lequel vous n’avez pas de signature d’accès partagé.  Consultez [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).
      
    4.  **OK :** SQL Server se connecte au stockage Microsoft Azure en utilisant les informations d’identification SQL spécifiées et ouvre la boîte de dialogue **Localiser le fichier de sauvegarde dans Microsoft Azure**. Les fichiers de sauvegarde résidant dans le conteneur de stockage s’affichent dans cette page. Sélectionnez le fichier à utiliser pour la restauration, puis cliquez sur **OK**. Vous revenez dans la boîte de dialogue **Sélectionner les unités de sauvegarde** et, quand vous cliquez sur **OK** , vous revenez dans la boîte de dialogue principale **Restaurer** où vous pourrez effectuer la restauration. 
  
     [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
     [Restaurer la base de données &#40;page Fichiers&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
     [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
##  <a name="Examples"></a> Exemples de code  
 Cette section contient les exemples suivants :  
  
-   [Créer des informations d'identification](#credential)  
  
-   [Sauvegarde d'une base de données complète](#complete)  
    
-   [Restauration jusqu'à une date et heure en utilisant STOPAT](#PITR)  
  
> [!NOTE]  
>  Pour obtenir un didacticiel sur l’utilisation de SQL Server 2016 avec le service de stockage d’objets blob Microsoft Azure, consultez [Tutorial: Using the Microsoft Azure Blob storage service with SQL Server 2016 databases](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)(Didacticiel : Utilisation du service de stockage d’objets blob Microsoft Azure avec des bases de données SQL Server 2016).  
  
###  <a name="SAS"></a> Créer une signature d’accès partagé  
 L’exemple suivant crée des signatures d’accès partagé utilisables pour créer des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un conteneur nouvellement créé. Le script crée une signature d’accès partagé associée à une stratégie d’accès stockée. Pour plus d’informations, voir [Signatures d’accès partagé, partie 1 : présentation du modèle SAP](http://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/). Le script écrit également la commande T-SQL requise pour créer les informations d’identification sur SQL Server. 

> [!NOTE] 
> L’exemple nécessite Microsoft Azure Powershell. Pour plus d’informations sur l’installation et l’utilisation d’Azure Powershell, consultez [Installation et configuration d’Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).  
> Ces scripts ont été vérifiés à l’aide d’Azure PowerShell 5.1.15063. 


**Signature d’accès partagé associée à une stratégie d’accès stockée**  
  
```Powershell  
# Define global variables for the script  
$prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
$subscriptionName='<your subscription name>'   # the name of subscription name you will use  
$locationName = '<a data center location>'  # the data center region you will use  
$storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
$containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
$policyName = $prefixName + 'policy' # the name of the SAS policy  


# Set a variable for the name of the resource group you will create or use  
$resourceGroupName=$prefixName + 'rg'   

# adds an authenticated Azure account for use in the session   
Login-AzureRmAccount

# set the tenant, subscription and environment for use in the rest of   
Set-AzureRmContext -SubscriptionName $subscriptionName   

# create a new resource group - comment out this line to use an existing resource group  
New-AzureRmResourceGroup -Name $resourceGroupName -Location $locationName   

# Create a new ARM storage account - comment out this line to use an existing ARM storage account  
New-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   

# Get the access keys for the ARM storage account  
$accountKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  

# Create a new storage account context using an ARM storage account  
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].value 

# Creates a new container in blob storage  
$container = New-AzureStorageContainer -Context $storageContext -Name $containerName  
$cbc = $container.CloudBlobContainer  

# Sets up a Stored Access Policy and a Shared Access Signature for the new container  
$policy = New-AzureStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission "rwld"
$sas = New-AzureStorageContainerSASToken -Policy $policyName -Context $storageContext -Container $containerName


# Gets the Shared Access Signature for the policy  
$policy = new-object 'Microsoft.WindowsAzure.Storage.Blob.SharedAccessBlobPolicy'  
$sas = $cbc.GetSharedAccessSignature($policy, $policyName)  
Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

# Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
Write-Host 'Credential T-SQL'  
$tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
$tSql | clip  
Write-Host $tSql  
```  

Après avoir exécuté le script, copiez la commande `CREATE CREDENTIAL` dans un outil de requête, connectez-vous à une instance de SQL Server et exécutez la commande pour créer les informations d’identification avec la signature d’accès partagé. 

###  <a name="credential"></a> Créer des informations d'identification  
 Les exemples suivants créent des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l’authentification auprès du service de stockage d’objets blob Microsoft Azure. Procédez de l'une des manières suivantes : 
  
1.  **Utilisation d’une signature d’accès partagé**  

   Si vous avez exécuté le script pour créer la signature d’accès partagé ci-dessus, copiez `CREATE CREDENTIAL` dans un éditeur de requête connecté à votre instance de SQL Server et exécutez la commande. 

   Le code T-SQL suivant est un exemple qui crée les informations d’identification pour utiliser une signature d’accès partagé. 

   ```sql  
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
      SECRET = '<SAS_TOKEN>';  
   ```  
  
2.  **Utilisation d’une identité de compte de stockage et d’une clé d’accès**  
  
   ```sql 
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = '<mycredentialname>')  
   CREATE CREDENTIAL [<mycredentialname>] WITH IDENTITY = '<mystorageaccountname>'  
   ,SECRET = '<mystorageaccountaccesskey>';  
   ```  
  
###  <a name="complete"></a> Effectuer une sauvegarde complète de la base de données  
 Les exemples suivants effectuent une sauvegarde complète de la base de données AdventureWorks2016 vers le service de stockage d’objets blob Microsoft Azure. Procédez de l'une des manières suivantes :   
  
  
2.  **Vers une URL en utilisant une signature d’accès partagé**  
  
   ```sql  
   BACKUP DATABASE AdventureWorks2016   
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak';  
   GO   
   ```  

1.  **Vers une URL en utilisant une identité de compte de stockage et une clé d’accès**  
  
   ```sql
   BACKUP DATABASE AdventureWorks2016  
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
         WITH CREDENTIAL = '<mycredentialname>'   
        ,COMPRESSION  
        ,STATS = 5;  
   GO   
   ```  
  

  
  
###  <a name="PITR"></a> Restauration jusqu'à une date et heure en utilisant STOPAT  
 L’exemple suivant restaure la base de données exemple AdventureWorks2016 dans l’état où elle était à un point dans le temps, et illustre une opération de restauration.  
  
1.  **À partir d’une URL en utilisant une signature d’accès partagé**  
  
   ```sql
   RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.bak'   
   WITH MOVE 'AdventureWorks2016_data' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.mdf'  
   ,MOVE 'AdventureWorks2016_log' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.ldf'  
   ,NORECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO   
  
   RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
   WITH   
   RECOVERY   
   ,STOPAT = 'May 18, 2015 5:35 PM'   
   GO  
   ```  
  
## <a name="see-also"></a> Voir aussi  
 [Meilleures pratiques et dépannage de sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)   
 [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)   
 [Tutorial: Using the Microsoft Azure Blob storage service with SQL Server 2016 databases](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
  
