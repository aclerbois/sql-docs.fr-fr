---
title: Résoudre les problèmes de collecte de données pour l’apprentissage - SQL Server
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 9b0fdd8d198675720188d6ab2417be97a9280c57
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708407"
---
# <a name="troubleshoot-data-collection-for-machine-learning"></a>Résoudre les problèmes de collecte de données pour l’apprentissage
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les méthodes de collecte de données que vous devez utiliser lors de la tentative de résolution des problèmes sur votre propre ou à l’aide du client de Microsoft prennent en charge. 

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage Services (R et Python)


## <a name="sql-server-version-and-edition"></a>Édition et la version de SQL Server

SQL Server 2016 R Services est la première version de SQL Server pour inclure la prise en charge intégrée de R. SQL Server 2016 avec le Service Pack 1 (SP1) inclut plusieurs améliorations majeures, y compris la possibilité d’exécuter des scripts externes. Si vous êtes un client SQL Server 2016, vous devez envisager d’installer le SP1 ou version ultérieure.

SQL Server 2017 ajouté l’intégration de langage Python. Impossible d’obtenir fonctionnalité d’intégration Python dans les versions antérieures.

Pour obtenir edition et versions, consultez cet article, qui répertorie les numéros de build pour chacun de la [versions de SQL Server](https://social.technet.microsoft.com/wiki/contents/articles/783.sql-server-versions.aspx#Service_Pack_editions).

Selon l’édition de SQL Server, vous utilisez, certaines fonctionnalités d’apprentissage peut être indisponible ou limité. La liste suivante d’articles de la formation de fonctionnalités d’ordinateur dans les éditions Enterprise, Developer, Standard et Express.

* [Éditions et des fonctionnalités prises en charge de SQL Server](https://docs.microsoft.com/sql/sql-server/editions-and-components-of-sql-server-2016)
* [Fonctionnalités de R et Python par les éditions de SQL Server](r/differences-in-r-features-between-editions-of-sql-server.md)

## <a name="r-language-and-tool-versions"></a>Versions de langage et les outils R

En général, la version de Microsoft R qui est installé lorsque vous sélectionnez la fonctionnalité Services de R ou la fonction Machine Learning Services est déterminée par le numéro de version de SQL Server. Si vous mettez à niveau ou de correctif logiciel SQL Server, vous devez également mettre à niveau ou ses composants R de correctif logiciel.

Pour obtenir la liste des versions et des liens vers des téléchargements de composants R, consultez [installer les composants d’apprentissage ordinateur sans accès à internet](https://docs.microsoft.com/sql/advanced-analytics/r/installing-ml-components-without-internet-access). Sur les ordinateurs ayant accès à internet, la version requise de R est identifiée et installée automatiquement.

Il est possible de mettre à niveau les composants R Server séparément à partir du moteur de base de données SQL Server, dans un processus appelé liaison. Par conséquent, la version de R que vous utilisez lorsque vous exécutez le code R dans SQL Server peut-être différer selon la version installée de SQL Server et que si vous avez migré le serveur vers la dernière version de R.

### <a name="determine-the-r-version"></a>Déterminer la version de R

Pour déterminer la version de R, le plus simple consiste à obtenir les propriétés d’exécution en exécutant une instruction telle que les éléments suivants :

```SQL
exec sp_execute_external_script
       @language = N'R'
       , @script = N'
# Transform R version properties to data.frame
OutputDataSet <- data.frame(
  property_name = c("R.version", "Revo.version"), 
  property_value = c(R.Version()$version.string, Revo.version$version.string),
  stringsAsFactors = FALSE)
# Retrieve properties like R.home, libPath & default packages
OutputDataSet <- rbind(OutputDataSet, data.frame(
  property_name = c("R.home", "libPaths", "defaultPackages"),
  property_value = c(R.home(), .libPaths(), paste(getOption("defaultPackages"), collapse=", ")),
  stringsAsFactors = FALSE)
)
'
WITH RESULT SETS ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));

```

> [!TIP] 
> Si R Services ne fonctionne pas, essayez d’exécuter uniquement la partie du script R à partir de l’interface RGui.

En dernier recours, vous pouvez ouvrir des fichiers sur le serveur pour déterminer la version installée. Pour ce faire, recherchez le fichier de rlauncher.config pour obtenir l’emplacement du runtime R et le répertoire de travail actuel. Nous vous recommandons de faire et ouvrir une copie du fichier afin que vous ne modifiez pas accidentellement des propriétés.

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name\MSSQL\Binn\rlauncher.config`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Binn\rlauncher.config`

Pour obtenir la version de R et les versions de RevoScaleR, ouvrez une invite de commandes R, ou ouvrez l’interface RGui qui est associé à l’instance.

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instancename>\R_SERVICES\bin\x64\RGui.exe`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES\bin\x64\RGui.exe`


La console R affiche les informations de version au démarrage. Par exemple, la version suivante représente la configuration par défaut pour SQL Server 2017 CTP 2.0 :

    *Microsoft R Open 3.3.3*
    
    *The enhanced R distribution from Microsoft*
    
    *Microsoft packages Copyright (C) 2017 Microsoft*
    
    *Loading Microsoft R Server packages, version 9.1.0.*


## <a name="python-versions"></a>Versions de Python

Il existe plusieurs façons d’obtenir la version de Python. La plus simple consiste à exécuter cette instruction à partir de Management Studio ou tout autre outil de requête SQL :

```SQL
-- Get Python runtime properties:
exec sp_execute_external_script
       @language = N'Python'
       , @script = N'
import sys
import pkg_resources
OutputDataSet = pandas.DataFrame(
                    {"property_name": ["Python.home", "Python.version", "Revo.version", "libpaths"],
                    "property_value": [sys.executable[:-10], sys.version, pkg_resources.get_distribution("revoscalepy").version, str(sys.path)]}
)
'
with WITH RESULT SETS (SQL keywords) ((PropertyName nvarchar(100), PropertyValue nvarchar(4000)));
```

Si la Machine Learning Services n’est pas en cours d’exécution, vous pouvez déterminer la version installée de Python en examinant le fichier pythonlauncher.config. Nous vous recommandons de faire et ouvrir une copie du fichier afin que vous ne modifiez pas accidentellement des propriétés.

1. Pour SQL Server 2017 uniquement : `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog\pythonlauncher.config `
2. Obtenir la valeur de **PYTHONHOME**.
3. Obtenir la valeur du répertoire de travail actuel.


> [!NOTE]
> Si vous avez installé les Python et R dans SQL Server 2017, le répertoire de travail et le pool de comptes de travail sont partagés pour les langues R et Python.

## <a name="are-multiple-instances-of-r-or-python-installed"></a>Plusieurs instances de R ou Python installé ?

Vérifiez si plusieurs copies des bibliothèques R est installé sur l’ordinateur. Cette duplication peut se produire si :

* Lors de l’installation, vous sélectionnez R Services (de-de base de données) et R Server (autonome). 
* Vous installez le Client Microsoft R en plus de SQL Server.
* Un autre ensemble de bibliothèques R a été installé à l’aide des outils R pour Visual Studio, R Studio, le Client Microsoft R ou un autre IDE R.
* L’ordinateur héberge plusieurs instances de SQL Server, et une seule instance utilise l’apprentissage.

Les mêmes conditions s’appliquent à Python.

Si vous trouvez que plusieurs bibliothèques, ou runtimes sont installés, assurez-vous que vous obtenez uniquement les erreurs associées aux runtimes Python ou R qui sont utilisées par l’instance de SQL Server.

## <a name="origin-of-errors"></a>Origine des erreurs

Les erreurs que vous voyez lorsque vous tentez d’exécuter le code R peuvent provenir d’une des sources suivantes :

- Moteur de base de données SQL Server, y compris la procédure stockée sp_execute_external_script
- Le serveur SQL Server approuvée Launchpad 
- Autres composants de l’infrastructure d’extensibilité, y compris les lanceurs R et Python et des processus satellites
- Base de données de fournisseurs, tels que Microsoft Open Connectivity (ODBC)
- Langage R

Lorsque vous travaillez avec le service pour la première fois, il peut être difficile de déterminer quels messages sont issus les services. Il est recommandé que vous capturez non seulement le texte du message exacte, mais le contexte dans lequel vous avez vu le message. Notez le logiciel client que vous utilisez pour exécuter le code machine learning :

- Vous utilisez Management Studio ? Une application externe ?
- Vous exécutez du code R dans un client distant, ou directement dans une procédure stockée ?

## <a name="sql-server-log-files"></a>Fichiers journaux SQL Server

Obtenir le plus récent journal des erreurs de SQL Server. L’ensemble complet de journaux d’erreurs se compose des fichiers à partir du répertoire de journal par défaut suivant :

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\Log\ExtensibilityLog`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.SQL2016\MSSQL\Log\ExtensibilityLog`

> [!NOTE] 
> Le nom exact des dossiers diffère selon le nom d’instance.


## <a name="errors-returned-by-spexecuteexternalscript"></a>Erreurs retournées par sp_execute_external_script

Obtenir le texte complet des erreurs sont retournées, si elle existe, lorsque vous exécutez la commande sp_execute_external_script. 

Pour supprimer les problèmes de R ou Python pas prises en compte, vous pouvez exécuter ce script, ce qui démarre le runtime R ou Python et transmet des données dans les deux sens.

**Pour R**

```sql
exec sp_execute_external_script @language =N'R',  
@script=N'OutputDataSet<-InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

**Pour Python**

```sql
exec sp_execute_external_script @language =N'Python',  
@script=N'OutputDataSet= InputDataSet',  
@input_data_1 =N'select 1 as hello'  
with result sets (([hello] int not null));  
go
```

## <a name="errors-generated-by-the-extensibility-framework"></a>Erreurs générées par l’infrastructure d’extensibilité

SQL Server génère des journaux distincts pour les exécutions de langage de script externe. Ces erreurs ne sont pas générés par le langage Python ou R. Elles sont générées à partir des composants d’extensibilité dans SQL Server, y compris linguistiques lanceurs et leurs processus satellite.

Vous pouvez obtenir ces journaux à partir d’emplacements par défaut suivants :

* SQL Server 2016
  
  `C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\MSSQL\Log\ExtensibilityLog`

* SQL Server 2017
  
  `C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\MSSQL\Log\ExtensibilityLog `

> [!NOTE] 
> Le nom de dossier exact diffère selon le nom d’instance. Selon votre configuration, le dossier peut être sur un autre lecteur.

Par exemple, les messages de journal suivantes sont liées à l’infrastructure d’extensibilité :

* *Échec de LogonUser pour l’utilisateur MSSQLSERVER01*
  
  Cela peut indiquer que les comptes de travail qui exécutent des scripts externes ne peuvent pas accéder à l’instance.

* *InitializePhysicalUsersPool a échoué* 
  
  Ce message peut signifier que vos paramètres de sécurité empêchent le programme d’installation à partir de la création du pool de comptes de travail qui sont nécessaires pour exécuter des scripts externes.

* *Échoué de l’initialisation du Gestionnaire de contexte de sécurité* 

* *Échoué de l’initialisation du Gestionnaire de sessions satellites*

## <a name="system-events"></a>Événements système

1. Ouvrez l’Observateur d’événements Windows et recherchez le **événement système** journal pour les messages qui contiennent la chaîne *Launchpad*. 
2. Ouvrez le fichier ExtLaunchErrorlog et recherchez la chaîne *ErrorCode*. Examinez le message associé avec le code d’erreur.

Par exemple, les messages suivants sont des erreurs système courantes liées à l’infrastructure d’extensibilité SQL Server : 

* *Le service Launchpad de SQL Server (MSSQLSERVER) a échoué car l’erreur suivante :  <text>*

* *Le service n’a pas répondu à la demande de démarrage ou de contrôle en temps voulu.* 

* *Un délai d’attente a été atteint (120000 en millisecondes) pendant l’attente pour le service Launchpad de SQL Server (MSSQLSERVER) pour se connecter.* 

## <a name="dump-files"></a>Fichiers de vidage

Si vous savez sur le débogage, vous pouvez utiliser les fichiers de vidage pour analyser une défaillance dans le Launchpad.

1. Recherchez le dossier qui contient les journaux d’amorçage d’installation de SQL Server. Par exemple, dans SQL Server 2016, le chemin d’accès par défaut est C:\Program Files\Microsoft SQL Server\130\Setup Bootstrap\Log.
2. Ouvrez le sous-dossier de journal d’amorçage est spécifique à l’extensibilité.
3. Si vous devez envoyer une demande de prise en charge, ajoutez tout le contenu de ce dossier dans un fichier compressé. Par exemple, C:\Program Files\Microsoft SQL Server\130\Setup Bootstrap\Log\LOG\ExtensibilityLog.
  
L’emplacement exact peut être différent sur votre système, et il peut être sur un lecteur autre que votre lecteur C. Veillez à obtenir les journaux pour l’instance où l’apprentissage est installé. 


## <a name="configuration-settings"></a>Paramètres de configuration

Cette section répertorie les composants supplémentaires ou des fournisseurs qui peuvent être une source d’erreurs lorsque vous exécutez des scripts R ou Python.

### <a name="what-network-protocols-are-available"></a>Les protocoles réseau sont disponibles ?

Machine Learning Services nécessite les protocoles réseau suivants pour la communication interne entre les composants d’extensibilité et pour la communication avec les clients externes de R ou Python.

* Canaux nommés
* TCP/IP

Ouvrez le Gestionnaire de Configuration SQL Server pour déterminer si un protocole est installé et, s’il est installé, afin de déterminer si elle est activée.

### <a name="security-configuration-and-permissions"></a>Autorisations et la configuration de sécurité

Pour les comptes de travail :

1. Dans le panneau de configuration, ouvrez **utilisateurs et groupes**, recherchez le groupe utilisé pour exécuter des tâches de script externe. Par défaut, le groupe est **SQLRUserGroup**.
2. Vérifiez que le groupe existe et qu’il contient le compte d’au moins un travail.
3. Dans SQL Server Management Studio, sélectionnez l’instance où sont exécutés les travaux R ou Python, sélectionnez **sécurité**, puis déterminez s’il existe une ouverture de session pour SQLRUserGroup.
4. Examinez les autorisations pour le groupe d’utilisateurs.

Pour les comptes d’utilisateur individuels :

1. Déterminer si l’instance prend en charge l’authentification en Mode mixte, uniquement les connexions SQL ou l’authentification Windows uniquement. Ce paramètre affecte votre R ou exigences de code Python.
2. Pour chaque utilisateur ayant besoin d’exécuter le code R, déterminez le niveau requis d’autorisations sur chaque base de données où les objets seront écrites à partir de R, les données seront accessibles ou objets seront créés.
3. Pour activer l’exécution du script, créer des rôles ou ajouter des utilisateurs aux rôles suivants, selon les besoins :

   - Tout sauf *db_owner*: nécessitent EXECUTE ANY EXTERNAL SCRIPT.
   - *db_datawriter*: pour écrire les résultats à partir de R ou Python. 
   - *db_ddladmin*: pour créer des objets. 
   - *db_datareader*: lire les données qui sont utilisées par le code R ou Python. 
4. Notez que si vous avez modifié les comptes de démarrage par défaut lors de l’installation de SQL Server 2016.
5. Si un utilisateur a besoin installer de nouveaux packages R ou utiliser des packages R qui ont été installées par d’autres utilisateurs, vous devrez peut-être activer la gestion des packages sur l’instance et lui attribuer des autorisations supplémentaires. Pour plus d’informations, consultez [activer ou désactiver la gestion des packages R](r\r-package-how-to-enable-or-disable.md).

### <a name="what-folders-are-subject-to-locking-by-antivirus-software"></a>Les dossiers qui sont soumises à verrouillage par un logiciel antivirus ?

Le logiciel antivirus peut verrouiller des dossiers, ce qui empêche le programme d’installation de l’apprentissage de fonctionnalités et l’exécution du script réussie. Déterminer si tous les dossiers dans l’arborescence de SQL Server sont soumis à une analyse antivirus.

Toutefois, lorsque plusieurs fonctions ou services sont installées sur une instance, il peut être difficile à énumérer tous les dossiers possibles qui sont utilisées par l’instance. Par exemple, lorsque de nouvelles fonctionnalités sont ajoutées, les nouveaux dossiers doivent être identifiés et exclus.

En outre, certaines fonctionnalités de créer des dossiers dynamiquement lors de l’exécution. Par exemple, les tables OLTP en mémoire, les procédures stockées et fonctions créent nouveaux répertoires lors de l’exécution. Souvent, ces noms de dossiers contiennent des GUID et ne peut pas être prédite. Le Launchpad approuvé de SQL Server crée des répertoires de travail pour R et Python script de travaux.

Car il ne seront peut-être pas possible d’exclure tous les dossiers qui sont requises par le processus SQL Server et de ses fonctionnalités, nous vous recommandons d’exclure l’arborescence entière du répertoire de l’instance de SQL Server.

### <a name="is-the-firewall-open-for-sql-server-does-the-instance-support-remote-connections"></a>Est le pare-feu ouvert pour SQL Server ? L’instance prend en charge les connexions à distance ?

1. Pour déterminer si SQL Server prend en charge les connexions à distance, consultez [configurer des connexions au serveur distant](../database-engine/configure-windows/view-or-configure-remote-server-connection-options-sql-server.md).

2. Déterminer si une règle de pare-feu a été créée pour SQL Server. Pour des raisons de sécurité, dans une installation par défaut, il ne peut pas possible pour un client R ou Python à distance pour se connecter à l’instance. Pour plus d’informations, consultez [résolution des problèmes de connexion à SQL Server](../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md).


## <a name="see-also"></a>Voir aussi

[Résoudre les problèmes d’apprentissage dans SQL Server](machine-learning-troubleshooting-faq.md)
