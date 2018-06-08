---
title: Composants de SQL Server pour prendre en charge R | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: fa29a924b34bbe5737a89f5b111c92053b62d36b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="components-in-sql-server-to-support-r"></a>Composants de SQL Server pour prendre en charge R
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans SQL Server 2016 et 2017, le moteur de base de données inclut des composants facultatifs qui prennent en charge d’extensibilité pour les langages de script externe, y compris R et Python. Prise en charge pour le langage R dans SQL Server 2016 ; prise en charge Python a été ajoutée dans SQL Server 2017 Machine Learning Services.

Cette rubrique décrit les nouveaux composants fonctionnent spécialement avec le langage R. Pour en savoir plus sur le fonctionnement de ces composants avec R open source, consultez [R interopérabilité](r-interoperability-in-sql-server.md)

## <a name="components-and-providers"></a>Composants et les fournisseurs

En plus de l’interpréteur de commandes qui charge R et exécute le code R, comme cela est décrit dans la vue d’ensemble de l’architecture, [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] inclut les composants supplémentaires suivants.

### <a name="launchpad"></a>Launchpad 

Le [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] est un service fourni par [!INCLUDE[ssCurrent_md](../../includes/sscurrent-md.md)] pour prendre en charge l’exécution de scripts externes, comme le fait que le service d’indexation et de requête de recherche en texte intégral lance un hôte distinct pour le traitement des requêtes de texte intégral.

Le service Launchpad démarre uniquement les lanceurs approuvés qui sont publiés par Microsoft, ou qui ont été certifiés par Microsoft comme répondant aux exigences de gestion des performances et des ressources. L’appellation les lanceurs spécifiques au langage est simple :

  + R - RLauncher.dll
  + Python - PythonLauncher.dll

Le service [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] s’exécute sous son propre compte d’utilisateur. Chaque processus satellite pour un runtime de langage spécifique hérite du compte d’utilisateur de Launchpad. Pour plus d’informations sur la configuration et le contexte de sécurité du Launchpad, consultez [vue d’ensemble de la sécurité](../../advanced-analytics/r/security-overview-sql-server-r.md).

### <a name="bxlserver-and-sql-satellite"></a>BxlServer et les satellites SQL

BxlServer est un fichier exécutable fourni par Microsoft qui gère la communication entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et le runtime R et également implémente les fonctions RevoScaleR. Il crée les objets de travail Windows utilisés pour contenir des sessions R, provisionne des dossiers de travail sécurisés pour chaque travail R et utilise SQL Satellite pour gérer le transfert de données entre R et [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

En effet, BxlServer est un complément de R qui fonctionne avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] pour prendre en charge l’intégration de R avec SQL Server. BXL signifie binaires Exchange language et fait référence à un format de données utilisé pour déplacer efficacement des données entre SQL Server et les processus externes tels que R. BxlServer.dll est également installé lorsque vous installez le Client Microsoft R ou Microsoft R Server.

SQL Satellite est une nouvelle API d’extensibilité dans SQL Server 2016, fournie par le moteur de base de données pour prendre en charge le code externe ou les runtimes externes implémentés à l’aide de C ou C++. BxlServer utilise SQL Satellite pour la communication avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

SQL Satellite utilise un format de données personnalisé et optimisé pour accélérer les transferts de données entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et les langages de script externes. Il effectue des conversions de type et il définit les schémas des jeux de données d’entrée et de sortie pendant les communications entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et R.

BxlServer utilise SQL Satellite pour les tâches suivantes :

  + Lecture des données d’entrée
  + Écriture des données de sortie
  + Obtention des arguments d’entrée
  + Écriture des arguments de sortie
  + Gestion des erreurs
  + Erreurs au client et la sortie standard de l’écriture

SQL Satellite peut être surveillé à l’aide d’événements étendus. Pour plus d’informations, consultez [Extended Events for SQL Server R Services](extended-events-for-sql-server-r-services.md) (Événements étendus pour SQL Server R Services).

## <a name="communication-channels-between-components"></a>Canaux de communication entre les composants

