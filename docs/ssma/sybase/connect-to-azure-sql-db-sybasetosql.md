---
title: Se connecter à la base de données SQL Azure (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 96538007-1099-40c8-9902-edd07c5620ee
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: f73b6e88e457ef74a82512876715a333d0a52d71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-azure-sql-db--sybasetosql"></a>Se connecter à la base de données SQL Azure (SybaseToSQL)
Utilisez la connexion à la boîte de dialogue base de données SQL Azure pour se connecter à la base de données de la base de données SQL Azure que vous souhaitez migrer.  
  
Pour accéder à cette boîte de dialogue, dans le **fichier** menu, sélectionnez **se connecter à la base de données SQL Azure**. Si vous êtes déjà connecté, la commande est **se reconnecter à la base de données SQL Azure.**  
  
## <a name="options"></a>Options  
**Nom de serveur**  
  
Sélectionnez ou entrez le nom du serveur pour la connexion à la base de données SQL Azure.  
  
**Base de données**  
  
Sélectionnez, entrez ou **Parcourir** le nom de la base de données.  
  
> [!IMPORTANT]  
> SSMA pour Sybase ne prend pas en charge la connexion à la base de données master dans la base de données SQL Azure.  
  
**Nom d'utilisateur**  
  
Entrez le nom d’utilisateur SSMA utilisera pour se connecter à la base de données de la base de données SQL Azure  
  
**Mot de passe**  
  
Entrez le mot de passe correspondant au nom d'utilisateur indiqué.  
  
**Chiffrer**  
  
SSMA recommande une connexion chiffrée à la base de données SQL Azure.  
  
## <a name="create-azure-database"></a>Créer une base de données Azure  
S’il n’y a aucune base de données dans le compte de base de données SQL Azure, vous pouvez créer la première base de données.  
  
Pour créer une nouvelle base de données pour la première fois, suivez les étapes suivantes  
  
1.  Cliquez sur le bouton Parcourir qui est présent dans la connexion à la boîte de dialogue base de données SQL Azure  
  
2.  S’il n’y a aucune base de données, les deux options suivantes s’affichent.  
  
    1.  **(bases de données introuvables)**  qui est désactivé et grisé tout le temps  
  
    2.  **Créer la nouvelle base de données** qui est activé uniquement lorsqu’il n’y aucune base de données sur le compte de base de données SQL Azure. Lorsque vous cliquez sur cet élément de menu, la boîte de dialogue Créer une base de données Azure est présent avec la taille et le nom de la base de données.  
  
3.  Au moment de la création de la base de données, les deux paramètres suivants figurent en tant qu’entrée :  
  
    1.  **Nom de la base de données :** Entrez le nom de la base de données.  
  
    2.  **Taille de la base de données :** sélectionner la taille de la base de données dont vous avez besoin pour créer le compte de base de données SQL Azure.  
  
