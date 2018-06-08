---
title: Bibliothèques de package de R et Python dans SQL Server R et l’apprentissage de configuration de SQL Server par défaut | Documents Microsoft
description: Packages R et Python installés par SQL Server pour R Services, R Server, Machine Learning Services (de-de base de données) et la Machine Learning Server (autonome)
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/29/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 9362d6e9dc98f80beabc301f43e755b205b40a10
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707287"
---
# <a name="default-r-and-python-packages-in-sql-server"></a>Par défaut R et Python packages dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article répertorie les packages R et Python installés avec SQL Server et où trouver la bibliothèque de package.  

## <a name="r-package-list-for-sql-server"></a>Liste des packages R pour SQL Server

Packages R sont installés avec [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md) et [SQL Server 2017 Machine Learning Services](../install/sql-machine-learning-services-windows-install.md) lorsque vous sélectionnez la fonctionnalité R pendant l’installation. 

.         | 2016 | 2017 | Description |
|----------------|--------------|--------------|-------------|
| [RevoScaleR](https://docs.microsoft.com/r-server/r-reference/revoscaler/revoscaler)  | 8.0.3 | 9.2 | Utilisé pour les contextes de calcul à distance, de diffusion en continu, l’exécution parallèle des fonctions rx pour l’importation de données et de transformation, de modélisation, de visualisation et d’analyse. |
| [sqlrutils](https://docs.microsoft.com/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 8.0.3 | 9.2 |Utilisé pour inclure un script R dans les procédures stockées. |
| [MicrosoftML](https://docs.microsoft.com/r-server/r-reference/microsoftml/microsoftml-package)| néant | 9.2 | Ajoute des algorithmes d’apprentissage automatique dans R. | 
| [olapR](https://docs.microsoft.com/machine-learning-server/r-reference/olapr/olapr) | néant  | 9.2 | Utilisé pour écrire des instructions MDX dans R. |

MicrosoftML et olapR sont disponibles par défaut dans SQL Server 2017 Machine Learning Services. Sur une instance de SQL Server 2016 R Services, vous pouvez ajouter ces packages via un [mise à niveau du composant](use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md). Une mise à niveau du composant obtient également les versions plus récentes des packages (par exemple, les versions plus récentes de RevoScaleR incluent des fonctions pour la gestion des packages sur SQL Server).

## <a name="python-package-list-for-sql-server"></a>Liste des packages Python pour SQL Server

Packages Python sont uniquement disponibles dans SQL Server 2017 lorsque vous installez [SQL Server 2017 Machine Learning Services](../install/sql-machine-learning-services-windows-install.md) et sélectionnez la fonctionnalité de Python.

| .         | 2017    |  Description |
| -----------------|-------------|------------|
| [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.2 | Utilisé pour les contextes de calcul à distance, de diffusion en continu, l’exécution parallèle des fonctions rx pour l’importation de données et de transformation, de modélisation, de visualisation et d’analyse. |
| [microsoftml](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.2 | Ajoute des algorithmes d’apprentissage automatique dans Python. |

## <a name="open-source-r-in-your-installation"></a>R Open source dans votre installation

Prise en charge R inclut open source sont afin que vous pouvez appeler des fonctions R base et installer des packages supplémentaires tierces et open source. Prise en charge du langage R inclut les fonctionnalités principales comme **base**, **statistiques**, **utils**et d’autres. Une installation de base de R inclut également de nombreux exemples de datasets et les outils R standards tels que **RGui** (un éditeur interactif léger) et **RTerm** (une invite de commandes R). 

La distribution de R open source inclus dans votre installation est [Microsoft R ouvrir Diffusée](https://mran.microsoft.com/open). MRO ajoute la valeur de base R en incluant les autres packages open source tels que le [Intel Math Kernel Library](https://en.wikipedia.org/wiki/Math_Kernel_Library).

Le tableau suivant récapitule les versions de R fournis par MRO à l’aide du programme d’installation de SQL Server.

|Version             | Version de R       |
|--------------------|-----------------|
| [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md) | 3.2.2   | 
| [SQL Server 2017 apprentissage Services](../install/sql-machine-learning-services-windows-install.md) | 3.3.3 |

Vous ne devez jamais manuellement remplacer la version de R installé par le programme d’installation de SQL Server avec des versions plus récentes sur le web. Les packages Microsoft R sont basées sur des versions spécifiques de la modification de R. votre installation pourrait déstabiliser il.

## <a name="open-source-python-in-your-installation"></a>Python Open source dans votre installation

SQL Server 2017 ajoute des composants de Python. Lorsque vous sélectionnez l’option de langage Python, Anaconda 4.2 distribution est installée. En plus des bibliothèques de code Python, l’installation standard inclut des exemples de données, les tests unitaires et les exemples de scripts. 

SQL Server 2017 Machine Learning est la première version de R et Python prennent en charge.

|Version             | Version de anaconda| Packages de Microsoft    |
|--------------------|-----------------|-----------------------|
| SQL Server 2017 Machine Learning Services  | 4.2 sur Python 3.5 | revoscalepy, microsoftml |

Vous ne devez jamais manuellement remplacer la version de Python installé par le programme d’installation de SQL Server avec des versions plus récentes sur le web. Les packages Microsoft Python sont basées sur des versions spécifiques de Anaconda. Modification de votre installation, il peut déstabiliser.

## <a name="component-upgrades"></a>Mises à niveau du composant

Après l’installation initiale, les packages R et Python sont actualisés via les service packs et mises à jour cumulatives, mais les mises à niveau de la version complète sont uniquement possibles par *liaison* à la stratégie de prise en charge du cycle de vie moderne. Liaison modifie le modèle de maintenance. Par défaut, après une installation initiale, les packages R sont actualisés via les service packs et mises à jour cumulatives. Autres packages et des mises à niveau de la version complète des composants R sont possibles seulement via les mises à niveau de produit (à partir de SQL Server 2016 à SQL Server 2017) ou par R de liaison prend en charge Microsoft Machine Learning Server. Pour plus d’informations, consultez [des composants R de mise à niveau et Python dans SQL Server](use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md).

## <a name="package-library-location"></a>Emplacement de package de bibliothèque

Lorsque vous installez un apprentissage avec SQL Server, une bibliothèque de package unique est créée au niveau de l’instance pour chaque langue que vous installez. Sous Windows, la bibliothèque de l’instance est un dossier sécurisé est enregistré avec SQL Server.

Tout script ou un code que s’exécute dans-base de données sur SQL Server doit charger les fonctions de la bibliothèque de l’instance. SQL Server ne peut pas accéder aux packages installés dans d’autres bibliothèques. Cela vaut pour les clients distants. Lors de la connexion au serveur à partir d’un client distant, tout code R ou Python que vous souhaitez exécuter dans le contexte de calcul de serveur permet uniquement les packages installés dans la bibliothèque de l’instance.

Pour protéger des ressources du serveur, la bibliothèque d’instance par défaut peut être modifiée uniquement par un administrateur de l’ordinateur. Si vous n’êtes pas le propriétaire de l’ordinateur, vous devrez peut-être obtenir l’autorisation d’un administrateur pour installer des packages pour cette bibliothèque. 

#### <a name="file-path-for-in-database-engine-instances"></a>Chemin d’accès pour les instances du moteur de la base de données

Le tableau suivant indique l’emplacement du fichier de base de données et la version de R et Python combinaisons d’instance du moteur. MSSQL13 indique SQL Server 2016 et R uniquement. MSSQL14 indique SQL Server 2017 et a des dossiers R et Python. 

Chemins de fichier incluent également les noms d’instance. SQL Server installe [instances du moteur de base de données](../../database-engine/configure-windows/database-engine-instances-sql-server.md) comme l’instance par défaut (MSSQLSERVER) ou comme une instance nommée définie par l’utilisateur. Si SQL Server est installé comme instance nommée, vous verrez ce nom ajouté comme suit : `MSSQL13.<instance_name>`.

|Version et la langue  | Chemin d’accès par défaut|
|----------------------|------------|
| SQL Server 2016 |C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES\library|
| SQL Server 2017 avec R|C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\R_SERVICES\library |
| SQL Server 2017 avec Python |C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages |


#### <a name="file-path-for-standalone-server-installations"></a>Chemin d’accès pour les installations serveur autonome

Le tableau suivant répertorie les chemins d’accès par défaut des fichiers binaires lors de l’installation de SQL Server 2016 R Server (autonome) ou SQL Server 2017 Machine Learning Server (autonome). 

|Version| Installation|Chemin d’accès par défaut|
|-------|-------------|------------|
| SQL Server 2016|R Server (autonome)| C:\Program Files\Microsoft SQL Server\130\R_SERVER|
|SQL Server 2017|Apprentissage de serveur, avec R |C:\Program Files\Microsoft SQL Server\140\R_SERVER|
|SQL Server 2017|Apprentissage de serveur, avec Python |C:\Program Files\Microsoft SQL Server\140\PYTHON_SERVER|

> [!NOTE]
> Si vous trouvez d’autres dossiers ayant des noms similaires sous-dossier et les fichiers, vous disposez probablement d’une installation autonome de Microsoft R Server ou [serveur Machine Learning](https://docs.microsoft.com/machine-learning-server/). Ces produits de serveur ont des programmes d’installation différents et de chemins d’accès (à savoir, C:\Program Files\Microsoft\R Server\R_SERVER ou C:\Program Files\Microsoft\ML SERVER\R_SERVER). Pour plus d’informations, consultez [installer Machine Learning pour Windows Server](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install) ou [installer R Server 9.1 pour Windows](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows).

## <a name="next-steps"></a>Étapes suivantes

+ [Obtenir les informations sur le package](determine-which-packages-are-installed-on-sql-server.md)
+ [Installer de nouveaux packages R](install-additional-r-packages-on-sql-server.md)
+ [Installer de nouveaux packages Python](../python/install-additional-python-packages-on-sql-server.md)
+ [Activer la gestion de packages R à distance](r-package-how-to-enable-or-disable.md)
+ [Fonctions RevoScaleR pour la gestion de packages R](use-revoscaler-to-manage-r-packages.md)
+ [Synchronisation de package R](package-install-uninstall-and-sync.md)
+ [miniCRAN pour référentiel de packages R local](create-a-local-package-repository-using-minicran.md)
