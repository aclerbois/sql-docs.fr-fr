---
title: Installer de nouveaux packages Python sur SQL Server Machine Learning | Documents Microsoft
description: Ajouter des nouveaux packages Python à SQL Server 2017 Machine Learning Services (de-de base de données) et Machine Learning Server (autonome)
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/10/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: fa1ed2612fb88653a7259af0675b496fac4a6723
ms.sourcegitcommit: df382099ef1562b5f2d1cd506c1170d1db64de41
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="install-new-python-packages-on-sql-server"></a>Installer de nouveaux packages Python sur SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit comment installer de nouveaux packages Python sur une instance de SQL Server 2017 Machine Learning Services. En général, le processus d’installation de nouveaux packages est similaire à celui dans un environnement de Python standard. Toutefois, quelques étapes supplémentaires sont nécessaires si le serveur ne dispose pas d’une connexion internet.

Pour déterminer où les packages sont installés, ou les packages installés, consultez [sur les packages R d’obtenir ou Python](../r/determine-which-packages-are-installed-on-sql-server.md).

## <a name="prerequisites"></a>Configuration requise

+ Vous devez avoir installé SQL Server 2017 Machine Learning Services (de-de base de données) avec l’option de langage Python. Pour obtenir des instructions, consultez [installer SQL Server 2017 Machine Learning Services (de-de base de données)](../install/sql-machine-learning-services-windows-install.md).

+ Pour chaque instance de serveur, vous devez installer une copie distincte du package. Les packages ne peuvent pas être partagés entre les instances.

+ Packages doivent être 3.5 Python conforme et s’exécutent sur Windows. 

+ Évaluez si le package est adapté pour une utilisation dans l’environnement SQL Server. En général, un serveur de base de données prend en charge plusieurs applications et services et ressources sur le système de fichiers peuvent être limité, ainsi que les connexions au serveur. Dans de nombreux cas, un accès à Internet est bloqué entièrement.

    Autres problèmes courants incluent l’utilisation de la mise en réseau de la fonctionnalité est bloquée sur le serveur ou par le pare-feu ou les packages avec des dépendances qui ne peut pas être installés sur un ordinateur Windows. 

    Certains packages Python populaires (par exemple, ballon) effectuer des tâches telles que le développement web qui fonctionnent mieux dans un environnement autonome. Nous vous recommandons d’utiliser Python dans la base de données pour des tâches telles que l’apprentissage automatique, qui nécessitent un traitement intensif des données qui bénéficient d’une intégration étroite avec le moteur de base de données, plutôt que simplement en interrogeant la base de données.

+ Un accès administratif au serveur est requis pour installer des packages.

## <a name="add-a-new-python-package"></a>Ajouter un nouveau package de Python

Pour cet exemple, nous supposons que vous souhaitez installer un nouveau package directement sur l’ordinateur SQL Server.

