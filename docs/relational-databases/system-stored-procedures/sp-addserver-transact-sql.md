---
title: sp_addserver (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_addserver
- sp_addserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addserver
- renaming servers
- machine names [SQL Server]
- computer names
ms.assetid: 160a6b29-5e80-44ab-80ec-77d4280f627c
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4263ae95f80518504fca71cf5622b4d3c65c850b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddserver-transact-sql"></a>sp_addserver (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Définit le nom de l’instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque l’ordinateur qui héberge [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est renommé, utilisez **sp_addserver** pour informer l’instance de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] du nouveau nom d’ordinateur. Cette procédure doit être exécutée sur toutes les instances de la [!INCLUDE[ssDE](../../includes/ssde-md.md)] hébergé sur l’ordinateur. Le nom de l’instance de la [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas être modifié. Pour modifier le nom d'une instance nommée, installez une nouvelle instance portant le nom souhaité, détachez les fichiers de base de données de l'ancienne instance, attachez les bases de données à la nouvelle instance, puis supprimez l'ancienne instance. Vous pouvez également créer un nom d'alias client sur l'ordinateur client, en redirigeant la connexion vers un autre nom de serveur et d'instance ou la combinaison **serveur:port** sans modifier le nom de l'instance sur l'ordinateur serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addserver [ @server = ] 'server' ,  
     [ @local = ] 'local'   
     [ , [ @duplicate_ok = ] 'duplicate_OK' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@server =** ] **'***server***'**  
 Indique le nom du serveur. Les noms de serveurs doivent être uniques et suivre les règles de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows concernant les noms des ordinateurs, bien que l'utilisation d'espaces ne soit pas autorisée. *server* est de type **sysname**et n'a pas de valeur par défaut.  
  
 Lorsque plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installés sur un ordinateur, chacune fonctionne comme s’il se trouve sur un serveur distinct. Spécifier une instance nommée en faisant référence à *server* en tant que *nomserveur\nominstance*.  
  
 [  **@local =** ] **'LOCAL'**  
 Spécifie que le serveur est ajouté comme un serveur local. **@local** est **varchar (10)**, avec NULL comme valeur par défaut. Spécification de **@local** en tant que **LOCAL** définit **@server** en tant que le nom du serveur local et le @@SERVERNAME fonction pour retourner la valeur de *server*.  
  
 Le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte à cette variable le nom de l'ordinateur. Par défaut, le nom d’ordinateur est les manière dont les utilisateurs se connectent à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans configuration supplémentaire nécessaire.  
  
 La définition locale entre en vigueur qu’après le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est redémarré. Seul un serveur local peut être défini dans chaque instance de la [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 [ **@duplicate_ok =** ] **'duplicate_OK'**  
 Spécifie si les noms de serveur en double sont autorisés. **@duplicate_OK** est **varchar(13)**, avec NULL comme valeur par défaut. **@duplicate_OK** peut avoir uniquement la valeur **duplicate_OK** ou NULL. Si **duplicate_OK** est spécifié et le nom du serveur qui est déjà ajouté existe, aucune erreur n’est levée. Si les paramètres nommés ne sont pas utilisés, **@local** doit être spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Pour définir ou désactivez les options de serveur, utilisez **sp_serveroption**.  
  
 **sp_addserver** ne peut pas être utilisé à l’intérieur d’une transaction définie par l’utilisateur.  
  
 À l’aide de **sp_addserver** pour ajouter un serveur distant n’est plus disponible. Utilisez de préférence [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) .  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **setupadmin** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie le [!INCLUDE[ssDE](../../includes/ssde-md.md)] entrée pour le nom de l’ordinateur hébergeant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à `ACCOUNTS`.  
  
```  
sp_addserver 'ACCOUNTS', 'local';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Renommer un ordinateur qui héberge une Instance autonome de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_dropserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_helpserver & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  
