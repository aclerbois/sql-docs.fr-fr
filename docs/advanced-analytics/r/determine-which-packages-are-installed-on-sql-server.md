---
title: Obtenir des informations de package de R et Python sur SQL Server Machine Learning | Documents Microsoft
description: Déterminer la version du package R et Python, vérifier l’installation et obtenir la liste des packages installés sur SQL Server R Services ou des Services de Machine Learning.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/29/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 85ea4658ca8b60fc24d7e4f7849de1655eab6082
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707887"
---
#  <a name="get-r-and-python-package-information"></a>Obtenir des informations sur le package R et Python
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Parfois, lorsque vous travaillez avec plusieurs environnements ou les installations de R ou Python, vous devez vérifier que le code que vous utilisez est à l’aide de l’environnement attendu pour Python ou l’espace de travail correct pour R. Par exemple, si vous avez mis à niveau les composants par le biais d’apprentissage automatique [liaison](use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md), le chemin d’accès à la bibliothèque R peut être dans un dossier autre que celui de la valeur par défaut. En outre, si vous installez R ou une instance du serveur autonome, vous pouvez avoir plusieurs bibliothèques R sur votre ordinateur.

Les exemples de scripts R et Python dans cet article vous montrent comment obtenir le chemin d’accès et la version des packages utilisé par SQL Server.

## <a name="get-the-r-library-location"></a>Obtenir l’emplacement de la bibliothèque R

Pour n’importe quelle version de SQL Server, exécutez l’instruction suivante pour vérifier la [bibliothèque de package par défaut R](installing-and-managing-r-packages.md) pour l’instance actuelle :

```sql
EXECUTE sp_execute_external_script  
  @language = N'R',
  @script = N'OutputDataSet <- data.frame(.libPaths());'
WITH RESULT SETS (([DefaultLibraryName] VARCHAR(MAX) NOT NULL));
GO
```

Si vous le souhaitez, vous pouvez utiliser [rxSqlLibPaths](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqllibpaths) dans les versions plus récentes de RevoScaleR dans SQL Server 2017 Machine Learning Services ou [R Services mis R au moins RevoScaleR 9.0.1](use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md). Cette procédure stockée retourne le chemin d’accès de la bibliothèque de l’instance et la version de RevoScaleR utilisé par SQL Server :

```sql
EXECUTE sp_execute_external_script
  @language =N'R',
  @script=N'
  sql_r_path <- rxSqlLibPaths("local")
  print(sql_r_path)
  version_info <-packageVersion("RevoScaleR")
  print(version_info)'
```

> [!NOTE]
> Le [rxSqlLibPaths](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqllibpaths) fonction peut être exécutée uniquement sur l’ordinateur local. La fonction ne peut pas retourner des chemins d’accès de bibliothèque pour les connexions distantes.

**Résultats**

```text
STDOUT message(s) from external script: 
[1] "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER1000/R_SERVICES/library"
[1] '9.3.0'
```

## <a name="get-the-python-library-location"></a>Obtenir l’emplacement de la bibliothèque de Python

Pour **Python** dans SQL Server 2017, exécutez l’instruction suivante pour vérifier que la bibliothèque par défaut pour l’instance actuelle. Cet exemple retourne la liste des dossiers inclus dans les Python `sys.path` variable. La liste inclut le répertoire actif et le chemin d’accès de la bibliothèque standard.

```sql
EXECUTE sp_execute_external_script
  @language =N'Python',
  @script=N'import sys; print("\n".join(sys.path))'
```

**Résultats**

```text
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\python35.zip
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\DLLs
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\Sphinx-1.5.4-py3.5.egg
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\win32
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\win32\lib
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\Pythonwin
C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\setuptools-27.2.0-py3.5.egg
```

