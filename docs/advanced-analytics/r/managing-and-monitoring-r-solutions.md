---
title: Gérer et surveiller des solutions d’apprentissage machine dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 4806224a1606fff58f63f6083fa577aa4066c795
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585701"
---
# <a name="managing-and-monitoring-machine-learning-solutions"></a>Gestion et surveillance des solutions machine learning
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les fonctionnalités de SQL Server Machine Learning Services qui sont pertinents pour les administrateurs de base de données qui ont besoin pour commencer à travailler avec des solutions R et Python.

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services

## <a name="security"></a>Sécurité

Les administrateurs de base de données doivent fournir l’accès aux données non seulement aux chercheurs de données mais aussi des développeurs de rapports, les analystes d’entreprise et des consommateurs de données d’entreprise. L’intégration de R (et Python maintenant) dans SQL Server offre de nombreux avantages à l’administrateur de base de données qui prend en charge le rôle de science des données.

+ L’architecture de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] protège vos bases de données et isole l’exécution des sessions R du fonctionnement de l’instance de base de données.

+ Vous pouvez spécifier qui est autorisé à exécuter des scripts R et vous assurer que les données utilisées dans les travaux R sont gérées à l’aide des rôles de sécurité définis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

+ L’administrateur de base de données peut utiliser des rôles pour gérer l’installation des packages R et l’exécution de scripts R et Python.

Pour plus d'informations, consultez ces ressources :

+ [Considérations relatives à la sécurité du runtime R dans SQL Server](../../advanced-analytics/r/security-considerations-for-the-r-runtime-in-sql-server.md)

+ [Vue d’ensemble de la sécurité R](../r/security-overview-sql-server-r.md)

+ [Vue d’ensemble de la sécurité Python](../python/security-overview-sql-server-python-services.md)

+ [Par défaut R et Python packages dans SQL Server](installing-and-managing-r-packages.md)

## <a name="configuration-and-management"></a>Configuration et gestion

Les administrateurs de base de données doivent intégrer des projets et priorités concurrents dans un point de contact unique : le serveur de base de données. Ils doivent prendre en charge analytique tout en conservant l’intégrité des magasins de données opérationnelles et de rapport. L’intégration d’apprentissage avec SQL Server offre de nombreux avantages à l’administrateur de base de données, qui sert de plus en plus un rôle essentiel dans le déploiement d’une infrastructure efficace pour la science des données.

+ Les sessions R et Python sont exécutées dans un processus séparé pour garantir que votre serveur continue à exécuter comme d’habitude, même si l’exécution du script externe a des problèmes.

+ Comptes d’utilisateur physique faibles sont utilisés pour contenir et isoler l’activité de script externe.

+ L’administrateur peut utiliser les outils de gestion de ressources SQL Server standard pour contrôler la quantité de ressources allouées au runtime R, afin d’éviter que des calculs massifs mettent en péril les performances globales du serveur.

Pour plus d'informations, consultez ces ressources :

+ [Gouvernance des ressources pour R Services](../r/resource-governance-for-r-services.md)

+ [Configurer et gérer les Extensions d’Analytique avancée](../r/configure-and-manage-advanced-analytics-extensions.md)
