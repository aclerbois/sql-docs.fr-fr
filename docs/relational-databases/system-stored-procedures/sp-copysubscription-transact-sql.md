---
title: sp_copysubscription (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- sp_copysubscription
- sp_copysubscription_TSQL
helpviewer_keywords:
- sp_copysubscription
ms.assetid: 3c56cd62-2966-4e87-a986-44cb3fd0b760
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3cae06562e37cf10c2fa94934eedb20880b4391a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spcopysubscription-transact-sql"></a>sp_copysubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

    
> [!IMPORTANT]  
>  La fonctionnalité concernant les abonnements pouvant être attachés est déconseillée et sera retirée dans une version ultérieure. Elle ne doit pas être utilisée dans tout nouveau travail de développement. Dans le cas des publications de fusion partitionnées par le biais de filtres paramétrés, nous vous recommandons d'utiliser plutôt les nouvelles fonctionnalités d'instantanés partitionnés qui simplifient l'initialisation de larges volumes d'abonnements. Pour plus d’informations, voir [Instantanés des publications de fusion avec des filtres paramétrés](../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md). Dans le cas de publications qui ne sont pas partitionnées, vous pouvez initialiser un abonnement par le biais d'une sauvegarde. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 Copie une base de données d'abonnement contenant des abonnements par extraction de données (pull) mais pas d'abonnements par envoi de données (push). Seules les bases de données monofichier peuvent être copiées. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_copysubscription [ @filename = ] 'file_name'  
    [ , [ @temp_dir = ] 'temp_dir' ]  
    [ , [ @overwrite_existing_file = ] overwrite_existing_file]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@filename =**] **'***nom_fichier***'**  
 Chaîne spécifiant le chemin d'accès complet, y compris le nom de fichier, vers lequel une copie du fichier de données (.mdf) est enregistrée. *nom de fichier* est **nvarchar (260)**, sans valeur par défaut.  
  
 [  **@temp_dir=**] **'***temp_dir***'**  
 Nom du répertoire contenant les fichiers temporaires. *temp_dir* est **nvarchar (260)**, avec NULL comme valeur par défaut. Si NULL, le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] répertoire de données par défaut sera utilisé. Le répertoire doit contenir suffisamment d'espace pour stocker un fichier d'une taille équivalente à celle de tous les fichiers de bases de données d'abonnés réunis.  
  
 [  **@overwrite_existing_file=**] **'***overwrite_existing_file***'**  
 Indicateur booléen facultatif qui spécifie s’il faut ou non remplacer un fichier existant portant le même nom que celui spécifié dans **@filename**. *overwrite_existing_file*est **bits**, avec une valeur par défaut **0**. Si **1**, il remplace le fichier spécifié par **@filename**, si elle existe. Si **0**, la procédure stockée échoue si le fichier existe et que le fichier n’est pas remplacé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_copysubscription** est utilisé dans tous les types de réplication pour copier une base de données d’abonnement dans un fichier au lieu d’appliquer une capture instantanée sur l’abonné. La base de données doit être configurée pour prendre uniquement en charge les abonnements par extraction de données (pull). Les utilisateurs détenant les autorisations appropriées peuvent réaliser des copies de la base de données d'abonnement, puis copier ou transporter le fichier d'abonnement (.msf) vers un autre Abonné, ou le lui transmettre par courrier électronique en vue de son attachement en tant qu'abonnement.  
  
 La taille de la base de données d'abonnement copiée doit être inférieure à 2 gigaoctets (Go).  
  
 **sp_copysubscription** est uniquement pris en charge pour les bases de données avec des abonnements clients et ne peut pas être exécutée lorsque la base de données a des abonnements serveur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_copysubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Autres emplacements du dossier d’instantanés](../../relational-databases/replication/alternate-snapshot-folder-locations.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
