---
title: Résolution des problèmes et Forum aux questions pour l’apprentissage dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/31/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 6ef72ae56973e695b96f0dfac7c0a3414bca5225
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707357"
---
# <a name="troubleshoot-machine-learning-in-sql-server"></a>Résoudre les problèmes d’apprentissage dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Utilisez cette page comme point de départ pour le travail sur des problèmes connus.

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage Services (R et Python)

## <a name="known-issues"></a>Problèmes connus

Les articles suivants décrivent les problèmes connus avec les versions actuelles et précédentes :

+ [Problèmes connus pour R Services](../advanced-analytics/known-issues-for-sql-server-machine-learning-services.md)
+ [Notes de publication de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md)
+ [Notes de publication de SQL Server 2017](../sql-server/sql-server-2017-release-notes.md)

## <a name="how-to-gather-system-information"></a>Comment recueillir des informations système

Si vous avez rencontré une erreur, ou que vous devez comprendre un problème dans votre environnement, il est important de collecter des informations connexes systématiquement. L’article suivant fournit une liste d’informations qui facilite le dépannage élaborés d’auto-assistance ou d’une demande de support technique.

+ [Collecte de données pour la résolution des problèmes de l’apprentissage](data-collection-ml-troubleshooting-process.md)

## <a name="setup-and-configuration-guides"></a>Guides d’installation et configuration

Si vous n’avez pas configuré apprentissage avec SQL Server, ou si vous souhaitez ajouter la fonctionnalité, commencez ici :

+ [Installer SQL Server 2017 Machine Learning Services (de-de base de données)](install/sql-machine-learning-services-windows-install.md)
+ [Installer SQL Server 2017 Machine Learning Server (autonome)](install/sql-machine-learning-standalone-windows-install.md)
+ [Installer SQL Server 2016 R Services (de-de base de données)](install/sql-r-services-windows-install.md)
+ [Installer SQL Server 2016 R Server (autonome)](install/sql-r-standalone-windows-install.md)
+ [Programme d’installation de ligne de commande](install/sql-ml-component-commandline-install.md)
+ [Configuration en mode hors connexion (sans Internet)](install/sql-ml-component-install-without-internet-access.md)

### <a name="configuration"></a>Configuration

Les articles suivants contiennent des informations sur les valeurs par défaut et comment personnaliser la configuration pour l’apprentissage sur une instance :

+ [Modifier le pool de comptes d’utilisateur pour SQL Server R Services](r/modify-the-user-account-pool-for-sql-server-r-services.md)  
+ [Configurer et gérer les extensions d’analytique avancée](r/configure-and-manage-advanced-analytics-extensions.md)  
+ [Comment créer un pool de ressources](r/how-to-create-a-resource-pool-for-r.md)
+ [Optimisation pour les charges de travail R](r/operationalizing-your-r-code.md)
