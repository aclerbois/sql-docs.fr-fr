---
title: sp_pdw_add_network_credentials (SQL Data Warehouse) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 0729eeff-ac7e-43f0-80fa-ff5346a75985
caps.latest.revision: 10
author: ronortloff
ms.author: rortloff
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 181e40128ec2f8f347039d60409577d25694ea39
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="sppdwaddnetworkcredentials-sql-data-warehouse"></a>sp_pdw_add_network_credentials (entrepôt de données SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Cette option stocke les informations d’identification réseau dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et les associe à un serveur. Par exemple, utilisez cette procédure stockée pour permettre à [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] les autorisations de lecture/écriture pour effectuer la sauvegarde de base de données et de restauration sur un serveur cible, ou pour créer une sauvegarde d’un certificat utilisé pour le chiffrement transparent des données appropriées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
sp_pdw_add_network_credentials 'target_server_name',  'user_name', ꞌpasswordꞌ  
```  
  
## <a name="arguments"></a>Arguments  
 '*target_server_name*'  
 Spécifie le nom d’hôte serveur cible ou l’adresse IP. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] accède à ce serveur en utilisant les informations d’identification de nom d’utilisateur et mot de passe transférées à cette procédure stockée.  
  
 Pour vous connecter via le réseau InfiniBand, utilisez l’adresse InfiniBand IP du serveur cible.  
  
 *nom_du_serveur_cible* est défini comme nvarchar(337).  
  
 '*user_name*'  
 Spécifie le nom d’utilisateur qui dispose des autorisations pour accéder au serveur cible. Si les informations d’identification existent déjà pour le serveur cible, ils seront mis à jour pour les nouvelles informations d’identification.  
  
 *user_name* est défini comme nvarchar (513).  
  
 '*mot de passe*ꞌ  
 Spécifie le mot de passe *nom_utilisateur*.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Requiert **ALTER SERVER STATE** autorisation.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Une erreur se produit si l’ajout d’informations d’identification ne réussit pas sur le nœud de contrôle et de tous les nœuds de calcul.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Cette procédure stockée ajoute des informations d’identification réseau pour le compte NetworkService de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Le compte NetworkService exécute chaque instance de SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le nœud de contrôle et les nœuds de calcul. Par exemple, lorsqu’une opération de sauvegarde s’exécute, le nœud de contrôle et de chaque nœud de calcul utilisera les informations d’identification du compte NetworkService pour accéder en lecture et l’autorisation d’écriture sur le serveur cible.  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-add-credentials-for-performing-a-database-backup"></a>A. Ajouter des informations d’identification pour effectuer une sauvegarde de base de données  
 L’exemple suivant associe le nom et mot de passe utilisateur pour le seattle\david d’utilisateur de domaine à un serveur cible qui a une adresse IP de 10.172.63.255. Seattle\david de l’utilisateur dispose des autorisations de lecture/écriture sur le serveur cible. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] est de stocker ces informations d’identification et de les utiliser pour lire et écrire vers et depuis le serveur cible, si nécessaire pour la sauvegarde et les opérations de restauration.  
  
```  
EXEC sp_pdw_add_network_credentials '10.172.63.255', 'seattle\david', '********';  
```  
  
 La commande de sauvegarde nécessite que le nom du serveur entré une adresse IP.  
  
> [!NOTE]  
>  Pour effectuer la sauvegarde de base de données sur InfiniBand, veillez à utiliser l’adresse InfiniBand IP du serveur de sauvegarde.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_pdw_remove_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
  

