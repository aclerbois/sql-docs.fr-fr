---
title: Synchronisation des package R pour SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 5aa19e54917a421567c5ede2013e019de609d8b6
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585225"
---
# <a name="r-package-synchronization-for-sql-server"></a>Synchronisation des package R pour SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

La version de RevoScaleR inclus dans SQL Server 2017 inclut la possibilité de synchroniser les collections de packages R entre le système de fichiers et l’instance et de base de données où les packages sont utilisés.

Cette fonctionnalité a été fournie pour le rendre plus facile de sauvegarder des regroupements de package de R associés aux bases de données SQL Server. À l’aide de cette fonctionnalité, un administrateur peut le restaurer non seulement de la base de données, mais tous les packages R qui ont été utilisés par les spécialistes de données dans cette base de données.

Cet article décrit la fonctionnalité de synchronisation de package et comment utiliser le [rxSyncPackages](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsyncpackages) (fonction) pour effectuer les tâches suivantes :

+ Synchroniser une liste des packages pour un ensemble de base de données SQL Server

+ Synchroniser les packages utilisés par un utilisateur individuel, ou par un groupe d’utilisateurs

+ Si un utilisateur se déplace vers un autre serveur SQL Server, vous pouvez effectuer une sauvegarde de base de données de l’utilisateur et restaurez-la sur le nouveau serveur, et les packages de l’utilisateur seront installés sur le système de fichiers sur le nouveau serveur, comme requis par R.

Par exemple, vous pouvez utiliser la synchronisation de package dans ces scénarios :

+ L’administrateur a restauré une instance de SQL Server à un nouvel ordinateur et demande aux utilisateurs de se connecter à partir de leurs clients R et exécuter `rxSyncPackages` pour actualiser et restaurer les packages.

+ Vous pensez que d’un package de R sur le système de fichiers est endommagé, vous exécuter `rxSyncPackages` sur le serveur SQL Server.

## <a name="requirements"></a>Spécifications

Avant que vous pouvez utiliser la synchronisation de package, vous devez disposer de la version appropriée de Microsoft R ou Machine Learning Server. Cette fonctionnalité est fournie dans Microsoft R version 9.1.0 ou version ultérieure. 

Vous devez également activer le [fonctionnalité de gestion de package](r-package-how-to-enable-or-disable.md) sur le serveur.

### <a name="determine-whether-your-server-supports-package-management"></a>Déterminer si votre serveur prend en charge la gestion des packages

Cette fonctionnalité est disponible dans SQL Server 2017 CTP 2 ou version ultérieure.

Vous pouvez ajouter cette fonctionnalité à une instance de SQL Server 2016 par la mise à niveau de l’instance pour utiliser la dernière version de Microsoft R. Pour plus d’informations, consultez [SqlBindR.exe utilisé pour mettre à niveau de SQL Server R Services](use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md).

### <a name="enable-the-package-management-feature"></a>Activer la fonctionnalité de gestion de package

Pour utiliser la synchronisation de package requiert l’activation de la nouvelle fonctionnalité de gestion de package sur l’instance de SQL Server et sur les bases de données individuelles. Pour plus d’informations, consultez [activer ou désactiver la gestion des packages pour SQL Server](r-package-how-to-enable-or-disable.md).

1. L’administrateur du serveur Active la fonctionnalité de l’instance de SQL Server.
2. Pour chaque base de données, l’administrateur accorde aux utilisateurs individuels d’installer ou de partager des packages R, à l’aide de rôles de base de données.

Dans ce cas, vous pouvez utiliser les fonctions RevoScaleR, tel que [rxInstallPackages](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxinstallpackages) pour installer les packages dans une base de données.  Pour plus d’informations sur les utilisateurs et les packages qu’ils peuvent utiliser sont stockés dans l’instance de SQL Server. 

Lorsque vous ajoutez un nouveau package à l’aide des fonctions de gestion de package, les deux enregistrements dans SQL Server et le système de fichiers sont mis à jour. Ces informations peuvent être utilisées pour restaurer les informations de package pour la base de données entière.

