---
title: Conseils pour l’utilisation de packages R sont installés dans les bibliothèques utilisateur sur SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/30/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e52a6f4a9a3830aab01e54819804785a7069c9d2
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708467"
---
# <a name="tips-for-using-r-packages-in-sql-server"></a>Conseils pour l’utilisation de packages R dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article contient des sections distinctes pour les bases de données qui ne sont pas familiarisés avec R et les développeurs expérimentés dans R, qui sont des accès de package inconnu dans une instance de SQL Server.

## <a name="new-to-r"></a>Vous débutez avec R

En tant qu’administrateur de l’installation de R packages pour la première fois, à savoir quelques notions de base sur la gestion des packages R peuvent vous aider à commencer.

### <a name="package-dependencies"></a>Dépendances de package

Packages R dépendent souvent plusieurs packages, certains d'entre eux peuvent être disponibles dans la bibliothèque de R par défaut utilisée par l’instance. Parfois, un package requiert une version différente d’un package dépendant qui est déjà installé. Dépendances de package sont indiquées dans un fichier de DESCRIPTION incorporé dans le package, mais ne sont parfois pas complets. Vous pouvez utiliser un package appelé [iGraph](http://igraph.org/r/) pour entièrement exprimer le graphique de dépendance.

Si vous avez besoin d’installer plusieurs packages ou souhaitez vous assurer que tous les membres de votre organisation Obtient le type de package approprié et la version, nous vous recommandons d’utiliser le [miniCRAN](https://mran.microsoft.com/package/miniCRAN) package pour analyser la chaîne de dépendance terminée. minicRAN crée un référentiel local qui peut être partagé entre plusieurs utilisateurs ou ordinateurs. Pour plus d’informations, consultez [créer un référentiel de package local à l’aide de miniCRAN](create-a-local-package-repository-using-minicran.md).

### <a name="package-sources-versions-and-formats"></a>Formats, des versions et des sources de package

Il existe plusieurs sources de packages R, tels que [CRAN](https://cran.r-project.org/) et [Bioconductor](https://www.bioconductor.org/). Le site officiel du langage R (<https://www.r-project.org/>) répertorie la plupart de ces ressources. Microsoft propose [MRAN](https://mran.microsoft.com/) pour sa distribution open source r ([MRO](https://mran.microsoft.com/open)) et d’autres packages. De nombreux packages sont publiés sur GitHub, où les développeurs peuvent obtenir le code source.

Packages R s’exécuter sur plusieurs plates-formes informatiques. N’oubliez pas que les versions que vous installez sont binaires Windows.

### <a name="know-which-library-you-are-installing-to-and-which-packages-are-already-installed"></a>Connaître la bibliothèque dans laquelle vous effectuez l’installation et lesquelles packages sont déjà installés.

Si vous avez modifié précédemment l’environnement R sur l’ordinateur, avant d’installer quoi que ce soit, vérifiez que la variable d’environnement R `.libPath` utilise simplement un chemin d’accès.

Ce chemin d’accès doit pointer vers le dossier R_SERVICES pour l’instance. Pour plus d’informations, notamment la façon de déterminer les packages qui sont déjà installés, consultez [packages par défaut de R et Python dans SQL Server](installing-and-managing-r-packages.md).

## <a name="new-to-sql-server"></a>Nouveautés de SQL Server

En tant que R développeur travaille sur l’exécution de code sur SQL Server, les stratégies de sécurité qui protège le serveur limitent votre capacité à contrôler l’environnement R.

### <a name="r-user-libraries-not-supported-on-sql-server"></a>Bibliothèques d’utilisateur R : non pris en charge sur SQL Server

Les développeurs R qui ont besoin d’installer de nouveaux packages R sont habitués à l’installation des packages à volonté, à l’aide d’un privés, la bibliothèque de l’utilisateur chaque fois que la bibliothèque par défaut n’est pas disponible, ou lorsque le développeur n’est pas un administrateur sur l’ordinateur. Par exemple, dans un environnement de développement R classique, l’utilisateur devez ajouter l’emplacement du package à la variable d’environnement R `libPath`, ou référence le chemin d’accès complet du package, comme suit :

```R
library("c:/Users/<username>/R/win-library/packagename")
```

Cela ne fonctionne pas lors de l’exécution des solutions R dans SQL Server, car les packages R doivent être installés dans une bibliothèque spécifique par défaut qui est associée à l’instance. Lorsqu’un package n’est pas disponible dans la bibliothèque par défaut, vous obtenez cette erreur lorsque vous essayez d’appeler le package :

*Erreur dans library(xxx) : il n’existe aucun package appelé 'package-name'*

### <a name="avoid-package-not-found-errors"></a>Éviter les erreurs de « package introuvable »

+ Éliminer les dépendances sur les bibliothèques de l’utilisateur. 

    Il est pratique de développement incorrecte pour installer les packages R requis dans une bibliothèque d’utilisateur personnalisée, comme il peut entraîner des erreurs si une solution est exécutée par un autre utilisateur qui n’a pas accès à l’emplacement de la bibliothèque.

    En outre, si un package est installé dans la bibliothèque par défaut, le runtime R charge le package à partir de la bibliothèque par défaut, même si vous spécifiez une version différente dans le code R.

+ Modifiez votre code s’exécute dans un environnement partagé.

+ Évitez d’installer des packages dans le cadre d’une solution. Si vous n’êtes pas autorisé à installer des packages, le code échoue. Même si vous avez les autorisations nécessaires pour installer des packages, vous devez procéder séparément du reste du code que vous souhaitez exécuter.

+ Vérifiez votre code pour vous assurer qu’il ne contient pas d’appel à des packages désinstallés.

+ Mettre à jour votre code pour supprimer des références directes aux chemins des packages R ou des bibliothèques R. 

+ Ignore la bibliothèque de package est associée à l’instance. Pour plus d’informations, consultez [packages par défaut de R et Python dans SQL Server](installing-and-managing-r-packages.md).

## <a name="see-also"></a>Voir aussi

+ [Installer de nouveaux packages R](install-additional-r-packages-on-sql-server.md)
+ [Installer de nouveaux packages Python](../python/install-additional-python-packages-on-sql-server.md)
+ [Didacticiels, exemples, solutions](../tutorials/machine-learning-services-tutorials.md)