Pour plus d’informations sur la variable `sys.path` et la façon dont il est utilisé pour définir le chemin de recherche de l’interpréteur de modules, consultez la [documentation de Python](https://docs.python.org/2/tutorial/modules.html#the-module-search-path)

## <a name="list-all-packages"></a>Liste de tous les packages

Il existe plusieurs méthodes que vous pouvez obtenir une liste complète des packages actuellement installés. L’un des avantages de l’exécution des commandes de liste de package à partir de sp_execute_external_script sont que vous avez la garantie pour obtenir les packages installés dans la bibliothèque de l’instance.

### <a name="r"></a>R

L’exemple suivant utilise la fonction R `installed.packages()` dans un [!INCLUDE [tsql](..\..\includes\tsql-md.md)] procédure stockée pour obtenir une matrice des packages qui ont été installés dans la bibliothèque R_SERVICES pour l’instance actuelle. Ce script retourne les champs nom et la version de package dans le fichier de DESCRIPTION, seul le nom est retourné.

```SQL
EXECUTE sp_execute_external_script
  @language=N'R',
  @script = N'str(OutputDataSet);
  packagematrix <- installed.packages();
  Name <- packagematrix[,1];
  Version <- packagematrix[,3];
  OutputDataSet <- data.frame(Name, Version);',
  @input_data_1 = N''
WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
```

Pour plus d’informations sur le paramètre facultatif et des champs par défaut pour le champ de DESCRIPTION du package R, consultez [ https://cran.r-project.org ](https://cran.r-project.org/doc/manuals/R-exts.html#The-DESCRIPTION-file).

### <a name="python"></a>Python

Le `pip` module est installé par défaut et prend en charge de nombreuses opérations pour les packages de liste installé, en plus de ceux pris en charge par Python standard. Vous pouvez exécuter `pip` à partir d’une Python invite de commandes, bien entendu, mais vous pouvez également appeler certaines fonctions pip de `sp_execute_external_script`.

```sql
EXECUTE sp_execute_external_script 
  @language = N'Python', 
  @script = N'
  import pip
  import pandas as pd
  installed_packages = pip.get_installed_distributions()
  installed_packages_list = sorted(["%s==%s" % (i.key, i.version)
     for i in installed_packages])
  df = pd.DataFrame(installed_packages_list)
  OutputDataSet = df'
WITH RESULT SETS (( PackageVersion nvarchar (150) ))
```

Lors de l’exécution `pip` à partir de la ligne de commande, il existe de nombreuses autres fonctions utiles : `pip list` Obtient tous les packages qui sont installés, tandis que `pip freeze` répertorie les packages installés par `pip`et ne répertorie pas les packages qui lui-même pip dépend. Vous pouvez également utiliser `pip freeze` pour générer un fichier de dépendance.

## <a name="find-a-single-package"></a>Rechercher un package unique

Si vous avez installé un package et que vous souhaitez vous assurer qu’il est disponible pour une instance particulière de SQL Server, vous pouvez exécuter l’appel de procédure stockée suivante pour charger le package et de retourner uniquement les messages.

### <a name="r"></a>R

Cet exemple recherche et charge la bibliothèque RevoScaleR, s’il est disponible.

```sql
EXECUTE sp_execute_external_script  
  @language =N'R',
  @script=N'require("RevoScaleR")'
GO
```

+ Si le package est trouvé, un message est retourné : « Commandes exécutée avec succès. »

+ Si le package ne peut pas être situé ou chargé, vous obtenez une erreur qui contient le texte : « il n’existe aucun package appelé 'MissingPackageName' »

### <a name="python"></a>Python

La vérification équivalente pour Python peut être effectuée à partir de Python shell, à l’aide de `conda` ou `pip` commandes. Vous pouvez également exécuter cette instruction dans une procédure stockée :

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
  import pip
  import pkg_resources
  pckg_name = "revoscalepy"
  pckgs = pandas.DataFrame([(i.key) for i in pip.get_installed_distributions()], columns = ["key"])
  installed_pckg = pckgs.query(''key == @pckg_name'')
  print("Package", pckg_name, "is", "not" if installed_pckg.empty else "", "installed")'
```

## <a name="get-package-information-in-r-and-python-tools"></a>Obtenir des informations sur le package dans les outils R et Python

Toutes les instructions précédentes supposent que vous utilisez un outil SQL Server tels que Management Studio (SSMS). Si vous préférez à l’aide des outils R et Python, les instructions suivantes expliquent comment obtenir des informations de bibliothèque et de package à partir d’une ligne de commande R ou Python.

### <a name="r-commands"></a>Commandes R

La bibliothèque de l’instance pour R est \Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\R_SERVICES\library.

Lancer les outils standards de R à partir de ces emplacements pour assurer l’utilisation des packages à partir de la bibliothèque de l’instance :

+ \MSSQL14. MSSQLSERVER\R_SERVICES\bin\R.exe pour une invite de commandes R
+ \MSSQL14. MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe pour une application de console R

Vous pouvez utiliser les commandes standard de R ou les commandes de RevoScaleR pour obtenir des informations de package. Le package RevoScaleR est chargé automatiquement. 

Retourner des informations sur le package RevoScaleR :

    > print(Revo.version)

Retourne une liste de tous les packages installés :

    > installed.packages()

Retourner la version de r :

    > packageDescription("base")

### <a name="python-commands"></a>Commandes de Python

Prise en charge de Python est SQL Server 2017 uniquement. La bibliothèque de l’instance pour Python est \Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\PYTHON_SERVICES\.

Ouvrez la fenêtre de commande Python à partir de cet emplacement pour assurer l’utilisation des packages à partir de la bibliothèque de l’instance :

+ \MSSQL14. MSSQLSERVER\PYTHON_SERVICES\Python.exe

Ouvrir l’aide interactive :

    > help()

Tapez un nom de module à l’invite d’aide pour obtenir le contenu du package, la version et emplacement du fichier :

    help> revoscalepy

<a name="pip-conda"></a>

### <a name="python-package-managers-pip-and-conda"></a>Python package gestionnaires (Pip et Conda)

Anaconda inclut les outils Python pour gérer les packages. Sur la valeur par défaut instance, Pip, Conda et autres outils sont accessibles à \Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\PYTHON_SERVICES\Scripts.

Le programme d’installation de SQL Server n’ajoute pas de Pip ou Conda au chemin du système et sur une instance de SQL Server de production, en conservant non essentielles des exécutables hors du chemin d’accès est une meilleure pratique. Toutefois, pour les environnements de développement et de test, vous pouvez ajouter le dossier Scripts à la variable d’environnement de chemin d’accès système pour exécuter des processus Pip et Conda sur la commande à partir de n’importe quel emplacement.

1. Accédez à C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\PYTHON_SERVICES\Scripts

1. Avec le bouton droit **conda.exe** > **exécuter en tant qu’administrateur**, puis entrez `conda list` pour retourner une liste de packages installés dans l’environnement actuel.

1. De même, avec le bouton droit **pip.exe** > **exécuter en tant qu’administrateur**, puis entrez `pip list` pour retourner les mêmes informations. 


## <a name="next-steps"></a>Étapes suivantes

+ [Installer de nouveaux packages R](install-additional-r-packages-on-sql-server.md)
+ [Installer de nouveaux packages Python](../python/install-additional-python-packages-on-sql-server.md)
+ [Didacticiels, exemples, solutions](../tutorials/machine-learning-services-tutorials.md)