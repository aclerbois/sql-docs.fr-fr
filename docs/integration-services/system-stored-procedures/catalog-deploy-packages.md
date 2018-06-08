---
title: catalog.deploy_packages | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 8e861df6-d103-4d84-8438-e822533f6849
caps.latest.revision: 4
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f79688853f2c165364c1057a77d03dc01ad5fbd9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogdeploypackages"></a>catalog.deploy_packages
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Déploie un ou plusieurs packages dans un dossier du catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ou met à jour un package existant qui a été déployé.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
[catalog].[deploy_packages]     [ @folder_name = ] folder_name,    [ @project_name = ] project_name,    [ @packages_table = ] packages_table,     [ @operation_id OUTPUT ] operation_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @folder_name = ] *folder_name*  
 Nom du dossier. *folder_name* est de type **nvarchar(128)**.  
  
 [ @project_name = ] *project_name*  
 Nom du projet dans le dossier. *project_name* est de type **nvarchar(128)**.  
  
 [ @packages_table = ] *packages_table*  
 Contenu binaire du (des) fichier(s) de package (.dtsx) [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. *packages_table* est de type **[catalog].[Package_Table_Type]**  
  
 [ @operation_id = ] *operation_id*  
 Retourne l'identificateur unique de l'opération de déploiement. *operation_id* est de type **bigint**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations CREATE_OBJECTS sur le projet ou autorisations MODIFY sur le package pour mettre à jour un package.  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur de cette procédure stockée :  
  
-   Un paramètre fait référence à un objet qui n’existe pas, un paramètre essaie de créer un objet qui existe déjà, ou un paramètre n’est pas valide d’une autre manière.  
  
-   L'utilisateur n'a pas des autorisations suffisantes.  
  
  
