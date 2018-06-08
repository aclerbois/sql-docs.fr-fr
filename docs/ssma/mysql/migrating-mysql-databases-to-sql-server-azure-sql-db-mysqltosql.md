---
title: 'Migrer des bases de données MySQL vers SQL Server : base de données SQL Azure | Documents Microsoft'
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
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
ms.assetid: 8006f9a0-394d-4238-8dc5-44255134628b
caps.latest.revision: 12
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 6ab959da887abd67ec7f80eae94cb4bfcacd0883
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrating-mysql-databases-to-sql-server---azure-sql-db-mysqltosql"></a>Migration des bases de données de MySQL vers SQL Server - base de données SQL Azure (MySQLToSql)
SQL Server Migration Assistant (SSMA) pour MySQL est un environnement complet qui vous permet de migrer rapidement des bases de données MySQL vers SQL Server ou SQL Azure. À l’aide de SSMA pour MySQL, vous pouvez examiner les données et les objets de base de données, évaluer les bases de données pour la migration, migrer des objets de la base de données vers SQL Server ou SQL Azure et puis migrer les données vers SQL Server ou SQL Azure.  
  
## <a name="recommended-migration-process"></a>Processus de Migration recommandé  
Pour migrer des objets et des données à partir de bases de données MySQL vers SQL Server ou SQL Azure, procédez comme suit :  
  
1.  [Utilisation de projets SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/working-with-ssma-projects-mysqltosql.md).  
  
    Après avoir créé le projet, vous pouvez définir les options de mappage de type, de migration et de conversion de projet. Pour plus d’informations sur les paramètres de projet, consultez [définition des Options de projet &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md). Pour plus d’informations sur la personnalisation des mappages de types de données, consultez [MySQL de mappage et les Types de données SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
2.  [Se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
3.  [Connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
4.  [Mappage des bases de données MySQL pour les schémas SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md)  
  
5.  [Connexion à la base de données SQL Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
  
6.  Si vous le souhaitez, [évaluation de bases de données MySQL pour la Conversion &#40;MySQLToSQL&#41; ](../../ssma/mysql/assessing-mysql-databases-for-conversion-mysqltosql.md) pour évaluer des objets de base de données pour la conversion et estimer le temps de conversion.  
  
7.  [Conversion des bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
8.  [Synchronisation](http://msdn.microsoft.com/en-us/ac993a6d-0283-4823-8793-6b217677dfa3)  
  
9. Pour cela, dans une des manières suivantes :  
  
    -   Enregistrer un script et exécutez-le sur SQL Server ou SQL Azure.  
  
    -   Synchroniser les objets de base de données.  
  
10. [Migration de données MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
11. Si nécessaire, mettez à jour des applications de base de données.  
  
> [!NOTE]  
> Vous ne pouvez pas migrer les schémas Information_schema et MySQL.  
  
## <a name="see-also"></a>Voir aussi  
[L’installation de SSMA pour MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
[Prise en main de SSMA pour MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/getting-started-with-ssma-for-mysql-mysqltosql.md)  
  
