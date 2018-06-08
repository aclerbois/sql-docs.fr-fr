---
title: Installer les composants de l’apprentissage de SQL Server sans accès à internet | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/02/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 289f304cf445882981fb110e9c00a395cac90e5f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585611"
---
# <a name="install-sql-server-machine-learning-components-without-internet-access"></a>Installer les composants sans accès à internet d’apprentissage automatique SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Par défaut, les programmes d’installation de se connectent à des sites de téléchargement Microsoft pour obtenir requis et les composants mis à jour pour apprentissage sur SQL Server. Si les contraintes de pare-feu empêchent le programme d’installation d’atteindre ces sites, vous pouvez utiliser un appareil connecté à internet pour télécharger les fichiers, transférer des fichiers sur un serveur en mode hors connexion, puis exécutez le programme d’installation.

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

> [!NOTE]  
> Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.  
 
 ###  <a name="bkmk_ga_instalpatch"></a> Installer le correctif obligatoire 

Microsoft a identifié un problème avec la version spécifique des fichiers binaires Microsoft VC++ 2013 Runtime qui sont installés en tant que composants requis par SQL Server. Si cette mise à jour des fichiers binaires du runtime VC n’est pas installée, SQL Server risque de rencontrer des problèmes de stabilité dans certains scénarios. Avant d’installer SQL Server, suivez les instructions données dans [Notes de publication de SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) pour voir si votre ordinateur nécessite un correctif pour les fichiers binaires du runtime VC.  


## <a name="download-cab-files"></a>Télécharger des fichiers .cab

Sur un serveur connecté à internet, téléchargez les fichiers .cab requis pour une installation hors connexion. Le programme d’installation utilise les fichiers .cab pour installer des fonctionnalités supplémentaires.

