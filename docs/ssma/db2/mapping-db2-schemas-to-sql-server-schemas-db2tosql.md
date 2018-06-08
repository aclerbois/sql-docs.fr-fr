---
title: Mappage des schémas de DB2 aux schémas SQL Server (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
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
ms.assetid: 05ff7bd4-e60b-4f48-a893-bc2346aa9a8a
caps.latest.revision: 5
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 3ef34e3db8b5d38810efd24ad93ce70de8b0a3cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-db2-schemas-to-sql-server-schemas-db2tosql"></a>Mappage des schémas de DB2 aux schémas SQL Server (DB2ToSQL)
Dans DB2, chaque base de données a un ou plusieurs schémas. Par défaut, SSMA migre tous les objets dans un schéma DB2 à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] nommé pour le schéma de base de données. Toutefois, vous pouvez personnaliser le mappage entre les schémas de DB2 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données.  
  
## <a name="db2-and-sql-server-schemas"></a>DB2 et des schémas SQL Server  
Une base de données DB2 contient des schémas. Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] contient plusieurs bases de données, chacune d’elles peut comporter plusieurs schémas.  
  
Le concept de DB2 d’un schéma est mappé à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] concept d’une base de données et d’un de ses schémas. Par exemple, DB2 peut avoir un schéma nommé **HR**. Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] peut avoir une base de données nommée **HR**, et dans cette base de données, des schémas. Un schéma est la **dbo** (ou le propriétaire de la base de données) schéma. Par défaut, le schéma DB2 **HR** seront mappées à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données et le schéma **HR.dbo**. SSMA fait référence à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] combinaison de la base de données et le schéma en tant que schéma.  
  
Vous pouvez modifier le mappage entre DB2 et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] schémas.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données cible et le schéma  
Dans SSMA, vous pouvez mapper un schéma DB2 à éventuellement [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] schéma.  
  
**Pour modifier la base de données et le schéma**  
  
1.  Dans l’Explorateur de métadonnées de DB2, sélectionnez **schémas**.  
  
    Le **schéma de mappage** onglet est disponible lorsque vous sélectionnez une base de données individuel, le **schémas** dossier ou les schémas individuels. La liste dans le **schéma de mappage** onglet personnalisé pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur le **schéma de mappage** onglet.  
  
    Vous verrez une liste de tous les schémas de DB2, suivi d’une valeur cible. Cette cible est représentée dans une notation de deux parties (*database.schema*) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] où vos objets et données seront migrées.  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez modifier, puis cliquez sur **modifier**.  
  
    Dans le **choisir un schéma cible** boîte de dialogue, vous pourrez consulter pour la base de données cible disponible et de schéma ou de type de la base de données et le schéma de nom dans la zone de texte dans une notation de deux parties (database.schema), puis **OK**.  
  
4.  La cible est modifiée sur le **schéma de mappage** onglet.  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper la base de données source vers une base de données cible. Par défaut la base de données source est mappé à la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible en cours de mappage est inexistant sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], puis vous serez invité avec un message **« la base de données et/ou un schéma n’existe pas dans la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] métadonnées. Il est créé lors de la synchronisation. Voulez-vous continuer ? »** Cliquez sur Oui. De même, vous pouvez mapper le schéma non existants de schéma sous la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données qui sera créé pendant la synchronisation.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Retour à la base de données par défaut et le schéma  
Si vous personnalisez le mappage entre un schéma DB2 et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] schéma, vous pouvez rétablir les valeurs par défaut le mappage.  
  
**Pour rétablir la base de données par défaut et le schéma**  
  
1.  Sous l’onglet mappage de schéma, sélectionnez n’importe quelle ligne, puis cliquez sur **rétablir par défaut** pour rétablir la base de données par défaut et le schéma.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous souhaitez analyser la conversion d’objets DB2 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des objets, vous pouvez [rapport de Migration de données (SSMA commun)](http://msdn.microsoft.com/en-us/bbfb9d88-5a98-4980-8d19-c5d78bd0d241).  
  
## <a name="see-also"></a>Voir aussi  
[Connexion à SQL Server &#40;DB2eToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2etosql.md)  
[Bases de données DB2 migration vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
