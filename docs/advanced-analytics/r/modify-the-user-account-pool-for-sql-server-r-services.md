---
title: Modifier le pool de comptes d’utilisateur pour l’apprentissage de SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 77b84e3117b0a1366f3d0b5f9d74802d938bc86b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="modify-the-user-account-pool-for-sql-server-machine-learning"></a>Modifier le pool de comptes d’utilisateur pour l’apprentissage de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans le cadre du processus d’installation de [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)], un *pool de comptes d’utilisateurs* Windows est créé pour prendre en charge l’exécution de tâches par le service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]. L’objectif de ces comptes de travail est pour isoler l’exécution simultanée de scripts externes par différents utilisateurs SQL.

Cet article décrit la configuration par défaut, la sécurité et la capacité pour les comptes de travail et comment modifier la configuration par défaut.

**S’applique à :** [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)], [!INCLUDE[sscurrent-md](../../includes/sscurrent-md.md)] [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]

## <a name="worker-accounts-used-for-external-script-execution"></a>Comptes de travail utilisés pour l’exécution du script externe

Le groupe de compte Windows est créé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le programme d’installation pour chaque instance sur lequel machine learning est installé et activé.

-   Dans une instance par défaut, le nom du groupe est **SQLRUserGroup**. Le nom est le même que vous utilisiez R, Python ou les deux.
-   Dans une instance nommée, le nom du groupe par défaut a pour suffixe le nom de l’instance (par exemple, **SQLRUserGroupMyInstanceName**).

Par défaut, le pool de comptes d’utilisateurs contient 20 comptes d’utilisateurs. Dans la plupart des cas, 20 est suffisante pour prendre en charge les tâches d’apprentissage automatique, mais vous pouvez modifier le nombre de comptes. Le nombre maximal de comptes est 100.
-  Dans une instance par défaut, les comptes individuels sont nommés **MSSQLSERVER01** à **MSSQLSERVER20**.
-   Pour une instance nommée, les comptes individuels sont nommés en fonction du nom de l’instance (par exemple, **MyInstanceName01** à **MyInstanceName20**).

Si plusieurs instances utilise l’apprentissage, l’ordinateur possède plusieurs groupes d’utilisateurs. Un groupe ne peut pas être partagé entre les instances.

### <a name = "HowToChangeGroup"> </a>Comment modifier le nombre de comptes de travail

Pour modifier le nombre d’utilisateurs dans le pool de comptes, vous devez modifier les propriétés du service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)], comme décrit ci-dessous.

Les mots de passe associés à chaque compte d’utilisateur sont générés de manière aléatoire, mais vous pouvez les modifier une fois les comptes créés.

