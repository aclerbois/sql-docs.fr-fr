---
title: Nouveautés de l’Assistant de Migration de données (SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 02/02/2018
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, new features
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: 2468bb700588310a397530f27c6c6d6960e75b06
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="whats-new-in-data-migration-assistant"></a>Nouveautés de l’Assistant Migration de données

Cette rubrique répertorie les ajouts dans chaque version de données de Migration Assistant (DMA).

## <a name="dma-v34"></a>DMA v3.4
La version de v3.4 de DMA inclut les ajouts suivants :
- Prise en charge 2017 du serveur SQL en tant que source pour les migrations vers la base de données SQL Azure.
- Améliorations apportées à la stabilité, de performances et d’évaluation de la vérification de règle.

## <a name="dma-v33"></a>DMA v3.3
La version v3.3 de DMA permet la migration d’une instance de SQL Server locale vers la nouvelle version de SQL Server 2017, Windows et Linux. Pendant le processus de migration globale pour Windows et Linux sont identiques, la migration vers SQL Server 2017 pour Linux nécessite quelques considérations supplémentaires.

### <a name="specifying-the-back-up-path"></a>En spécifiant le chemin de sauvegarde
Linux et Windows utilisent des formats de chemin d’accès différent. Par conséquent, la migration vers 2017 du serveur SQL sur Linux nécessite que l’utilisateur fournit des versions de Windows et Linux du chemin d’accès à l’emplacement du fichier physique. Cela s’effectue de différentes façons selon l’emplacement du fichier physique.
Si le fichier de sauvegarde physique est sur un ordinateur exécutant :
- Linux, utilisez un « samba' partage pour partager le fichier avec d’autres ordinateurs sur le réseau.
-   Windows, utilisez la commande 'mnt' pour monter le partage sur l’ordinateur exécutant Linux.

> [!NOTE]
> Détails de l’utilisation d’un partage 'samba' ou la commande 'mnt' sont dépasse le cadre de cet article.

### <a name="migrating-windows-logins"></a>Migrer les connexions Windows
Pendant la migration des comptes de connexion Active Directory (AD) est officiellement pris en charge par 2017 du serveur SQL sur Linux, il requiert une configuration supplémentaire pour fonctionner correctement. Reportez-vous à la rubrique [l’authentification Active Directory avec SQL Server sur Linux](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-active-directory-authentication) pour plus d’informations sur la configuration des connexions Active Directory sur 2017 du serveur SQL sur Linux. Après l’installation est terminée, vous pouvez migrer des comptes de connexion Active Directory comme d’habitude. Standard de l’authentification SQL fonctionne comme prévu, sans les paramètres supplémentaires.

## <a name="dma-v32"></a>DMA v3.2
La version de v3.2 de DMA inclut les ajouts suivants :

- Migration de schéma et les données sont activées à partir de bases de données SQL Server locale à la base de données SQL Azure avec un nouveau flux de travail de migration.

- Pendant la migration du schéma de base de données SQL Azure, DMA scripts vos objets de base de données source, fournit des conseils sur la façon de résoudre les éventuels problèmes de compatibilité, puis déploie votre schéma vers Azure.

## <a name="dma-v31"></a>DMA v3.1
La version 3.1 de DMA inclut les ajouts suivants :

- Recommandations d’évaluation améliorée pour les bases de données SQL Azure en termes de classements de base de données, l’utilisation de procédures stockées du système non pris en charge et les objets CLR.

- Guide d’évaluation pour les niveaux de compatibilité 130, 120, 110 et 100 lors de la migration de bases de données SQL Azure.

## <a name="dma-v30"></a>DMA v3.0
La version de la version 3.0 de DMA étend l’évaluation de la base de données SQL Azure pour fournir des recommandations complètes pour aider à résoudre les problèmes liés à :

- Problèmes de blocage de la migration.

- Partiellement ou non pris en charge des fonctionnalités et fonctions.

## <a name="dma-v21"></a>DMA v2.1
La version v2.1 de DMA inclut les ajouts suivants :
- Prise en charge de ligne de commande pour l’exécution des évaluations en mode sans assistance, ce qui permet d’exécuter des évaluations à l’échelle. Pour plus d’informations, reportez-vous à la rubrique [exécuter données Assistant de Migration à partir de la ligne de commande](dma-commandline.md).

- Améliorations des performances lorsque les utilisateurs de lancement et fermer DMA.

- La possibilité de configurer le délai de connexion SQL. Pour plus d’informations, reportez-vous à la rubrique [paramètres de Configuration de l’Assistant Migration de données](dma-configurationsettings.md).

## <a name="dma-v20"></a>DMA v2.0
La version v2.0 de DMA inclut améliorées recommandations de fonctionnalité de base de données Stretch pour fournir des tables hiérarchisés appropriées qui optimisent les économies de stockage.

## <a name="dma-v10"></a>DMA v1.0
La version 1.0 de DMA est la version initiale, et il fournit pour :
- Détection de problèmes qui peuvent affecter une mise à niveau vers une version locale de SQL Server. Les résultats sont décrits comme des problèmes de compatibilité, et ils sont classés dans les domaines suivants :
    -   Modifications avec rupture
    - Changements de comportement
    - Fonctionnalités déconseillées

- Détection des nouvelles fonctionnalités de la plateforme cible SQL Server que la base de données peut bénéficier d’une mise à niveau. Les conclusions sont décrits comme des recommandations de fonctionnalité, et ils sont classés dans les domaines suivants :
    - Performance
    - Sécurité
    - Stockage

-   Expérience utilisateur moderne pour effectuer des évaluations.

## <a name="see-also"></a>Voir aussi

[Vue d’ensemble de l’Assistant de Migration de données](../dma/dma-overview.md)
