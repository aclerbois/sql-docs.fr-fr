---
title: '@@SERVERNAME (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- '@@SERVERNAME'
- '@@SERVERNAME_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@SERVERNAME function'
- local servers [SQL Server]
ms.assetid: b0ef33fb-954a-4294-b05b-a87c14ce25a3
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7b59ab089a0e2782a326d2d3f913f109869ef32f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="x40x40servername-transact-sql"></a>&#x40;&#x40;SERVERNAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Renvoie le nom du serveur local qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

 ![Icône Lien de l’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
@@SERVERNAME  
```  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar**  
  
## <a name="remarks"></a>Notes   
 Lors de l'installation, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit le nom du serveur avec le nom de l'ordinateur. Pour modifier le nom du serveur, utilisez **sp_addserver**, puis redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Quand plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées, @@SERVERNAME retourne les informations relatives au nom du serveur local suivantes si le nom du serveur local n’a pas changé depuis l’installation.  
  
|Instance|Informations sur le serveur|  
|--------------|------------------------|  
|Instance par défaut|'*nom_serveur*'|  
|Instance nommée|'*nom_serveur*\\*nom_instance*'|  
|Instance de cluster de basculement - instance par défaut|'*nom_serveur_virtuel*'|  
|Instance de cluster de basculement - instance nommée|'*nom_serveur_virtuel*\\*nom_instance*'|  
  
 Bien que la fonction @@SERVERNAME et la propriété SERVERNAME de la fonction SERVERPROPERTY puissent renvoyer des chaînes de mêmes formats, les informations peuvent être différentes. La propriété SERVERNAME rapporte automatiquement les modifications apportées au nom réseau de l'ordinateur.  
  
 En revanche, @@SERVERNAME ne signale pas les modifications de ce type. @@SERVERNAME signale les modifications apportées au nom du serveur local à l'aide de la procédure stockée **sp_addserver** ou **sp_dropserver**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre l'utilisation de `@@SERVERNAME`.  
  
```  
SELECT @@SERVERNAME AS 'Server Name'  
```  
  
 Un exemple d'ensemble de résultats est présenté ci-dessous.  
  
```  
Server Name  
---------------------------------  
ACCTG  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctions de configuration &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)  
  
  
