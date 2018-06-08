---
title: xp_cmdshell (Transact-SQL) | Documents Microsoft
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
- xp_cmdshell
- xp_cmdshell_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_cmdshell
ms.assetid: 18935cf4-b320-4954-b6c1-e007fcefe358
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 73609eae141fd96f9a2f01bccc3cf18347a0fc00
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="xpcmdshell-transact-sql"></a>xp_cmdshell (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Engendre une commande d'environnement Windows et lui transmet une chaîne à traiter. Toute sortie est retournée sous forme de lignes de texte.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
xp_cmdshell { 'command_string' } [ , no_output ]  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *chaîne_de_commande* **'**  
 Chaîne qui contient une commande à transmettre au système d'exploitation. *chaîne_de_commande* est **varchar(8000)** ou **nvarchar (4000)**, sans valeur par défaut. *chaîne_de_commande* ne peut pas contenir plusieurs jeux de guillemets doubles. Une seule paire de guillemets doubles est requise si les espaces sont présents dans les chemins d’accès de fichier ou noms de programmes référencés dans *chaîne_de_commande*. Si l'incorporation d'espaces est source de problèmes, recourez à des noms de fichiers FAT 8.3.  
  
 **no_output**  
 Paramètre facultatif indiquant qu'aucune sortie ne doit être retournée au client.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 L'exécution de l'instruction `xp_cmdshell` ci-dessous retourne le contenu du répertoire en cours.  
  
```  
EXEC xp_cmdshell 'dir *.exe';  
GO  
```  
  
 Les lignes sont retournées dans un **nvarchar (255)** colonne. Si le **no_output** option est utilisée, seuls les éléments suivants seront affichera :  
  
```  
The command(s) completed successfully.  
```  
  
## <a name="remarks"></a>Notes  
 Le processus Windows engendré par **xp_cmdshell** a les mêmes droits de sécurité que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service.  
  
 **xp_cmdshell** fonctionne de manière synchrone. Le contrôle n'est renvoyé à l'appelant que lorsque la commande de l'interpréteur de commandes est terminée.  
  
 **xp_cmdshell** peut être activé et désactivé à l’aide de la gestion basée sur des stratégies ou en exécutant **sp_configure**. Pour plus d’informations, consultez [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md) et [xp_cmdshell Server Configuration Option](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
> [!IMPORTANT]  
>  Si **xp_cmdshell** est exécutée dans un lot et retourne une erreur, le traitement échoue. Cela représente un changement de comportement. Dans les versions antérieures de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le traitement continue de s’exécuter.  
  
## <a name="xpcmdshell-proxy-account"></a>Compte proxy xp_cmdshell  
 Lorsqu’elle est appelée par un utilisateur qui n’est pas un membre de la **sysadmin** rôle de serveur fixe **xp_cmdshell** se connecte à Windows en utilisant le nom de compte et le mot de passe stockés dans les informations d’identification nommée **## xp_cmdshell_proxy_account ##**. Si ces informations d’identification de proxy n’existent pas, **xp_cmdshell** échoue.  
  
 Les informations d’identification du compte proxy peuvent être créée en exécutant **sp_xp_cmdshell_proxy_account**. Cette procédure stockée accepte comme arguments un nom d'utilisateur et un mot de passe Windows. Par exemple, la commande ci-dessous crée des informations d'identification de proxy pour l'utilisateur de domaine Windows `SHIPPING\KobeR` qui possède le mot de passe Windows `sdfh%dkc93vcMt0`.  
  
```  
EXEC sp_xp_cmdshell_proxy_account 'SHIPPING\KobeR','sdfh%dkc93vcMt0';  
```  
  
 Pour plus d’informations, consultez [sp_xp_cmdshell_proxy_account &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Étant donné que les utilisateurs malveillants essaient quelquefois d’élever leurs privilèges à l’aide de **xp_cmdshell**, **xp_cmdshell** est désactivée par défaut. Utilisez **sp_configure** ou **gestion basée sur** pour l’activer. Pour plus d’informations, consultez [xp_cmdshell (option de configuration de serveur)](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 Lorsque le premier est activé, **xp_cmdshell** nécessite l’autorisation CONTROL SERVER pour exécuter et le processus Windows créé par **xp_cmdshell** a le même contexte de sécurité que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service a souvent plus d’autorisations que nécessaire pour le travail effectué par le processus créé par **xp_cmdshell**. Pour améliorer la sécurité, l’accès à **xp_cmdshell** doit être limitée aux utilisateurs disposant de privilèges élevés.  
  
 Pour autoriser les non-administrateurs à utiliser **xp_cmdshell**et autoriser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour créer des processus enfants avec le jeton de sécurité d’un compte avec moins de privilèges, procédez comme suit :  
  
1.  Créez et personnalisez un compte d'utilisateur local Windows ou un compte de domaine avec les privilèges minimaux requis par vos processus.  
  
2.  Utilisez le **sp_xp_cmdshell_proxy_account** procédure système pour configurer **xp_cmdshell** à utiliser ce compte avec moins de privilèges.  
  
    > [!NOTE]  
    >  Vous pouvez également configurer ce compte proxy à l’aide [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en double-cliquant sur **propriétés** sur votre serveur de nom dans l’Explorateur d’objets, puis en recherchant le **sécurité** onglet pour le **compte proxy du serveur** section.  
  
3.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], à l’aide de la base de données master, exécutez le `GRANT exec ON xp_cmdshell TO '<somelogin>'` instruction pour donner à spécifiques non -**sysadmin** aux utilisateurs la possibilité d’exécuter **xp_cmdshell**. La connexion spécifiée doit être mappée à un utilisateur dans la base de données master.  
  
 À présent les non-administrateurs peuvent lancer des processus de système d’exploitation avec **xp_cmdshell** et ces processus sont exécutés avec les autorisations du compte proxy que vous avez configuré. Les utilisateurs avec l’autorisation CONTROL SERVER (membres de la **sysadmin** rôle serveur fixe) continueront de recevoir les autorisations de le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service pour les processus enfants lancés par **xp_cmdshell**.  
  
 Pour déterminer le compte Windows utilisé par **xp_cmdshell** lors du lancement du processus du système d’exploitation, exécutez l’instruction suivante :  
  
```  
xp_cmdshell 'whoami.exe'  
  
```  
  
 Pour déterminer le contexte de sécurité pour une autre connexion, exécutez ce qui suit :  
  
```  
EXECUTE AS LOGIN = '<other_login>' ;  
GO  
xp_cmdshell 'whoami.exe' ;  
REVERT ;  
  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-a-list-of-executable-files"></a>A. Retour d'une liste de fichiers exécutables  
 L'exemple ci-dessous montre la procédure stockée étendue `xp_cmdshell` exécutant une commande de répertoire.  
  
```  
EXEC master..xp_cmdshell 'dir *.exe''  
```  
  
### <a name="b-returning-no-output"></a>B. Aucun renvoi d'information en sortie  
 L'exemple ci-dessous utilise `xp_cmdshell` pour exécuter une chaîne de commande sans retourner les informations en sortie au client.  
  
```  
USE master;  
  
EXEC xp_cmdshell 'copy c:\SQLbcks\AdvWorks.bck  
    \\server2\backups\SQLbcks, NO_OUTPUT';  
GO  
```  
  
### <a name="c-using-return-status"></a>C. Utilisation de l'état de retour  
 Dans l’exemple suivant, la `xp_cmdshell` procédure stockée étendue suggère également l’état de retour. La valeur du code de retour est stockée dans la variable `@result`.  
  
```  
DECLARE @result int;  
EXEC @result = xp_cmdshell 'dir *.exe';  
IF (@result = 0)  
   PRINT 'Success'  
ELSE  
   PRINT 'Failure';  
```  
  
### <a name="d-writing-variable-contents-to-a-file"></a>D. Écriture du contenu des variables dans un fichier  
 Dans l'exemple ci-dessous, le contenu de la variable `@var` est écrit dans un fichier nommé `var_out.txt` dans le répertoire actif sur le serveur.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'Hello world';  
SET @cmd = 'echo ' + @var + ' > var_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
### <a name="e-capturing-the-result-of-a-command-to-a-file"></a>E. Capture du résultat d’une commande dans un fichier  
 Dans l'exemple ci-dessous, le contenu du répertoire actif est écrit dans un fichier nommé `dir_out.txt` dans le répertoire actif du serveur.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'dir/p';  
SET @cmd = @var + ' > dir_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées étendues générales &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_cmdshell (option) de Configuration de serveur](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md)   
 [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)   
 [sp_xp_cmdshell_proxy_account &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md)  
  
  
