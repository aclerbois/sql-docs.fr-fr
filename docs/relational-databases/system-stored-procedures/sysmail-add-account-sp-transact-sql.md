---
title: sysmail_add_account_sp (Transact-SQL) | Documents Microsoft
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
- sysmail_add_account_sp
- sysmail_add_account_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_account_sp
ms.assetid: 65e15e2e-107c-49c3-b12c-f4edf0eb1617
caps.latest.revision: 40
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: eab3408b20a7c144ba68d57ceba8d4acf3e54f15
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailaddaccountsp-transact-sql"></a>sysmail_add_account_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un nouveau compte de messagerie de base de données contenant des informations sur un compte SMTP.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_add_account_sp  [ @account_name = ] 'account_name',  
    [ @email_address = ] 'email_address' ,  
    [ [ @display_name = ] 'display_name' , ]  
    [ [ @replyto_address = ] 'replyto_address' , ]  
    [ [ @description = ] 'description' , ]  
    [ @mailserver_name = ] 'server_name'   
    [ , [ @mailserver_type = ] 'server_type' ]  
    [ , [ @port = ] port_number ]  
    [ , [ @username = ] 'username' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @use_default_credentials = ] use_default_credentials ]  
    [ , [ @enable_ssl = ] enable_ssl ]  
    [ , [ @account_id = ] account_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@account_name** =] **'***account_name***'**  
 Nom du compte à ajouter. *account_name* est **sysname**, sans valeur par défaut.  
  
 [ **@email_address** =] **'***email_address***'**  
 Adresse de messagerie électronique à partir de laquelle envoyer le message. Cette adresse doit être une adresse de messagerie Internet. *email_address* est **nvarchar (128)**, sans valeur par défaut. Par exemple, un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peut envoyer des messages électroniques à partir de l’adresse **SqlAgent@Adventure-Works.com**.  
  
 [ **@display_name** =] **'***nom_complet***'**  
 Nom complet à utiliser sur des messages électroniques à partir de ce compte. *nom_complet* est **nvarchar (128)**, avec NULL comme valeur par défaut. Par exemple, un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peut afficher le nom **SQL Server Agent Automated Mailer** sur les messages électroniques.  
  
 [ **@replyto_address** =] **'***replyto_address***'**  
 Adresse à laquelle les réponses aux messages de ce compte sont envoyées. *replyto_address* est **nvarchar (128)**, avec NULL comme valeur par défaut. Par exemple, les réponses à un compte pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peut accéder à l’administrateur de base de données, **danw@Adventure-Works.com**.  
  
 [ **@description** =] **'***description***'**  
 Est une description pour le compte. *Description* est **nvarchar (256)**, avec NULL comme valeur par défaut.  
  
 [ **@mailserver_name** =] **'***nom_serveur***'**  
 Nom ou adresse IP du serveur de messagerie SMTP à utiliser pour ce compte. L’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de résoudre la *nom_serveur* en une adresse IP. *nom_serveur* est **sysname**, sans valeur par défaut.  
  
 [ **@mailserver_type** =] '*server_type*'  
 Type de serveur de messagerie. *server_type* est **sysname**, avec une valeur par défaut **'SMTP'**...  
  
 [ **@port** =] *numéro_port*  
 Numéro de port du serveur de messagerie. *numéro_port* est **int**, avec la valeur par défaut est 25.  
  
 [ **@username** =] **'***nom d’utilisateur***'**  
 Nom d'utilisateur servant lors de la connexion au serveur de messagerie. *nom d’utilisateur* est **nvarchar (128)**, avec NULL comme valeur par défaut. Lorsque ce paramètre possède la valeur NULL, la messagerie de base de données n'utilise pas d'authentification pour ce compte. Si le serveur de messagerie ne nécessite pas d'authentification, utilisez NULL comme nom d'utilisateur.  
  
 [ **@password** =] **'***mot de passe***'**  
 Mot de passe servant lors de la connexion au serveur de messagerie. *mot de passe* est **nvarchar (128)**, avec NULL comme valeur par défaut. Il est inutile de fournir un mot de passe, sauf si un nom d'utilisateur est spécifié.  
  
 [ **@use_default_credentials** =] use_default_credentials  
 Spécifie si le courrier électronique doit être envoyé au serveur SMTP en utilisant les informations d'identification du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. **use_default_credentials** est de type bit, avec 0 comme valeur par défaut. Lorsque la valeur de ce paramètre est définie sur 1, la messagerie de base de données utilise les informations d'identification du moteur de base de données [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Lorsque ce paramètre est 0, la messagerie de base de données envoie le **@username** et **@password** dans le cas contraire, les paramètres, le cas échéant, envoie des messages sans **@username** et **@password** paramètres.  
  
 [ **@enable_ssl** =] enable_ssl  
 Spécifie si la messagerie de base de données chiffre les communications à l’aide du protocole SSL (Secure Sockets Layer). **Enable_ssl** est de type bit, avec 0 comme valeur par défaut.  
  
 [ **@account_id** =] *account_id* sortie  
 Retourne l'ID de compte du nouveau compte. *account_id* est **int**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Messagerie de base de données fournit des paramètres distincts pour **@email_address**, **@display_name**, et **@replyto_address**. Le **@email_address** paramètre correspond à l’adresse à partir de laquelle le message est envoyé. Le **@display_name** paramètre est le nom indiqué dans le **à partir de :** champ du message électronique. Le **@replyto_address** paramètre correspond à l’adresse où la réponse au message électronique sera envoyée. Par exemple, un compte utilisé pour l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut envoyer des messages électroniques à partir d'une adresse de messagerie utilisée uniquement pour l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les messages provenant de cette adresse doivent afficher un nom convivial, afin que les destinataires puissent aisément déterminer que le message a été envoyé par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si un destinataire répond au message, la réponse doit arriver à l'administrateur de bases de données et non à l'adresse utilisée par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ce scénario, le compte utilise **SqlAgent@Adventure-Works.com** comme adresse de messagerie. Le nom d’affichage est défini sur **SQL Server Agent Automated Mailer**. Le compte utilise **danw@Adventure-Works.com** en tant qu’adresse de réponse, donc les réponses aux messages envoyés depuis ce compte aller à l’administrateur de base de données plutôt qu’à l’adresse de messagerie pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Agent. En fournissant des paramètres indépendants pour ces trois paramètres, la messagerie de base de données vous permet de configurer des messages afin qu'ils répondent à vos besoins.  
  
 Le **@mailserver_type** paramètre prend en charge la valeur **'SMTP'**.  
  
 Lorsque **@use_default_credentials** est 1, le courrier est envoyé au serveur SMTP avec les informations d’identification de le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Lorsque **@use_default_credentials** est égal à 0 et un **@username** et **@password** sont spécifiés pour un compte, le compte utilise l’authentification SMTP. Le **@username** et **@password** sont les informations d’identification utilisé par le compte pour le serveur SMTP, pas les informations d’identification pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou du réseau sur l’ordinateur.  
  
 La procédure stockée **sysmail_add_account_sp** est dans le **msdb** de base de données et est détenue par le **dbo** schéma. La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée un compte nommé `AdventureWorks Administrator`. Ce compte utilise l'adresse de messagerie électronique `dba@Adventure-Works.com` en envoie du courrier électronique au serveur de messagerie SMTP `smtp.Adventure-Works.com`. Les messages électroniques envoyés à partir de ce compte affichent `AdventureWorks Automated Mailer` sur la **à partir de :** ligne du message. Les réponses aux messages sont envoyés vers `danw@Adventure-Works.com`.  
  
```  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte de messagerie de base de données](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
