---
title: Composants pour l’intégration avec SQL Server Machine Learning Python | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: f00735c78b59a9eec41f8ef4ae77fb6accf013d1
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="components-in-sql-server-to-support-python-integration"></a>Composants de SQL Server pour prendre en charge l’intégration Python
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

À compter de SQL Server 2017, Machine Learning Services prend en charge Python comme un langage externe qui peut être exécuté à partir de T-SQL ou exécutés à distance à l’aide de SQL Server en tant que le contexte de calcul.

Cette rubrique décrit les composants de SQL Server 2017 qui prennent en charge d’extensibilité en général et le langage Python spécifiquement.

## <a name="sql-server-components-and-providers"></a>Fournisseurs et les composants de SQL Server

Pour configurer 2017 du serveur SQL pour autoriser l’exécution du script Python est un processus en plusieurs étapes.

1. Installer la fonctionnalité d’extensibilité.
2. Activer la fonctionnalité de l’exécution du script externe.
3. Redémarrez le service de moteur de base de données.

Étapes supplémentaires peuvent être requises pour prendre en charge l’exécution du script à distance.

Pour plus d’informations, consultez [installer SQL Server 2017 Machine Learning Services (de-de base de données)](../install/sql-machine-learning-services-windows-install.md).

### <a name="launchpad"></a>Launchpad

Le Launchpad approuvé de SQL Server est un service introduit dans SQL Server 2016 qui gère et exécute des scripts externes, comme le fait que le service d’indexation et de requête de recherche en texte intégral lance un hôte distinct pour le traitement des requêtes de texte intégral.

Le service Launchpad peut démarrer seulement approuvés lanceurs qui sont publiés par Microsoft, ou qui ont été certifiés par Microsoft en tant que les demandes de réunion pour la gestion des ressources et des performances.

+ Prend en charge de SQL Server 2017 R et Python 3.5
+ SQL Server 2016 prend en charge R

Le service [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] s’exécute sous son propre compte d’utilisateur.

> [!TIP]
> Si vous modifiez le compte qui exécute le Launchpad, veillez à le faire à l’aide du Gestionnaire de Configuration SQL Server pour vous assurer que les modifications sont écrites dans les fichiers associés.

Pour exécuter des tâches dans un langage pris en charge spécifique, Launchpad Obtient un compte sécurisé de travail du pool et démarre un processus satellite pour gérer le runtime externe :

+ RLauncher.dll pour le langage R
+ Pythonlauncher.dll pour Python 3.5

Chaque processus satellite hérite le compte d’utilisateur de Launchpad et utilise ce compte de processus de travail pour la durée de l’exécution du script. Si le script Python utilise les traitements parallèles, ils sont créés sous le compte du même travail.

Pour plus d’informations sur le contexte de sécurité du Launchpad, consultez [sécurité](security-overview-sql-server-python-services.md).

### <a name="bxlserver-and-sql-satellite"></a>BxlServer et les satellites SQL

