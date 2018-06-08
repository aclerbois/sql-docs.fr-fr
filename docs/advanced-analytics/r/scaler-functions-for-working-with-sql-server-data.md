---
title: Les fonctions RevoScaleR pour travailler avec des données SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: d97a75cdc7da6a05847373b259ef562353d578ca
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="revoscaler-functions-for-working-with-sql-server-data"></a>Fonctions RevoScaleR pour travailler avec des données SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cette rubrique fournit une vue d’ensemble des principales fonctions fournis dans RevoScaleR pour travailler avec des données SQL Server.

Pour obtenir une liste complète des fonctions ScaleR et comment les utiliser, consultez le [Microsoft R Server](https://docs.microsoft.com/r-server/r-reference/revoscaler/revoscaler) référence.

## <a name="create-sql-server-data-sources"></a>Créer des sources de données SQL Server

Les fonctions suivantes permettent de définir une source de données [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. Un objet source de données est un conteneur qui spécifie une chaîne de connexion assortie du jeu de données de votre choix, défini sous la forme d’une table, d’une vue ou d’une requête. Les appels aux procédures stockées ne sont pas pris en charge.

+ [RxSqlServerData](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxsqlserverdata) -définir une [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] objet source de données.

+ [RxOdbcData](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxodbcdata) -créer des objets de données pour les autres bases de données ODBC. 

## <a name="perform-ddl-statements"></a>Exécuter des instructions DDL

Vous pouvez exécuter des instructions DDL à partir de R, si vous avez les autorisations nécessaires sur l’instance et de la base de données. Les fonctions suivantes utilisent des appels ODBC pour exécuter des instructions DDL ou de récupérer le schéma de base de données.

+ `rxSqlServerTableExists` et [rxSqlServerDropTable](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxsqlserverdroptable) -supprimer un [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] de table ou de vérifier l’existence d’une table de base de données ou d’un objet

+ [rxExecuteSQLDDL](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxexecutesqlddl) -exécuter une commande de langage de définition de données (DDL) qui définit ou manipule les objets de base de données. Cette fonction ne peut pas retourner des données et est utilisée uniquement pour récupérer ou modifier le schéma d’objet ou les métadonnées.

## <a name="define-or-manage-compute-contexts"></a>Définir ou gérer les contextes de calcul

Les fonctions suivantes permettent de définir un nouveau contexte de calcul, d’en changer ou d’identifier le contexte de calcul actif.

+ [RxComputeContext](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxcomputecontext) : crée un contexte de calcul.

+ [rxInSqlServer](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxinsqlserver) : génère un contexte de calcul SQL Server qui permet aux fonctions **ScaleR** de s’exécuter dans SQL Server R Services. Ce contexte de calcul est actuellement pris en charge uniquement pour les instances SQL Server sur Windows.

+ `rxGetComputeContext` et [rxSetComputeContext](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxgetcomputecontext) - obtenir ou définir le contexte de calcul active.

## <a name="move-data-and-transform-data"></a>Déplacer les données et de transformer des données

Après avoir créé un objet de source de données, vous pouvez utiliser l’objet pour charger des données, de transformer des données ou d’écrire les nouvelles données vers la destination spécifiée. Selon la taille des données contenues dans la source, vous pouvez aussi définir la taille de lot dans la source de données et déplacer des données en blocs.

+ [méthodes de rxOpen](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxopen-methods) - Vérifiez si une source de données est disponible, ouvrir ou fermer une source de données, lire des données à partir d’une source de, écrire des données dans la cible et fermer une source de données

+ [rxImport](https://docs.microsoft.com/r-server/r-reference/revoscaler/rximport) -déplacer des données d’une source de données dans le stockage de fichiers ou dans une trame de données.

+ [rxDataStep](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxdatastep) -transformer les données lors de son déplacement entre des sources de données.

Les fonctions suivantes peuvent être utilisées pour créer un magasin de données local dans le format XDF. Ce fichier peut être utile quand la quantité des données utilisées est trop importante pour être transférée en seul lot à partir de la base de données ou pour tenir dans la mémoire.

Par exemple, si vous déplacez régulièrement des grandes quantités de données à partir d’une base de données vers une station de travail locale, au lieu de la requête la base de données à plusieurs reprises pour chaque opération de R, vous pouvez utiliser le fichier XDF comme un type de cache pour enregistrer les données localement et puis les utiliser dans votre espace de travail R.

+ [RxXdfData](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxxdfdata) -créer un objet de données XDF

+ [rxReadXdf](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxreadxdf) -lit les données à partir d’un fichier XDF dans une trame de données

Pour plus d’informations sur l’utilisation de ces fonctions, y compris l’utilisation des données sources autres que les [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], consultez [faire Guide pour l’analyse de données dans Microsoft R](https://docs.microsoft.com/r-server/r/how-to-introduction).
