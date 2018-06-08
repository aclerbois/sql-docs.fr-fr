---
title: Migration de données Access vers SQL Server - base de données SQL Azure (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- bulk loading data
- data, loading into SQL Azure
- data, loading into SQL Server
- migrating databases, loading data
- migrating databases, options
- options, migrating data
- SQL Azure, migrating data into
- SQL Server, migrating data into
ms.assetid: f3b18af7-1af0-499d-a00d-a0af94895625
caps.latest.revision: 17
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 747ffdc89104deb4fcbe356065b9b81f383231ab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrating-access-data-into-sql-server---azure-sql-db-accesstosql"></a>Migration de données Access vers SQL Server - base de données SQL Azure (AccessToSQL)
Une fois que vous venez de créer les objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez migrer des données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
## <a name="setting-migration-options"></a>Définition des Options de Migration  
Avant de migrer des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, passez en revue les options de migration de projet dans le **les paramètres de projet** boîte de dialogue. Dans cette boîte de dialogue, vous pouvez définir la taille de lot de migration, le verrouillage de table, la contrainte de vérification, le déclencheur d’insertion déclenchement, identité et la valeur null, gérer et comment gérer des dates qui sont hors de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] plage. Pour plus d’informations, consultez [paramètres du projet (Migration)](http://msdn.microsoft.com/en-us/4caebc9c-8680-4b99-a8fa-89c43161c95d).  
  
## <a name="migrating-data"></a>Migration des données  
Migration de données sont une opération de chargement en masse qui déplace les lignes de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure dans les transactions. Le nombre de lignes doit être chargé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure lors de chaque transaction est configuré dans les paramètres du projet.  
  
Pour afficher les messages de la migration, assurez-vous que le volet de résultats est visible. S’il n’est pas le cas, sur le **vue** menu, sélectionnez **sortie**.  
  
**Pour migrer des données**  
  
1.  Assurez-vous que vous avez chargé les objets de base de données Access dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
2.  Dans l’Explorateur de métadonnées de l’accès, sélectionnez les objets qui contiennent les données que vous souhaitez migrer :  
  
    -   Pour migrer les données pour une base de données entière, sélectionnez la case à cocher en regard du nom de la base de données.  
  
    -   Pour migrer des données à partir des tables individuelles, développez la base de données, **Tables**, puis sélectionnez la case à cocher en regard de la table. Pour omettre les données des tables individuelles, désactivez la case à cocher.  
  
3.  Avec le bouton droit **bases de données** , puis sélectionnez **migrer des données**.  
  
Vous pouvez également migrer des données en dehors de SSMA à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **bcp** utilitaire de ligne de commande ou [!INCLUDE[ssISnoversion](../../includes/ssisnoversion_md.md)]. Pour plus d’informations sur ces outils, consultez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="next-step"></a>Étape suivante  
Si vous avez des applications de base de données Access que vous souhaitez continuer à utiliser après la migration, lier les tables de base de données Access à le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des tables SQL Azure. Pour plus d’informations, consultez [liaison des Applications de l’accès à SQL Server](http://msdn.microsoft.com/en-us/82374ad2-7737-4164-a489-13261ba393d4).  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)  
[Conversion de paramètre et les Options de Migration](http://msdn.microsoft.com/en-us/0a7304df-2f35-4453-96ef-7ac83dea1167)  
  