Si vous exécutez [Process Explorer](https://technet.microsoft.com/sysinternals/processexplorer.aspx) pendant l’exécution d’un travail de Python, vous pouvez voir une ou plusieurs instances de BxlServer.

**BxlServer** est un fichier exécutable fourni par Microsoft qui gère la communication entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et Python (ou R). Il crée les objets de tâche Windows qui sont utilisés pour contenir les sessions de script externe, dispositions sécurisé dossiers de travail pour chaque tâche de script externe et utilise SQL Satellite pour gérer le transfert de données entre le runtime externe et [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

En effet, BxlServer est un complément Python qui fonctionne avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] pour transférer des données et de gérer des tâches. BXL signifie binaires Exchange language et fait référence à un format de données utilisé pour déplacer efficacement des données entre SQL Server et les processus externes. BxlServer est également une partie importante de Microsoft R Client et Microsoft R Server.

**SQL Satellite** est une API d’extensibilité inclus dans le moteur de base de données à partir de SQL Server 2016, qui prend en charge le code externe ou les runtimes externes implémentés à l’aide de C ou C++.

BxlServer utilise SQL Satellite pour les tâches suivantes :

+ Lecture des données d’entrée
+ Écriture des données de sortie
+ Obtention des arguments d’entrée
+ Écriture des arguments de sortie
+ Gestion des erreurs
+ Réécriture de STDOUT et de STDERR sur le client

Satellite de SQL utilise un format de données personnalisé qui est optimisé pour le transfert de données rapides entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et les langages de script externe. Il effectue les conversions de type et définit les schémas des jeux de données d’entrée et de sortie pendant les communications entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et l’exécution du script externe.

Le Satellite SQL peut être analysé à l’aide de windows (xEvents) d’événements étendus. Pour plus d’informations, consultez [événements étendus pour R](../../advanced-analytics/r/extended-events-for-sql-server-r-services.md).

## <a name="communication-channels-between-components"></a>Canaux de communication entre les composants

+ **TCP/IP**

  Par défaut, les communications internes entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et le Satellite SQL utiliser TCP/IP.

+ **Canaux nommés**

  Le transport des données internes entre BxlServer et [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] par le biais de SQL Satellite utilise un format de données propriétaire et compressé pour améliorer les performances. Au format BXL, à l’aide de canaux nommés, les données sont échangées entre les Python et BxlServer.

+ **ODBC**

  Communications entre les clients de science des données externes et les [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] instance utilisent ODBC. Le compte qui envoie le script des travaux à [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] doit avoir les autorisations pour se connecter à l’instance et pour exécuter des scripts externes.

  En outre, selon la tâche, le compte peut-être ces autorisations :

  + Lecture de données utilisée par le travail
  + Écrire des données dans des tables : par exemple, lorsque l’enregistrement des résultats dans une table
  + Créer des objets de base de données : par exemple, si l’enregistrement de script externe en tant que partie d’une nouvelle procédure stockée.

  Lorsque [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] est utilisé comme contexte de calcul pour Python script exécuté à partir d’un client distant, et le fichier exécutable Python doit récupérer des données à partir d’une source externe, ODBC est utilisé pour l’écriture différée. [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] mappe l’identité de l’utilisateur qui émet la commande à distance à l’identité de l’utilisateur sur l’instance actuelle et exécute la commande ODBC à l’aide des informations d’identification de l’utilisateur. La chaîne de connexion nécessaire pour effectuer cet appel ODBC est obtenue à partir du code client.

## <a name="interaction-of-components"></a>Interaction des composants

Les diagrammes suivants illustrent l’interaction des composants SQL Server avec le runtime Python dans chacun des scénarios pris en charge : l’exécution du script de base de données et à distance en cours d’exécution à partir d’un terminal de Python, à l’aide d’un contexte de calcul de SQL Server.

### <a name="python-scripts-executed-in-database"></a>Python scripts exécutés dans la base de données

Lorsque vous exécutez Python « dans » [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], vous devez encapsuler le script Python à l’intérieur d’une procédure stockée spéciale, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Une fois que le script a été incorporé dans la procédure stockée, toute application qui peut appeler une procédure stockée peut initier l’exécution du code Python.  Par la suite [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] gère l’exécution de code, comme indiqué dans le diagramme suivant.

![script de base de données python](../../advanced-analytics/python/media/script-in-db-python2.png)

1. Une demande pour le runtime Python est indiquée par le paramètre `@language='Python'` passé à la procédure stockée. SQL Server envoie cette demande au service Launchpad.
2. Le service Launchpad démarre le service de lancement approprié ; Dans ce cas, PythonLauncher.
3. PythonLauncher démarre le processus de Python35 externe.
4. BxlServer coordonne avec le runtime Python pour gérer les échanges de données et le stockage des résultats du travail.
5. SQL Satellite gère les communications sur les tâches associées et les processus avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
6. BxlServer utilise SQL Satellite pour communiquer l’état et les résultats à [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
7. [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] obtient les résultats, puis ferme les processus et tâches associés.

### <a name="python-scripts-executed-from-a-remote-client"></a>Scripts Python exécutés à partir d’un client distant

Vous pouvez exécuter des scripts Python à partir d’un ordinateur distant, par exemple un ordinateur portable et les exécuter dans le contexte de l’ordinateur SQl Server, si ces conditions sont remplies :

+ Vous concevez les scripts de manière appropriée
+ L’ordinateur distant a installé les bibliothèques d’extensibilité qui sont utilisés par les Services de Machine Learning. Le [revoscalepy](what-is-revoscalepy.md) package est requis pour utiliser les contextes de calcul à distance.

Le diagramme suivant résume le flux de travail global lorsque les scripts sont envoyées à partir d’un ordinateur distant.

![sqlcc à distance à partir de python](../../advanced-analytics/python/media/remote-sqlcc-from-python3.png)

1. Pour les fonctions qui sont prises en charge **revoscalepy**, le runtime Python appelle une fonction de liaison, qui à son tour appelle BxlServer.
2. BxlServer est inclus avec Machine Learning Services (de-de base de données) et s’exécute dans un processus séparé du runtime Python.
3. BxlServer détermine la cible de la connexion et établit une connexion à l’aide d’ODBC, en passant les informations d’identification fournies dans le cadre de la chaîne de connexion dans le script Python.
4. BxlServer ouvre une connexion à l’instance [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
5. Lorsqu’un runtime de script externe est appelé, le service Launchpad est appelé, ce qui lance ensuite le service de lancement approprié : dans ce cas, PythonLauncher.dll. Par la suite, le traitement de code Python est géré dans un flux de travail similaire à celle lorsque le code Python est appelé à partir d’une procédure stockée T-SQL.
6. PythonLauncher effectue un appel à l’instance de Python qui est installé sur le [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ordinateur.
7. Les résultats sont retournés à BxlServer.
8. SQL Satellite gère la communication avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et le nettoyage des objets de travail associés.
9. [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] renvoie les résultats au client.

## <a name="next-steps"></a>Étapes suivantes

[Présentation de l’architecture pour Python dans SQL Server](architecture-overview-sql-server-python.md)
