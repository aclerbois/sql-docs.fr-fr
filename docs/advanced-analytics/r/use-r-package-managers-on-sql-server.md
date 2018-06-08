---
title: Installer de nouveaux packages R sur SQL Server Machine Learning Services | Documents Microsoft
description: Ajouter de nouveaux packages R pour SQL Server 2016 R Services ou SQL Server 2017 Machine Learning Services (de-de base de données)
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/29/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 8f7b0ee2b623f6a208a92823948bf0295bae33f6
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707497"
---
# <a name="use-r-package-managers-to-install-r-packages-on-sql-server"></a>Gestionnaires de packages R permet d’installer des packages R sur SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Vous pouvez utiliser les outils R standard pour installer de nouveaux packages sur une instance de SQL Server 2016 ou SQL Server 2017, en fournissant l’ordinateur a un port ouvert 80 et dispose de droits d’administrateur.

> [!IMPORTANT] 
> Veillez à installer des packages à la bibliothèque par défaut qui est associée à l’instance actuelle. Ne jamais installer de packages vers un répertoire de l’utilisateur.

Cette procédure utilise RGui, mais vous pouvez utiliser RTerm ou tout autre R de ligne de commande outil qui prend en charge l’accès avec élévation de privilèges.

## <a name="install-a-package-using-rgui"></a>Installer un package à l’aide de RGui

1. [Déterminer l’emplacement de la bibliothèque de l’instance](installing-and-managing-r-packages.md). Accédez au dossier où sont installés les outils R. Par exemple, le chemin d’accès par défaut pour une instance par défaut de SQL Server 2017 est comme suit : `C:\Program Files\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`

1. RGui.exe d’avec le bouton droit, puis sélectionnez **exécuter en tant qu’administrateur**. Si vous n’avez pas les autorisations requises, contactez l’administrateur de base de données et fournir une liste des packages que vous avez besoin.

1. À partir de la ligne de commande, si vous connaissez le nom du package, vous pouvez taper : `install.packages("the_package-name")` des guillemets doubles sont requises pour le nom du package.

1. Lorsque vous êtes invité pour un site miroir, sélectionnez n’importe quel site qui est adapté à votre emplacement.

Si le package cible dépend de packages supplémentaires, le programme d’installation de R télécharge les dépendances et les installe pour vous automatiquement.

Si vous avez plusieurs instances de SQL Server, tels que les instances côte à côte de SQL Server 2016 R Services et SQL Server 2017 Machine Learning Services, exécutez installation séparément pour chaque instance si vous souhaitez utiliser le package dans les deux contextes. Les packages ne peuvent pas être partagés entre les instances.

## <a name = "bkmk_offlineInstall"></a> Installation hors connexion à l’aide des outils R

Si le serveur n’a pas accès à internet, des étapes supplémentaires sont requises pour préparer les packages. Pour installer des packages R sur un serveur qui n’a pas accès à internet, vous devez :

+ Analyser les dépendances à l’avance.
+ Téléchargez le package cible sur un ordinateur connecté à Internet.
+ Télécharger tous les packages requis sur le même ordinateur et placer tous les packages dans une archive de package unique.
+ Code postal de l’archive si elle n’est pas déjà dans un format compressé.
+ Copiez l’archive de package vers un emplacement sur le serveur.
+ Installez le package cible en spécifiant le fichier d’archive en tant que source.

> [!IMPORTANT] 
>  Assurez-vous que vous analysez toutes les dépendances et télécharger **tous les** requis packages **avant** début de l’installation. Nous vous recommandons de [miniCRAN](https://mran.microsoft.com/package/miniCRAN) pour ce processus. Ce package de R utilise une liste des packages que vous souhaitez installer, analyse des dépendances et obtient tous les fichiers pour vous. miniCRAN crée ensuite un référentiel unique que vous pouvez copier sur l’ordinateur serveur. Pour plus d’informations, consultez [créer un référentiel de package local à l’aide de miniCRAN](create-a-local-package-repository-using-minicran.md)

Cette procédure suppose que vous avez préparé tous les packages que vous avez besoin, dans un format compressé et que vous êtes prêt à les copier sur le serveur.

1. Copie le package compressé de fichiers, ou pour plusieurs packages, le référentiel complet qui contient tous les packages compressés format, à un emplacement accessible au serveur.

2. Ouvrez le dossier sur le serveur où sont installés les outils R pour l’instance. Par exemple, si vous utilisez l’invite de commandes Windows sur un système avec SQL Server 2016 R Services, basculez vers `C:\Program Files\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`.

3. Avec le bouton droit sur RGui ou RTerm et sélectionnez **exécuter en tant qu’administrateur**.

4. Exécutez la commande R `install.packages` et spécifiez le package ou le nom du référentiel et l’emplacement des fichiers compressés.

    ```R
    install.packages("C:\\Temp\\Downloaded packages\\mynewpackage.zip", repos=NULL)
    ```

    Cette commande extrait le package R `mynewpackage` à partir de son fichier zip local, si vous avez enregistré la copie dans le répertoire `C:\Temp\Downloaded packages`, puis installe le package sur l’ordinateur local. Si le package possède des dépendances, le programme d’installation vérifie pour les packages existants dans la bibliothèque. Si vous avez créé un référentiel qui inclut les dépendances, le programme d’installation installe les packages requis ainsi.

    Si tous les packages requis ne sont pas présents dans la bibliothèque de l’instance et ne peut pas être trouvés dans les fichiers, l’installation du package cible échoue.

## <a name="see-also"></a>Voir aussi

+ [Installer de nouveaux packages R](install-additional-r-packages-on-sql-server.md)
+ [Installer de nouveaux packages Python](../python/install-additional-python-packages-on-sql-server.md)
+ [Didacticiels, exemples, solutions](../tutorials/machine-learning-services-tutorials.md)