Le package est installé dans cet exemple est [CNTK](https://docs.microsoft.com/cognitive-toolkit/), une infrastructure pour l’apprentissage de profondeur à partir de Microsoft qui prend en charge la personnalisation, la formation et le partage de différents types de réseaux neuronaux.

> [!TIP]
> Besoin d’aide pour la configuration de vos outils Python ? Consultez les blogs :
> 
> [Mise en route avec les Services Web Python à l’aide du serveur de Machine Learning](https://blogs.msdn.microsoft.com/mlserver/2017/12/13/getting-started-with-python-web-services-using-machine-learning-server/)
> 
> [David Crook : Outils d’analyse Microsoft cognitifs + VS Code](http://dacrook.com/cntk-vs-code-awesome/)

### <a name="step-1-download-the-windows-version-of-the-python-package"></a>Étape 1. Télécharger la version Windows du package Python

+ Si vous installez des packages Python sur un serveur sans accès à internet, vous devez télécharger le fichier WHL vers un autre ordinateur et puis copiez-le sur le serveur.

    Par exemple, sur un ordinateur distinct, vous pouvez télécharger le fichier WHL à partir de ce site [ https://cntk.ai/PythonWheel/CPU-Only ](https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl), puis copiez le fichier `cntk-2.1-cp35-cp35m-win_amd64.whl` dans un dossier local sur l’ordinateur SQL Server.

+ SQL Server 2017 utilise Python 3.5. 

> [!IMPORTANT]
> Assurez-vous que vous obtenez la version Windows du package. Si le fichier se termine par .gz, il n’est probablement pas la bonne version.

Cette page contient des téléchargements pour plusieurs plateformes et plusieurs versions de Python : [configurer CNTK](https://docs.microsoft.com/cognitive-toolkit/Setup-CNTK-on-your-machine)

### <a name="step-2-open-a-python-command-prompt"></a>Étape 2. Ouvrez une invite de commandes de Python

Recherchez l’emplacement de bibliothèque de Python par défaut utilisé par SQL Server. Si vous avez installé plusieurs instances, recherchez le dossier PYTHON_SERVICE pour l’instance où vous souhaitez ajouter le package.

Par exemple, si la Machine Learning Services a été installé à l’aide des valeurs par défaut et apprentissage automatique est activée sur l’instance par défaut, le chemin d’accès serait comme suit :

    `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

Ouvrez l’invite de commande Python associé à l’instance.

> [!TIP]
> Pour futures de débogage et de test, vous souhaiterez configurer un environnement de Python spécifique à la bibliothèque de l’instance.

### <a name="step-3-install-the-package-using-pip"></a>Étape 3. Installez le package à l’aide du pip

+ Si vous êtes habitué à l’aide de la ligne de commande Python, utilisez PIP.exe pour installer les nouveaux packages. Vous pouvez trouver la **pip** programme d’installation dans le `Scripts` sous-dossier. 

  Le programme d’installation de SQL Server n’ajoute pas de Scripts pour le chemin d’accès système. Si vous obtenez une erreur `pip` n’est pas reconnu comme une commande interne ou externe, vous pouvez ajouter le dossier Scripts à la variable de chemin d’accès dans Windows.

  Le chemin d’accès complet de le **Scripts** dossier dans une installation par défaut est la suivante :

    C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\PYTHON_SERVICES\Scripts

+ Si vous utilisez Visual Studio 2017 ou Visual Studio 2015 avec les extensions de Python, vous pouvez exécuter `pip install` à partir de la **environnements Python** fenêtre. Cliquez sur **Packages**et dans la zone de texte, indiquez le nom ou l’emplacement du package à installer. Vous n’avez pas besoin de type `pip install`; il est renseigné pour vous automatiquement. 

    - Si l’ordinateur a accès à Internet, fournissez le nom du package, ou l’URL d’un package spécifique et la version. 
    
    Par exemple, pour installer la version de CNTK est pris en charge pour Windows et Python 3.5, vous devez spécifier l’URL de téléchargement : `https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl`

    - Si l’ordinateur n’a pas accès à internet, vous devez télécharger le fichier WHL avant de commencer l’installation. Ensuite, spécifiez le chemin d’accès local et le nom. Par exemple, collez le chemin d’accès et le fichier pour installer le fichier WHL téléchargé depuis le site suivant : `"C:\Downloads\CNTK\cntk-2.1-cp35-cp35m-win_amd64.whl"`

Vous pouvez être invité à élever les autorisations pour terminer l’installation.

En tant qu’installation suit son cours, vous pouvez voir les messages d’état dans la fenêtre d’invite de commandes :

```python
pip install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
Collecting cntk==2.1 from https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
  Downloading https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl (34.1MB)
    100% |################################| 34.1MB 13kB/s
...
Installing collected packages: cntk
Successfully installed cntk-2.1
```


### <a name="step-4-load-the-package-or-its-functions-as-part-of-your-script"></a>Étape 4. Charger le package ou ses fonctions dans le cadre de votre script.

Lors de l’installation est terminée, vous pouvez commencer immédiatement à l’aide du package comme décrit dans l’étape suivante.

Pour des exemples d’apprentissage approfondie à l’aide de CNTK, consultez ces didacticiels : [API Python pour CNTK](https://cntk.ai/pythondocs/tutorials.html)

Pour utiliser des fonctions à partir du package dans votre script, insérez la norme `import <package_name>` instruction dans la ligne initiale du script :

```python
import numpy as np
import cntk as cntk
cntk._version_
```

## <a name="list-installed-packages-using-conda"></a>Répertorier les packages installés à l’aide de conda

Il existe différentes façons dont vous pouvez obtenir une liste des packages installés. Par exemple, vous pouvez afficher les packages installés dans le **environnements Python** windows de Visual Studio.

Si vous utilisez la ligne de commande Python, vous pouvez utiliser **Pip** ou **conda** Gestionnaire de package, inclus dans l’environnement Anaconda Python ajoutée par le programme d’installation de SQL Server.

En supposant que vous avez ajouté le dossier Scripts à la variable d’environnement PATH, pour exécuter cette commande à partir de l’invite de commandes de l’administrateur pour répertorier les packages dans votre environnement de Python. Dans le cas contraire, consultez [sur les packages R d’obtenir et Python](../r/determine-which-packages-are-installed-on-sql-server.md#pip-conda) pour les pointeurs sur la façon d’exécuter les outils Python dans SQL Server.

```python
conda list
```

Pour plus d’informations sur **conda** et comment vous pouvez l’utiliser pour créer et gérer plusieurs environnements Python, consultez [gestion des environnements avec conda](https://conda.io/docs/user-guide/tasks/manage-environments.html).
