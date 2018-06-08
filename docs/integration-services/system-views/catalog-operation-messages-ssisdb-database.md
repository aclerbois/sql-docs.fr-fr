---
title: catalog.operation_messages (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- catalog.operation_messages view [Integration Services]
- operation_messages view [Integration Services]
ms.assetid: 0b3cbe38-ce24-47ca-83ef-6538a5299d1a
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 920d2b8ab5eb2d0635fbdac7170fbe5d2ecfe8ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogoperationmessages-ssisdb-database"></a>catalog.operation_messages (base de données SSISDB)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Affiche des messages entrés pendant des opérations dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|operation_message_id|**bigint**|Identificateur (ID) unique du message.|  
|operation_id|**bigint**|ID unique de l'opération.|  
|message_time|**datetimeoffset(7)**|Date et heure de création du message.|  
|message_type|**smallint**|Type de message affiché.|  
|message_source_type|**smallint**|ID du type de source du message.|  
|message|**nvarchar(max)**|Texte du message.|  
|extended_info_id|**bigint**|ID des informations supplémentaires relatives au message d’opération, trouvé dans la vue [extended_operation_info](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md).|  
  
## <a name="remarks"></a>Notes   
 Cette vue affiche une ligne pour chaque message entré pendant une opération dans le catalogue. Le message peut être généré par le serveur, par le processus d'exécution du package ou par le moteur d'exécution.  
  
 Cette vue affiche les types de message suivants :  
  
|Valeur **message_type**|Description|  
|-----------------------------|-----------------|  
|-1|Unknown|  
|120|Error|  
|110|Avertissement|  
|70|Informations|  
|10|Pré-valider|  
|20|Post-valider|  
|30|Pré-exécuter|  
|40|Post-exécuter|  
|60|Progress|  
|50|StatusChange|  
|100|QueryCancel|  
|130|TaskFailed|  
|90|Diagnostic|  
|200|Custom|  
|140|DiagnosticEx<br /><br /> Chaque fois qu'une tâche d'exécution de package exécute un package enfant, enregistre cet événement. Le message d'événement inclut les valeurs de paramètre passées aux packages enfants.<br /><br /> La valeur de la colonne de message pour DiagnosticEx est du texte XML.|  
|400|NonDiagnostic|  
|80|VariableValueChanged|  
  
 Cette vue affiche les types de source de message suivants.  
  
|**message_source_type**|Description|  
|-------------------------------|-----------------|  
|10|API d'entrée, telles que T-SQL et les procédures stockées CLR|  
|20|Processus externe utilisé pour exécuter le package (ISServerExec.exe)|  
|30|Objets de niveau package|  
|40|Tâches de flux de contrôle|  
|50|Conteneurs de flux de contrôle|  
|60|Tâche de flux de données|  
  
## <a name="permissions"></a>Autorisations  
 Cette vue requiert l'une des autorisations suivantes :  
  
-   Autorisation READ sur l'opération  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
> [!NOTE]  
>  Lorsque vous avez l'autorisation pour effectuer une opération sur le serveur, vous avez également l'autorisation pour consulter les informations de l'opération. La sécurité au niveau de la ligne est imposée ; uniquement les lignes que vous avez l'autorisation d'afficher s'affichent.  
  
  
