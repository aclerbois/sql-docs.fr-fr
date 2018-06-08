---
title: T-SQL (créer une bibliothèque externe) permet d’installer des packages R sur SQL Server Machine Learning Services | Documents Microsoft
description: Ajouter de nouveaux packages R à SQL Server 2017 Machine Learning Services (de-de base de données)
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/30/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 7a5a0c394e9a26244661a4ae712d20583c1f1c99
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585481"
---
# <a name="use-t-sql-create-external-library-to-install-r-packages-on-sql-server-2017-machine-learning-services"></a>T-SQL (créer une bibliothèque externe) permet d’installer des packages R sur SQL Server 2017 Machine Learning Services
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article explique comment installer de nouveaux packages R sur une instance de SQL Server où l’apprentissage automatique est activé. Il existe plusieurs approches sélectionnables. À l’aide de T-SQL convient le mieux pour les administrateurs de serveur qui ne sont pas familiarisés avec R.

**S’applique à :**  [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]

Le [créer une bibliothèque externe](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) instruction permet d’ajouter un package ou un ensemble de packages à une base de données spécifique ou à une instance sans exécuter R ou directement le code Python. Toutefois, cette méthode requiert la préparation du package et les autorisations de base de données supplémentaires.

+ Tous les packages doivent être disponibles en tant qu’un fichier zip local, plutôt que téléchargé à la demande à partir d’internet.

+ Toutes les dépendances doivent être identifiés par le nom et la version et inclus dans le fichier zip. L’instruction échoue si les packages nécessaires ne sont pas disponibles, notamment les dépendances de package en aval. 

+ Vous devez être **db_owner** ou avoir l’autorisation de créer une bibliothèque externe dans un rôle de base de données. Pour plus d’informations, consultez [créer une bibliothèque externe](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).

## <a name="download-packages-in-archive-format"></a>Télécharger les packages au format d’archive

Si vous installez un package unique, téléchargez le package au format compressé.

Il est plus courant d’installer plusieurs packages en raison de dépendances de package. Lorsqu’un package requiert d’autres packages, vous devez vérifier que tous les sont accessibles à l’autre lors de l’installation. Nous vous recommandons de [création d’un référentiel local](create-a-local-package-repository-using-minicran.md) à l’aide de [miniCRAN](http://andrie.github.io/miniCRAN/) pour assembler une collection complète des packages, ainsi que [igraph](http://igraph.org/r/) pour l’analyse des dépendances de packages. L’installation d’une version incorrecte d’un package ou l’omission d’une dépendance de package peut entraîner une Échec de l’instruction créer une bibliothèque externe. 

## <a name="copy-the-file-to-a-local-folder"></a>Copiez le fichier dans un dossier local

Copiez le fichier compressé qui contient tous les packages vers un dossier local sur le serveur. Si vous n’avez pas de l’accès au système de fichiers sur le serveur, vous pouvez également passer un package complet en tant que variable, à l’aide d’un format binaire. Pour plus d’informations, consultez [créer une bibliothèque externe](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="run-the-statement-to-upload-packages"></a>Exécutez l’instruction pour télécharger des packages

Ouvrir un **requête** fenêtre, à l’aide d’un compte avec des privilèges d’administrateur.

Exécutez l’instruction T-SQL `CREATE EXTERNAL LIBRARY` pour télécharger de la collection de package compressé pour la base de données.

Par exemple, l’instruction suivante noms comme source du package d’un référentiel de miniCRAN contenant le **randomForest** package, ainsi que de ses dépendances. 

```SQL
CREATE EXTERNAL LIBRARY randomForest
FROM (CONTENT = 'C:\Temp\Rpackages\randomForest_4.6-12.zip')
WITH (LANGUAGE = 'R');
```

Vous ne pouvez pas utiliser un nom arbitraire ; le nom de bibliothèque externe doit avoir le même nom que vous utiliserez lors du chargement ou de l’appel.

## <a name="verify-package-installation"></a>Vérifier l’installation du package

Si la bibliothèque est créée avec succès, vous pouvez exécuter le package dans SQL Server, en l’appelant à l’intérieur d’une procédure stockée.
    
```SQL
EXEC sp_execute_external_script
@language =N'R',
@script=N'library(randomForest)'
```

## <a name="see-also"></a>Voir aussi

+ [Obtenir les informations sur le package](determine-which-packages-are-installed-on-sql-server.md)
+ [Didacticiels de R](../tutorials/sql-server-r-tutorials.md)
+ [Guides de procédures](sql-server-machine-learning-tasks.md)