Version  |Télécharger le lien  |
---------|---------|
**Version initiale de SQL Server 2017** |
Microsoft R Open     |[SRO_3.3.3.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851496)|
Microsoft R Server      |[SRS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851507)|
Python de Microsoft Open     |[SPO_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851502) |
Python de Microsoft Server    |[SPS_9.2.0.24_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851508) |
**SQL Server 2017 CU1** |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server      |[SRS_9.2.0.100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851501)|
Python de Microsoft Open     |aucune modification ; utiliser le précédent |
Python de Microsoft Server    |[SPS_9.2.0.100_1033.cab](https://go.microsoft.com/fwlink/?LinkId=851500) |
**SQL Server 2017 CU2** |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server      |aucune modification ; utiliser le précédent|
Python de Microsoft Open     |aucune modification ; utiliser le précédent|
Python de Microsoft Server    |aucune modification ; utiliser le précédent|
**SQL Server 2017 CU3** |
Microsoft R Open     |[SRO_3.3.3.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863894)|
Microsoft R Server      |[SRS_9.2.0.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863893)|
Python de Microsoft Open     |aucune modification ; utiliser le précédent|
Python de Microsoft Server    |[SPS_9.2.0.300_1033.cab](https://go.microsoft.com/fwlink/?LinkId=863892)|
**SQL Server 2017 CU4** |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server      |[SRS_9.2.0.400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866212&clcid=1033)|
Python de Microsoft Open     |aucune modification ; utiliser le précédent|
Python de Microsoft Server    |[SPS_9.2.0.400_1033.cab](https://go.microsoft.com/fwlink/?LinkId=866213&clcid=1033)|
**SQL Server 2017 CU5** |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server      |[SRS_9.2.0.500_1033.cab](https://go.microsoft.com/fwlink/?LinkId=869052&clcid=1033)|
Python de Microsoft Open     |aucune modification ; utiliser le précédent|
Python de Microsoft Server    |[SPS_9.2.0.500_1033.cab](https://go.microsoft.com/fwlink/?LinkId=869053&clcid=1033)|
**SQL Server 2017 CU6** |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server      |[SRS_9.2.0.600_1033.cab](https://go.microsoft.com/fwlink/?LinkId=871074&clcid=1033)|
Python de Microsoft Open     |aucune modification ; utiliser le précédent|
Python de Microsoft Server    |[SPS_9.2.0.600_1033.cab](https://go.microsoft.com/fwlink/?LinkId=871073&clcid=1033)|
**SQL Server 2017 CU7** |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server      |modification de o ; utiliser le précédent|
Python de Microsoft Open     |aucune modification ; utiliser le précédent|
Python de Microsoft Server    |aucune modification ; utiliser le précédent|


### <a name="bkmk_2016Installers"></a>Téléchargements pour SQL Server 2016

> [!IMPORTANT]
> 
> Lors de l’installation de SQL Server 2016 SP1 CU4 ou SP1 CU5 hors connexion, téléchargez SRO_3.2.2.16000_1033.cab. Si vous avez téléchargé SRO_3.2.2.13000_1033.cab FWLINK 831785 comme indiqué dans la boîte de dialogue d’installation, renommez le fichier en tant que SRO_3.2.2.16000_1033.cab avant d’installer la mise à jour Cumulative.

Version  |Télécharger le lien  |
---------|---------|
**SQL Server 2016 RTM**     |
Microsoft R Open     |[SRO_3.2.2.803_1033.cab](https://go.microsoft.com/fwlink/?LinkId=761266)
Microsoft R Server     |[SRS_8.0.3.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=735051)
**SQL Server 2016 CU 1**     |
Microsoft R Open     |[SRO_3.2.2.10000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=808803)
Microsoft R Server     |[SRS_8.0.3.10000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=808805)
**SQL Server 2016 CU 2**     |
Microsoft R Open     |[SRO_3.2.2.12000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=827398)
Microsoft R Server     |[SRS_8.0.3.12000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=827399)
**SQL Server 2016 CU 3**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server     | aucune modification ; utiliser le précédent |
**SQL Server 2016 CU 4**     |
Microsoft R Open     |[SRO_3.2.2.13000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=831785)|
Microsoft R Server     |[SRS_8.0.3.13000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=831676)|
**SQL Server 2016 CU 5**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server     |aucune modification ; utiliser le précédent|
**SQL Server 2016 CU 6**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server     |[SRS_8.0.3.14000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=850316)  |
**SQL Server 2016 CU 7**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server     |aucune modification ; utiliser le précédent |
**SQL Server 2016 SP 1**     |
Microsoft R Open     |[SRO_3.2.2.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824879)
Microsoft R Server     |[SRS_8.0.3.15000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=824881)
**SQL Server 2016 SP 1 CU1**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server     |aucune modification ; utiliser le précédent|
**SQL Server 2016 SP1 CU2**     |
Microsoft R Open     |[SRO_3.2.2.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836819)|
Microsoft R Server    |[SRS_8.0.3.16000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=836818)|
**SQL Server 2016 SP 1 CU3**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server     |aucune modification ; utiliser le précédent|
**GDR et SQL Server 2016 SP 1 CU4**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server    |[SRS_8.0.3.17000_1033.cab](https://go.microsoft.com/fwlink/?LinkId=850317)
**SQL Server 2016 SP 1 CU5**     |
Microsoft R Open     |aucune modification ; utiliser le précédent|
Microsoft R Server    |aucune modification ; utiliser le précédent |

Si vous souhaitez afficher le code source pour Microsoft R, il est disponible en téléchargement sous forme d’archive .tar format : [programmes d’installation de télécharger le serveur R](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows#download)

### <a name = "bkmk_OtherComponents"></a>Autres conditions préalables

Selon votre environnement, vous devrez peut-être effectuer des copies locales des programmes d’installation pour les prérequis suivants.

Composant  |Version
---------|---------
[Fournisseur Microsoft OLE DB AS pour SQL Server 2016](https://go.microsoft.com/fwlink/?linkid=834405)     |  13.0.1601.5
[Microsoft .NET Core](https://go.microsoft.com/fwlink/?linkid=834319)     | 1.0.1
[Microsoft MPI](https://go.microsoft.com/fwlink/?linkid=834316)     | 7.1.12437.25
[Microsoft Visual C++ 2013 Redistributable](https://go.microsoft.com/fwlink/?linkid=799853)     | 12.0.30501.0
[Microsoft Visual C++ 2015 Redistributable](https://go.microsoft.com/fwlink/?linkid=828641)     | 14.0.23026.0

## <a name="transfer-files"></a>Transfert de fichiers

Transférer le support d’installation de SQL Server compressé et les fichiers que vous avez déjà téléchargé sur l’ordinateur sur lequel vous installez le programme d’installation.

Placez les fichiers CAB dans un dossier tel que **télécharge** ou le dossier temporaire de l’utilisateur programme d’installation : C:\Users < nom_utilisateur > \AppData\Local\Temp.

Placez le fichier en_sql_server_2017.iso dans un dossier. Double-cliquez sur **setup.exe** pour commencer l’installation.

### <a name="run-setup"></a>Exécutez le programme d’installation

Lorsque vous exécutez le programme d’installation de SQL Server sur un ordinateur soit déconnecté d’internet, le programme d’installation ajoute un **installation hors connexion** page de l’Assistant afin que vous pouvez spécifier l’emplacement des fichiers .cab que vous avez copié à l’étape précédente.

1. Démarrer l’Assistant Installation de SQL Server.

2. Lorsque l’Assistant Installation affiche la page de gestion des licences pour R open source ou les composants de Python, cliquez sur **accepter**. Acceptation des termes du contrat de licence vous permet de passer à l’étape suivante.

3. Dans le **installation hors connexion** page **chemin d’installation**, spécifiez le dossier contenant les fichiers .cab que vous avez copiée précédemment.

4. Continuer suivant les instructions à l’écran pour terminer l’installation.

Une fois l’installation terminée, redémarrez le service et ensuite configurer le serveur pour activer l’exécution du script, comme décrit dans [installer SQL Server 2017 Machine Learning Services (de-de base de données)](sql-machine-learning-services-windows-install.md) ou [installer SQL Server 2016 R Services (de-de base de données)](sql-r-services-windows-install.md).

## <a name="slipstream-upgrades-for-offline-servers"></a>Mises à niveau de l’installation intégrée pour les serveurs en mode hors connexion

Une installation intégrée renvoie à la possibilité d’appliquer un correctif ou une mise à jour à une installation d’instance défaillante dans le but de corriger les problèmes existants. L’avantage de cette méthode est que SQL Server est mis à jour en même temps que vous effectuez l’installation, ce qui évite d’avoir à effectuer par la suite un redémarrage séparé.

+ Si le serveur n’a pas accès à Internet, vous devez télécharger le programme d’installation de SQL Server, puis télécharger les versions correspondantes des programmes d’installation des composants R **avant** de commencer le processus de mise à jour.  Les composants R ne sont pas inclus par défaut avec SQL Server.

+ Si vous ajoutez ces composants à une installation existante, utilisez la version mise à jour du programme d’installation de SQL Server et la version correspondante de mise à jour des composants supplémentaires. Lorsque vous spécifiez que la fonctionnalité de R doit être installé, le programme d’installation recherche la version correspondante des programmes d’installation pour les composants d’apprentissage automatique.

## <a name="get-help"></a>Obtenir de l’aide

Besoin d’aide avec l’installation ou de mise à niveau ? Pour obtenir des réponses aux questions les plus fréquentes et les problèmes connus, consultez l’article suivant :

* [Mise à niveau et installation FAQ - Machine Learning Services](../r/upgrade-and-installation-faq-sql-server-r-services.md)

Pour vérifier l’état d’installation de l’instance et résoudre les problèmes courants, essayez de ces rapports personnalisés.

* [Rapports personnalisés pour SQL Server R Services](../r/monitor-r-services-using-custom-reports-in-management-studio.md)

Cet article par l’équipe de prise en charge des Services R montre comment effectuer une installation sans assistance ou une mise à niveau de R services dans SQL Server 2016 : [déploiement R Services sur des ordinateurs sans accès à Internet](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/do-it-right-deploying-sql-server-r-services-on-computers-without-internet-access/).


## <a name="next-steps"></a>Étapes suivantes

Les développeurs R peuvent démarrer avec des exemples simples et découvrez comment R fonctionne avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Didacticiel : Exécuter R dans T-SQL](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md)
+ [Didacticiel : De base de données analytique pour les développeurs R](../tutorials/sqldev-in-database-r-for-sql-developers.md)

Les développeurs Python peuvent apprendre à utiliser Python avec SQL Server en suivant ces didacticiels :

+ [Didacticiel : Exécutez Python dans T-SQL](../tutorials/run-python-using-t-sql.md)
+ [Didacticiel : De base de données analytique pour les développeurs Python](../tutorials/sqldev-in-database-python-for-sql-developers.md)

Pour afficher des exemples d’apprentissage automatique qui sont basées sur des scénarios concrets, consultez [Machine learning didacticiels](../tutorials/machine-learning-services-tutorials.md).

