---
title: Présentation de l’architecture de prise en charge du langage R dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 3784fe62669b3f6d29e94bea799e19f9eadad5f0
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="architecture-overview-for-r-in-sql-server"></a>Présentation de l’architecture de R dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cette section fournit une vue d’ensemble de l’architecture de SQL Server 2016 R Services et de SQL Server 2017 Machine Learning Services.

L’architecture de l’architecture d’extensibilité est la même ou très similaire pour le serveur SQL Server 2016 et versions 2017 de SQL Server et également pour R et Python. Toutefois, pour simplifier la discussion, cet article traite uniquement les composants R, y compris les nouveaux composants ajoutés dans le moteur de base de données SQL Server pour prendre en charge l’exécution du script externe, la sécurité, les bibliothèques R et l’interopérabilité avec open source R.

Des détails supplémentaires sont fournies dans les liens pour chaque section.

## <a name="r-interoperability"></a>Interopérabilité de R

SQL Server 2016 R Services et SQL Server 2017 Machine Learning Services (de-de base de données) installent une distribution open source de R, ainsi que les packages fournis par Microsoft qui prennent en charge le traitement distribué et/ou parallèle.

L’architecture est conçue pour que des scripts externes à l’aide de R s’exécutent dans un processus séparé à partir de SQL Server. Les utilisateurs actuels de R doivent être en mesure de déplacer leur code R et de l’exécuter dans T-SQL en apportant des modifications relativement mineures.

Pour mettre à l’échelle de votre solution ou utiliser le traitement parallèle, nous vous recommandons d’utiliser le [RevoScaleR](https://docs.microsoft.com/r-server/r-reference/revoscaler/revoscaler) package ou le [MicrosoftML](https://docs.microsoft.com/r-server/r-reference/microsoftml/microsoftml-package) package. Si vous n’utilisez pas les capacités informatiques distribuées fournies par ces bibliothèques, vous pouvez toujours obtenir des améliorations de performances en exécutant votre code R dans le contexte de SQL Server.

Pour plus d’informations sur les composants de scripts externes qui sont installés, ou l’interaction de SQL Server avec R, consultez [R interopérabilité](../../advanced-analytics/r/r-interoperability-in-sql-server.md)

## <a name="components-to-support-r-integration"></a>Composants pour prendre en charge l’intégration R

L’infrastructure d’extensibilité introduite dans SQL Server 2016 se poursuit dans SQL Server 2017. Les composants d’extensibilité sont utilisés par SQL Server pour lancer le runtime externe pour R, pour passer des données entre R et le moteur de base de données et pour coordonner les tâches parallèles nécessaires pour un travail de R.

Le rôle de ces composants supplémentaires est afin d’améliorer la vitesse des données exchange et la compression, tout en fournissant une plateforme sécurisée et hautes performances pour l’exécution de scripts externes.

Pour une description détaillée des composants qui prennent en charge R, comme le [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] et RLauncher, consultez [nouveaux composants](../../advanced-analytics/r/new-components-in-sql-server-to-support-r.md).

## <a name="security"></a>Sécurité

Lorsque vous exécutez le code R à l’aide de la Machine Learning Services ou SQL Server R Services, tous les scripts R sont exécutées en dehors du processus SQL Server, pour fournir une plus grande facilité de gestion et de sécurité. Cette isolation des processus contienne la valeur true que vous exécutez le script R dans le cadre d’une procédure stockée, ou vous connecter à la tâche d’ordinateur SQL Server à partir d’un ordinateur distant et démarrez une tâche qui utilise le serveur comme le contexte de calcul.

SQL Server intercepte toutes les demandes de tâches, sécurise la tâche et ses données à l’aide d’objets de travail Windows et assure la sécurité sur les données à l’aide de comptes d’utilisateur SQL Server et les rôles de base de données.

En appliquant la sécurité au niveau de la table, la base de données et le niveau de l’instance de SQL Server, les données sont conservées dans les limites de la conformité. L’administrateur de base de données peut contrôler qui a la possibilité d’exécuter des travaux R, et qui a la possibilité d’installer ou de partager des packages R. L’administrateur peut également surveiller l’utilisation de scripts R par les utilisateurs distants ou locaux et surveiller et gérer la consommation de ressources.

## <a name="next-steps"></a>Étapes suivantes

[Composants prenant en charge l’intégration de R](new-components-in-sql-server-to-support-r.md)

[Interopérabilité de R](r-interoperability-in-sql-server.md)

[Vue d’ensemble de la sécurité](security-overview-sql-server-r.md)