+ **TCP/IP**

    Par défaut, les communications internes entre [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et le Satellite SQL utiliser TCP/IP.

+ **Canaux nommés**

    Transport de données interne entre le service BxlServer et [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] via SQL Satellite utilise un format de données de propriétaires, compressée pour améliorer les performances. Les données entre la mémoire R et BxlServer sont échangées via des canaux nommés au format BXL.

+ **ODBC**

    Communications entre les clients de science des données externes et les [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] instance utilisent ODBC. Le compte utilisé pour l’envoi des travaux R à [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] doit avoir l’autorisation de se connecter à l’instance et l’autorisation d’exécuter des scripts externes. De plus, ce compte doit être autorisé à accéder à toutes les données utilisées par le travail, à écrire des données (par exemple, pour enregistrer les résultats dans une table) ou à créer des objets de base de données (par exemple, pour enregistrer des fonctions R dans une nouvelle procédure stockée).

    Quand [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] est utilisé comme contexte de calcul pour un travail R envoyé d’un client distant, et que la commande R doit récupérer des données d’une source externe, ODBC est utilisé pour l’écriture différée. [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] mappe l’identité de l’utilisateur qui émet la commande R à distance à l’identité de l’utilisateur sur l’instance actuelle, puis exécute la commande ODBC à l’aide des informations d’identification de cet utilisateur. La chaîne de connexion nécessaire pour effectuer cet appel ODBC est obtenue à partir du code client.

    Des appels ODBC supplémentaires peuvent être effectués à l’intérieur du script à l’aide du package **RODBC**. RODBC est un package R couramment utilisé pour accéder aux données dans des bases de données relationnelles. Toutefois, il est généralement moins rapide que les fournisseurs comparables utilisés par [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. De nombreux scripts R utilisent des appels au RODBC incorporés comme moyen de récupérer les jeux de données « secondaires » à utiliser dans les analyses. Par exemple, la procédure stockée qui forme un modèle peut définir une requête SQL pour obtenir les données d’apprentissage d’un modèle, mais utiliser un appel RODBC incorporé pour obtenir d’autres facteurs, effectuer des recherches ou obtenir de nouvelles données de sources externes telles que des fichiers texte ou Excel.

    Le code suivant illustre un appel RODBC incorporé dans un script R :

    ```R
    library(RODBC);
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    dbhandle <- odbcDriverConnect(connStr)
    OutputDataSet <- sqlQuery(dbhandle, "select * from table_name");
    ```

+ **D’autres protocoles**

    Processus devront peut-être travailler dans « blocs » ou transférer des données à un client à distance peuvent également utiliser le. Format XDF pris en charge par le transfert de données Microsoft R. réel est via des objets BLOB codé.

## <a name="interaction-of-components"></a>Interaction des composants

L’architecture des composants décrite ci-dessus permet de garantir que le code R open source peut fonctionner « en l’état », tout en améliorant nettement les performances du code exécuté sur un ordinateur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. Le mécanisme d’interaction des composants avec le runtime R et le moteur de base de données [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] diffère légèrement selon le mode de lancement du code R. Les principaux scénarios sont résumés dans cette section.

### <a name="r-scripts-executed-from-sql-server-in-database"></a>Scripts R exécutés à partir des données dans SQL Server

Le code R lancé de « l’intérieur » de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] est exécuté en appelant une procédure stockée. Par conséquent, toute application qui peut appeler une procédure stockée peut lancer l’exécution du code R.  [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] gère ensuite l’exécution du code R comme indiqué dans le diagramme suivant.

![rsql_indb780-01](media/script_in-db-r.png)

1. Une demande du runtime R est indiquée par le paramètre _@language='R'_ passé à la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] envoie cette demande au service Launchpad.
2. Le service Launchpad démarre le lanceur approprié, RLauncher dans ce cas.
3. RLauncher démarre le processus R externe.
4. BxlServer se coordonne avec le runtime R pour gérer les échanges de données avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et le stockage des résultats du travail.
5. SQL Satellite gère les communications sur les tâches associées et les processus avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
6. BxlServer utilise SQL Satellite pour communiquer l’état et les résultats à [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
7. [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] obtient les résultats, puis ferme les processus et tâches associés.

### <a name="r-scripts-executed-from-a-remote-client"></a>Scripts R exécutés à partir d’un client distant

Lors de la connexion à partir d’un client de science des données à distance qui prend en charge de Microsoft R, vous pouvez exécuter des fonctions R dans le contexte de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] en utilisant les fonctions RevoScaleR. Ce flux de travail, qui est différent du précédent, est résumé dans le diagramme suivant.

![rsql_fromR2db-01](media/remote-sqlcc-from-r2.png)

1. Pour les fonctions RevoScaleR, le runtime R appelle une fonction de liaison qui à son tour appelle BxlServer.
2. BxlServer est fourni avec Microsoft R et s’exécute dans un processus distinct du runtime R.
3. BxlServer détermine la cible de la connexion et démarre une connexion à l’aide d’ODBC, en passant les informations d’identification fournies dans la chaîne de connexion de l’objet de source de données R.
4. BxlServer ouvre une connexion à l’instance [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
5. Pour un appel de R, Launchpad service est appelé, qui est activé démarre le service de lancement approprié, RLauncher. Le traitement du code R est ensuite semblable au processus d’exécution du code R à partir de T-SQL.
6. RLauncher effectue un appel à l’instance du runtime R qui est installé sur l’ordinateur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
7. Les résultats sont retournés à BxlServer.
8. SQL Satellite gère la communication avec [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et le nettoyage des objets de travail associés.
9. [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] renvoie les résultats au client.

## <a name="next-steps"></a>Étapes suivantes

[Vue d’ensemble de l’architecture](architecture-overview-sql-server-r.md)

[Vue d’ensemble de la sécurité](security-overview-sql-server-r.md)

[Considérations sur la sécurité](security-considerations-for-the-r-runtime-in-sql-server.md)
