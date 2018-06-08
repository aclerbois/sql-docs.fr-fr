---
title: catalog.execution_property_override_values | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 83cbdd6f-ddde-47bf-abde-36bd24272621
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4b47ba51eca7c6494090870ba65e7c4472974988
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogexecutionpropertyoverridevalues"></a>catalog.execution_property_override_values
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche les valeurs de remplacement de la propriété définies pendant l'exécution du package.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|property_id|**bigint**|Identificateur unique de valeur de remplacement de la propriété.|  
|execution_id|**bigint**|ID unique de l'instance d'exécution.|  
|property_path|**nvarchar(4000)**|Chemin d'accès à la propriété dans le package.|  
|property_value|**nvarchar(max)**|Valeur de remplacement de la propriété.|  
|sensible|**bit**|Lorsque la valeur est 1, la propriété est sensible et est chiffrée lorsqu'elle est stockée. Lorsque la valeur est 0, la propriété n'est pas sensible et la valeur est stockée dans en texte en clair.|  
  
## <a name="remarks"></a>Notes   
 Cette vue affiche une ligne pour chaque exécution dans laquelle les valeurs des propriétés ont été remplacées à l’aide de la section **Substitutions de propriété** sous l’onglet **Avancé** de la boîte de dialogue **Exécuter le package**. Le chemin d’accès de la propriété est dérivé de la propriété **Chemin d’accès au package** de la tâche du package.  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'instance d'exécution  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
