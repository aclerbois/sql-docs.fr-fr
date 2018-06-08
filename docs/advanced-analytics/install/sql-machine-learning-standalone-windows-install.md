---
title: Installer SQL Server 2017 Machine Learning Server (autonome) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: cb906a8a05221204ec10310d652f6891861d35e2
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708267"
---
# <a name="install-sql-server-2017-machine-learning-server-standalone-on-windows"></a>Installer SQL Server 2017 d’apprentissage Server (autonome) sur Windows
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Le programme d’installation de SQL Server inclut la possibilité d’installer un serveur qui s’exécute en dehors de SQL Server d’apprentissage. Cette option peut être utile si vous avez besoin développer de hautes performances machine learning solutions peuvent utiliser à distance les contextes de calcul, basculement indifféremment entre le serveur local et un ordinateur de formation serveur sur un cluster Spark ou sur un autre serveur SQL Server instance.
  
Cet article décrit comment utiliser le programme d’installation de SQL Server pour installer la version autonome de **SQL serveur 2017 Machine Learning**. 

## <a name="bkmk_prereqs"> </a> Liste de vérification de préinstallation

SQL Server 2017 est requis. Si vous disposez de SQL Server 2016, vous devez installer [(autonome) du serveur SQL Server 2016 R](sql-r-standalone-windows-install.md) à la place.

Si vous avez installé une version antérieure, telles que SQL Server 2016 R Server (autonome) ou Microsoft R Server, désinstallez l’installation existante avant de continuer.

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="run-setup"></a>Exécutez le programme d’installation

Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.

1. Démarrer l’Assistant Installation de SQL Server 2017.

2. Cliquez sur le **Installation** et sélectionnez **installation de la nouvelle Machine Learning Server (autonome)**.
    
     ![Installer Machine Learning serveur autonome](media/2017setup-installation-page-mlsvr.png "démarrer l’installation de la Machine Learning serveur autonome")

3. Une fois la vérification des règles terminée, acceptez les termes du contrat de licence de SQL Server et sélectionnez une nouvelle installation.

4. Sur le **sélection des fonctionnalités** page, ce qui suit options doivent être déjà sélectionnées :

    - Microsoft Machine Learning Server (autonome)

    - R et Python est sélectionnées par défaut. Vous pouvez désélectionner les deux langages, mais nous vous recommandons d’installer au moins une des langues prises en charge.

     ![Installer Machine Learning serveur autonome](media/2017setup-features-page-mlsvr-rpy.png "démarrer l’installation de la Machine Learning serveur autonome")
    
    Toutes les autres options peuvent être ignorées. 
    
    > [!NOTE]
    > Évitez d’installer le **fonctionnalités partagées** si l’ordinateur est déjà Machine Learning Services est installé pour l’analytique des bases de données dans SQL Server. Cette opération crée les bibliothèques en double.
    > 
    > En outre, tandis que les scripts R ou Python en cours d’exécution dans SQL Server sont gérés par SQL Server, par conséquent, comme n’étant ne pas en conflit avec la mémoire utilisée par d’autres services de moteur de base de données, serveur autonome machine learning n’a aucuns contraintes et peut interférer avec d’autres opérations de base de données . Enfin, l’accès à distance via une session RDP, qui est souvent utilisé pour une Opérationnalisation rapides, est généralement bloqué par les administrateurs de base de données.
    > 
    > Pour ces raisons, nous déconseillons généralement que vous installez Machine Learning Server (autonome) sur un ordinateur distinct à partir de la Machine Learning Services SQL Server.

5.  Accepter les termes du contrat de licence pour télécharger et installer les composants d’apprentissage automatique. Si vous installez les deux langages, un contrat de licence distinct est requis pour Microsoft R et pour Python.
    
     ![Contrat de licence de Python](media/2017setup-python-license.png "contrat de licence de Python")
    
    Installation de ces composants et les composants requis que peuvent nécessiter, peut prendre un certain temps. Lorsque le bouton **Accepter** n’est plus disponible, vous pouvez cliquer sur **Suivant**.

6.  Sur la page **Prêt pour l’installation** , vérifiez vos sélections, et cliquez sur **Installer**.

### <a name="default-installation-folders"></a>Dossiers d’installation par défaut

