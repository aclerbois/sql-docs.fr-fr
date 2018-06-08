---
title: Exécuter des packages dans SQL Server Integration Services (SSIS) Scale Out | Microsoft Docs
ms.description: This article describes how to run SSIS packages in Scale Out
ms.custom: ''
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: scale-out
ms.reviewer: douglasl
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
caps.latest.revision: 1
author: haoqian
ms.author: haoqian
manager: craigg
f1_keywords:
- sql13.ssis.ssms.ispackageexecuteinscaleout.f1
ms.openlocfilehash: 3566938f25a646206e203810a84451829b0206cb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="run-packages-in-integration-services-ssis-scale-out"></a>Exécuter des packages dans Integration Services (SSIS) Scale Out
Après avoir déployé les packages sur le serveur Integration Services, vous pouvez les exécuter dans Scale Out en utilisant l’une des méthodes suivantes :

-   [Boîte de dialogue Exécuter le package dans Scale Out](#scale_out_dialog)

-   [Procédures stockées](#stored_proc)

-   [travaux de l'Agent SQL Server](#sql_agent)

## <a name="scale_out_dialog"></a> Exécuter des packages avec la boîte de dialogue Exécuter le package dans Scale Out

1. Ouvrez la boîte de dialogue Exécuter le package dans Scale Out.

    Dans [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur Integration Services. Dans l’Explorateur d’objets, développez l’arborescence pour afficher les nœuds sous **Catalogues Integration Services**. Cliquez avec le bouton droit sur le nœud **SSISDB** ou sur le projet ou le package que vous souhaitez exécuter, puis cliquez sur **Exécuter dans Scale Out**.

2. Sélectionnez les packages et définissez les options.

    Dans la page **Sélection des packages**, sélectionnez un ou plusieurs packages à exécuter. Définissez l’environnement, les paramètres, les gestionnaires de connexions et les options avancées pour chaque package. Cliquez sur un package pour définir ces options.
    
    Sous l’onglet **Avancé**, définissez une option Scale Out appelée **Nombre de nouvelles tentatives** pour indiquer le nombre de nouvelles tentatives d’exécution du package en cas d’échec.

    > [!NOTE]
    > L’option **Vider en cas d’erreurs** ne fonctionne que si le compte exécutant le service Scale Out Worker est administrateur sur l’ordinateur local.

3. Sélectionnez les ordinateurs Worker.

    Dans la page **Sélection des ordinateurs**, sélectionnez les ordinateurs Scale Out Worker pour exécuter les packages. Par défaut, tout ordinateur est autorisé à exécuter les packages. 

   > [!NOTE] 
   > Les packages sont exécutés avec les informations d’identification des comptes d’utilisateur des services Scale Out Worker. Passez en revue ces informations d’identification dans la page **Sélection des ordinateurs**. Par défaut, le compte est `NT Service\SSISScaleOutWorker140`.

   > [!WARNING]
   > Les exécutions de packages déclenchées par différents utilisateurs sur le même ordinateur Worker sont effectuées avec les mêmes informations d’identification. Il n’existe aucune frontière de sécurité entre elles. 

4. Exécutez les packages et affichez des rapports.

    Cliquez sur **OK** pour démarrer les exécutions de package. Pour afficher le rapport d’exécution d’un package, cliquez sur le package dans l’Explorateur d’objets, cliquez sur **Rapports**, sur **Toutes les exécutions**, et recherchez l’exécution.
    
## <a name="stored_proc"></a> Exécuter des packages avec des procédures stockées

1.  Créez des exécutions.

    Appelez `[catalog].[create_execution]` pour chaque package. Affectez la valeur `True` au paramètre **@runinscaleout**. Si les ordinateurs Scale Out Worker ne sont pas tous autorisés à exécuter le package, affectez la valeur `False` au paramètre **@useanyworker**. Pour plus d’informations sur cette procédure stockée et sur le paramètre **@useanyworker**, consultez [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md). 

2. Définissez les paramètres d’exécution.

    Appelez `[catalog].[set_execution_parameter_value]` pour chaque exécution.

3. Définissez les Scale Out Workers.

    Appelez `[catalog].[add_execution_worker]`. Si tous les ordinateurs sont autorisés à exécuter le package, il est inutile d’appeler cette procédure stockée. 

4. Démarrez les exécutions.

    Appelez `[catalog].[start_execution]`. Définissez le paramètre **@retry_count** pour indiquer le nombre de nouvelles tentatives d’exécution du package en cas d’échec.
    
### <a name="example"></a> Exemple
L’exemple suivant exécute deux packages, `package1.dtsx` et `package2.dtsx`, dans Scale Out avec un Scale Out Worker.  

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder1', @project_name=N'project1', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO

Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package2.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder2', @project_name=N'project2', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO
```

### <a name="permissions"></a>Autorisations
Pour exécuter des packages dans Scale Out, vous devez disposer de l’une des autorisations suivantes :

-   L’appartenance au rôle de base de données **ssis_admin**  

-   L’appartenance au rôle de base de données **ssis_cluster_executor**  
  
-   L’appartenance au rôle serveur **sysadmin**  

## <a name="set-default-execution-mode"></a>Définir le mode d’exécution par défaut
Pour définir le mode d’exécution par défaut pour les packages sur **Scale Out**, effectuez les opérations suivantes :

1.  Dans l’Explorateur d’objets de SSMS, cliquez avec le bouton droit sur le nœud **SSISDB**, puis sélectionnez **Propriétés**.

2.  Dans la boîte de dialogue **Propriétés du catalogue**, définissez **Mode d’exécution par défaut à l’échelle du serveur** sur **Scale Out**.

Une fois ce mode d’exécution par défaut défini, vous n’avez plus besoin de spécifier le paramètre **@runinscaleout** quand vous appelez la procédure stockée `[catalog].[create_execution]`. Les packages sont exécutés automatiquement dans Scale Out. 

![Mode d’exécution](media\exe-mode.PNG)

Pour redéfinir le mode d’exécution par défaut afin que les packages ne soient plus exécutés par défaut en mode Scale Out, définissez **Mode d’exécution par défaut à l’échelle du serveur** sur **Serveur**.

## <a name="sql_agent"></a> Exécuter le package dans le travail de SQL Server Agent
Dans un travail SQL Server Agent, vous pouvez exécuter un package SSIS en tant qu’étape du travail. Pour exécuter le package dans Scale Out, définissez le mode d’exécution par défaut sur **Scale Out**. Une fois le mode d’exécution par défaut défini sur **Scale Out**, les packages dans les travaux SQL Server Agent sont exécutés en mode Scale Out.

## <a name="next-steps"></a>Étapes suivantes
-   [Résoudre les problèmes de Scale Out](troubleshooting-scale-out.md)
