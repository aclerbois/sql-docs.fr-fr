---
title: sp_tableoption (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_tableoption_TSQL
- sp_tableoption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tableoption
ms.assetid: 0a57462c-1057-4c7d-bce3-852cc898341d
caps.latest.revision: 60
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 33027f15081102cca289923b858a4a960a1359e4
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptableoption-transact-sql"></a>sp_tableoption (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Définit les valeurs d'option des tables définies par l'utilisateur. sp_tableoption peut être utilisé pour contrôler le comportement dans la ligne des tables avec **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **xml**, **texte**, **ntext**, **image**, ou les colonnes de type défini par l’utilisateur volumineuses.  
  
> [!IMPORTANT]  
>  La fonctionnalité text in row sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour stocker les données de valeur élevée, nous vous recommandons d’utiliser de la **varchar (max)**, **nvarchar (max)** et **varbinary (max)** des types de données.  
  

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_tableoption [ @TableNamePattern = ] 'table'   
     , [ @OptionName = ] 'option_name'   
     ,[ @OptionValue =] 'value'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @TableNamePattern =] '*table*'  
 Spécifie le nom qualifié ou non d'une table de base de données définie par l'utilisateur. Si un nom de table complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données en cours. Vous ne pouvez pas définir simultanément les options des tables pour plusieurs tables. *table* est **nvarchar(776)**, sans valeur par défaut.  
  
 [ @OptionName =] '*option_name*'  
 Spécifie un nom d'option de table. *option_name* est **varchar (35)**, sans valeur par défaut NULL. *option_name* peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|table lock on bulk load|Désactivée (valeur par défaut), oblige le processus de chargement en masse effectué sur les tables définies par l'utilisateur à obtenir des verrous de lignes. Activée, oblige le processus de chargement en masse effectué sur les tables définies par l'utilisateur à obtenir un verrou de mise à jour en bloc.|  
|insert row lock|N'est plus pris en charge.<br /><br /> Cette option n'a aucun effet sur le comportement de verrouillage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et elle n'est incluse qu'à des fins de compatibilité des scripts et des procédures existants.|  
|text in row|Si la valeur est OFF ou 0 (désactivé, valeur par défaut), le comportement en cours n'est pas modifié, et la ligne ne contient pas d'objet BLOB.<br /><br /> Lorsque spécifié et @OptionValue a la valeur ON (activé) ou une valeur entière comprise entre 24 et 7000, les nouvelles **texte**, **ntext**, ou **image** chaînes sont stockées directement dans la ligne de données. Tous les objets BLOB (objet binaire volumineux : **texte**, **ntext**, ou **image** données) sont convertis au format text in row lorsque la valeur de l’objet BLOB est mise à jour. Pour plus d'informations, consultez la section Notes.|  
|large value types out of row|1 = **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **xml** et les colonnes de type volumineux définis par l’utilisateur (UDT) dans la table sont stockées hors ligne, avec un pointeur de 16 octets vers la racine.<br /><br /> 0 = **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **xml** et les valeurs UDT volumineuses sont stockées directement dans la ligne de données, jusqu'à une limite de 8 000 octets et tant que la valeur peut être contenue dans l’enregistrement. Si la valeur ne tient pas dans l'enregistrement, un pointeur est stocké dans la ligne et le reste est stocké hors de la ligne dans l'espace de stockage LOB. La valeur par défaut est 0.<br /><br /> Le type défini par l'utilisateur (UDT) volumineux s'applique à : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. <br /><br /> Utilisez l’option TEXTIMAGE_ON de [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) pour spécifier un emplacement de stockage des types de données volumineuses. |  
|format de stockage vardecimal|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Lorsque la valeur est TRUE, ON ou 1, la table désignée est activée pour le format de stockage vardecimal. Lorsque la valeur est FALSE, OFF ou 0, la table n'est pas activée pour le format de stockage vardecimal. Format de stockage VarDecimal peut être activé uniquement lorsque la base de données a été activée pour le format de stockage vardecimal à l’aide de [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md). Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, **vardecimal** le format de stockage est déconseillé. Utilisez plutôt la compression ROW. Pour plus d'informations, consultez [Data Compression](../../relational-databases/data-compression/data-compression.md). La valeur par défaut est 0.|  
  
 [ @OptionValue =] '*valeur*'  
 Est si le *option_name* est activé (TRUE, ON ou 1) ou désactivé (FALSE, OFF ou 0). *valeur* est **varchar(12)**, sans valeur par défaut. *valeur* respecte la casse.  
  
 Pour l'option text in row, les valeurs d'option valides sont 0, ON, OFF ou un entier compris entre 24 et 7 000. Lorsque *valeur* est activée, les valeurs par défaut de limite à 256 octets.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) ou numéro d'erreur (échec)  
  
## <a name="remarks"></a>Notes  
 La procédure sp_tableoption peut être utilisée uniquement pour définir les valeurs des options des tables définies par l'utilisateur. Pour afficher les propriétés des tables, utilisez OBJECTPROPERTY.  
  
 L'option text in row de sp_tableoption peut être activée ou désactivée uniquement pour les tables qui contiennent des colonnes de texte. Si la table ne contient pas de colonne de texte, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur.  
  
 Lorsque l’option text in row est activée, le @OptionValue paramètre permet aux utilisateurs de spécifier la taille maximale à stocker dans une ligne pour un objet BLOB. La valeur par défaut est de 256 octets, mais les valeurs peuvent être comprises entre 24 et 7 000 octets.  
  
 **texte**, **ntext**, ou **image** chaînes sont stockées dans la ligne de données si les conditions suivantes s’appliquent :  
  
