---
title: table_constraint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CONSTRAINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table_constraint
ms.assetid: ac2a11e0-cc77-4e27-b107-4fe5bc6f5195
caps.latest.revision: 59
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ecd1872760fcfb9a54b9365b0e72a1c9e3ad8083
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-table-tableconstraint-transact-sql"></a>ALTER TABLE table_constraint (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Spécifie les propriétés d’une contrainte PRIMARY KEY, UNIQUE, FOREIGN KEY ou CHECK, ou encore une définition DEFAULT ajoutée à une table par le biais de l’instruction [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
[ CONSTRAINT constraint_name ]   
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        (column [ ASC | DESC ] [ ,...n ] )  
        [ WITH FILLFACTOR = fillfactor   
        [ WITH ( <index_option>[ , ...n ] ) ]  
        [ ON { partition_scheme_name ( partition_column_name ... )  
          | filegroup | "default" } ]   
    | FOREIGN KEY   
        ( column [ ,...n ] )  
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]   
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]   
        [ NOT FOR REPLICATION ]   
    | DEFAULT constant_expression FOR column [ WITH VALUES ]   
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )  
}  
```  
  
## <a name="arguments"></a>Arguments  
 CONSTRAINT  
 Spécifie le début d'une définition de contrainte PRIMARY KEY, UNIQUE, FOREIGN KEY ou CHECK, ou d'une définition DEFAULT.  
  
 *constraint_name*  
 Nom de la contrainte. Les noms de contrainte doivent suivre les règles des [identificateurs](../../relational-databases/databases/database-identifiers.md), sauf que le nom ne peut pas commencer par un signe dièse (#). En l'absence de constraint_name, un nom généré par le système est affecté à la contrainte.  
  
 PRIMARY KEY  
 Contrainte garantissant l’intégrité de l’entité d’une ou de plusieurs colonnes spécifiées au moyen d’un index unique. Une seule contrainte PRIMARY KEY peut être créée par table.  
  
 UNIQUE  
 Contrainte assurant l'intégrité de l'entité d'une colonne ou de plusieurs colonnes données au moyen d'un seul index.  
  
 CLUSTERED et NONCLUSTERED  
 Spécifie qu'un index, cluster ou non-cluster, est créé pour la contrainte PRIMARY KEY ou UNIQUE. La valeur par défaut des contraintes PRIMARY KEY est CLUSTERED. La valeur par défaut des contraintes UNIQUE est, elle, NONCLUSTERED.  
  
 Si une contrainte ou un index cluster existe déjà pour la table, vous ne pouvez pas spécifier CLUSTERED. Dans ce cas, l'option par défaut d'une contrainte PRIMARY KEY devient NONCLUSTERED.  
  
 Les colonnes dont le type de données est **ntext**, **text**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml** ou **image** ne peuvent pas être spécifiées comme colonnes pour un index.  
  
 *column*  
 Colonne ou liste de colonnes spécifiées entre parenthèses, utilisée dans une nouvelle contrainte.  
  
 [ **ASC** | DESC ]  
 Indique l'ordre de tri de la ou des colonnes impliquées dans les contraintes de table. La valeur par défaut est ASC.  
  
 WITH FILLFACTOR **=***fillfactor*  
 Spécifie le remplissage par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] des pages d'index utilisées pour stocker les données d'index. Les valeurs *fillfactor* spécifiées par l’utilisateur doivent être comprises entre 1 et 100. Si aucune valeur n'est spécifiée, la valeur par défaut est 0.  
  
> [!IMPORTANT]  
>  Dans la documentation, l’indication que WITH FILLFACTOR = *fillfactor* constitue l’unique option d’indexation s’appliquant aux contraintes PRIMARY KEY ou UNIQUE est maintenue dans un but de compatibilité ascendante, mais ne sera plus indiquée ainsi dans les versions à venir. D’autres options d’indexation peuvent être spécifiées dans la clause [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md) d’ALTER TABLE.  
  
 ON { *partition_scheme_name ***(*** partition_column_name***)** | *filegroup*| **"** default **"** }  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie le lieu de stockage de l'index créé pour la contrainte. Si *partition_scheme_name* est spécifié, l’index est partitionné et les partitions qui en découlent sont mappées aux groupes de fichiers indiqués par *partition_scheme_name*. Si *filegroup* est spécifié, l’index est créé dans le groupe de fichiers nommé. Si **"** default **"** est spécifié ou si l’option ON n’est pas du tout définie, l’index est créé dans le même groupe de fichiers que celui de la table. Si l'option ON est spécifiée lors de l'ajout d'un index cluster pour une contrainte PRIMARY KEY ou UNIQUE, la table dans son intégralité est déplacée dans le groupe de fichiers spécifié lorsque l'index cluster est créé.  
  
 Dans ce contexte, default ne constitue pas un mot clé. Il correspond plutôt à un identificateur du groupe de fichiers par défaut et doit être délimité, comme dans ON **"** default **"** ou ON **[** default **]**. Si **"** default **"** est spécifié, l’option QUOTED_IDENTIFIER doit être ON pour la session active. Il s'agit du paramètre par défaut.  
  
 FOREIGN KEY REFERENCES  
 Contrainte assurant l'intégrité référentielle des données d'une colonne. Une contrainte FOREIGN KEY exige que chaque valeur de la colonne existe dans la colonne spécifiée de la table référencée.  
  
 *referenced_table_name*  
 Table référencée par la contrainte FOREIGN KEY.  
  
 *ref_column*  
 Colonne ou liste de colonnes, entre parenthèses, référencée par la nouvelle contrainte FOREIGN KEY.  
  
 ON DELETE { **NO ACTION** | CASCADE | SET NULL | SET DEFAULT }  
 Spécifie l'action qui se produit dans les lignes de la table modifiée, si ces lignes comportent une relation référentielle et que la ligne référencée est supprimée de la table parent. La valeur par défaut est NO ACTION.  
  
 NO ACTION  
 Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] déclenche une erreur et la suppression de la ligne dans la table parent est annulée.  
  
 CASCADE  
 Les lignes correspondantes sont supprimées de la table de référence pour celles supprimées de la table parent.  
  
 SET NULL  
 Toutes les valeurs composant la clé étrangère sont définies sur NULL si la ligne correspondante se trouvant à l'origine dans la table parent est supprimée. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent pouvoir cependant être définies sur NULL.  
  
 SET DEFAULT  
 Toutes les valeurs composant la clé étrangère sont définies sur leur valeur par défaut si la ligne correspondante se trouvant à l'origine dans la table parent est supprimée. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent disposer cependant de valeur par défaut. Si une colonne peut être affectée de la valeur NULL et qu'aucune valeur par défaut n'est définie, NULL constitue alors la valeur par défaut de la colonne de façon implicite.  
  
 Ne spécifiez pas CASCADE si la table est incluse dans une publication de fusion qui utilise des enregistrements logiques. Pour plus d’informations sur les enregistrements logiques, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 L'action ON DELETE CASCADE ne peut pas être définie si un déclencheur ON DELETE de l'option INSTEAD OF existe déjà dans la table en cours de modification.  
  
 Par exemple, dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la table **ProductVendor** a une relation référentielle avec la table **Vendor**. La clé étrangère **ProductVendor.VendorID** référence la clé primaire **Vendor.VendorID**.  
  
 Si une instruction DELETE est exécutée sur une ligne de la table **Vendor** et qu’une action ON DELETE CASCADE est spécifiée pour **ProductVendor.VendorID**, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie la présence des lignes dépendantes dans la table **ProductVendor**. Si elle existe, les lignes dépendantes se trouvant dans la table **ProductVendor** sont supprimées, ainsi que chaque ligne référencée correspondante dans la table **Vendor**.  
  
 En revanche, si la valeur NO ACTION est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur et restaure l’action de suppression sur la ligne de la table **Vendor** quand au moins une ligne de la table **ProductVendor** y fait référence.  
  
 ON UPDATE { **NO ACTION** | CASCADE | SET NULL | SET DEFAULT }  
 Spécifie l'action qui se produit sur les lignes de la table modifiée, si chacune de ces lignes possède une relation référentielle et que la ligne référencée correspondante est mise à jour dans la table parent. La valeur par défaut est NO ACTION.  
  
 NO ACTION  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] déclenche une erreur et la mise à jour de la ligne dans la table parente est restaurée.  
  
 CASCADE  
 Les lignes correspondantes sont mises à jour dans la table de référence si la ligne de la table parent est mise à jour.  
  
 SET NULL  
 Toutes les valeurs composant la clé étrangère sont définies sur NULL si la ligne correspondante se trouvant à l'origine dans la table parent est mise à jour. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent pouvoir cependant être définies sur NULL.  
  
 SET DEFAULT  
 Toutes les valeurs composant la clé étrangère sont définies sur leur valeur par défaut si la ligne correspondante se trouvant à l'origine dans la table parent est mise à jour. Pour que cette contrainte s'applique, les colonnes clés étrangères doivent disposer cependant de valeur par défaut. Si une colonne peut être affectée de la valeur NULL et qu'aucune valeur par défaut n'est définie, NULL constitue alors la valeur par défaut de la colonne de façon implicite.  
  
 Ne spécifiez pas CASCADE si la table est incluse dans une publication de fusion qui utilise des enregistrements logiques. Pour plus d’informations sur les enregistrements logiques, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 L'action ON UPDATE CASCADE, SET NULL ou SET DEFAULT ne peut pas être définie si le déclencheur ON UPDATE de l'option INSTEAD OF existe déjà dans la table en cours de modification.  
  
 Par exemple, dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la table **ProductVendor** a une relation référentielle avec la table **Vendor**. La clé étrangère **ProductVendor.VendorID** référence la clé primaire **Vendor.VendorID**.  
  
 Si une instruction UPDATE est exécutée sur une ligne de la table **Vendor** et qu’une action ON UPDATE CASCADE est spécifiée pour **ProductVendor.VendorID**, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie la présence des lignes dépendantes dans la table **ProductVendor**. Si elle existe, la ligne dépendante se trouvant dans la table **ProductVendor** est alors mise à jour en plus de la ligne référencée correspondante dans la table **Vendor**.  
  
 Par contre, si la valeur NO ACTION est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] déclenche une erreur et annule l’action de mise à jour de chaque ligne dans la table **Vendor** si au moins une ligne fait référence à chaque ligne dans la table **ProductVendor**.  
  
 NOT FOR REPLICATION  
 **S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Peut être indiqué aussi bien pour les contraintes FOREIGN KEY que CHECK. Si cette clause est spécifiée pour une contrainte, la contrainte n'est pas appliquée lorsque les agents de réplication effectuent des opérations d'insertion, de mise à jour ou de suppression.  
  
 DEFAULT  
 Spécifie la valeur par défaut de la colonne. Une définition DEFAULT peut servir à définir les valeurs d'une nouvelle colonne dans les lignes de données existantes. Elle ne peut pas être ajoutée à une colonne de type **timestamp**, à une propriété IDENTITY, à une définition DEFAULT existante ou à une valeur par défaut liée. Si la colonne a déjà une valeur par défaut, celle-ci doit être supprimée avant que la nouvelle valeur par défaut puisse être ajoutée. Si une valeur par défaut est spécifiée pour une colonne de type défini par l’utilisateur, le type doit prendre en charge une conversion implicite de *constant_expression* vers le type défini par l’utilisateur. Pour maintenir la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un nom de contrainte peut être affecté à une définition DEFAULT.  
  
 *constant_expression*  
 Valeur littérale, valeur NULL ou fonction système utilisée comme valeur par défaut de la colonne. Si *constant_expression* est utilisé avec une colonne de type personnalisé [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], l’implémentation de ce dernier doit prendre en charge une conversion implicite de *constant_expression* vers le type de données personnalisé.  
  
 FOR *column*  
 Spécifie la colonne associée à une définition DEFAULT au niveau de la table.  
  
 WITH VALUES  
 Spécifie que la valeur donnée dans DEFAULT *constant_expression* est stockée dans une nouvelle colonne ajoutée aux lignes existantes. WITH VALUES peut uniquement être spécifiée si l'option DEFAULT est spécifiée dans une clause ADD COLUMN. Si la colonne ajoutée accepte les valeurs NULL et que l'option WITH VALUES est spécifiée, la valeur par défaut est stockée dans la nouvelle colonne ajoutée aux lignes existantes. Si WITH VALUES n'est pas indiquée pour des colonnes acceptant les valeurs NULL, la valeur NULL est stockée dans la nouvelle colonne des lignes existantes. Si la nouvelle colonne n'accepte pas les valeurs NULL, la valeur par défaut est stockée dans les nouvelles lignes, que l'option WITH VALUES soit spécifiée ou pas.  
  
 CHECK  
 Contrainte qui assure l'intégrité du domaine en limitant les valeurs possibles pouvant être entrées dans une ou plusieurs colonnes.  
  
 *logical_expression*  
 Expression logique utilisée dans une contrainte CHECK et retournant TRUE ou FALSE. L’argument *logical_expression* utilisé avec les contraintes CHECK ne peut pas référencer une autre table, mais peut référencer d’autres colonnes de la même table pour la même ligne. L'expression ne peut pas faire référence à un type de données alias.  
  
## <a name="remarks"></a>Notes   
 Lorsque des contraintes CHECK ou FOREIGN KEY sont ajoutées, toutes les données existantes sont vérifiées afin de détecter d'éventuelles violations de contraintes, à moins que l'option WITH NOCHECK ne soit spécifiée. Si tel est le cas, l'instruction ALTER TABLE échoue et une erreur est renvoyée. Lorsqu'une nouvelle contrainte PRIMARY KEY ou UNIQUE est ajoutée à une colonne existante, les données de la ou des colonnes doivent être uniques. L'instruction ALTER TABLE échoue si des valeurs sont en double. L'option WITH NOCHECK n'a aucun effet lorsque vous ajoutez une contrainte PRIMARY KEY ou UNIQUE.  
  
 Chaque contrainte PRIMARY KEY et UNIQUE génère un index. Quel que soit le nombre de contraintes UNIQUE et PRIMARY KEY, le nombre d'index sur la table ne peut en aucun cas dépasser 999 index cluster et 1 index cluster. Les contraintes FOREIGN KEY ne génèrent pas automatiquement un index. Toutefois, les colonnes clés étrangères sont souvent employées dans les critères de jointure dans des requêtes grâce à la correspondance de la ou des colonnes de la contrainte de clé étrangère d'une table avec la ou les colonne(s) clé(s) unique(s) ou primaire(s) de l'autre table. Un index sur les colonnes clés étrangères permet au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de rechercher rapidement des données associées dans la table de clés étrangères.  
  
## <a name="examples"></a>Exemples  
 Pour obtenir des exemples, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
