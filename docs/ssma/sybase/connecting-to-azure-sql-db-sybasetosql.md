---
title: Connexion à la base de données SQL Azure (SybaseToSQL) | Documents Microsoft
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
ms.assetid: 9e77e4b0-40c0-455c-8431-ca5d43849aa7
caps.latest.revision: 7
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: dc13a12e706faf8151c3bdea586533a97a9e71fd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-to-azure-sql-db-sybasetosql"></a>Connexion à la base de données SQL Azure (SybaseToSQL)
Pour migrer des bases de données Sybase vers base de données SQL Azure, vous devez vous connecter à l’instance cible de la base de données SQL Azure. Lorsque vous vous connectez, SSMA Obtient les métadonnées relatives à toutes les bases de données dans l’instance de base de données SQL Azure et affiche les métadonnées de la base de données dans l’Explorateur de métadonnées de base de données SQL Azure. SSMA stocke les informations de l’instance de base de données SQL Azure que vous êtes connecté à, mais ne stockez pas les mots de passe.  
  
Votre connexion à la base de données SQL Azure reste active jusqu'à ce que vous fermez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter à la base de données SQL Azure si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu'à ce que le chargement des objets de base de données dans la base de données SQL Azure et de migrer les données.  
  
Métadonnées relatives à l’instance de base de données SQL Azure ne sont pas synchronisée automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans l’Explorateur de métadonnées de base de données SQL Azure, vous devez manuellement mettre à jour les métadonnées de la base de données SQL Azure. Pour plus d’informations, consultez la section « Synchronisation des métadonnées Azure SQL DB » plus loin dans cette rubrique.  
  
## <a name="required-azure-sql-db-permissions"></a>Autorisations de base de données SQL Azure requises  
Le compte qui est utilisé pour se connecter à la base de données SQL Azure nécessite des autorisations différentes selon les actions réalisées par le compte :  
  
1.  Pour convertir des objets de Sybase vers [!INCLUDE[tsql](../../includes/tsql_md.md)] syntaxe, pour mettre à jour les métadonnées à partir de la base de données SQL Azure, ou pour enregistrer la syntaxe converti pour les scripts, le compte doit avoir l’autorisation de se connecter à l’instance de base de données SQL Azure.  
  
2.  Pour charger des objets de base de données dans la base de données SQL Azure, la spécification de l’autorisation minimale est l’appartenance à la **db_owner** rôle de base de données dans la base de données cible.  
  
## <a name="establishing-a-azure-sql-db-connection"></a>L’établissement d’une connexion de base de données SQL Azure  
Avant de convertir des objets de base de données Sybase à la syntaxe de base de données SQL Azure, vous devez établir une connexion à l’instance de base de données SQL Azure dans lequel vous souhaitez migrer les bases de données ou la base de données Sybase.  
  
Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données où les objets et les données seront migrées. Vous pouvez personnaliser ce mappage au niveau du schéma de Sybase après la connexion à la base de données SQL Azure. Pour plus d’informations, consultez [mappage Sybase ASE schémas pour les schémas SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md)  
  
> [!WARNING]  
> Avant d’essayer de se connecter à la base de données SQL Azure, assurez-vous que l’instance de base de données SQL Azure est en cours d’exécution et peut accepter des connexions.  
  
**Pour vous connecter à la base de données SQL Azure**  
  
1.  Sur le **fichier** menu, sélectionnez **se connecter à la base de données SQL Azure**(cette option est activée après la création d’un projet).  
  
    Si vous êtes jamais connecté à la base de données SQL Azure, le nom de la commande sera **se reconnecter à la base de données SQL Azure**  
  
2.  Dans la boîte de dialogue de connexion, entrez ou sélectionnez le nom du serveur de base de données SQL Azure.  
  
3.  Entrez, sélectionnez ou **Parcourir** le nom de la base de données.  
  
4.  Entrez ou sélectionnez **nom d’utilisateur**.  
  
5.  Entrez le **mot de passe**.  
  
6.  SSMA recommande une connexion chiffrée à la base de données SQL Azure.  
  
7.  Cliquez sur **Se connecter**.  
  
> [!IMPORTANT]  
> SSMA pour Sybase ne prend pas en charge la connexion à **master** base de données dans la base de données SQL Azure.  
  
## <a name="synchronizing-azure-sql-db-metadata"></a>Synchronisation des métadonnées de base de données SQL Azure  
Les métadonnées sur les bases de données de base de données SQL Azure ne sont pas automatiquement mis à jour. Les métadonnées dans l’Explorateur de métadonnées de base de données SQL Azure sont un instantané des métadonnées lorsque vous tout d’abord connecté à la base de données SQL Azure, ou la dernière que vous mettre manuellement à jour des métadonnées. Vous pouvez mettre à jour manuellement les métadonnées pour toutes les bases de données, ou pour tout de la base de données unique ou d’un objet de base de données.  
  
**Pour synchroniser les métadonnées**  
  
1.  Assurez-vous que vous êtes connecté à la base de données SQL Azure.  
  
2.  Dans l’Explorateur de métadonnées de base de données SQL de Azure, sélectionnez la case à cocher en regard de la base de données ou le schéma de base de données que vous souhaitez mettre à jour.  
  
    Par exemple, pour mettre à jour les métadonnées de toutes les bases de données, activez la case en regard de bases de données.  
  
3.  Cliquez sur les bases de données, ou l’individuel de la base de données ou le schéma de base de données, puis sélectionnez **synchroniser avec la base de données**.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage entre les schémas de Sybase et les bases de données de base de données SQL Azure et les schémas, consultez [les schémas de mappage Sybase ASE à des schémas SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md)  
  
-   Pour personnaliser les options de configuration pour les projets, consultez [définition des Options de projet &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md)  
  
-   Pour personnaliser le mappage des types de données source et cible, consultez [mappage Sybase ASE et Types de données SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md)  
  
-   Si vous n’avez pas à effectuer ces tâches, vous pouvez convertir les définitions d’objets de base de données Sybase dans les définitions d’objets de base de données SQL Azure. Pour plus d’informations, consultez [convertir les objets de base de données Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de Sybase ASE à SQL Server - base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
