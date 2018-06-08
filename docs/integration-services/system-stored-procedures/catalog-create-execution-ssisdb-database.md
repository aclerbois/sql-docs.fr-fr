---
title: catalog.create_execution (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 45d0c2f6-1f38-445f-ac06-e2a01f6ac600
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: bb8d73d15fea799cc160920d5bbd92b903c21991
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalogcreateexecution-ssisdb-database"></a>catalog.create_execution (base de données SSISDB)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Crée une instance d'exécution dans le catalogue [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Cette procédure stockée utilise le niveau de journalisation par défaut du serveur.  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
catalog.create_execution [@folder_name = folder_name  
     , [@project_name =] project_name  
     , [@package_name =] package_name  
  [  , [@reference_id =] reference_id ]  
  [  , [@use32bitruntime =] use32bitruntime ] 
  [  , [@runinscaleout =] runinscaleout ]
  [  , [@useanyworker =] useanyworker ] 
     , [@execution_id =] execution_id OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
 [@folder_name =] *folder_name*  
 Nom du dossier qui contient le package qui sera exécuté. *folder_name* est de type **nvarchar(128)**.  
  
 [@project_name =] *project_name*  
 Nom du projet qui contient le package à exécuter. *project_name* est de type **nvarchar(128)**.  
  
 [@package_name =] *package_name*  
 Nom du package qui sera exécuté. *package_name* est de type **nvarchar(260)**.  
  
 [@reference_id =] *reference_id*  
 Identificateur unique d'une référence environnementale. Ce paramètre est facultatif. *reference_id* est de type **bigint**.  
  
 [@use32bitruntime =] *use32bitruntime*  
 Indique si l'exécution 32 bits doit être utilisée pour exécuter le package sur un système d'exploitation 64 bits. Utilisez la valeur 1 pour exécuter le package avec l’exécution 32 bits quand un système d’exploitation 64 bits est exécuté. Utilisez la valeur 0 pour exécuter le package avec l'exécution 64 bits lorsqu'un système d'exploitation 64 bits est exécuté. Ce paramètre est facultatif. *Use32bitruntime* est de type **bit**.  
 
 [@runinscaleout =] *runinscaleout*  
 Indique si l’exécution a lieu dans Scale Out. Utilisez la valeur 1 pour exécuter le package dans Scale Out. Utilisez la valeur 0 pour exécuter le package sans Scale Out. Ce paramètre est facultatif. S’il n’est pas spécifié, sa valeur est définie sur DEFAULT_EXECUTION_MODE dans [SSISDB].[catalog].[catalog_properties]. *runinscaleout* est de type **bit**. 
 
[@useanyworker =] *useanyworker*  
Indique si un Scale Out Worker est autorisé à effectuer l’exécution.

-   Utilisez la valeur 1 pour exécuter le package avec n’importe quel Scale Out Worker. Quand vous définissez `@useanyworker` sur true, tout Worker dont le nombre maximal de tâches (tel que spécifié dans son fichier de configuration) n’est pas encore atteint est disponible pour exécuter le package. Pour plus d’informations sur le fichier de configuration du worker, consultez [Integration Services (SSIS) Scale Out Worker](../scale-out/integration-services-ssis-scale-out-worker.md).

-   Utilisez la valeur 0 pour indiquer que tous les Scale Out Workers ne sont pas autorisés à exécuter le package. Quand vous définissez `@useanyworker` sur false, vous devez spécifier les Workers qui sont autorisés à exécuter le package à l’aide de Scale Out Manager ou en appelant la procédure stockée `[catalog].[add_execution_worker]`. Si vous spécifiez un worker qui exécute déjà un autre package, le worker termine l’exécution du package en cours avant de demander une autre exécution.

Ce paramètre est facultatif. S’il n’est pas spécifié, sa valeur est définie sur 1. *useanyworker* est de type **bit**. 
  
 [@execution_id =] *execution_id*  
 Retourne l'identificateur unique d'une instance d'exécution. *execution_id* est de type **bigint**.  

  
## <a name="remarks"></a>Notes   
 Une exécution est utilisée pour spécifier les valeurs de paramètre qui sont utilisées par un package pendant une instance d'exécution unique du package.  
  
 Si une référence environnementale est spécifiée avec le paramètre *reference_id*, la procédure stockée remplit les paramètres du package et du projet avec les valeurs littérales ou les valeurs référencées des variables d’environnement correspondantes. Si la référence environnementale est spécifiée, les valeurs de paramètre par défaut sont utilisées pendant l'exécution du package. Pour déterminer exactement quelles valeurs sont utilisées pour une instance particulière d’exécution, utilisez la valeur du paramètre de sortie *execution_id* de cette procédure stockée et interrogez la vue [execution_parameter_values](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md).  
  
 Seuls les packages marqués comme packages de point d'entrée peuvent être spécifiés dans une exécution. Si un package qui n'est pas un point d'entrée est spécifié, l'exécution échoue.  
  
## <a name="example"></a> Exemple  
 L’exemple suivant appelle catalog.create_execution pour créer une instance d’exécution pour le package Child1.dtsx, qui n’est pas dans Scale Out. Project1 Integration Services contient le package. L'exemple appelle catalog.set_execution_parameter_value afin de définir des valeurs pour les paramètres Parameter1, Parameter2 et LOGGING_LEVEL. L'exemple appelle catalog.start_execution pour démarrer une instance d'exécution.  
  
```sql  
Declare @execution_id bigint  
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Child1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'TestDeply4', @project_name=N'Integration Services Project1', @use32bitruntime=False, @reference_id=Null  
Select @execution_id  
DECLARE @var0 sql_variant = N'Child1.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter1', @parameter_value=@var0  
DECLARE @var1 sql_variant = N'Child2.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter2', @parameter_value=@var1  
DECLARE @var2 smallint = 1  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var2  
EXEC [SSISDB].[catalog].[start_execution] @execution_id  
GO  
```  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et EXECUTE sur le projet et, si applicable, autorisations READ sur les environnements référencés  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  

 Si @runinscaleout a pour valeur 1, cette procédure stockée requiert l’une des autorisations suivantes :
 
-   Appartenance au rôle de base de données **ssis_admin**

-   Appartenance au rôle de base de données **ssis_cluster_executor**

-   Appartenance au rôle serveur **sysadmin**
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit quelques conditions qui peuvent générer une erreur ou un avertissement :  
  
-   Le package n’existe pas.  
  
-   L’utilisateur n’a pas les autorisations appropriées.  
  
-   La référence environnementale, *reference_id*, n’est pas valide.  
  
-   Le package spécifié n'est pas un package de point d'entrée.  
  
-   Le type de données de la variable d'environnement référencée est différent du type de données du paramètre du projet ou du package.  
  
-   Le projet ou le package contient des paramètres qui requièrent des valeurs, mais aucune valeur n'a été affectée.  
  
-   Les variables d’environnement référencées ne peuvent pas être trouvées dans l’environnement spécifié par la référence environnementale, *reference_id*.  
  
## <a name="see-also"></a> Voir aussi  
 [catalog.start_execution &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md)   
 [catalog.set_execution_parameter_value &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
 [catalog.add_execution_worker &#40;base de données SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-add-execution-worker-ssisdb-database.md)  
  
