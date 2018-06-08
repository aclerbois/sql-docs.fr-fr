---
title: Définir et utiliser les contextes de calcul (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: cb559b0acfc77ddb7e48cc0b3cacf76d421481ac
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="define-and-use-compute-contexts-sql-and-r-deep-dive"></a>Définir et utiliser les contextes de calcul (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Cette leçon présente la [RxInSqlServer](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxinsqlserver) (fonction), qui vous permet de définir un contexte de calcul pour SQL Server, puis exécutez des calculs complexes sur le serveur, plutôt que sur votre ordinateur local. 

RevoScaleR prend en charge plusieurs contextes de calcul, afin que vous pouvez exécuter le code R dans Hadoop ou Spark dans base de données. Pour SQL Server, vous définissez le serveur, et la fonction gère les tâches de création des objets de connexion et en passant entre l’ordinateur local et le contexte d’exécution à distance de la base de données.

La fonction qui crée le serveur SQL Server de calcul contexte utilise les informations suivantes :

- Chaîne de connexion pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance
- Spécification du mode de traitement de sortie
- Arguments facultatifs qui vous activez le traçage ou de spécifient le niveau de trace
- Spécification facultative d’un répertoire de données partagées

## <a name="create-and-set-a-compute-context"></a>Créer et définir un contexte de calcul

1. Spécifiez la chaîne de connexion pour l’instance où les calculs sont effectués.  Vous pouvez réutiliser la chaîne de connexion que vous avez créé précédemment. Vous pouvez créer une chaîne de connexion différente si vous souhaitez déplacer les calculs sur un autre serveur, ou utiliser une connexion différente pour effectuer des tâches.

    **Utilisation d’une connexion SQL**

      ```R
      sqlConnString <- "Driver=SQL Server;Server=<SQL Server instance name>; Database=<database name>;Uid=<SQL user name>;Pwd=<password>"
      ```

    **Avec l’authentification Windows**

      ```R
      sqlConnString <- "Driver=SQL Server;Server=instance_name;Database=DeepDive;Trusted_Connection=True"
      ```
2. Spécifiez la façon dont vous voulez gérer la sortie. Dans le code suivant, vous indiquez que la session R sur la station de travail doit toujours attendre les résultats de la tâche R, mais pas retourner la sortie de console des calculs distants.
  
    ```R
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    ```
  
    L’argument *wait* pour **RxInSqlServer** prend en charge ces options :
  
    -   **TRUE**. Le travail est configuré en tant que le blocage et ne retourne pas jusqu'à ce qu’elle s’est terminée ou a échoué.  Pour plus d’informations, consultez [distribué et le calcul parallèle dans Machine Learning Server](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-distributed-computing).
  
    -   **FALSE**. Les travaux sont configurées comme non bloquant et retournent immédiatement, ce qui vous permet de continuer à exécuter un autre code R. Toutefois, même en mode non bloquant, la connexion client à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être maintenue pendant l’exécution de la tâche.

3. Si vous le souhaitez, vous pouvez spécifier l’emplacement d’un répertoire local pour une utilisation partagée par la session R locale et distant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordinateur et ses comptes.

    ```R
    sqlShareDir <- paste("c:\\AllShare\\", Sys.getenv("USERNAME"), sep="")
    ```
    
4. Si vous souhaitez créer manuellement un répertoire spécifique pour le partage, vous pouvez ajouter une ligne comme suit :

    ```
    dir.create(sqlShareDir, recursive = TRUE)
    ```

    Pour déterminer le dossier utilisé pour le partage, exécutez `rxGetComputeContext()`, qui retourne le contexte de calcul plus d’informations sur en cours. Pour plus d’informations, consultez la [documentation de référence ScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/).

4. Avoir préparé toutes les variables, de les fournir en tant qu’arguments à la **RxInSqlServer** constructeur, pour créer le *objet de contexte de calcul*.

    ```R
    sqlCompute <- RxInSqlServer(  
         connectionString = sqlConnString,
         wait = sqlWait,
         consoleOutput = sqlConsoleOutput)
    ```
    
    La syntaxe de **RxInSqlServer** est quasiment identique à celui de la **RxSqlServerData** fonction que vous avez utilisé précédemment pour définir la source de données. Toutefois, il existe des différences importantes.
      
    - L’objet source de données (défini à l’aide de la fonction [RxSqlServerData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqlserverdata)) spécifie où sont stockées les données.
    
    - En revanche, le contexte de calcul, défini à l’aide de la fonction [RxInSqlServer](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxinsqlserver) indique où les agrégations et autres calculs doivent avoir lieu.
    
    La définition d’un contexte de calcul n’affecte pas les autres calculs R génériques que vous pouvez effectuer sur votre station de travail, et ne modifie pas la source des données. Vous pouvez par exemple, définir un fichier texte local comme source de données, mais modifier le contexte de calcul de SQL Server et effectuer toutes vos lectures et résumés des données sur l’ordinateur SQL Server.

## <a name="enable-tracing-on-the-compute-context"></a>Activer le suivi dans le contexte de calcul

Il arrive parfois que les opérations fonctionnent sur votre contexte local, mais qu’elles rencontrent des problèmes quand elles sont exécutées dans un contexte de calcul distant. Si vous souhaitez analyser les problèmes ou suivre les performances, vous pouvez activer le suivi dans le contexte de calcul, pour que la résolution des problèmes d’exécution soit prise en charge.

1. Créer un nouveau contexte de calcul qui utilise la même chaîne de connexion, mais ajouter les arguments *traceEnabled* et *traceLevel* à la **RxInSqlServer** constructeur.

    ```R
    sqlComputeTrace <- RxInSqlServer(
        connectionString = sqlConnString,
        #shareDir = sqlShareDir,
        wait = sqlWait,
        consoleOutput = sqlConsoleOutput,
        traceEnabled = TRUE,
        traceLevel = 7)
    ```
  
    Dans cet exemple, la propriété *traceLevel* a la valeur 7, ce qui signifie « afficher toutes les informations de traçage ».

2. Pour changer de contexte de calcul, utilisez la fonction [rxSetComputeContext](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) et spécifiez le contexte par son nom.

    ```R
    rxSetComputeContext( sqlComputeTrace)
    ```

    > [!NOTE]
    > 
    > Pour ce didacticiel, utilisez le contexte de calcul qui n’a pas le traçage activé. 
    > 
    > Toutefois, si vous décidez d’utiliser le suivi, sachez que votre expérience peut-être être affectée par une connectivité réseau. Sachez également que, car les performances de l’option de suivi n’a pas été testé pour toutes les opérations.

Dans l’étape suivante, que vous apprenez à utiliser les contextes, localement ou à exécuter le code R sur le serveur de calcul.

## <a name="next-step"></a>Étape suivante

[Créer et exécuter des scripts R](../../advanced-analytics/tutorials/deepdive-create-and-run-r-scripts.md)

## <a name="previous-step"></a>Étape précédente

[Interroger et modifier les données SQL Server](../../advanced-analytics/tutorials/deepdive-query-and-modify-the-sql-server-data.md)
