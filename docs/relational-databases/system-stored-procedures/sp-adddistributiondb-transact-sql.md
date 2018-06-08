---
title: sp_adddistributiondb (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 04/30/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_adddistributiondb_TSQL
- sp_adddistributiondb
helpviewer_keywords:
- sp_adddistributiondb
ms.assetid: e9bad56c-d2b3-44ba-a4d7-ff2fd842e32d
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7c542df69b94ae6022b8914b050f97d4295e6df5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spadddistributiondb-transact-sql"></a>sp_adddistributiondb (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée une nouvelle base de données de distribution et installe le schéma du serveur de distribution. La base de données de distribution stocke les procédures, le schéma et les métadonnées utilisés dans la réplication. Cette procédure stockée est exécutée sur la base de données master du serveur de distribution afin de créer la base de données de distribution et d'installer les tables et les procédures stockées nécessaires à la distribution de la réplication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adddistributiondb [ @database= ] 'database'   
    [ , [ @data_folder= ] 'data_folder' ]   
    [ , [ @data_file= ] 'data_file' ]   
    [ , [ @data_file_size= ] data_file_size ]   
    [ , [ @log_folder= ] 'log_folder' ]   
    [ , [ @log_file= ] 'log_file' ]   
    [ , [ @log_file_size= ] log_file_size ]   
    [ , [ @min_distretention= ] min_distretention ]   
    [ , [ @max_distretention= ] max_distretention ]   
    [ , [ @history_retention= ] history_retention ]   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @createmode= ] createmode ]  
    [ , [ @from_scripting = ] from_scripting ] 
    [ , [ @deletebatchsize_xact = ] deletebatchsize_xact ] 
    [ , [ @deletebatchsize_cmd = ] deletebatchsize_cmd ] 
```  
  
## <a name="arguments"></a>Arguments  
 [  **@database=**] *base de données »*  
 Nom de la base de données de distribution à créer. *base de données* est **sysname**, sans valeur par défaut. Si la base de données spécifiée existe déjà et n'est pas déjà marquée comme base de données de distribution, les objets nécessaires à l'activation de la distribution sont installés ; la base de données est également marquée comme base de données de distribution. Si la base de données spécifiée est déjà activée comme base de données de distribution, une erreur est renvoyée.  
  
 [  **@data_folder=**] **' *** argument dossier_de_données '*  
 Nom du répertoire utilisé pour stocker le fichier de données de la base de données de distribution. *argument dossier_de_données* est **nvarchar (255)**, avec NULL comme valeur par défaut. Si sa valeur est NULL, le répertoire de données de cette instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisé, par exemple `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`.  
  
 [  **@data_file=**] **'***data_file***'**  
 Nom du fichier de base de données. *data_file* est **nvarchar (255)**, avec une valeur par défaut **base de données**. Si la valeur est NULL, la procédure stockée crée un nom de fichier en utilisant le nom de la base de données.  
  
 [  **@data_file_size=**] *taille_du_fichier_de_données*  
 Taille initiale du fichier de données en mégaoctets (MB). *l’argument taille_du_fichier_de_données i*s **int**, avec une valeur par défaut de 5 Mo.  
  
 [  **@log_folder=**] **'***dossier_journal***'**  
 Est le nom du répertoire du fichier journal de base de données. *l’argument dossier_journal* est **nvarchar (255)**, avec NULL comme valeur par défaut. Si sa valeur est NULL, le répertoire de données de cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisé, par exemple `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`.  
  
 [  **@log_file=**] **'***fichier_journal***'**  
 Est le nom du fichier journal. *fichier_journal* est **nvarchar (255)**, avec NULL comme valeur par défaut. Si la valeur est NULL, la procédure stockée crée un nom de fichier en utilisant le nom de la base de données.  
  
 [  **@log_file_size=**] *log_file_size*  
 Taille initiale du fichier journal en mégaoctets (MB). *l’argument taille_du_fichier_journal* est **int**, avec une valeur par défaut 0 Mo, ce qui signifie que la taille du fichier est créée à l’aide du journal plus petit taille de fichier autorisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [  **@min_distretention=**] *rétention_de_distribution_minimale*  
 Période de rétention minimale, en heures, avant que les transactions soient supprimées de la base de données de distribution. *rétention_de_distribution_minimale* est **int**, avec une valeur 0 par défaut.  
  
 [  **@max_distretention=**] *max_distretention*  
 Est la période de rétention maximale, en heures, avant la suppression des transactions. *max_distretention* est **int**, avec une valeur par défaut de 72 heures. Les abonnements qui n'ont pas reçu de commandes répliquées plus anciennes que la période de rétention maximale de la distribution sont marqués comme inactifs et doivent être réinitialisés. Une instruction RAISERROR 21011 est émise pour chaque abonnement inactif. La valeur **0** signifie que les transactions répliquées n’est pas stockées dans la base de données de distribution.  
  
 [  **@history_retention=**] *history_retention*  
 Nombre d'heures de rétention de l'historique. *history_retention* est **int**, avec une valeur par défaut de 48 heures.  
  
 [  **@security_mode=**] *security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un serveur de distribution. *security_mode* est **int**, avec 1 comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification ; **1** Spécifie l’authentification intégrée de Windows.  
  
 [  **@login=**] **'***connexion***'**  
 Nom de connexion utilisé lors de la connexion au serveur de distribution pour créer la base de données de distribution. Cela est nécessaire si *security_mode* a la valeur **0**. *login* est de type **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@password=**] **'***mot de passe***'**  
 Mot de passe utilisé lors de la connexion au serveur de distribution. Cela est nécessaire si *security_mode* a la valeur **0**. *mot de passe* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@createmode=**] *mode de création*  
 *mode de création* est **int**, avec une valeur par défaut 1, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**1** (par défaut)|CRÉER une base de données ou utiliser l’existant de base de données, puis appliquez **instdist.sql** pour créer des objets de réplication dans la base de données de distribution.|  
|**2**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 [  **@from_scripting =** ] *from_scripting*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
 
 [  **@deletebatchsize_xact=**] *deletebatchsize_xact*  
 Spécifie la taille du lot à utiliser pendant le nettoyage des transactions expirées à partir des tables MSRepl_Transactions. *deletebatchsize_xact* est **int**, avec la valeur par défaut est 5000. Ce paramètre a été introduit dans SQL Server 2017, suivi par les versions de SQL Server 2012 SP4 et SQL Server 2016 SP2.  

 [  **@deletebatchsize_cmd=**] *deletebatchsize_cmd*  
 Spécifie la taille du lot à utiliser lors du nettoyage des commandes expirées à partir des tables MSRepl_Commands. *deletebatchsize_cmd* est **int**, avec la valeur par défaut est 2000. Ce paramètre a été introduit dans SQL Server 2017, suivi par les versions de SQL Server 2012 SP4 et SQL Server 2016 SP2. 
 
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adddistributiondb** est utilisée dans tous les types de réplication. Toutefois, cette procédure stockée s'exécute uniquement sur un serveur de distribution.  
  
 Vous devez configurer le serveur de distribution en exécutant [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) avant d’exécuter **sp_adddistributiondb**.  
  
 Exécutez **sp_adddistributor** avant d’exécuter **sp_adddistributiondb**.  
  
## <a name="example"></a>Exemple  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_adddistributiondb**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_dropdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)  
  
  
