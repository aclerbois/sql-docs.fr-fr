---
title: Applet de commande Remove-RoleMember | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: eec431908e2159f7021613c086f1278cb2954b43
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="remove-rolemember-cmdlet"></a>Applet de commande Remove-RoleMember
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Supprimez un membre du rôle spécifié d'une base de données Analysis Services.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
## <a name="syntax"></a>Syntaxe  
 `Remove-RoleMember [-MemberName] <System.String> [-Database] <System.String> [-RoleName] <System.String> [<CommonParameters>]`  
  
 `Remove-RoleMember [-DatabaseRole] <Microsoft.AnalysisServices.Role> [-MemberName] <System.String>  [<CommonParameters>]`  
  
## <a name="description"></a> Description  
 L'applet de commande Remove-RoleMember supprime un membre existant d'un rôle d'une base de données Analysis Services.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-membername-string"></a>-MemberName \<chaîne >  
 Spécifie l'utilisateur ou le groupe Windows à supprimer du rôle.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-database-string"></a>-De base de données \<chaîne >  
 Spécifie la base de données à laquelle le rôle appartient.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|1|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-rolename-string"></a>-RoleName \<chaîne >  
 Spécifie le rôle duquel vous supprimez des membres.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|2|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-databaserole-string"></a>-DatabaseRole \<chaîne >  
 Spécifie l'objet Microsoft.AnalysisServices.Role duquel le membre est supprimé. Utilisez ce paramètre comme une alternative aux paramètres –Database et –RoleName, lorsque vous souhaitez fournir le rôle de base de données via le pipeline.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true (ByPropertyName)|  
|Accepter les caractères génériques ?|false|  
  
### <a name="commonparameters"></a>\<Paramètres_courants >  
 La commande cmdlet prend en charge les paramètres communs : -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer et -OutVariable. Pour plus d’informations, consultez [About_CommonParameters](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Aucun.  
  
## <a name="example-1"></a>Exemple 1  
  
```  
PS SQLSERVER:\sqlas\localhost\default> remove-rolemember –membername “adventure-works\bobh” –database “AdventureWorks” –rolename “Reader”  
```  
  
 Cette commande supprime un compte d'utilisateur de domaine Windows du rôle Lecteur pour la base de données AdventureWorks exécutée sur une instance par défaut locale.  
  
## <a name="example-2"></a>Exemple 2  
  
```  
PS SQLSERVER:\sqlas\localhost\default> $roles= dir .\databases\AWTEST\Roles  
PS SQLSERVER:\sqlas\localhost\default> $roles  
PS SQLSERVER:\sqlas\localhost\default> remove-rolemember –membername:“adventure-works\bobh” –databaserole:$roles[0]  
```  
  
 La ligne 1 ajoute tous les rôles de la base de données AWTEST au pipeline. La ligne 2, où vous tapez $roles à l'invite, affiche le tableau de rôles. La ligne 3 supprime l'utilisateur Windows « adventure-works\bobh » du premier rôle dans le tableau.  
  
## <a name="example-3"></a>Exemple 3  
  
```  
PS SQLSERVER:\sqlas\localhost\default\Databases\AWTEST\Roles> $roles=dir  
PS SQLSERVER:\sqlas\localhost\default\Databases\AWTEST\Roles> $roles[0] | Remove-rolemember –membername “adventure-works\bobh”  
```  
  
 Cette commande supprime un compte d'utilisateur de domaine Windows du premier rôle dans un tableau, où le tableau est créé en répertoriant les enfants du dossier Rôles, dans le contexte d'une base de données spécifique (AWTEST).  
  

  
  
