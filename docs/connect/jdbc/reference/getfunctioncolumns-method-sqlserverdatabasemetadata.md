---
title: Méthode getFunctionColumns (SQLServerDatabaseMetaData) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e2b0e0f7-717c-48e6-bcd2-a325d938a833
caps.latest.revision: 27
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d0b3a45d117f665922b7078921aa9b8a2959717b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getfunctioncolumns-method-sqlserverdatabasemetadata"></a>Méthode getFunctionColumns (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une description des paramètres de fonctions système ou utilisateur du catalogue spécifié et un type de retour.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public ResultSet getFunctionColumns(java.lang.String catalog,  
                       java.lang.String schemaPattern,  
                       java.lang.String functionNamePattern  
                       java.lang.String columnNamePattern)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *catalog*  
  
 A **chaîne** qui contient le nom du catalogue. Si la chaîne est vide « », le résultat inclut les fonctions sans catalogue. S’il s’agit **null**, le nom du catalogue n’est pas utilisé pour la recherche.  
  
 *schemaPattern*  
  
 A **chaîne** qui contient le modèle de nom de schéma. Si la chaîne est vide « », le résultat inclut les fonctions sans schéma. S’il s’agit **null**, le nom de schéma n’est pas utilisé pour la recherche.  
  
 *functionNamePattern*  
  
 A **chaîne** qui contient le nom d’une fonction.  
  
 *columnNamePattern*  
  
 A **chaîne** qui contient le nom d’un paramètre.  
  
## <a name="return-value"></a>Valeur retournée  
 A [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getFunctionColumns est spécifiée par la méthode getFunctionColumns dans l’interface java.sql.DatabaseMetaData.  
  
 Cette méthode retourne uniquement les fonctions et paramètres qui correspondent au nom de schéma, de fonction et de paramètre spécifié avec le catalogue spécifié.  
  
 Chaque ligne du jeu de résultats inclut les colonnes suivantes pour une description de paramètre, une description de colonne ou un type de retour :  
  
|Nom|Type| Description|  
|----------|----------|-----------------|  
|FUNCTION_CAT|**String**|Nom de la base de données qui contient la fonction.|  
|FUNCTION_SCHEM|**String**|Schéma pour la fonction.|  
|FUNCTION_NAME|**String**|Nom de la fonction.|  
|COLUMN_NAME|**String**|Nom d'un paramètre ou d'une colonne.|  
|COLUMN_TYPE|**courte**|**Le type de la colonne. Il peut prendre l’une des valeurs suivantes :**<br /><br /> functionColumnUnknown (0) : type inconnu.<br /><br /> functionColumnIn (1) : paramètre d'entrée.<br /><br /> functionColumnInOut (2) : paramètre d'entrée/sortie.<br /><br /> functionColumnOut (3) : paramètre de sortie.<br /><br /> functionReturn (4) : valeur de retour de fonction.<br /><br /> functionColumnResult (5) : un paramètre ou une colonne est une colonne dans le jeu de résultats.|  
|DATA_TYPE|**smallint**|Valeur de Java.sql.Types du type de données SQL.|  
|TYPE_NAME|**String**|Nom du type de données.|  
|PRECISION|**int**|Nombre total de chiffres significatifs.|  
|LENGTH|**int**|La longueur des données en octets.|  
|SCALE|**courte**|Le nombre de chiffres à droite de la virgule décimale.|  
|RADIX|**courte**|Base pour les types numériques.|  
|NULLABLE|**courte**|Indique si la valeur de paramètre ou de retour peut contenir un **null** valeur.<br /><br /> **Il peut prendre l’une des valeurs suivantes :**<br /><br /> functionNoNulls (0) : la valeur NULL n'est pas autorisée.<br /><br /> functionNullable (1) : la valeur NULL est autorisée.<br /><br /> functionNullableUnknown (2) : inconnue.|  
|REMARKS|**String**|Commentaires sur une colonne ou un paramètre.|  
|COLUMN_DEF|**String**|La valeur par défaut de la colonne.<br /><br /> **Remarque :** ces informations sont disponibles avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] et n’est spécifique au pilote JDBC.|  
|SQL_DATA_TYPE|**smallint**|Cette colonne est le même que le **DATA_TYPE** colonne, à l’exception de la **datetime** et ISO **intervalle** des types de données.<br /><br /> **Remarque :** ces informations sont disponibles avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] et n’est spécifique au pilote JDBC.|  
|SQL_DATETIME_SUB|**smallint**|Le **datetime** ISO **intervalle** sous-code si la valeur de **SQL_DATA_TYPE** est **SQL_DATETIME** ou **SQL_INTERVAL**. Pour les données les types autres que **datetime** et ISO **intervalle**, cette colonne est NULL.<br /><br /> **Remarque :** ces informations sont disponibles avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] et n’est spécifique au pilote JDBC.|  
|CHAR_OCTET_LENGTH|**int**|Longueur maximale des paramètres ou colonnes binaires et basés sur des caractères. Pour les autres types de données, il s'agit de la valeur NULL.|  
|ORDINAL_POSITION|**int**|Pour les paramètres d'entrée et de sortie, elle représente la position commençant à 1.<br /><br /> Pour les colonnes de jeux de résultats, il s'agit de la position de la colonne dans le jeu de résultats commençant à 1.<br /><br /> Pour la valeur de retour, il s'agit de la valeur 0.|  
|IS_NULLABLE|**String**|Détermine l'acceptation des valeurs NULL par un paramètre ou une colonne.<br /><br /> Il peut prendre l’une des valeurs suivantes :<br /><br /> **Oui**: le paramètre ou la colonne peut inclure des valeurs NULL.<br /><br /> **Ne**: le paramètre ou la colonne ne peut pas inclure des valeurs NULL.<br /><br /> Chaîne vide (« ») : inconnue.|  
|SS_TYPE_CATALOG_NAME|**String**|Nom du catalogue qui contient le type défini par l'utilisateur (UDT).|  
|SS_TYPE_SCHEMA_NAME|**String**|Nom du schéma qui contient le type défini par l'utilisateur (UDT).|  
|SS_UDT_CATALOG_NAME|**String**|Type défini par l'utilisateur (UDT) du nom complet.|  
|SS_UDT_SCHEMA_NAME|**String**|Nom du catalogue dans lequel un nom de collection de schémas XML est défini. Si le nom du catalogue est introuvable, cette variable contient une chaîne vide.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|**String**|Nom du schéma dans lequel un nom de collection de schémas XML est défini. Si le nom du schéma est introuvable, la chaîne est vide.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|**String**|Nom d'une collection de schémas XML. Si le nom est introuvable, la chaîne est vide.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|**String**|Nom du catalogue qui contient le type défini par l'utilisateur (UDT).|  
|SS_XML_SCHEMACOLLECTION_NAME|**String**|Nom du schéma qui contient le type défini par l'utilisateur (UDT).|  
|SS_DATA_TYPE|**tinyint**|Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] type de données qui est utilisé par les procédures stockées étendues.<br /><br /> **Remarque** pour plus d’informations sur les types de données retournés par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)], consultez « Types de données (Transact-SQL) » dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] la documentation en ligne.|  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
