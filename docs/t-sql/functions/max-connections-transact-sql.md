---
title: '@@MAX_CONNECTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- '@@MAX_CONNECTIONS'
- '@@MAX_CONNECTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- simultaneous connections [SQL Server]
- maximum number of simultaneous user connections
- '@@MAX_CONNECTIONS function'
- connections [SQL Server], simultaneous
- number of simultaneous user connections
ms.assetid: 57eb9f4b-548f-4212-9684-a11d831c4732
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2cd500fa1d680db44fe47b1ab4992513a65eee0a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="x40x40maxconnections-transact-sql"></a>&#x40;&#x40;MAX_CONNECTIONS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie le nombre maximal de connexions utilisateur simultanément autorisées sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le nombre renvoyé n'est pas nécessairement le nombre actuellement configuré.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
@@MAX_CONNECTIONS  
```  
  
## <a name="return-types"></a>Types de retour  
 **entier**  
  
## <a name="remarks"></a>Notes   
 Le nombre réel de connexions utilisateur autorisées dépend également de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous utilisez ainsi que des limites de vos applications et de votre matériel.  
  
 Pour reconfigurer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin d’autoriser un plus petit nombre de connexions, utilisez **sp_configure**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre le renvoi du nombre maximal de connexions utilisateur sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cet exemple suppose que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas été reconfiguré pour un nombre de connexions utilisateur inférieur.  
  
```  
SELECT @@MAX_CONNECTIONS AS 'Max Connections';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Max Connections  
---------------  
32767            
```  
  
## <a name="see-also"></a> Voir aussi  
 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Fonctions de configuration](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [Configurer l’option de configuration du serveur user connections](../../database-engine/configure-windows/configure-the-user-connections-server-configuration-option.md)  
  
  