1. Ouvrez le Gestionnaire de configuration SQL Server, puis sélectionnez **SQL Server Services**.
2. Double-cliquez sur le service SQL Server Launchpad et arrêtez-le s’il est en cours d’exécution.
3.  Sous l’onglet **Service**, vérifiez que le mode de démarrage Automatique est sélectionné. Scripts externes ne peut pas démarrer lorsque la zone de lancement n’est pas en cours d’exécution.
4.  Cliquez sur l’onglet **Avancé** et, si nécessaire, modifiez le **Nombre d’utilisateurs externes**. Ce paramètre contrôle le nombre d’utilisateurs différents SQL peut exécuter script externe simultanément des sessions. La valeur par défaut est 20 comptes. Le nombre maximal d’utilisateurs est 100.
5. Vous pouvez éventuellement affecter à l’option **Réinitialiser le mot de passe des utilisateurs externes** la valeur _Oui_ si une stratégie exigeant le changement périodique des mots de passe est en place dans votre organisation. Cette opération regénère les mots de passe chiffrés que gère Launchpad pour les comptes d’utilisateurs. Pour plus d’informations, consultez [Appliquer la stratégie des mots de passe](#bkmk_EnforcePolicy).
6.  Redémarrez le service Launchpad.

## <a name="managing-machine-learning-workloads"></a>Gestion des charges de travail machine learning

Le nombre de comptes dans ce pool détermine le nombre de sessions de script externe peut être actif simultanément.  Par défaut, 20 comptes sont créés, ce qui signifie que 20 différents utilisateurs pouvant avez R ou Python sessions actives en même temps. Vous pouvez augmenter le nombre de comptes de travail, si vous prévoyez d’exécuter des scripts simultanés plus de 20.

Lorsque le même utilisateur exécute plusieurs scripts externes simultanément, toutes les sessions s’utiliseront le même compte de processus de travail. Par exemple, un seul utilisateur peut avoir 100 différents scripts R s’exécutant simultanément, en tant que permettent les ressources, mais tous les scripts sont exécute à l’aide d’un compte de travail unique.

Le nombre de comptes de travail que vous pouvez prendre en charge et le nombre de sessions simultanées autorisées par tout utilisateur unique peut exécuter, est limitée uniquement par les ressources du serveur. En général, la mémoire est le premier goulot d’étranglement que vous rencontrez quand vous utilisez le runtime R.

Les ressources qui peuvent être utilisées par les scripts Python ou R sont régies par SQL Server. Nous vous recommandons de surveiller l’utilisation des ressources à l’aide de vues de gestion dynamique de SQL Server, ou d’examiner les compteurs de performances sur l’objet de traitement Windows associé, puis d’ajuster l’utilisation de la mémoire du serveur en conséquence. Si vous disposez de SQL Server Enterprise Edition, vous pouvez allouer les ressources utilisées pour l’exécution de scripts externes en configurant un [pool de ressources externes](../../advanced-analytics/r-services/how-to-create-a-resource-pool-for-r.md).

Pour plus d’informations sur la gestion de machine learning de capacité de la tâche, voir les articles suivants :

- [Configuration de SQL Server pour R Services](../../advanced-analytics/r/sql-server-configuration-r-services.md)
-  [Étude de cas de performances pour R Services](../../advanced-analytics/r/performance-case-study-r-services.md)

## <a name="security"></a>Sécurité

Chaque groupe d’utilisateurs est associé au service [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] sur une instance spécifique et ne peut pas prendre en charge les travaux R qui s’exécutent sur d’autres instances.

Pour chaque compte de travail, quand la session est active, un dossier temporaire est créé pour stocker les objets de script, les résultats intermédiaires et d’autres informations utilisées par R et SQL Server pendant l’exécution du script R. Ces fichiers de travail, situés sous le dossier ExtensibilityData, sont uniquement accessibles aux administrateurs et sont nettoyés par SQL Server une fois le script terminé. 

Pour plus d’informations, consultez [Vue d’ensemble de la sécurité](../../advanced-analytics/r-services/security-overview-sql-server-r.md).

### <a name="bkmk_EnforcePolicy"></a>Application de la stratégie de mot de passe

Si votre organisation a mis en place une stratégie exigeant le changement périodique des mots de passe, vous devrez peut-être forcer le service Launchpad à regénérer les mots de passe chiffrés qu’il gère pour ses comptes de travail.  

Pour activer ce paramètre et forcer l’actualisation des mots de passe, ouvrez le volet **Propriétés** du service Launchpad dans le Gestionnaire de configuration SQL Server, cliquez sur **Avancé**, puis affectez à **Réinitialiser le mot de passe des utilisateurs externes** la valeur **Oui**. Quand vous appliquez ce changement, les mots de passe sont immédiatement regénérés pour tous les comptes d’utilisateurs. Pour utiliser le script R après ce changement, vous devez redémarrer le service Launchpad. À ce stade, les mots de passe qui viennent d’être générés sont lus. 

Pour réinitialiser les mots de passe à intervalles réguliers, vous pouvez définir cet indicateur manuellement ou utiliser un script.

### <a name="additional-permission-required-to-support-remote-compute-contexts"></a>Autres autorisations nécessaires pour prendre en charge les contextes de calcul à distance

Par défaut, le groupe de comptes de travail R **ne dispose pas** d’autorisations de connexion sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle il est associé. Cela peut poser problème si des utilisateurs R se connectent à SQL Server à partir d’un client distant pour exécuter des scripts R, ou si un script utilise ODBC pour obtenir des données supplémentaires. 

Pour vérifier que ces scénarios sont pris en charge, l’administrateur de base de données doit accorder au groupe de comptes de travail R l’autorisation de se connecter à l’instance de SQL Server sur laquelle le script R sera exécuté (autorisations **Se connecter à**). Cela est appelé *l’authentification implicite*et permet à SQL Server exécuter des scripts R à l’aide des informations d’identification de l’utilisateur distant.

> [!NOTE]
> Cette limitation ne s’applique pas si vous utilisez des connexions SQL pour exécuter des scripts R à partir d’une station de travail distante, car les informations d’identification de connexion SQL sont transmises explicitement du client R vers l’instance SQL Server, puis vers ODBC.


### <a name="how-to-enable-implied-authentication"></a>Comment activer l’authentification implicite

1. Ouvrez SQL Server Management Studio en tant qu’administrateur sur l’instance où vous allez exécuter le code R ou Python.

2. Exécutez le script suivant. Veillez à modifier le nom du groupe d’utilisateurs, si vous avez modifié la valeur par défaut, ainsi que le nom de l’ordinateur et le nom de l’instance.

    ```sql
    USE [master]
    GO
    
    CREATE LOGIN [computername\SQLRUserGroup] FROM WINDOWS WITH DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[language]
    GO
    ````

