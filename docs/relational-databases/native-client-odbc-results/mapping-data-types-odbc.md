---
title: Mappage des Types de données (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-results
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- mapping data types [ODBC]
- result sets [ODBC], data types
- ODBC data types, mapping
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC], mapping
- sql_variant data type
- SQL Server Native Client ODBC driver, data types
ms.assetid: 4ba0924d-9fca-4c48-aced-0a8d817b3dde
caps.latest.revision: 33
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 2fa9ef65906c70a3542fbdd617661c59a64bbad9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-data-types-odbc"></a>Mappage de types de données (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] maps de pilote ODBC Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données SQL aux types de données ODBC SQL. Les sections suivantes traitent des types de données SQL [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des types de données SQL ODBC auxquels ils sont mappés. Elles décrivent également les types de données SQL ODBC et leurs types de données C ODBC correspondants, ainsi que les conversions prises en charge et par défaut.  
  
> [!NOTE]  
>  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **timestamp** mappe au type de données SQL_BINARY ou SQL_VARBINARY ODBC de type de données, car les valeurs de **timestamp** colonnes ne sont pas **datetime** valeurs, mais **Binary (8)** ou **varbinary (8)** valeurs qui indiquent la séquence de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] activité sur la ligne. Si le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client rencontre une valeur SQL_C_WCHAR (Unicode) qui correspond à un nombre impair d'octets, l'octet impaire de fin est tronqué.  
  
## <a name="dealing-with-sqlvariant-data-type-in-odbc"></a>Traitement du type de données sql_variant dans ODBC  
 Le **sql_variant** colonne de type de données peut contenir l’un des types de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’exception des objets volumineux (LOB), telles que **texte**, **ntext**, et **image**. Par exemple, la colonne peut contenir **smallint** les valeurs de certaines lignes, **float** valeurs pour les autres lignes, et **char/nchar** valeurs dans le reste.  
  
 Le **sql_variant** type de données est similaire à la **Variant** Microsoft Visual Basic® type de données.  
  
### <a name="retrieving-data-from-the-server"></a>Récupération de données à partir du serveur  
 ODBC n’a pas un concept des types variants, de limiter l’utilisation de la **sql_variant** type de données avec un pilote ODBC dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si la liaison est spécifiée, la **sql_variant** type de données doit être lié à un des types de données ODBC documentés. **SQL_CA_SS_VARIANT_TYPE**, un nouvel attribut spécifique à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client, retourne le type de données d’une instance dans le **sql_variant** colonne à l’utilisateur.  
  
 Si aucune liaison n’est spécifié, le [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) fonction peut être utilisée pour déterminer le type de données d’une instance dans le **sql_variant** colonne.  
  
 Pour récupérer **sql_variant** données procédez comme suit.  
  
1.  Appelez **SQLFetch** pour positionner à la ligne récupérée.  
  
2.  Appelez **SQLGetData**, en spécifiant SQL_C_BINARY comme type et 0 pour la longueur des données. Cela force le pilote pour lire le **sql_variant** en-tête. L’en-tête fournit le type de données de cette instance dans le **sql_variant** colonne. **SQLGetData** retourne la taille (en octets) de la valeur.  
  
3.  Appelez [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md) en spécifiant **SQL_CA_SS_VARIANT_TYPE** en tant que sa valeur d’attribut. Cette fonction retourne le type de données C de l’instance dans le **sql_variant** colonne au client.  
  
 Voici un segment de code qui illustre les étapes précédentes.  
  
```  
while ((retcode = SQLFetch (hstmt))==SQL_SUCCESS)  
{  
    if (retcode != SQL_SUCCESS && retcode != SQL_SUCCESS_WITH_INFO)  
    {  
        SQLError (NULL, NULL, hstmt, NULL,   
                    &lNativeError,szError,MAX_DATA,&sReturned);  
        printf_s ("%s\n",szError);  
        goto Exit;  
    }  
    retcode = SQLGetData (hstmt, 1, SQL_C_BINARY,   
                                pBuff,0,&Indicator);//Figure out the length  
    if (retcode != SQL_SUCCESS_WITH_INFO && retcode != SQL_SUCCESS)  
    {  
        SQLError (NULL, NULL, hstmt, NULL, &lNativeError,   
                    szError,MAX_DATA,&sReturned);  
        printf_s ("%s\n",szError);  
        goto Exit;  
    }  
    printf_s ("Byte length : %d ",Indicator); //Print out the byte length  
  
    int iValue = 0;  
    retcode = SQLColAttribute (hstmt, 1, SQL_CA_SS_VARIANT_TYPE, NULL,   
                                        NULL,NULL,&iValue);  //Figure out the type  
    printf_s ("Sub type = %d ",iValue);//Print the type, the return is C_type of the column]  
  
// Set up a new binding or do the SQLGetData on that column with   
// the appropriate type  
}  
```  
  
 Si l’utilisateur crée la liaison à l’aide de [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md), le pilote lit les métadonnées et les données. Le pilote convertit ensuite les données en un type ODBC approprié tel que spécifié dans la liaison.  
  
### <a name="sending-data-to-the-server"></a>Envoi de données au serveur  
 **SQL_SS_VARIANT**, un nouveau type de données spécifiques à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client, est utilisé pour les données envoyées à un **sql_variant** colonne. Lors de l’envoi des données au serveur à l’aide de paramètres (par exemple, INSERT INTO TableName VALUES ( ?, ?)), [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) est utilisée pour spécifier les informations de paramètre, y compris le type C et le correspondant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client convertit le type de données C à un des approprié **sql_variant** sous-types.  
  
## <a name="see-also"></a>Voir aussi  
 [Le traitement des résultats &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
