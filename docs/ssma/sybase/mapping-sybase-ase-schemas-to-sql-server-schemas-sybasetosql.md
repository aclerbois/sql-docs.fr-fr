---
title: Mappage des schémas de Sybase ASE aux schémas SQL Server (SybaseToSQL) | Documents Microsoft
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
helpviewer_keywords:
- Schema Mapping
ms.assetid: 2c927003-c49d-4fe1-8e3e-5b2899166268
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: ffcec58a7c90bb7e95efd7e770ad7f49610a7cd2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql"></a>Sybase ASE schémas de mappage de schémas SQL Server (SybaseToSQL)
Dans Sybase Adaptive Server Enterprise (ASE), chaque base de données a un ou plusieurs schémas. Par défaut, SSMA migre tous les objets dans une base de données et le schéma à la même base de données et le même schéma dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. Toutefois, vous pouvez personnaliser le mappage entre ASE et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des schémas et des bases de données SQL Azure.  
  
## <a name="ase-and-sql-server-or-sql-azure-schemas"></a>ASE et SQL Server ou SQL Azure schémas  
ASE et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure à la fois spécifier les bases de données et leurs schémas à l’aide de la notation deux partie en tant que *database.schema*. Par exemple, dans un environnement app service **démonstration** de base de données, il peut y avoir un **dbo** schéma. Que la paire base de données et de schéma est spécifiée en tant que **demo.dbo**. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure a la même base de données et le schéma, la paire est également spécifiée en tant que **demo.dbo**.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données cible et le schéma  
Dans SSMA, vous pouvez mapper un schéma ASE à éventuellement [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou schéma de SQL Azure.  
  
**Pour modifier la base de données et le schéma**  
  
1.  Dans l’Explorateur de métadonnées de Sybase, sélectionnez **bases de données**.  
  
    Le **schéma de mappage** onglet est disponible lorsque vous sélectionnez une base de données individuel, le **schémas** dossier ou les schémas individuels. La liste dans le **schéma de mappage** onglet personnalisé pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur le **schéma de mappage** onglet.  
  
    Vous verrez une liste de toutes les bases de données ASE avec leurs schémas, suivis d’une valeur cible. Cette cible est représentée dans une notation de deux parties (*database.schema*) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure où vos objets et données seront migrées.  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez modifier, puis cliquez sur **modifier**.  
  
4.  Dans le **choisir un schéma cible** boîte de dialogue, vous pourrez consulter pour la base de données cible disponible et de schéma ou de type de la base de données et le schéma de nom dans la zone de texte dans une notation de deux parties (database.schema), puis **OK**.  
  
5.  La cible est modifiée sur le **schéma de mappage** onglet.  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper la base de données source vers une base de données cible. Par défaut la base de données source est mappé à la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible en cours de mappage est inexistant sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], puis vous serez invité avec un message **« la base de données et/ou un schéma n’existe pas dans la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] métadonnées. Il est créé lors de la synchronisation. Voulez-vous continuer ? »** Cliquez sur Oui. De même, vous pouvez mapper le schéma non existants de schéma sous la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données qui sera créé pendant la synchronisation.  
  
-   Mappage à SQL Azure  
  
Vous pouvez mapper la base de données source à la base de données SQL Azure cibles connectés ou à n’importe quel schéma dans la base de données SQL Azure cibles connectés. Si vous mappez la source de schéma à n’importe quel schéma inexistante sous base de données cibles connectés, vous serez invité avec un message **« schéma n’existe pas dans les métadonnées de la cible. Il est créé lors de la synchronisation. Voulez-vous continuer ? »** Cliquez sur Oui.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Retour à la base de données par défaut et le schéma  
Si vous personnalisez le mappage entre un schéma ASE et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou schéma de SQL Azure, vous pouvez rétablir les valeurs par défaut le mappage.  
  
**Pour rétablir la base de données par défaut et le schéma**  
  
1.  Sous l’onglet mappage de schéma, sélectionnez n’importe quelle ligne, puis cliquez sur **rétablir par défaut** pour rétablir la base de données par défaut et le schéma.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous souhaitez analyser la conversion des objets de Sybase ASE dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de SQL Azure, vous pouvez [créer un rapport de conversion](http://msdn.microsoft.com/en-us/eb996b7c-1eef-4f73-b5e6-2fa6faf7336c). Dans le cas contraire, vous pouvez [convertir les définitions d’objets de base de données ASE](http://msdn.microsoft.com/en-us/509cb65d-2f54-427a-83d7-37919cc4e3e3) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou définitions d’objets SQL Azure.  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de Sybase ASE à SQL Server - base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