-   l'option text in row est activée ;  
  
-   La longueur de la chaîne est plus courte que la limite spécifiée dans @OptionValue  
  
-   l'espace disque disponible s'avère suffisant dans la ligne de données.  
  
 Lorsque les chaînes BLOB sont stockées dans la ligne de données, lire et écrire le **texte**, **ntext**, ou **image** les chaînes peuvent être aussi rapides que la lecture ou écriture de chaînes de caractères et binaires. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas besoin d'accéder à des pages séparées pour lire ou écrire la chaîne BLOB.  
  
 Si un **texte**, **ntext**, ou **image** chaîne est supérieure à la limite spécifiée ou de l’espace disponible dans la ligne, les pointeurs sont stockés dans la ligne à la place. Les conditions concernant le stockage des chaînes BLOB dans la ligne sont toujours applicables : la ligne de données doit disposer d'un espace suffisant pour contenir les pointeurs.  
  
 Les chaînes d'objets BLOB et les pointeurs stockés dans la ligne d'une table sont considérés comme des chaînes de longueur variable. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'utilise que le nombre d'octets nécessaires au stockage de la chaîne ou du pointeur.  
  
 Les chaînes d'objets BLOB existantes ne sont pas converties immédiatement lorsque l'option text in row est activée pour la première fois. Ces chaînes ne sont converties que lors de leur mise à jour. De même, lorsque le texte de la limite est augmenté, les **texte**, **ntext**, ou **image** chaînes déjà dans la ligne de données ne pas être converties à la nouvelle limite le moment où ils sont mis à jour.  
  
> [!NOTE]  
>  La désactivation de l'option text in row ou la réduction de sa limite nécessite la conversion de tous les objets BLOB, ce qui peut rallonger le processus, en fonction du nombre de chaînes d'objets BLOB à convertir. La table est verrouillée au cours du processus de conversion.  
  
 Une variable de table, comprenant une fonction chargée de retourner une variable de table, possède automatiquement l'option text in row activée avec une limite incluse par défaut de 256 octets. Cette option ne peut pas être modifiée.  
  
 L’option text in row prend en charge les fonctions TEXTPTR, WRITETEXT, UPDATETEXT et READTEXT. Les utilisateurs peuvent lire des parties d'objet BLOB avec la fonction SUBSTRING(), mais sans oublier que les pointeurs de texte en ligne ont des durées et des limites différentes des autres pointeurs de texte.  
  
 Pour rétablir une table du format de stockage vardecimal au format de stockage décimal normal, la base de données doit être en mode de récupération SIMPLE. Le changement de mode de récupération va rompre la séquence de journaux de transactions consécutifs à des fins de sauvegarde. Par conséquent, vous devez créer une sauvegarde de base de données complète après avoir supprimé le format de stockage vardecimal d'une table.  
  
 Si vous convertissez une existant LOB colonne de type (text, ntext ou image) en types de valeurs élevées de petite à moyenne (varchar (max), nvarchar (max), ou varbinary et la plupart des instructions ne font pas référence les colonnes de type de valeur élevée dans votre environnement, envisagez de **large_value_types_out_of_row** à **1** pour obtenir des performances optimales. Lorsque le **large_value_types_out_of_row** valeur de l’option est modifié, existant varchar (max), nvarchar (max), varbinary (max), et les valeurs xml ne sont pas immédiatement converties. Le stockage des chaînes est modifié lorsqu'elles sont mises à jour. Les nouvelles valeurs insérées dans une table sont stockées en fonction de l'option de table active. Pour des résultats immédiats, soit effectuer une copie des données et reremplissez la table après avoir modifié le **large_value_types_out_of_row** définition ou de mettre à jour chaque colonne de types de valeurs élevées de petite à moyenne à elle-même, afin que le stockage des chaînes est modifié avec l’option de table en vigueur. Vous pouvez également recréer les index sur la table après la mise à jour ou le nouveau remplissage afin de condenser la table. 
    
  
## <a name="permissions"></a>Autorisations  
 L'exécution de sp_tableoption nécessite une autorisation ALTER sur la table.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-storing-xml-data-out-of-the-row"></a>A. Stockage des données xml hors de la ligne  
 L’exemple suivant spécifie que le **xml** les données dans le `HumanResources.JobCandidate` table doivent être stockées hors ligne.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'HumanResources.JobCandidate', 'large value types out of row', 1;  
```  
  
### <a name="b-enabling-vardecimal-storage-format-on-a-table"></a>B. Activation du format de stockage vardecimal sur une table  
 L’exemple suivant modifie le `Production.WorkOrderRouting` table pour stocker le `decimal` de type de données dans le `vardecimal` le format de stockage.  

```sql  
USE master;  
GO  
-- The database must be enabled for vardecimal storage format  
-- before a table can be enabled for vardecimal storage format  
EXEC sp_db_vardecimal_storage_format 'AdventureWorks2012', 'ON';  
GO  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'Production.WorkOrderRouting',   
   'vardecimal storage format', 'ON';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
