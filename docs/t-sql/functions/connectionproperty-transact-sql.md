---
title: CONNECTIONPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CONNECTIONPROPERTY_TSQL
- CONNECTIONPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- CONNECTIONPROPERTY statement
ms.assetid: 6bd9ccae-af77-4a05-b97f-f8ab41cfde42
caps.latest.revision: 25
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 51685541435f4ab0192792341d2a29210dc9f504
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connectionproperty-transact-sql"></a>CONNECTIONPROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Pour une demande arrivant sur le serveur, cette fonction retourne des informations sur les propriétés de la connexion unique qui prend en charge cette demande.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CONNECTIONPROPERTY ( property )  
```  
  
## <a name="arguments"></a>Arguments  
*property*  
Propriété de la connexion. *property* peut avoir l’une des valeurs suivantes :
  
|Valeur|Type de données|Description|  
|---|---|---|
|net_transport|**nvarchar(40)**|Retourne le protocole de transport physique utilisé par cette connexion. Cette valeur ne peut pas être NULL. Les valeurs de retour possibles sont :<br /><br /> **HTTP**<br /> **Canal nommé**<br /> **Session**<br /> **Mémoire partagée**<br /> **SSL**<br /> **TCP**<br /><br /> et<br /><br /> **VIA**<br /><br /> Remarque : Retourne toujours **Session** lorsque MARS (Multiple Active Result Sets) est activé pour une connexion et que le regroupement de connexions est activé.|  
|protocol_type|**nvarchar(40)**|Retourne le type de protocole de charge utile. Il effectue la distinction entre TDS (TSQL) et SOAP. Autorise la valeur NULL.|  
|auth_scheme|**nvarchar(40)**|Renvoie le schéma d’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’une connexion. Le schéma d'authentification est soit l'authentification Windows (NTLM, KERBEROS, DIGEST, BASIC, NEGOTIATE), soit l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. N'accepte pas la valeur NULL.|  
|local_net_address|**varchar(48)**|Retourne l'adresse IP sur le serveur ciblé par cette connexion spécifique. Disponible uniquement pour les connexions qui utilisent le fournisseur de transport TCP. Autorise la valeur NULL.|  
|local_tcp_port|**Int**|Retourne le port TCP du serveur ciblé par cette connexion s'il s'agissait d'une connexion qui utilise le transport TCP. Autorise la valeur NULL.|  
|client_net_address|**varchar(48)**|Demande l'adresse du client qui tente de se connecter à ce serveur. Autorise la valeur NULL.|  
|physical_net_transport|**nvarchar(40)**|Retourne le protocole de transport physique utilisé par cette connexion. Précis lorsque plusieurs jeux MARS (Multiple Active Result Set) sont activés pour une connexion.|  
|\<Toute autre chaîne>||Retourne la valeur NULL pour une entrée non valide.|  
  
## <a name="remarks"></a>Notes   
**local_net_address** et **local_tcp_port** renvoient la valeur NULL dans [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
Les valeurs renvoyées correspondent aux options indiquées pour les colonnes correspondantes dans la vue de gestion dynamique [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md). Exemple :
  
```sql
SELECT   
ConnectionProperty('net_transport') AS 'Net transport',   
ConnectionProperty('protocol_type') AS 'Protocol type';  
```  
  
## <a name="see-also"></a>Voir aussi
[sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
  
  
