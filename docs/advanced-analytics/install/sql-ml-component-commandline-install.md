---
title: Installation des composants de SQL Server machine learning d’invite de commandes | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 814e0f8172e02d9b02be95888c8dab286429e533
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707957"
---
# <a name="install-sql-server-machine-learning-components-from-the-command-line"></a>Installer les composants de SQL Server machine learning à partir de la ligne de commande
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fournit des instructions pour installer SQL Server d’apprentissage des composants à partir d’une ligne de commande :

+ [Instance de la nouvelle dans-base de données](#indb)
+ [Ajouter à une instance du moteur de base de données existante](#add-existing)
+ [Installation sans assistance](#silent)
+ [Nouveau serveur autonome](#shared-feature)

Vous pouvez spécifier une interaction en mode silencieux, de base ou complète avec l’interface utilisateur du programme d’installation. Cet article complète [installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md), couvrant les paramètres uniques aux composants d’apprentissage machine R et Python.

## <a name="pre-install-checklist"></a>Liste de vérification de préinstallation

+ Exécutez les commandes à partir d’une invite de commandes avec élévation de privilèges. 

+ Une instance du moteur de base de données est requise pour les installations de la base de données. Vous ne pouvez pas installer uniquement les fonctionnalités R ou Python, bien que vous puissiez [les ajouter de façon incrémentielle à une instance existante](#add-existing). Si vous souhaitez simplement R et Python sans le moteur de base de données, installez le [serveur autonome](#shared-feature).

+ N’installez pas sur un cluster de basculement. Le mécanisme de sécurité utilisé pour isoler les processus R et Python n’est pas compatible avec un environnement de cluster de basculement Windows Server.

+ N’installez pas sur un contrôleur de domaine. La partie de la Machine Learning Services du programme d’installation échoue.

+ Évitez d’installer autonomes et des instances de base de données sur le même ordinateur. Un serveur autonome est en concurrence pour les mêmes ressources, fragilisant ainsi les performances de ces deux installations.


## <a name="command-line-arguments"></a>Arguments de ligne de commande

L’argument de fonctionnalités est requis, comme le sont des contrats de termes de licence. 

En cas d’installation à partir de l’invite de commandes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le mode silencieux complet à l’aide du paramètre /Q ou le mode silencieux simple à l’aide du paramètre /QS. Le commutateur /QS affiche seulement la progression ; il n'accepte pas d'entrée et n'affiche aucun message d'erreur. Le paramètre /QS est pris en charge uniquement lorsque /Action=install est spécifié.

| Arguments | Description |
|-----------|-------------|
| / FONCTIONNALITÉS = AdvancedAnalytics | Installe la version de base de données : SQL Server 2017 Machine Learning Services (de-de base de données) ou SQL Server 2016 R Services (de-de base de données).  |
| / FONCTIONNALITÉS = SQL_INST_MR | S’applique à SQL Server 2017 uniquement. Coupler avec AdvancedAnalytics. Installe la fonctionnalité (de-de base de données) R, y compris Microsoft R Open et les packages R propriétaires. La fonctionnalité SQL Server 2016 R Services est R uniquement, donc il n’existe aucun paramètre pour cette version.|
| / FONCTIONNALITÉS = SQL_INST_MPY | S’applique à SQL Server 2017 uniquement. Coupler avec AdvancedAnalytics. Installe la fonctionnalité les Python (de-de base de données), y compris Anaconda et les packages Python propriétaires. |
| / FONCTIONNALITÉS = SQL_SHARED_MR | Installe la fonctionnalité de R pour la version autonome : SQL Server 2017 Machine Learning Server (autonome) ou SQL Server 2016 R Server (autonome). Un serveur autonome est une « fonctionnalité partagée « ne pas liée à une instance du moteur de base de données.|
| / FONCTIONNALITÉS = SQL_SHARED_MPY | S’applique à SQL Server 2017 uniquement. Installe la fonctionnalité de Python pour la version autonome : SQL Server 2017 Machine Learning Server (autonome). Un serveur autonome est une « fonctionnalité partagée « ne pas liée à une instance du moteur de base de données.|
| /IACCEPTROPENLICENSETERMS  | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation de composants R open source. |
| / IACCEPTPYTHONLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour utiliser les composants de Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indique que vous avez accepté les termes du contrat de licence pour l’utilisation de SQL Server.|
| / MRCACHEDIRECTORY | Programme d’installation hors connexion, définit le dossier contenant les fichiers CAB des composants R. |
| / MPYCACHEDIRECTORY | Programme d’installation hors connexion, définit le dossier contenant les fichiers CAB du composant Python. |


## <a name="indb"></a> Dans la base de données installations d’instance

Dans la base de données analytique est disponibles pour les instances du moteur de base de données, nécessaires pour ajouter le **AdvancedAnalytics** à votre installation. Vous pouvez installer une instance du moteur de base de données avec analytique avancée, ou [ajouter à une instance existante](#add-existing). 

Pour afficher les informations de progression sans l’interactive à l’écran invites, utilisez l’argument /qs.

> [!IMPORTANT]
> Après l’installation, les deux étapes de configuration supplémentaires restent. Intégration n’est pas terminée tant que ces tâches sont effectuées. Consultez [tâches de post-installation](#post-install) pour obtenir des instructions.

### <a name="sql-server-2017-database-engine-advanced-analytics-with-python-and-r"></a>SQL Server 2017 : moteur de base de données, advanced analytique avec Python et R

Pour une installation simultanée de l’instance du moteur de base de données, fournissez le nom d’instance et une connexion d’administrateur (Windows). Inclure les fonctionnalités pour l’installation de base et composants du langage, ainsi qu’acceptation de tous les termes du contrat de licence.

```  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Présente la même commande, mais avec une connexion SQL Server sur un moteur de base de données à l’aide de l’authentification mode mixte.

```
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SECURITYMODE=SQL /SAPWD="%password%" /SQLSYSADMINACCOUNTS="<sql-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Cet exemple est Python, indiquant que vous pouvez ajouter une langue en omettant une fonctionnalité.

```  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTPYTHONLICENSETERMS
```

### <a name="sql-server-2016-database-engine-and-advanced-analytics-with-r"></a>SQL Server 2016 : le moteur de base de données et analytique avancée avec R

Cette commande est identique à SQL Server 2017, mais sans les éléments de Python, qui ne sont pas disponible dans le programme d’installation de SQL Server 2016.

```  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS 
```

## <a name="post-install"></a> Configuration de post-installation (obligatoire)

Dans la base de données uniquement les installations s’applique.

Lorsque l’installation est terminée, vous disposez d’une instance du moteur de base de données avec des packages R et Python, Microsoft R et Python, Microsoft R Open, Anaconda, outils, des exemples et des scripts qui font partie de la distribution. 

Deux tâches plus sont requis pour terminer l’installation :

1. Redémarrez le service de moteur de base de données.

1. Activer les scripts externes avant de pouvoir utiliser la fonctionnalité. Suivez les instructions de [installer SQL Server 2017 Machine Learning Services (de-de base de données)](sql-machine-learning-services-windows-install.md) l’étape suivante. 

Pour SQL Server 2016, utilisez à la place cet article [installer SQL Server 2016 R Services (de-de base de données)](sql-r-services-windows-install.md).

## <a name="add-existing"></a> Ajouter d’analytique avancée à une instance du moteur de base de données existante

Lors de l’ajout d’analytique avancée de la base de données à une instance de moteur de base de données existante, indiquez le nom de l’instance. Par exemple, si vous avez installé précédemment un moteur de base de données SQL Server 2017 et les Python, vous pouvez utiliser cette commande pour ajouter de R.

```  
Setup.exe /qs /ACTION=Install /FEATURES=SQL_INST_MR /INSTANCENAME=MSSQLSERVER 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTROPENLICENSETERMS
```



## <a name="silent"></a> Installation sans assistance

Une installation sans assistance supprime la vérification des emplacements de fichier .cab. Pour cette raison, vous devez spécifier l’emplacement où les fichiers .cab doivent être décompressé. Vous pouvez le répertoire temporaire pour ce.
 
```  
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS 
/MRCACHEDIRECTORY=%temp% /MPYCACHEDIRECTORY=%temp%
```

## <a name="shared-feature"></a> Installations serveur autonome

Un serveur autonome est une « fonctionnalité partagée « ne pas liée à une instance du moteur de base de données. Les exemples suivants illustrent la syntaxe valide pour les deux versions.

SQL Server 2017 prend en charge les Python et R sur un serveur autonome :

```
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR,SQL_SHARED_MPY  
/IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```

SQL Server 2016 est R uniquement :

```
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR 
/IACCEPTROPENLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```

Lorsque l’installation est terminée, vous disposez d’un serveur, les packages de Microsoft, les distributions open source de R et Python, des outils, des exemples et de scripts qui font partie de la distribution. 

Pour ouvrir une fenêtre de console R, accédez à \Program files\Microsoft SQL Server\140 (ou 130) \R_SERVER\bin\x64 et double-cliquez sur **RGui.exe**. Vous débutez avec R ? Essayez de ce didacticiel : [commandes R de base et les fonctions RevoScaleR : 25 exemples courants](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler).

Pour ouvrir une commande Python, accédez à \Program files\Microsoft SQL Server\140\PYTHON_SERVER\bin\x64 et double-cliquez sur **python.exe**.

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