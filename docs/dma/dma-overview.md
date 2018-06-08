---
title: Vue d’ensemble de l’Assistant de Migration de données (SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2018
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, overview
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: dd681a6445c6759b0ec17e06dc0b4dbf24b3b72f
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707967"
---
# <a name="overview-of-data-migration-assistant"></a>Vue d’ensemble de l’Assistant de Migration de données

L’Assistant de Migration données (DMA) vous permet de mettre à niveau vers une plate-forme de données modernes en détectant les éventuels problèmes de compatibilité peuvent affecter les fonctionnalités de base de données dans votre nouvelle version de SQL Server et la base de données SQL Azure. DMA recommande des performances et des améliorations de la fiabilité de votre environnement cible et vous permet de déplacer votre schéma, les données et les objets de relation contenant-contenus de votre serveur source pour votre serveur cible.

> [!NOTE] 
> De grande taille (en termes de nombre et la taille des bases de données) migrations, il est recommandé d’utiliser le [Service de Migration de base de données Azure](https://docs.microsoft.com/azure/dms/dms-overview), qui peuvent migrer les bases de données à grande échelle.
  
## <a name="capabilities"></a>Fonctions

- Évaluer les instances de SQL Server sur site vers les bases de données SQL Azure. Le flux de travail d’évaluation vous aide à détecter les problèmes suivants qui peuvent affecter la migration de base de données SQL Azure et fournit des instructions détaillées sur la façon de les résoudre.

  - Problèmes de blocage de migration : permet de détecter les problèmes de compatibilité que la migration de bloc local SQL Server bases de données s pour les bases de données SQL Azure. DMA fournit des recommandations pour vous aider à résoudre ces problèmes.

  - Partiellement pris en charge ou non pris en charge des fonctionnalités : détecte partiellement pris en charge ou non pris en charge des fonctionnalités qui sont actuellement en cours d’utilisation sur l’instance de SQL Server source. DMA fournit qu'un ensemble complet des recommandations, d’autres approches sont disponibles dans Azure et les mesures de sorte que vous pouvez incorporer dans vos projets de migration.

- Détecter les problèmes qui peuvent affecter une mise à niveau vers un ordinateur local SQL Server. Ceux-ci sont décrits comme des problèmes de compatibilité et sont organisés dans les catégories suivantes :

  - Modifications avec rupture
  - Changements de comportement
  - Fonctionnalités déconseillées

- Découvrez les nouvelles fonctionnalités de la plateforme cible SQL Server que la base de données peut tirer parti d’après une mise à niveau. Ceux-ci sont décrits comme des recommandations de la fonctionnalité et sont organisés dans les catégories suivantes :

  - Performances
  - Sécurité
  - Stockage

- Migrer une instance de SQL Server locale vers une instance de SQL Server moderne, hébergée localement ou sur une machine virtuelle Azure (VM) qui est accessible à partir de votre réseau local. La machine virtuelle Azure est accessible à l’aide de VPN ou autres technologies. Le flux de travail de migration vous aide à migrer les composants suivants :

  - Schéma des bases de données
  - Données et les utilisateurs
  - Rôles de serveur
  - Connexions SQL Server et Windows

- Après la migration réussie, les applications peuvent se connecter pour les bases de données du serveur SQL cible en toute transparence.

## <a name="supported-source-and-target-versions"></a>Prise en charge les versions source et cible

DMA remplace toutes les versions précédentes du Conseiller de mise à niveau de SQL Server et doit être utilisé pour les mises à niveau pour la plupart des versions de SQL Server. Suivent les versions prises en charge de sources et cibles.

**Sources**
- SQL Server 2005
- SQL Server 2008
- SQL Server 2008 R2
- SQL Server 2012 
- SQL Server 2014
- SQL Server 2016
- SQL Server 2017 sur Windows

**Cibles**
- SQL Server 2012
- SQL Server 2014
- SQL Server 2016
- SQL Server 2017 sur Windows et Linux
- Azure SQL Database

> [!NOTE] 
> DMA ne prend pas en charge de base de données SQL Azure une Instance gérée en tant que cible.

## <a name="installation"></a>Installation

Pour installer DMA, téléchargez la dernière version de l’outil à partir de la [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595), puis exécutez le **DataMigrationAssistant.msi** fichier.

## <a name="see-also"></a>Voir aussi

[Évaluer la Migration de votre serveur SQL](../dma/dma-assesssqlonprem.md)

[L’Assistant Migration de données : Les paramètres de Configuration](../dma/dma-configurationsettings.md)

[Migration locale SQL Server à l’aide de l’Assistant Migration de données](../dma/dma-migrateonpremsql.md)

[Assistant de Migration de données : Meilleures pratiques](../dma/dma-bestpractices.md)