### <a name="permissions"></a>Autorisations

+ La personne qui exécute la fonction de synchronisation du package doit être une entité sur l’instance de SQL Server et de la base de données qui se trouvent les packages de sécurité.

+ L’appelant de la fonction doit être un membre d’un de ces rôles de gestion de package : **partagé rpkgs** ou **rpkgs-privée**.

+ Pour synchroniser les packages marqués comme **partagé**, la personne qui exécute la fonction doit être membre la **rpkgs partagé** rôle et les packages sont déplacés doivent avoir été installé dans une bibliothèque partagée étendue.

+ Pour synchroniser les packages marqués comme **privé**, soit le propriétaire du package ou l’administrateur doit exécuter la fonction et les packages doivent être privées.

+ Pour synchroniser des packages pour le compte d’autres utilisateurs, le propriétaire doit être b membre de la **db_owner** rôle de base de données.

## <a name="how-package-synchronization-works"></a>Comment fonctionne la synchronisation de package

Pour utiliser la synchronisation de package, appelez [rxSyncPackages](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxsyncpackages), qui est une nouvelle fonction dans [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler). 

Pour chaque appel à `rxSyncPackages`, vous devez spécifier une instance de SQL Server et la base de données. Ensuite, affiche les packages à synchroniser, ou spécifiez la portée du package.

1. Créer le contexte de calcul de SQL Server à l’aide de la `RxInSqlServer` (fonction). Si vous ne spécifiez pas un contexte de calcul, le contexte de calcul actuel est utilisé.

2. Indiquez le nom d’une base de données sur l’instance dans le contexte de calcul spécifiée. Les packages sont synchronisés par base de données.

3. Spécifiez les packages de la synchronisation à l’aide de l’argument de l’étendue.

    Si vous utilisez **privé** étendue, seuls les packages appartenant au propriétaire spécifié sont synchronisées. Si vous spécifiez **partagé** étendue, tous les packages non privée dans la base de données sont synchronisées. 
    
    Si vous exécutez la fonction sans spécifier soit **privé** ou **partagé** étendue, tous les packages sont synchronisées.

4. Si la commande réussit, les packages existants dans le système de fichiers sont ajoutés à la base de données avec l’étendue spécifiée et le propriétaire.

    Si le système de fichiers est endommagé, les packages sont restaurés en fonction de la liste mis à jour dans la base de données.

    Si la fonctionnalité de gestion de package n’est pas disponible sur la base de données cible, une erreur est générée : « la fonctionnalité de gestion de package n’est pas activée sur le serveur SQL Server ou la version est trop ancienne »

### <a name="example-1-synchronize-all-package-by-database"></a>Exemple 1. Synchroniser tous les packages par base de données

Cet exemple obtient les nouveaux packages à partir du système de fichiers local et installe les packages dans la base de données [TestDB]. Aucun propriétaire n’étant spécifique, la liste inclut tous les packages qui ont été installés pour les étendues privés et partagés.

```R
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

### <a name="example-2-restrict-synchronized-packages-by-scope"></a>Exemple 2. Limiter les packages synchronisés par l’étendue

Les exemples suivants synchroniser uniquement les packages dans l’étendue spécifiée.

```R
#Shared scope
rxSyncPackages(computeContext=computeContext, scope="shared", verbose=TRUE)

#Private scope
rxSyncPackages(computeContext=computeContext, scope="private", verbose=TRUE)
```

### <a name="example-3-restrict-synchronized-packages-by-owner"></a>Exemple 3. Limiter les packages synchronisés par propriétaire

L’exemple suivant montre comment synchroniser uniquement les packages qui ont été installés pour un utilisateur spécifique. Dans cet exemple, l’utilisateur est identifié par le nom de connexion SQL, *user1*.

```R
rxSyncPackages(computeContext=computeContext, scope="private", owner = "user1", verbose=TRUE))
```

## <a name="related-resources"></a>Ressources connexes

[Gestion des packages R pour SQL Server](install-additional-r-packages-on-sql-server.md)
