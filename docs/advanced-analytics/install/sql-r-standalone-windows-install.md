---
title: Installer SQL Server 2016 R Server (autonome) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e7e5b61cb8e41d818fc13d1cc97cd4d998256efc
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="install-sql-server-2016-r-server-standalone"></a>Installer SQL Server 2016 R Server (autonome)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit comment utiliser le programme d’installation de SQL Server 2016 à installer la version autonome de **SQL Server 2016 R Server**.

## <a name="bkmk_prereqs"> </a> Liste de vérification de préinstallation

SQL Server 2016 est requis. Si vous disposez de SQL Server 2017, veuillez installer [Machine Learning Server (autonome) de SQL Server 2017](sql-machine-learning-standalone-windows-install.md) à la place.

Si vous avez installé une version précédente de l’outils de Revolution Analytique ou les packages, vous devez tout d’abord les désinstaller. 

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

 ###  <a name="bkmk_ga_instalpatch"></a> Installer le correctif obligatoire 

Microsoft a identifié un problème avec la version spécifique des fichiers binaires Microsoft VC++ 2013 Runtime qui sont installés en tant que composants requis par SQL Server. Si cette mise à jour des fichiers binaires du runtime VC n’est pas installée, SQL Server risque de rencontrer des problèmes de stabilité dans certains scénarios. Avant d’installer SQL Server, suivez les instructions données dans [Notes de publication de SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) pour voir si votre ordinateur nécessite un correctif pour les fichiers binaires du runtime VC.  

## <a name="run-setup"></a>Exécutez le programme d’installation

Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.

1. Démarrer l’Assistant Installation de SQL Server 2016. Nous vous recommandons d’installer Service Pack 1 ou version ultérieure.

2. Sur le **Installation** , cliquez sur **installation nouvelle R Server (autonome)**.
    
     ![Démarrer le programme d’installation de R Server autonome](media/2016-setup-installation-rsvr.png "démarrer le programme d’installation de R serveur autonome")
    
3.  Dans la page **Sélection de fonctionnalités** , l’option suivante doit déjà être sélectionnée :
    
    **R Server (autonome)**  
    
    ![Fonctionnalité des sélections pour la version autonome du serveur R](media/2016setup-rserver-features.png "les sélections pour la version autonome du serveur R de fonctionnalités")
    
    Toutes les autres options peuvent être ignorées. 
    
    > [!NOTE]
    > Évitez d’installer le **fonctionnalités partagées** si vous exécutez le programme d’installation sur un ordinateur où R Services a déjà été installé pour l’analytique des bases de données dans SQL Server. Cette opération crée les bibliothèques en double.
    > 
    > Tandis que les scripts R en cours d’exécution dans SQL Server sont gérés par SQL Server, par conséquent, comme n’étant ne pas en conflit avec la mémoire utilisée par d’autres services de moteur de base de données, le serveur R autonome n’a aucuns contraintes et peut interférer avec d’autres opérations de base de données.
    > 
    > Nous déconseillons généralement installer R Server (autonome) sur un ordinateur distinct à partir de SQL Server R Services (de-de base de données).

4.  Acceptez les termes du contrat de licence pour télécharger et installer Microsoft R Open. Lorsque le bouton **Accepter** n’est plus disponible, vous pouvez cliquer sur **Suivant**.
    
    Installation de ces composants et les composants requis que peuvent nécessiter, peut prendre un certain temps.
    
5.  Sur la page **Prêt pour l’installation** , vérifiez vos sélections, et cliquez sur **Installer**.

## <a name="default-installation-folders"></a>Dossiers d’installation par défaut

Lorsque vous installez R Server à l’aide du programme d’installation de SQL Server, les bibliothèques R sont installés dans un dossier associé à la version de SQL Server que vous avez utilisé pour le programme d’installation. Dans ce dossier, vous y trouverez également des exemples de données, la documentation pour les packages de base R et documentation des outils de R et de runtime.

Toutefois, si vous avez installé Microsoft R Server à l’aide de Windows installer distinct (pas d’installation de SQL), ou si vous mettez à niveau à l’aide du programme d’installation Windows distinct, les bibliothèques R sont installés dans un dossier différent.

Bien que nous vous recommandons, si vous avez installé également une instance de SQL Server avec R Services (de-de base de données) sur le même ordinateur, une deuxième copie de R bibliothèques et des outils sont installés dans un dossier différent.

Le tableau suivant répertorie les chemins d’accès pour chaque installation.

|Version| Méthode d’installation | Dossier par défaut|
|----|----|----|
|R Server (autonome) |Assistant Installation de SQL Server 2016|`C:\Program Files\Microsoft SQL Server\130\R_SERVER`|
|R Server (autonome) |Le programme d’installation de Windows autonome|`C:\Program Files\Microsoft\R Server\R_SERVER`|
|Machine Learning Server (autonome) |  Assistant Installation de SQL Server 2017, avec option de langage R |`C:\Program Files\Microsoft SQL Server\140\R_SERVER`|
|Machine Learning Server (autonome) |  Assistant Installation de SQL Server 2017, avec option de langage Python |`C:\Program Files\Microsoft SQL Server\140\PYTHON_SERVER`|
|Machine Learning Server (autonome) |  Le programme d’installation de Windows autonome |`C:\Program Files\Microsoft\R Server\R_SERVER`|
|R Services (dans la base de données) |Assistant Installation de SQL Server 2016|`C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\R_SERVICES`|
|Machine Learning Services (en base de données) |Assistant Installation de SQL Server 2017, avec option de langage R|`C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES`  |
|Machine Learning Services (en base de données) |Assistant Installation de SQL Server 2017, avec option de langage Python| `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\PYTHON_SERVICES` |

## <a name="development-tools"></a>Outils de développement

Un développement IDE n’est pas installé dans le cadre du programme d’installation. Des outils supplémentaires ne sont pas requis, comme tous les outils standards sont inclus qui serait fournie avec une distribution de R ou Python.

Nous vous recommandons d’essayer la nouvelle version de [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)] ou [Python pour Visual Studio](https://docs.microsoft.com/en-us/visualstudio/python/installing-python-support-in-visual-studio). Visual Studio prend en charge à la fois R et Python, ainsi que les outils de développement de base de données, la connectivité avec SQL Server et des Outils BI. Toutefois, vous pouvez utiliser n’importe quel environnement de développement par défaut, y compris RStudio.
  
## <a name="get-help"></a>Obtenir de l’aide

Besoin d’aide avec l’installation ou de mise à niveau ? Pour obtenir des réponses aux questions les plus fréquentes et les problèmes connus, consultez l’article suivant :

* [Mise à niveau et installation FAQ - Machine Learning Services](../r/upgrade-and-installation-faq-sql-server-r-services.md)

Pour vérifier l’état d’installation de l’instance et résoudre les problèmes courants, essayez de ces rapports personnalisés.

* [Rapports personnalisés pour SQL Server R Services](../r/monitor-r-services-using-custom-reports-in-management-studio.md)

## <a name="next-steps"></a>Étapes suivantes

Les développeurs R peuvent démarrer avec des exemples simples et découvrez comment R fonctionne avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Didacticiel : Exécuter R dans T-SQL](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md).
+ [Didacticiel : De base de données analytique pour les développeurs R](../tutorials/sqldev-in-database-r-for-sql-developers.md)

Pour afficher des exemples d’apprentissage automatique qui sont basées sur des scénarios concrets, consultez [Machine learning didacticiels](../tutorials/machine-learning-services-tutorials.md).

