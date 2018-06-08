---
title: sp_add_log_file_recover_suspect_db (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_add_log_file_recover_suspect_db_TSQL
- sp_add_log_file_recover_suspect_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_file_recover_suspect_db
ms.assetid: b41ca3a5-7222-4c22-a012-e66a577a82f6
caps.latest.revision: 37
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7da1686b6b03b75e6ba40cd7d4f9686d8af17980
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddlogfilerecoversuspectdb-transact-sql"></a>sp_add_log_file_recover_suspect_db (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un fichier journal à un groupe de fichiers lorsque la récupération d'une base de données ne peut se terminer en raison d'une erreur 9002 (espace journal insuffisant). Une fois que le fichier est ajouté, **sp_add_log_file_recover_suspect_db** désactive le paramètre suspect et achève la récupération de la base de données. Les paramètres sont les mêmes que ceux de ALTER DATABASE *nom_base_de_données* ADD LOG FILE.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_file_recover_suspect_db [ @dbName= ] 'database' ,   
    [ @name = ] 'logical_file_name' ,   
    [ @filename= ] 'os_file_name' ,   
    [ @size = ] 'size' ,   
    [ @maxsize = ] 'max_size' ,   
    [ @filegrowth = ] 'growth_increment'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@dbName =** ] **'***base de données***'**  
 Nom de la base de données. *base de données* est **sysname**, sans valeur par défaut.  
  
 [  **@name=** ] **'***nom_fichier_logique***'**  
 Est le nom utilisé dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour référencer le fichier. Il doit être unique dans le serveur. *nom_fichier_logique* est **nvarchar (260)**, sans valeur par défaut.  
  
 [  **@filename =** ] **'***nom_fichier_se***'**  
 Chemin et nom de fichier utilisé par le système d'exploitation pour le fichier. Le fichier doit résider dans le serveur hébergeant [!INCLUDE[ssDE](../../includes/ssde-md.md)]. *nom_fichier_se* est **nvarchar (260)**, sans valeur par défaut.  
  
 [  **@size=** ] **' *** taille* **'**  
 Taille initiale du fichier. *taille* est **nvarchar (20)**, avec NULL comme valeur par défaut. Indiquez un nombre entier sans aucune décimale. Les indications Mo et Ko peuvent être utilisées pour indiquer qu'il s'agit de mégaoctets ou de kilo-octets. La valeur par défaut est Mo. La valeur minimale est 512 Ko. Si *taille* n’est pas spécifié, la valeur par défaut est 1 Mo.  
  
 [  **@maxsize=** ] **' *** max_size* **'**  
 Est la taille maximale que peut atteindre le fichier. *max_size* est **nvarchar (20)**, avec NULL comme valeur par défaut. Indiquez un nombre entier sans aucune décimale. Les indications Mo et Ko peuvent être utilisées pour indiquer qu'il s'agit de mégaoctets ou de kilo-octets. La valeur par défaut est Mo.  
  
 Si *max_size* n’est pas spécifié, la taille du fichier augmente jusqu'à ce que le disque est plein. Le journal des applications [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows avertit l'administrateur lorsqu'un disque est sur le point d'être saturé.  
  
 [  **@filegrowth=** ] **' *** growth_increment* **'**  
 Quantité d'espace ajoutée au fichier chaque fois qu'un espace supplémentaire s'avère nécessaire. *growth_increment* est **nvarchar (20)**, avec NULL comme valeur par défaut. La valeur 0 indique qu'aucun accroissement n'est autorisé. Indiquez un nombre entier sans aucune décimale. La valeur peut être exprimée en Mo, en Ko ou en pourcentage (%). Lorsque % est spécifié, la taille de l'incrément de croissance est le pourcentage spécifié de la taille du fichier au moment où l'incrémentation a lieu. Si un nombre est mentionné sans spécifier Mo, Ko ou %, la valeur par défaut est Mo.  
  
 Si *growth_increment* a la valeur NULL, la valeur par défaut est 10 % et la valeur de la taille minimale est de 64 Ko. La taille spécifiée est arrondie à la valeur multiple de 64 Ko la plus proche.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="permissions"></a>Autorisations  
 Exécution reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe. Ces autorisations ne sont pas transférables.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, la base de données `db1` a été déclarée suspecte lors de sa récupération en raison d'un espace journal insuffisant (erreur 9002).  
  
```  
USE master;  
GO  
EXEC sp_add_log_file_recover_suspect_db db1, logfile2,  
'C:\Program Files\Microsoft SQL  
    Server\MSSQL13.MSSQLSERVER\MSSQL\Data\db1_logfile2.ldf',   
    '1MB';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_add_data_file_recover_suspect_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-data-file-recover-suspect-db-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
