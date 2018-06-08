---
title: Modification du schéma d’une table temporelle à version contrôlée par le système | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 9dbe5a21-9335-4f8b-85fd-9da83df79946
caps.latest.revision: 13
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e370e98dbe0ce5c5f38cf4cac880abcb6500da5d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="changing-the-schema-of-a-system-versioned-temporal-table"></a>Modification du schéma d’une table temporelle à version contrôlée par le système
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Utilisez l’instruction **ALTER TABLE** pour ajouter, modifier ou supprimer une colonne.  
  
## <a name="examples"></a>Exemples  
 Voici quelques exemples illustrant comment modifier le schéma d’une table temporelle.  
  
```  
ALTER TABLE dbo.Department   
   ALTER COLUMN  DeptName varchar(100);   
  
ALTER TABLE dbo.Department   
   ADD WebAddress nvarchar(255) NOT NULL    
   CONSTRAINT DF_WebAddress DEFAULT 'www.mycompany.com';   
  
ALTER TABLE dbo.Department   
   ADD TempColumn INT;   
  
GO   
  
ALTER TABLE dbo.Department   
   DROP COLUMN TempColumn;  
  
/* Setting IsHidden property for period columns.   
Use ALTER COLUMN <period_column> DROP HIDDEN to clear IsHidden flag */  
  
ALTER TABLE dbo.Department   
   ALTER COLUMN SysStartTime ADD HIDDEN;   
  
ALTER TABLE dbo.Department   
   ALTER COLUMN SysEndTime ADD HIDDEN;  
  
```  
  
### <a name="important-remarks"></a>Remarques importantes  
  
-   L’autorisation**CONTROL** sur les tables actuelles et historiques est nécessaire pour modifier le schéma de la table temporelle.  
  
-   Pendant une opération **ALTER TABLE** , le système verrouille le schéma des deux tables.  
  
-   La modification de schéma spécifiée est propagée à la table historique de manière appropriée (selon le type de modification).  
  
-   Si vous ajoutez une colonne n’acceptant la valeur Null ou modifiez une colonne de sorte qu’elle n’accepte pas la valeur Null, vous devez spécifier la valeur par défaut des lignes existantes. Le système génère une valeur par défaut supplémentaire avec la même valeur et l’applique à la table historique. L’ajout de **DEFAULT** à une table non vide est une opération Taille des données dans toutes les éditions sauf dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition (pour laquelle c’est une opération de métadonnées).  
  
-   L’ajout de varchar(max), nvarchar(max), varbinary(max) ou de colonnes XML avec des valeurs par défaut est une opération de mise à jour des données dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Si la taille de ligne après l’ajout de colonne dépasse la limite, les nouvelles colonnes ne peuvent pas être ajoutées en ligne.  
  
-   Lorsque vous ajoutez à une table une colonne NOT NULL, envisagez de supprimer la contrainte par défaut sur la table historique, car toutes les colonnes de cette table sont automatiquement renseignées par le système.  
  
-   L’option Online (**WITH (ONLINE = ON**) n’a aucun effet sur **ALTER TABLE ALTER COLUMN** si la table temporelle a sa version contrôlée par le système. L’opération ALTER n’est pas effectuée en ligne, quelle que soit la valeur spécifiée pour l’option ONLINE.  
  
-   Vous pouvez utiliser **ALTER COLUMN** pour modifier la propriété **IsHidden** pour les colonnes de période.  
  
-   Vous ne pouvez pas utiliser directement **ALTER** pour les modifications de schéma suivantes. Pour ces types de modifications, définissez **SYSTEM_VERSIONING = OFF**.  
  
    -   Ajout d’une colonne calculée  
  
    -   Ajout d’une colonne **IDENTITY**  
  
    -   Ajout d’une colonne **SPARSE** ou modification d’une colonne en **SPARSE**lorsque la table historique est configurée avec **DATA_COMPRESSION = PAGE** ou **DATA_COMPRESSION = ROW**, qui est la valeur par défaut pour la table historique.  
  
    -   Ajout d’un **COLUMN_SET**  
  
    -   Ajout d’une colonne **ROWGUIDCOL** ou modification d’une colonne en **ROWGUIDCOL**  
  
         L’exemple suivant illustre la modification du schéma où le paramètre **SYSTEM_VERSIONING = OFF** est toujours requis (ajout de la colonne **IDENTITY** ).   
        Notez que cet exemple désactive la vérification de la cohérence des données. Cette vérification n’est pas nécessaire lorsque la modification du schéma s’effectue dans une transaction, car aucune modification simultanée de données n’est possible.  
  
        ```  
        BEGIN TRAN   
        ALTER TABLE [dbo].[CompanyLocation] SET (SYSTEM_VERSIONING = OFF);   
        ALTER TABLE [CompanyLocation] ADD Cntr INT IDENTITY (1,1);   
        ALTER TABLE [dbo].[CompanyLocationHistory] ADD Cntr INT NOT NULL DEFAULT 0;   
        ALTER TABLE [dbo].[CompanyLocation]    
        SET    
        (   
        SYSTEM_VERSIONING = ON (HISTORY_TABLE = [dbo].[CompanyLocationHistory])   
        );   
        COMMIT ;  
  
        ```  
  
 
## <a name="see-also"></a> Voir aussi  
 [Tables temporelles](../../relational-databases/tables/temporal-tables.md)   
 [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)   
 [Gérer la rétention des données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)   
 [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [Création d’une table temporelle avec contrôle de version du système](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)   
 [Modification des données dans une table temporelle avec système par version](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)   
 [Interrogation des données dans une table temporelle avec système par version](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)   
 [Arrêt du contrôle de version du système sur une table temporelle avec contrôle de version par le système](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)  
  
  
