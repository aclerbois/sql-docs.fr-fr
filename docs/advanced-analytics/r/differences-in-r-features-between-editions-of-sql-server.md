---
title: Disponibilité des fonctionnalités entre différentes éditions de SQL Server Machine - Learning Services | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: cdb3e54addb2137e7c0ae2d6462be9c75c542539
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="feature-availability-across-editions-of-sql-server-machine-learning-services"></a>Disponibilité des fonctionnalités entre différentes éditions de SQL Server Machine Learning Services
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
 
 Fonctionnalités de machine learning sont disponibles dans SQL Server 2016 et SQL Server 2017. Cet article répertorie les éditions qui fournit la fonctionnalité, décrit les limitations qui s’appliquent dans les éditions spécifiques et répertorie les fonctionnalités disponibles uniquement dans certaines éditions.


## <a name="sql-server-2017-features"></a>Fonctionnalités de SQL Server 2017

Éditions Enterprise et Developer ont la même couverture de fonctionnalité qui permet de générer des solutions pour une installation d’entreprise sans avoir au même coût. Bien que les éditions sont fonctionnellement équivalentes, utilisation de l’édition développeur n’est pas prise en charge pour les environnements de production.

La différence entre l’intégration de base et avancée est mise à l’échelle. Intégration avancée peut utiliser tous les cœurs disponibles pour le traitement parallèle de jeux de données à n’importe quelle taille de que votre ordinateur peut prendre en charge. Intégration de base est limitée à 2 cœurs et aux jeux de données dans la mémoire. 

Intégration de base et avancée s’applique aux instances (de-de base de données). Un serveur autonome n’est pas une fonctionnalité d’instance du moteur de base de données et est proposé comme option d’installation uniquement dans les éditions Developer et Enterprise.

|Fonctionnalité|Enterprise|Standard|Web|Express with Advanced Services|Express 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Intégration R de base|Oui|Oui|Oui|Oui|non|   
|Intégration R avancée|Oui|Non|Non|Non|non| 
|Intégration de Python de base|Oui|Oui|Oui|Oui|non|
|Intégration de Python avancée|Oui|Non|Non|Non|non| 
|Machine Learning Server (autonome)|Oui|Non|Non|Non|non|   

 > [!NOTE]
 > Un serveur (autonome) offre les [à l’Opérationnalisation](https://docs.microsoft.com/machine-learning-server/what-is-operationalization) des fonctionnalités qui sont incluses dans une installation Microsoft (non SQL-marque) serveur de R ou Machine Learning. À l’Opérationnalisation inclut le déploiement du service web et les fonctionnalités d’hébergement.
>
> Pour une installation (de-de base de données), l’approche équivalente pour les solutions à mettre en application exploite les fonctionnalités du moteur de base de données, lorsque vous convertissez un code à une fonction qui peut s’exécuter dans une procédure stockée.


## <a name="sql-server-2016-r-features"></a>Fonctionnalités de SQL Server 2016 R

SQL Server 2016 inclut l’intégration de R uniquement. Dans SQL Server 2016, intégration de R de base et avancée sont équivalentes à SQL Server 2017.

## <a name="r-feature-availability-in-azure-sql-database"></a>Disponibilité des fonctionnalités de R dans la base de données SQL Azure
  
Après une version test initial, R Services a été supprimé à partir de la base de données SQL Azure, en attente de développement. 

## <a name="performance-expectations-for-enterprise-edition"></a>Attentes en matière de performances de Enterprise Edition

Performances des solutions de machine learning [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est censé dépassent généralement des implémentations à l’aide de R classique, étant donné le même matériel. Car, dans SQL Server, des solutions R peuvent être exécutées à l’aide des ressources du serveur et parfois distribuées à plusieurs processus à l’aide de la **RevoScaleR** fonctions. 

Les utilisateurs pourront également voir des différences considérables des performances et une extensibilité accrues pour la même solution d’apprentissage s’exécuter dans Visual Studio d’Enterprise Edition. Standard Edition. Raisons prennent en charge parallèle traitement, l’augmentation de threads disponibles pour l’apprentissage et de diffusion en continu (ou de segmentation), ce qui permet les fonctions RevoScaleR gérer plus de données que vous pouvez tenir dans la mémoire. 

Toutefois, les performances même sur du matériel identiques peuvent être affectées par de nombreux facteurs en dehors du code R ou Python. Ces facteurs incluent les demandes concurrentes sur les ressources de serveur, le type de plan de requête qui est créé, les modifications de schéma, la nécessité de mettre à jour les statistiques ou en créer un nouveau plan de requête, la fragmentation et bien plus encore. Il est possible qu’une procédure stockée contenant du code R ou Python peut s’exécuter dans les secondes sous une charge de travail, mais prennent en minutes lorsque d’autres services en cours d’exécution.  Par conséquent, nous vous recommandons de surveiller plusieurs aspects des performances du serveur, y compris la mise en réseau pour les contextes de calcul à distance, lors de la mesure des performances d’une machine learning.

Nous recommandons également de configurer [du gouverneur de ressources](../../relational-databases/resource-governor/resource-governor.md) (disponible dans l’édition Enterprise) pour personnaliser la manière que les travaux de script externe sont classés par priorité ou gérées sous des charges de travail lourd du serveur. Vous pouvez définir des fonctions classifieur pour spécifier la source de la tâche de script externe et de hiérarchiser certaines charges de travail, de limiter la quantité de mémoire utilisée par les requêtes SQL et contrôler le nombre de traitements parallèles utilisées dans une charge de travail.

## <a name="see-also"></a>Voir aussi

+ [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)
+ [Éditions et composants de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)
+ [Obtenir des conseils sur le calcul des données volumineuses dans R (serveur d’apprentissage Machine)](https://docs.microsoft.com/machine-learning-server/r/tutorial-large-data-tips)
