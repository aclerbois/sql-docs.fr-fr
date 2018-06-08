---
title: Champs de descripteur de paramètre table | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-table-valued-parameters
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), descriptor fields
ms.assetid: 4e009eff-c156-4d63-abcf-082ddd304de2
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6205ed15e0771cc1900a458343defbbcb0e1384f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-valued-parameter-descriptor-fields"></a>Champs de descripteur de paramètre table
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  La prise en charge des paramètres table inclut de nouveaux champs spécifiques à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans les descripteurs de paramètre d'application (APD) ODBC et les descripteurs de paramètre d'implémentation (IPD).  
  
## <a name="remarks"></a>Notes  
  
|Nom|Emplacement|Type| Description|  
|----------|--------------|----------|-----------------|  
|SQL_CA_SS_TYPE_NAME|IPD|SQLTCHAR*|Nom du type de serveur du paramètre table.<br /><br /> Lorsqu’un nom de type de paramètre table est spécifié sur un appel à SQLBindParameter, il doit toujours être spécifié comme une valeur Unicode, même dans les applications qui sont générées comme applications ANSI. La valeur utilisée pour le paramètre *StrLen_or_IndPtr* doit être SQL_NTS ou la longueur de chaîne du nom multipliée par sizeof (WCHAR).<br /><br /> Lorsqu’un nom de type de paramètre table est spécifié via SQLSetDescField, il peut être spécifié à l’aide d’un littéral conforme à la façon dont l’application est créé. Le Gestionnaire de pilotes ODBC effectuera toute conversion Unicode requise.|  
|SQL_CA_SS_TYPE_CATALOG_NAME (lecture seule)|IPD|SQLTCHAR*|Catalogue où le type est défini.|  
|SQL_CA_SS_TYPE_SCHEMA_NAME|IPD|SQLTCHAR*|Schéma où le type est défini.|  
  
 Les applications ne doivent pas définir SQL_CA_SS_TYPE_CATALOG_NAME pour les paramètres table. Si tel est le cas, SQL_ERROR est retourné et un enregistrement de diagnostic est journalisé avec SQLSTATE = HY091 et le message « Identificateur de champ de descripteur non valide ».  
  
 Les attributs d'instruction et les champs d'en-tête de descripteur suivants s'appliquent aux paramètres table lorsque le focus de paramètre est défini sur un paramètre table :  
  
|Nom|Emplacement|Type| Description|  
|----------|--------------|----------|-----------------|  
|SQL_ATTR_PARAMSET_SIZE<br /><br /> (Équivaut à SQL_DESC_ARRAY_SIZE dans le descripteur APD.)|APD|SQLUINTEGER|Taille du tableau de mémoires tampon pour un paramètre table. Il s'agit du nombre maximal de lignes que les mémoires tampon peuvent prendre en charge ou de la taille des mémoires tampon dans les lignes ; la valeur du paramètre table peut elle-même avoir un nombre de lignes supérieur ou inférieur à la capacité des mémoires tampon. Valeur par défaut est 1.<br /><br /> Remarque : Si SQL_SOPT_SS_PARAM_FOCUS est défini sur sa valeur par défaut de 0, SQL_ATTR_PARAMSET_SIZE fait référence à l’instruction et spécifie le nombre de jeux de paramètres. Si SQL_SOPT_SS_PARAM_FOCUS est pour valeur l'ordinal d'un paramètre table, il fait référence au paramètre table et spécifie le nombre de lignes par jeu de paramètres pour le paramètre table.|  
|SQL_ATTR_PARAM _BIND_TYPE|APD|SQLINTEGER|La valeur par défaut est SQL_PARAM_BIND_BY_COLUMN.<br /><br /> Pour sélectionner la liaison selon les lignes, ce champ a pour valeur la longueur de la structure ou une instance d'une mémoire tampon qui sera liée à un jeu de lignes de paramètre table. Cette durée doit inclure l'espace pour toutes les colonnes dépendantes et tout remplissage de la structure ou de la mémoire tampon. Cela garantit que lorsque l'adresse d'une colonne dépendante est incrémentée de la longueur spécifiée, le résultat pointera vers le début de la même colonne dans la ligne suivante. Lorsque vous utilisez la **sizeof** opérateur en C ANSI, ce comportement est garanti.|  
|SQL_ATTR_PARAM_BIND_OFFSET_PTR|APD|SQLINTEGER*|La valeur par défaut est un pointeur null.<br /><br /> Si ce champ n'est pas NULL, le pilote déréférence le pointeur, ajoute la valeur déréférencée à chacun des champs différés dans l'enregistrement de descripteur (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR) et utilise les nouvelles valeurs de pointeur pour accéder aux valeurs de données.|  
  
 Ces champs sont uniquement valides avec des paramètres table et sont ignorés pour d'autres types de données.  
  
 SQL_CA_SS_TYPE_NAME est facultatif pour les appels de procédure stockée. Il doit être spécifié pour les instructions SQL qui ne sont pas des appels de procédure pour permettre au serveur de déterminer le type du paramètre table.  
  
 Si le nom de type est reqired et que le type de table pour le paramètre table est défini dans un schéma différent de la procédure stockée, SQL_CA_SS_TYPE_SCHEMA_NAME doit être spécifié dans le descripteur de paramètre d'implémentation (IPD). Sinon, le serveur ne pourra pas déterminer le type du paramètre table Cela entraîne une erreur lorsque vous appelez SQLExecute ou SQLExecDirect. L'erreur indiquera SQLSTATE = 07006 et le message « Violation de l'attribut de type de données restreint ».  
  
 Les colonnes de paramètre table peuvent utiliser la liaison selon les lignes ou selon les colonnes. La valeur par défaut est la liaison selon les colonnes. La liaison selon les lignes peut être spécifiée en définissant SQL_ATTR_PARAM_BIND_TYPE et SQL_ATTR_PARAM_BIND_OFFSET_PTR. Cela équivaut à la liaison selon les lignes de colonnes et de paramètres.  
  
 SQL_CA_SS_TYPE_CATALOG_NAME et SQL_CA_SS_TYPE_SCHEMA_NAME peuvent également être utilisés pour récupérer le catalogue et le schéma associés aux paramètres du type CLR défini par l'utilisateur. Ils constituent des alternatives aux attributs existants de schéma de catalogue spécifiques au type pour ces types.  
  
## <a name="see-also"></a>Voir aussi  
 [Paramètres table &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
