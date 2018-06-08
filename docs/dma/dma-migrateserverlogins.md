---
title: Migration des connexions SQL Server (Assistant Migration de données) | Documents Microsoft
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, login migration
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: 23da8fe364ffad914013719f54871e85213befc5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrating-sql-server-logins-using-data-migration-assistant"></a>Migration des connexions SQL Server à l’aide de l’Assistant Migration de données

Cet article fournit une vue d’ensemble de la migration des connexions SQL Server à l’aide de l’Assistant Migration de données. 

## <a name="key-concepts"></a>Concepts clés
Les concepts clés sont les suivantes.

- Vous pouvez migrer les connexions basées sur un principal Windows (par exemple, un utilisateur de domaine ou un groupe de domaine Windows). Vous pouvez également migrer les connexions créées en fonction de l’authentification SQL, appelée également des connexions SQL Server.

- L’Assistant Migration de données actuellement ne prend en charge les connexions associées à un certificat de sécurité autonome (connexions mappés au certificat), une clé asymétrique autonome (connexions mappées à la clé asymétrique) et les connexions mappées aux informations d’identification.

- L’Assistant Migration de données ne se déplace pas le **sa** principes de connexion et de serveur compris entre deux signes dièse (\#\#), qui sont à usage interne uniquement.

- Par défaut, Assistatn de Migration de données sélectionne toutes les connexions qualifiées à migrer. Si vous le souhaitez, vous pouvez sélectionner des connexions spécifiques à migrer. Lorsque l’Assistant Migration de données migre toutes les connexions d’accès complets, le mappage de l’utilisateur de la connexion reste intact dans les bases de données qui sont migrés. 

  Si vous envisagez de migrer des connexions spécifiques, veillez à sélectionner les connexions qui sont mappées à un ou plusieurs utilisateurs dans les bases de données sélectionnées pour la migration.

- Dans le cadre de la migration de la connexion, l’Assistant Migration de données également déplace les rôles serveur définis par l’utilisateur et ajoute les autorisations au niveau du serveur pour les rôles de serveur définis par l’utilisateur. Le propriétaire du rôle sera défini sur **sa** principal.

- Dans le cadre de la migration de la connexion, l’Assistant Migration de données affecte les autorisations aux éléments sécurisables sur la cible de SQL Server tels qu’ils existent sur la serveur source SQL Server. 

  Si la connexion existe déjà sur le serveur SQL cible, l’Assistant Migration de données migre uniquement les autorisations affectées aux éléments sécurisables et ne recrée pas la connexion entière.

- L’Assistant Migration de données rend le meilleur effort pour mapper la connexion aux utilisateurs de base de données si la connexion existe déjà sur le serveur cible.

- Il est recommandé que vous passez en revue les résultats de la migration pour comprendre l’état global de la migration de la connexion et les actions recommandées de post-migration.

## <a name="resources"></a>Ressources

[Assistant de Migration de données (DMA)](../dma/dma-overview.md)

[L’Assistant Migration de données : Les paramètres de Configuration](../dma/dma-configurationsettings.md)
