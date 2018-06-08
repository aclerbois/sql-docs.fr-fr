---
title: ad hoc distributed queries (option de configuration de serveur) | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- OPENROWSET function, ad hoc distributed queries option
- Ad Hoc Distributed Queries option
- ad hoc distributed queries
- 7415 (Database Engine Error)
- OPENDATASOURCE function, ad hoc distributed queries option
- ad hoc access
ms.assetid: 5b982015-e196-44c3-83b8-275fb9d769b2
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ffd499463bfceb7394058e74f37674dc946360bc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ad-hoc-distributed-queries-server-configuration-option"></a>ad hoc distributed queries (option de configuration de serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'autorise pas les requêtes distribuées ad hoc avec les fonctions OPENROWSET et OPENDATASOURCE. Lorsque cette option est définie sur 1, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise l'accès d'égal à égal. Lorsque cette option n'est pas définie ou lorsqu'elle est définie sur 0, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne permet pas non plus l'accès d'égal à égal.  
  
 Les requêtes distribuées ad hoc utilisent les fonctions OPENROWSET et OPENDATASOURCE pour la connexion aux sources de données distantes OLE DB. OPENROWSET et OPENDATASOURCE doivent être utilisés uniquement pour faire référence à des sources de données OLE DB faisant l'objet d'accès peu fréquents. Pour les sources de données faisant l'objet d'accès plus fréquents, définissez un serveur lié.  
  
> [!IMPORTANT]  
>  L'activation de l'utilisation de noms ad hoc signifie que toute connexion authentifiée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut accéder au fournisseur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les administrateurs doivent activer cette fonctionnalité pour les fournisseurs accessibles en toute sécurité via une connexion locale.  
  
## <a name="remarks"></a>Notes   
 Toute tentative d’établissement d’une connexion ad hoc alors que l’option **Requêtes distribuées ad hoc** n’est pas activée génère une erreur : Msg 7415, Niveau 16, État 1, Ligne 1  
  
 L'accès d'égal à égal au fournisseur OLE DB « Microsoft.ACE.OLEDB.12.0 » a été refusé. Vous devez accéder à ce fournisseur par le biais d'un serveur lié.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant active des requêtes distribuées ad hoc puis interroge un serveur nommé `Seattle1` à l'aide de la fonction `OPENROWSET` .  
  
```  
sp_configure 'show advanced options', 1;  
RECONFIGURE;
GO 
sp_configure 'Ad Hoc Distributed Queries', 1;  
RECONFIGURE;  
GO  
  
SELECT a.*  
FROM OPENROWSET('SQLNCLI', 'Server=Seattle1;Trusted_Connection=yes;',  
     'SELECT GroupName, Name, DepartmentID  
      FROM AdventureWorks2012.HumanResources.Department  
      ORDER BY GroupName, Name') AS a;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)  
  
  