Lorsque vous installez R Server ou un serveur d’apprentissage Machine à l’aide du programme d’installation de SQL Server, les bibliothèques R sont installés dans un dossier associé à la version de SQL Server que vous avez utilisé pour le programme d’installation. Dans ce dossier, vous y trouverez également des exemples de données, la documentation pour les packages de base R et documentation des outils de R et de runtime.

Toutefois, si vous installez à l’aide du programme d’installation Windows distinct, ou si vous mettez à niveau à l’aide du programme d’installation Windows distinct, les bibliothèques R sont installés dans un dossier différent.

Pour référence, si vous avez installé une instance de SQL Server avec R Services (de-de base de données) ou Machine Learning (de-de base de données), et cette instance est sur le même ordinateur, les bibliothèques R et les outils sont installés par défaut dans un dossier différent.

Le tableau suivant répertorie les chemins d’accès pour chaque installation.

|Version| Méthode d’installation | Dossier par défaut|
|----|----|----|
|SQL Server 2017 Machine Learning Server (autonome) |  Assistant Installation de SQL Server 2017 |`C:\Program Files\Microsoft SQL Server\140\R_SERVER` <br/>`C:\Program Files\Microsoft SQL Server\140\PYTHON_SERVER`|
|Microsoft Machine Learning Server (autonome) |  Le programme d’installation de Windows autonome |`C:\Program Files\Microsoft\ML Server\R_SERVER`<br/>`C:\Program Files\Microsoft\ML Server\PYTHON_SERVER`|
|SQL Server 2017 Machine Learning Services (de-de base de données) |Assistant Installation de SQL Server 2017, avec option de langage R|`C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES`  <br/>`C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\PYTHON_SERVICES` |
|SQL Server 2016 R Server (autonome) |  Assistant Installation de SQL Server 2016 |`C:\Program Files\Microsoft SQL Server\130\R_SERVER`|
|SQL Server 2016 R Services (de-de base de données) |Assistant Installation de SQL Server 2016|`C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\R_SERVICES`|

## <a name="development-tools"></a>Outils de développement

Un développement IDE n’est pas installé dans le cadre du programme d’installation. Des outils supplémentaires ne sont pas requis, comme tous les outils standards sont inclus qui serait fournie avec une distribution de R ou Python.

Nous vous recommandons d’essayer la nouvelle version de [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)] ou [Python pour Visual Studio](https://docs.microsoft.com/visualstudio/python/installing-python-support-in-visual-studio). Visual Studio prend en charge à la fois R et Python, ainsi que les outils de développement de base de données, la connectivité avec SQL Server et des Outils BI. Toutefois, vous pouvez utiliser n’importe quel environnement de développement par défaut, y compris RStudio.

## <a name="get-help"></a>Obtenir de l’aide

Besoin d’aide avec l’installation ou de mise à niveau ? Pour obtenir des réponses aux questions les plus fréquentes et les problèmes connus, consultez l’article suivant :

* [Mise à niveau et installation FAQ - Machine Learning Services](../r/upgrade-and-installation-faq-sql-server-r-services.md)

Pour vérifier l’état d’installation de l’instance et résoudre les problèmes courants, essayez de ces rapports personnalisés.

* [Rapports personnalisés pour SQL Server R Services](../r/monitor-r-services-using-custom-reports-in-management-studio.md)

## <a name="next-steps"></a>Étapes suivantes

Les développeurs R peuvent démarrer avec des exemples simples et découvrez comment R fonctionne avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Didacticiel : Exécuter R dans T-SQL](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md)
+ [Didacticiel : De base de données analytique pour les développeurs R](../tutorials/sqldev-in-database-r-for-sql-developers.md)

Les développeurs Python peuvent apprendre à utiliser Python avec SQL Server en suivant ces didacticiels :

+ [Didacticiel : Exécutez Python dans T-SQL](../tutorials/run-python-using-t-sql.md)
+ [Didacticiel : De base de données analytique pour les développeurs Python](../tutorials/sqldev-in-database-python-for-sql-developers.md)

Pour afficher des exemples d’apprentissage automatique qui sont basées sur des scénarios concrets, consultez [Machine learning didacticiels](../tutorials/machine-learning-services-tutorials.md).
