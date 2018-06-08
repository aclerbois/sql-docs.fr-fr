---
title: catalog.create_customized_logging_level | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 20b3ba0a-126f-49bf-b70f-61b2a0fcb750
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 85a3146a5ea99bf46b37a67d61bc4177ae750ebd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogcreatecustomizedlogginglevel"></a>catalog.create_customized_logging_level
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Créer un niveau de journalisation personnalisée. Pour plus d’informations sur les niveaux de journalisation personnalisée, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.create_customized_logging_level [ @level_name = ] level_name  
    , [ @level_description = ] level_description  
    , [ @profile_value = ] profile_value  
    , [ @event_value = ] event_value  
    , [ @level_id = ] level_id OUT   
```  
  
## <a name="arguments"></a>Arguments  
 [ @level_name = ] *level_name*  
 Nom du nouveau niveau de journalisation personnalisée.  
  
 *level_name* est de type **nvarchar(128)**.  
  
 [ @level_description = ] *level_description*  
 Description du nouveau niveau de journalisation personnalisée.  
  
 *level_description* est de type **nvarchar(1024)**.  
  
 [ @profile_value = ] *profile_value*  
 Statistiques que le nouveau niveau de journalisation personnalisée doit journaliser.  
  
 Les valeurs valides pour les statistiques sont indiquées ci-après. Ces valeurs correspondent aux valeurs sous l’onglet **Statistiques** de la boîte de dialogue **Gestion du niveau de journalisation personnalisée**.  
  
-   Exécution = 0  
  
-   Volume = 1  
  
-   Performances = 2  
  
 *profile_value* est de type **bigint**.  
  
 [ @event_value = ] *event_value*  
 Événements que le nouveau niveau de journalisation personnalisée doit journaliser.  
  
 Les valeurs valides pour les événements sont indiquées ci-après. Ces valeurs correspondent aux valeurs sous l’onglet **Événements** de la boîte de dialogue **Gestion du niveau de journalisation personnalisée**.  
  
|Événements sans contexte d’événement|Événements avec contexte d’événement|  
|----------------------------------|-------------------------------|  
|OnVariableValueChanged = 0<br /><br /> OnExecutionStatusChanged = 1<br /><br /> OnPreExecute = 2<br /><br /> OnPostExecute = 3<br /><br /> OnPreValidate = 4<br /><br /> OnPostValidate = 5<br /><br /> OnWarning = 6<br /><br /> OnInformation = 7<br /><br /> OnError = 8<br /><br /> OnTaskFailed = 9<br /><br /> OnProgress = 10<br /><br /> OnQueryCancel = 11<br /><br /> OnBreakpointHit = 12<br /><br /> OnCustomEvent = 13<br /><br /> Diagnostic = 14<br /><br /> DiagnosticEx = 15<br /><br /> NonDiagnostic = 16|OnVariableValueChanged_IncludeContext = 32<br /><br /> OnExecutionStatusChanged_IncludeContext = 33<br /><br /> OnPreExecute_IncludeContext = 34<br /><br /> OnPostExecute_IncludeContext = 35<br /><br /> OnPreValidate_IncludeContext = 36<br /><br /> OnPostValidate_IncludeContext = 37<br /><br /> OnWarning_IncludeContext = 38<br /><br /> OnInformation_IncludeContext = 39<br /><br /> OnError_IncludeContext = 40<br /><br /> OnTaskFailed_IncludeContext = 41<br /><br /> OnProgress_IncludeContext = 42<br /><br /> OnQueryCancel_IncludeContext= 43<br /><br /> OnBreakpointHit_IncludeContext = 44<br /><br /> OnCustomEvent_IncludeContext = 45<br /><br /> Diagnostic_IncludeContext = 46<br /><br /> DiagnosticEx_IncludeContext = 47<br /><br /> NonDiagnostic_IncludeContext = 48|  
  
 *event_value* est de type **bigint**.  
  
 [ @level_id = ] *level_id* OUT  
 ID du nouveau niveau de journalisation personnalisée.  
  
 *level_id* est de type **bigint**.  
  
## <a name="remarks"></a>Notes   
 Pour combiner plusieurs valeurs dans Transact-SQL pour l’argument *profile_value* ou *event_value*, suivez cet exemple. Pour capturer les événements OnError (8) et DiagnosticEx (15), la formule permettant de calculer *event_value* est `2^8 + 2^15 = 33024`.  
  
## <a name="return-codes"></a>Codes de retour  
 0 (succès)  
  
 Lorsque la procédure stockée échoue, elle génère une erreur.  
  
## <a name="result-set"></a>Jeu de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   L’appartenance au rôle de base de données **ssis_admin**  
  
-   L’appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit les conditions provoquant l'échec de la procédure stockée.  
  
-   L’utilisateur ne dispose pas des autorisations requises.  
  
  
