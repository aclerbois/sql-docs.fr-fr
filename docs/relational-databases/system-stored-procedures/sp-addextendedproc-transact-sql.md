---
title: sp_addextendedproc (Transact-SQL) | Documents Microsoft
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
- sp_addextendedproc_TSQL
- sp_addextendedproc
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addextendedproc
ms.assetid: c0d4b47b-a855-451e-90e5-5fb2d836ebfa
caps.latest.revision: 33
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2083d370479fa19049a083ef401574f21740929c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddextendedproc-transact-sql"></a>sp_addextendedproc (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Enregistre le nom d’une nouvelle procédure stockée étendue à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez l’ [intégration CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addextendedproc [ @functname = ] 'procedure' ,   
     [ @dllname = ] 'dll'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@functname =** ] **'***procédure***'**  
 Nom de la fonction à appeler dans la bibliothèque de liens dynamique (DLL). *procédure* est **nvarchar (517)**, sans valeur par défaut. *procédure* peut éventuellement inclure le nom du propriétaire sous la forme *propriétaire_fonction*.  
  
 [  **@dllname =** ] **'***dll***'**  
 Nom de la DLL qui contient la fonction. *DLL* est **varchar (255)**, sans valeur par défaut. Il est recommandé de spécifier le chemin complet d'accès à la DLL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Après la création d’une procédure stockée étendue, il doit être ajouté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de **sp_addextendedproc**. Pour plus d’informations, consultez [Ajout d’une procédure stockée étendue à SQL Server](../../relational-databases/extended-stored-procedures-programming/adding-an-extended-stored-procedure-to-sql-server.md).  
  
 Cette procédure peut être exécutée uniquement dans les **master** base de données. Pour exécuter une procédure stockée étendue à partir d’une base de données autre que **master**, qualifiez le nom de la procédure stockée étendue avec **master**.  
  
 **sp_addextendedproc** ajoute des entrées à la [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) vue de catalogue, l’inscription du nom de la nouvelle procédure stockée étendue avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il ajoute également une entrée dans le [sys.extended_procedures](../../relational-databases/system-catalog-views/sys-extended-procedures-transact-sql.md) affichage catalogue.  
  
> [!IMPORTANT]  
>  Les DLL existantes qui n'ont pas été inscrites avec leur chemin complet ne fonctionneront plus après une mise à niveau vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour corriger le problème, utilisez **sp_dropextendedproc** pour annuler l’inscription de la DLL, puis inscrivez-la avec **sp_addextendedproc**, en spécifiant le chemin d’accès complet.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_addextendedproc**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant ajoute le **xp_hello** la procédure stockée étendue.  
  
```  
USE master;  
GO  
EXEC sp_addextendedproc xp_hello, 'c:\xp_hello.dll';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_dropextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproc-transact-sql.md)   
 [sp_helpextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpextendedproc-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
