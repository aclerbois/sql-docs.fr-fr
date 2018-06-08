---
title: Type de données d’utilisation | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-results
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- ODBC data types
- ODBC data types, about ODBC data types
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC]
- SQL Server Native Client ODBC driver, data types
- data types [ODBC], about data types
ms.assetid: 4f19b0d6-94ac-4a98-a121-57d38787864c
caps.latest.revision: 34
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e8a08f91688a4a6c26fd138de51ee50b7b2a3974
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-usage"></a>Utilisation des types de données
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] imposent l’utilisation suivante des types de données.  
  
|Type de données|Limitation|  
|---------------|----------------|  
|Littéraux de date|Littéraux de date lorsque stockées dans une colonne SQL_TYPE_TIMESTAMP ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données de **datetime** ou **smalldatetime**), ont une valeur d’heure de 12:00:00.000 du matin.|  
|**Money** et **smallmoney**|Seules les parties de l’entier de la **money** et **smallmoney** des types de données sont significatifs. Si la partie décimale de SQL **money** les données sont tronquées lors de la conversion de type de données, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client retourne un avertissement, et non une erreur.|  
|SQL_BINARY (accepte les valeurs NULL)|En cas de connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 et versions antérieures, si une colonne SQL_BINARY accepte les valeurs NULL, les données stockées dans la source de données ne sont pas complétées avec des zéros. Lorsque des données à partir d’une telle colonne sont récupérées, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client complète avec des zéros à droite. Toutefois, les données créées dans des opérations effectuées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que la concaténation, ne font pas l'objet d'un tel remplissage.<br /><br /> Par ailleurs, lorsque des données sont placées dans une telle colonne dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 ou versions antérieures, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tronque les données à droite si elles sont trop longues pour tenir dans la colonne.<br /><br /> Remarque : Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne prend pas en charge la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 et versions antérieures.|  
|SQL_CHAR (troncation)|En cas de connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 et versions antérieures, lorsque les données sont placées dans une colonne SQL_CHAR, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les tronque à droite sans avertissement si les données sont trop longues pour tenir dans la colonne.<br /><br /> Remarque : Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne prend pas en charge la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 et versions antérieures.|  
|SQL_CHAR (accepte les valeurs NULL)|En cas de connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.0 et versions antérieure, si une colonne SQL_CHAR accepte les valeurs NULL, les données stockées dans la source de données ne sont pas complétées avec des espaces. Lorsque des données à partir d’une telle colonne sont récupérées, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client complète avec des espaces à droite. Toutefois, les données créées dans des opérations effectuées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que la concaténation, ne font pas l'objet d'un tel remplissage.<br /><br /> Remarque : Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne prend pas en charge la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 et versions antérieures.|  
|SQL_LONGVARBINARY, SQL_LONGVARCHAR, SQL_WLONGVARCHAR|Mises à jour des colonnes avec SQL_LONGVARBINARY, SQL_LONGVARCHAR ou SQL_WLONGVARCHAR types de données (à l’aide d’une clause WHERE) qui affectent plusieurs lignes sont entièrement pris en charge lorsqu’il est connecté à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6. *x* et versions ultérieures. Lorsqu’il est connecté à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4.2*x*, une erreur S1000, un « insertion/mise à jour partielle. L'insertion ou la mise à jour d'un texte ou de colonnes d'images n'a pas abouti. » est retournée si la mise à jour affecte plusieurs lignes.<br /><br /> Remarque : Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne prend pas en charge la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 et versions antérieures.|  
|Paramètres de fonction de chaîne|*exp_chaîne* paramètres à la chaîne de fonctions doivent être des données de type SQL_CHAR ou SQL_VARCHAR. Les types de données SQL_LONG_VARCHAR ne sont pas pris en charge dans les fonctions de chaîne. Le *nombre* paramètre doit être inférieure ou égale à 8 000, car les types de données SQL_CHAR et SQL_VARCHAR sont limitées à une longueur maximale de 8 000 caractères.|  
|Littéraux d'heure|Littéraux d’heure lorsque stockées dans une colonne SQL_TIMESTAMP ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données de **datetime** ou **smalldatetime**), ont une valeur de date du 1er janvier 1900.|  
|**timestamp**|Seule la valeur NULL peut être insérée manuellement dans un **timestamp** colonne. Toutefois, étant donné que **timestamp**les colonnes sont automatiquement mis à jour par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une valeur NULL est remplacée.|  
|**tinyint**|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **tinyint** type de données n’est pas signé. A **tinyint** colonne est liée à une variable de type de données SQL_C_UTINYINT par défaut.|  
|Types de données alias|Lorsqu’il est connecté à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4.2*x*, le pilote ODBC ajoute la valeur NULL à une définition de colonne qui ne déclare pas explicitement la possibilité de valeur NULL d’une colonne. Par conséquent, la possibilité de valeur NULL stockée dans la définition d'un type de données alias est ignorée.<br /><br /> Lorsqu’il est connecté à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 4.2*x*, type de colonnes avec un type de données alias qui a une base de données de **char** ou **binaire** et pour lequel aucune possibilité de valeur NULL n’est déclarée sont créés en tant que type de données **varchar** ou **varbinary**. [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md), [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md), et [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) retournent SQL_VARCHAR ou SQL_VARBINARY comme données de type pour ces colonnes. Les données qui sont récupérées de ces colonnes ne sont pas complétées.<br /><br /> Remarque : Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne prend pas en charge la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 et versions antérieures.|  
|Types de données LONG|*Data-at-execution* paramètres sont limités pour les types de données SQL_LONGVARCHAR et SQL_LONGVARBINARY.|  
|Types de valeur élevée|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client expose **varchar (max)**, **varbinary (max)**, et **nvarchar (max)** types en tant que SQL_VARCHAR, SQL_VARBINARY et SQL_WVARCHAR (respectivement) dans les API qui acceptent ou retournent des types de données ODBC SQL.|  
|Type défini par l'utilisateur (UDT)|Les colonnes UDT sont mappées en tant que SQL_SS_UDT. Si une colonne UDT est mappée explicitement à un autre type dans l'instruction SQL à l'aide des méthodes ToString() ou ToXMLString() du type UDT, ou via les fonctions CAST/CONVERT, le type de la colonne dans le jeu de résultats reflète le type réel vers lequel la colonne a été convertie.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne peut se lier à une colonne UDT sous forme binaire. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend uniquement en charge la conversion entre les types de données SQL_SS_UDT et SQL_C_BINARY.|  
|XML|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit automatiquement le code XML en texte Unicode. Le type XML est mappé en tant que SQL_SS_XML.|  
  
## <a name="see-also"></a>Voir aussi  
 [Le traitement des résultats &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
