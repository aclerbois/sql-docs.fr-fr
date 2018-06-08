---
title: catalog.revoke_permission (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- revoke_permission stored procedure [Integration Services]
- catalog.revoke_permission stored procedure [Integration Services]
ms.assetid: 850b9c26-5c7c-47b9-a61c-5cf9bb5948cf
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4a9f75b9f3fa96590410754a536d05d702b1832e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogrevokepermission-ssisdb-database"></a>catalog.revoke_permission (base de données SSISDB)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Révoque une autorisation sur un objet sécurisable dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql
catalog.revoke_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>Arguments  
 [ @object_type = ] *object_type*  
 Type d'objet sécurisable. Les types d’objets sécurisables incluent le dossier (`1`), le projet (`2`), l’environnement (`3`) et l’opération (`4`). *object_type* est de type **smallint***.*  
  
 [ @object_id = ] *object_id*  
 Identificateur unique (ID) de l’objet sécurisable. *object_id* est de type **bigint**.  
  
 [ @principal_id = ] *principal_id*  
 ID du principal auquel révoquer l'autorisation. *principal_id* est de type **int**.  
  
 [ @permission_type = ] *permission_type*  
 Type de l'autorisation. *permission_type* est de type **smallint**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès)  
  
 1 (object_class n’est pas valide)  
  
 2 (object_id n’existe pas)  
  
 3 (le principal n’existe pas)  
  
 4 (l’autorisation n’est pas valide)  
  
 5 (autre erreur)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes   
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations ASSIGN_PERMISSIONS sur l'objet  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="remarks"></a>Notes   
 Si permission_type est spécifié, la procédure stockée supprime l’autorisation affectée explicitement au principal pour l’objet. Même s'il n'y a pas de telles instances, la procédure retourne une valeur de code de réussite (`0`). Si permission_type est omis, la procédure stockée supprime toutes les autorisations du principal à l’objet.  
  
> [!NOTE]  
>  Le principal peut encore avoir l'autorisation spécifiée sur l'objet s'il est membre d'un rôle qui a l'autorisation spécifiée.  
  
 Cette procédure stockée vous permet de révoquer les types d'autorisation décrits dans le tableau suivant :  
  
|Valeur permission_type|Nom de l'autorisation|Description de l'autorisation|Types d'objet applicables|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|Permet au principal de lire des informations considérées comme faisant partie de l'objet, telles que les propriétés. Il n'autorise pas le principal à énumérer ou à lire le contenu d'autres objets contenus dans l'objet.|Dossier, projet, environnement, opération|  
|`2`|MODIFY|Permet au principal de modifier des informations considérées comme faisant partie de l'objet, telles que les propriétés. Il ne permet pas au principal de modifier d'autres objets contenus dans l'objet.|Dossier, projet, environnement, opération|  
|`3`|Exécutez|Permet au principal d'exécuter tous les packages dans le projet.|Projet|  
|`4`|MANAGE_PERMISSIONS|Permet au principal d'affecter des autorisations aux objets.|Dossier, projet, environnement, opération|  
|`100`|CREATE_OBJECTS|Permet au principal de créer des objets dans le dossier.|Dossier|  
|`101`|READ_OBJECTS|Permet au principal de lire tous les objets dans le dossier.|Dossier|  
|`102`|MODIFY_OBJECTS|Permet au principal de modifier tous les objets dans le dossier.|Dossier|  
|`103`|EXECUTE_OBJECTS|Permet au principal d'exécuter tous les packages de tous les projets dans le dossier.|Dossier|  
|`104`|MANAGE_OBJECT_PERMISSIONS|Permet au principal de gérer des autorisations sur tous les objets dans le dossier.|Dossier|  
  
  
