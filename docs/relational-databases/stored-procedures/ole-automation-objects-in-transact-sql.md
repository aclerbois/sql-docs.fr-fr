---
title: Objets OLE Automation dans Transact-SQL | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-ole
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], OLE Automation
- batches [SQL Server], OLE Automation
- OLE Automation [SQL Server]
- OLE Automation [SQL Server], about OLE Automation
ms.assetid: a887d956-4cd0-400a-aa96-00d7abd7c44b
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3cf6cfa987f1022107679c79206d9714b0fd9985
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-automation-objects-in-transact-sql"></a>Objets OLE Automation dans Transact-SQL
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] contient plusieurs procédures stockées système qui permettent aux objets OLE Automation d’être référencés dans les lots [!INCLUDE[tsql](../../includes/tsql-md.md)] , les procédures stockées et les déclencheurs. Les procédures stockées système sont exécutées en tant que procédures stockées étendues, et les objets OLE Automation invoqués par les procédures stockées sont exécutés dans l'espace d'adressage d'une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] de la même façon que les procédures stockées étendues.  
  
 Les procédures stockées OLE Automation permettent aux lots [!INCLUDE[tsql](../../includes/tsql-md.md)] de faire référence aux objets SQL-DMO et aux objets OLE Automation personnalisés, notamment ceux qui exposent l’interface **IDispatch** . Un serveur OLE in-process personnalisé qui est créé à l’aide de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] doit disposer d’un gestionnaire d’erreurs (spécifié avec l’instruction **On Error GoTo** ) pour les sous-routines **Class_Initialize** et **Class_Terminate** . Les erreurs non gérées dans les sous-routines **Class_Initialize** et **Class_Terminate** peuvent entraîner des erreurs imprévisibles, comme une violation d’accès dans une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Les gestionnaires d'erreurs sont également recommandés pour les autres sous-routines.  
  
 Pour utiliser un objet OLE Automation dans [!INCLUDE[tsql](../../includes/tsql-md.md)] , il est nécessaire d’appeler la procédure stockée système **sp_OACreate** pour créer une instance de cet objet dans l’espace d’adressage de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Lorsqu'une instance de l'objet a été créée, appelez les procédures stockées suivantes pour travailler avec les propriétés, méthodes et informations d'erreur de l'objet :  
  
-   **sp_OAGetProperty** obtient la valeur d’une propriété.  
  
-   **sp_OASetProperty** définit la valeur d’une propriété.  
  
-   **sp_OAMethod** appelle une méthode.  
  
-   **sp_OAGetErrorInfo** obtient les informations les plus récentes sur les erreurs.  
  
 Quand l’objet est devenu inutile, appelez **sp_OADestroy** pour désallouer l’instance de l’objet créée avec **sp_OACreate**.  
  
 Les objets OLE Automation retournent les données par l'intermédiaire de valeurs de propriétés et de méthodes. **sp_OAGetProperty** et **sp_OAMethod** retournent ces valeurs de données sous forme d’un ensemble de résultats.  
  
 La portée d'un objet OLE Automation est un traitement. Toutes les références à un objet doivent se trouver dans un seul traitement, une seule procédure stockée ou un seul déclencheur.  
  
 Lorsque des références à des objets sont faites, les objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLE Automation prennent en charge le passage à travers l'objet référencé pour atteindre les autres objets qu'il contient. Par exemple, quand l’objet SQL-DMO **SQLServer** est utilisé, il est possible de créer des références aux bases de données et aux tables qui se trouvent sur ce serveur.  
  
## <a name="related-content"></a>Contenu associé  
 [Syntaxe de hiérarchie des objets &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/object-hierarchy-syntax-transact-sql.md)  
  
 [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)  
  
 [Procédures OLE Automation (option de configuration de serveur)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)  
  
 [sp_OACreate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md)  
  
 [sp_OAGetProperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oagetproperty-transact-sql.md)  
  
 [sp_OASetProperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oasetproperty-transact-sql.md)  
  
 [sp_OAMethod &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oamethod-transact-sql.md)  
  
 [sp_OAGetErrorInfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oageterrorinfo-transact-sql.md)  
  
 [sp_OADestroy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oadestroy-transact-sql.md)  
  
  
