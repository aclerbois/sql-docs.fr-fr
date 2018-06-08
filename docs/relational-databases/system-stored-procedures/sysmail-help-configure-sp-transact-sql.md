---
title: sysmail_help_configure_sp (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_help_configure_sp
- sysmail_help_configure_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_configure_sp
ms.assetid: e598d4c8-3041-4965-b046-dce3a8e3d3e0
caps.latest.revision: 34
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0d000f176194551f844485bcab04bfd0e085d702
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailhelpconfiguresp-transact-sql"></a>sysmail_help_configure_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche les paramètres de configuration pour la messagerie de base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_help_configure_sp  [ [ @parameter_name = ] 'parameter_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [**@parameter_name** =] **'***nom_paramètre***'**  
 Nom du paramètre de configuration à extraire. Si spécifié, la valeur du paramètre de configuration est retournée dans le **@parameter_value** paramètre de sortie. Lorsqu’aucun **@parameter_name** est spécifié, cette procédure stockée renvoie un jeu de résultats contenant tous les paramètres de configuration de messagerie de base de données dans l’instance.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Lorsqu’aucun **@parameter_name** est spécifié, retourne un jeu de résultats avec les colonnes suivantes.  
  
||||  
|-|-|-|  
|Nom de colonne|Type de données| Description|  
|**paramname**|**nvarchar (256)**|Le nom du paramètre de configuration.|  
|**paramvalue**|**nvarchar (256)**|La valeur du paramètre de configuration.|  
|**description**|**nvarchar (256)**|Description du paramètre de configuration.|  
  
## <a name="remarks"></a>Notes  
 La procédure stockée **sysmail_help_configure_sp** répertorie les paramètres de configuration de base de données pour l’instance.  
  
 Lorsqu’un **@parameter_name** est spécifié, mais aucun paramètre de sortie n’est fournie pour **@parameter_value**, cette procédure stockée ne produit aucune sortie.  
  
 La procédure stockée **sysmail_help_configure_sp** est dans le **msdb** de base de données et est détenue par le **dbo** schéma. La procédure doit être appelée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre la liste des paramètres de configuration de la messagerie de base de données pour l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXECUTE msdb.dbo.sysmail_help_configure_sp ;  
```  
  
 Voici un exemple d'ensemble de résultats, modifié pour la longueur de ligne :  
  
```  
paramname                       paramvalue      description  
------------------------------- --------------- -----------------------------------------------------------------------------  
AccountRetryAttempts            1               Number of retry attempts for a mail server  
AccountRetryDelay               5000            Delay between each retry attempt to mail server  
DatabaseMailExeMinimumLifeTime  600             Minimum process lifetime in seconds  
DefaultAttachmentEncoding       MIME            Default attachment encoding  
LoggingLevel                    2               Database Mail logging level: normal - 1, extended - 2 (default), verbose - 3  
MaxFileSize                     1000000         Default maximum file size  
ProhibitedExtensions            exe,dll,vbs,js  Extensions not allowed in outgoing mails  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
