---
title: Interopérabilité de Python avec SQL Server Machine Learning | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 3ad52bb3774c1534ac745e1dc93d18c75260723c
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="python-interoperability-with-sql-server"></a>Interopérabilité de Python avec SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les composants de Python qui sont installés si vous activez la fonctionnalité **Machine Learning Services (de-de base de données)** et sélectionnez les Python comme la langue.

## <a name="python-components"></a>Composants de Python

[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ne modifiez pas les exécutables de Python. Le runtime Python est installé indépendamment des outils SQL et est exécuté en dehors de la [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] processus.

La distribution est associée à un spécifique [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] instance se trouve dans le dossier associé à l’instance.

Par exemple, si vous avez installé les Services de Machine Learning avec l’option de Python sur l’instance par défaut, regardez sous :

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

Installation de SQL Server 2017 Machine Learning Services ajoute la distribution Anaconda de Python. Plus précisément, les programmes d’installation Anaconda 3 sont utilisés, en fonction de la branche Anaconda 4.3. Niveau de SQL Server 2017 Python attendu est Python 3.5.

## <a name="new-python-packages-in-this-release"></a>Nouveaux packages Python dans cette version

Pour obtenir la liste des packages pris en charge par la distribution Anaconda, consultez le site d’analytique Continuum : [liste des packages Anaconda](https://docs.continuum.io/anaconda/pkg-docs)

Machine Learning Services dans SQL Server 2017 inclut également la nouvelle **revoscalepy** bibliothèque pour Python.

Cette bibliothèque fournit des fonctionnalités équivalentes à celles de la **RevoScaleR** package pour Microsoft R. En d’autres termes, il prend en charge la création de contextes de calcul à distance, ainsi que divers modèles d’apprentissage de machines évolutive, tel que **rxLinMod**. Pour plus d’informations sur RevoScaleR, consultez [distribué et le calcul parallèle avec ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing).

Le [microsoftml pour Python](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) package est installé dans le cadre de l’apprentissage de SQL Server lorsque vous ajoutez Python à votre installation. Ce package contient un grand nombre des algorithmes qui ont été optimisées pour la vitesse et la précision, ainsi que les transformations permettant de manipuler du texte et des images en ligne d’apprentissage. Pour plus d’informations, consultez [à l’aide du package MicrosoftML avec SQL Server](https://docs.microsoft.com/sql/advanced-analytics/using-the-microsoftml-package).

Microsoftml et revoscalepy sont étroitement liés ; sources de données utilisées dans microsoftml sont définies en tant qu’objets de revoscalepy. Calcul des limites de contexte dans transfert revoscalepy à microsoftml. À savoir, toutes les fonctionnalités sont disponibles pour les opérations locales, mais le basculement vers un contexte de calcul à distance requiert RxInSqlServer.

## <a name="using-python-in-sql-server"></a>À l’aide de Python dans SQL Server

Vous importez le **revoscalepy** module dans votre code Python et appeler des fonctions à partir du module, comme d’autres fonctions de Python.

Les données d’entrée pour Python doivent être sous forme de tableau. Tous les résultats de Python doivent être retournés sous la forme d’un **pandas** trame de données.

Vous pouvez exécuter votre code Python à l’intérieur de T-SQL, en incorporant le script dans une procédure stockée.

Ou, exécutez le code à partir d’un IDE Python local et que le script exécuté sur l’ordinateur SQL Server, en définissant un contexte de calcul à distance.

Vous pouvez utiliser des données locales, obtenir des données à partir de SQL Server ou d’autres sources de données ODBC ou utiliser le format de fichier XDF pour échanger des données avec d’autres sources, ou avec des solutions R.

**Pour plus d’informations**

+ Fonctions prises en charge : [What ' s revoscalepy](what-is-revoscalepy.md) 
+ Prise en charge des types de données Python : [Python bibliothèques et Types de données](python-libraries-and-data-types.md)
+ Prise en charge des sources de données : ODBC bases de données, SQL Server et les fichiers XDF
+ Prise en charge des contextes de calcul : local ou SQL Server

### <a name="licensing"></a>Gestionnaire de licences

Dans le cadre de l’installation des Services d’apprentissage Machine avec Python, vous devez accepter les termes du contrat de la licence publique GNU.

## <a name="see-also"></a>Voir aussi

[Bibliothèques et types de données Python](python-libraries-and-data-types.md)
