---
title: L’ordinateur SQL Server Learning Services avec Python | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: b8ef234b0e8c09e3d4be9488b7ac628c225e67f6
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="sql-server-machine-learning-services-with-python"></a>Ordinateur SQL Server Learning Services avec Python
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Python est un langage qui offre une grande souplesse et puissance pour diverses tâches d’apprentissage. Les bibliothèques Open source pour Python incluent plusieurs plateformes pour les réseaux neuronaux personnalisables, ainsi que les bibliothèques pour le traitement du langage naturel. À présent, cette langue largement utilisée est prise en charge dans l’apprentissage de SQL Server 2017.

Étant donné que les Python est intégré à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] moteur de base de données, vous pouvez conserver analytique proches des données et éliminer les coûts et les risques de sécurité liés au déplacement des données.  Vous pouvez déployer des solutions d’apprentissage machine en fonction de Python à l’aide d’outils pratiques et familiers, tels que Visual Studio. Les applications de production peuvent obtenir des prédictions, les modèles, ou les éléments visuels à partir du runtime Python 3.5 en appelant simplement un T-SQL de procédure stockée.

Cette version inclut la distribution Anaconda de Python, ainsi que le [revoscalepy](../python/what-is-revoscalepy.md) bibliothèque, afin d’améliorer l’évolutivité et des performances de vos solutions d’apprentissage.

Vous pouvez installer tout ce dont vous avez besoin pour commencer avec Python via le programme d’installation de SQL Server 2017 :

+ [**Services de formation (dans-base de données) de l’ordinateur**](../install/sql-machine-learning-services-windows-install.md): installer cette fonctionnalité, ainsi que le moteur de base de données SQL Server, pour activer l’exécution sécurisée des scripts Python sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordinateur.
  
     Lorsque vous activez cette fonctionnalité, les extensions sont installées dans le moteur de base de données pour prendre en charge l’exécution de scripts Python et un nouveau service est créé, le [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], pour gérer les communications entre le runtime Python et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.

+ [**Machine Learning Server (autonome)**](../install/sql-machine-learning-standalone-windows-install.md): Si vous n’avez pas besoin d’intégration de SQL Server, installez cette fonction pour obtenir un support technique de Python et R pour l’apprentissage distribuée.

## <a name="see-also"></a>Voir aussi

+ [Apprentissage et R Services (de-de base de données) de l’ordinateur SQL Server](../r/sql-server-r-services.md)
+ [SQL Server Machine Learning et R Server (autonome)](../r/r-server-standalone.md)
+ [Architecture de Python](architecture-overview-sql-server-python.md)
+ [Didacticiels de Python](../tutorials/sql-server-python-tutorials.md)
