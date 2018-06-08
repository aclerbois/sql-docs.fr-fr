---
title: Mappage des bases de données MySQL pour les schémas SQL Server (MySQLToSQL) | Documents Microsoft
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
helpviewer_keywords:
- Mapping, Modifying target database and schema
- Mapping, reverting to default database and schema
ms.assetid: 5c6fb445-92ae-4933-b77d-80230931c024
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 6fc24887570cd1cf4422705282514530308706d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-mysql-databases-to-sql-server-schemas-mysqltosql"></a>Mappage des bases de données MySQL pour les schémas SQL Server (MySQLToSQL)
Par défaut, SSMA pour MySQL migre tous les objets dans un schéma MySQL à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou nommée pour le schéma de la base de données SQL Azure. Toutefois, vous pouvez personnaliser le mappage entre les schémas de MySQL et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou bases de données SQL Azure.  
  
## <a name="mysql-and-sql-server-or-sql-azure-schemas"></a>MySQL et SQL Server ou SQL Azure schémas  
Le concept de MySQL d’un schéma est mappé au concept de base de données et un de ses schémas SQL Server. SSMA fait référence à la combinaison de SQL Server de la base de données et le schéma en tant que schéma.  
  
Le concept de MySQL d’un schéma est mappé au concept de base de données et un de ses schémas SQL Server. Par exemple, MySQL peut avoir un schéma nommé **HR**. Une instance de SQL Server peut avoir une base de données nommée **HR**, et dans cette base de données, des schémas. Un schéma est la **dbo** (ou le propriétaire de la base de données) schéma. Par défaut, le schéma MySQL **HR** seront mappées à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données et le schéma **HR.dbo**. SSMA fait référence à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] combinaison de la base de données et le schéma en tant que schéma.  
  
Vous pouvez modifier le mappage entre MySQL et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou les schémas de Azure.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données cible et le schéma  
Dans SSMA, vous pouvez mapper un schéma de MySQL vers tout disponible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou schéma de SQL Azure.  
  
**Pour modifier la base de données et le schéma**  
  
1.  Dans l’Explorateur de métadonnées MySQL, sélectionnez **schémas**.  
  
    Le **schéma de mappage** onglet est disponible lorsque vous sélectionnez les schémas individuels. La liste dans le **schéma de mappage** onglet personnalisé pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur le **schéma de mappage** onglet.  
  
    Vous verrez une liste de tous les schémas de MySQL, suivi d’une valeur cible. Cette cible est représentée dans une notation de deux parties (*database.schema*) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure où vos objets et données seront migrées.  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez modifier, puis cliquez sur **modifier**.  
  
    Dans le **choisir un schéma cible** boîte de dialogue, vous pourrez consulter pour la base de données cible disponible et de schéma ou de type de la base de données et le schéma de nom dans la zone de texte dans une notation de deux parties (database.schema), puis **OK**.  
  
4.  La cible est modifiée sur le **schéma de mappage** onglet.  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper la base de données source vers une base de données cible. Par défaut la base de données source est mappé à la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible en cours de mappage est inexistant sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], puis vous serez invité avec un message **« la base de données et/ou un schéma n’existe pas dans la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] métadonnées. Il est créé lors de la synchronisation. Voulez-vous continuer ? »** Cliquez sur Oui. De même, vous pouvez mapper le schéma non existants de schéma sous la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données qui sera créé pendant la synchronisation.  
  
-   Mappage à SQL Azure  
  
Vous pouvez mapper la base de données source à la cible connectée [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données ou à n’importe quel schéma dans la cible connectée [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Si vous mappez la source de schéma à n’importe quel schéma inexistante sous base de données cibles connectés, vous serez invité avec un message **« schéma n’existe pas dans les métadonnées de la cible. Il est créé lors de la synchronisation. Voulez-vous continuer ? »** Cliquez sur Oui.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Retour à la base de données par défaut et le schéma  
Si vous personnalisez le mappage entre un schéma de MySQL et un schéma de SQL Server, vous pouvez rétablir les valeurs par défaut le mappage.  
  
**Pour rétablir la base de données par défaut et le schéma**  
  
1.  Sous l’onglet mappage de schéma, sélectionnez n’importe quelle ligne, puis cliquez sur **rétablir par défaut** pour rétablir la base de données par défaut et le schéma.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous souhaitez analyser la conversion d’objets de MySQL dans des objets SQL Server ou SQL Azure, vous pouvez [créer un rapport de conversion](http://msdn.microsoft.com/en-us/2a56a003-3b0f-453a-963c-00c9e40933ec) dans le cas contraire, vous pouvez [convertir les définitions d’objets de base de données MySQL](http://msdn.microsoft.com/en-us/ac21850b-fb32-4704-9985-5759b7c688c7) en schémas SQL Server ou SQL Azure  
  
## <a name="see-also"></a>Voir aussi  
[Paramètres du projet &#40;Conversion&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
[Connexion à la base de données SQL Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[Connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
