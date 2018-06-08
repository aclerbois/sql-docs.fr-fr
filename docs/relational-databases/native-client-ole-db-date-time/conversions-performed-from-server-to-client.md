---
title: Conversions effectuées à partir du serveur au Client | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-date-time
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB], server to client
ms.assetid: 676fdf24-fb72-4ea0-a8d2-2b197da3c83f
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5ec71f4e45e32a34ddef9efe753cfad4e0b10f1e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="conversions-performed-from-server-to-client"></a>Conversions de serveur à client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Cette rubrique décrit les conversions date/heure effectuées entre [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (ou version ultérieure) et une application cliente écrite avec OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="conversions"></a>Conversions  
 Le tableau suivant décrit les conversions entre le type retourné au client et le type de la liaison. Paramètres de sortie, si ICommandWithParameters::SetParameterInfo a été appelée et que le type spécifié dans *pwszDataSourceType* ne correspond pas au type réel sur le serveur, une conversion implicite est effectué par le serveur, et le type retourné au client correspond au type spécifié par le biais ICommandWithParameters::SetParameterInfo. Cela peut provoquer des résultats de conversion inattendus lorsque les règles de conversion du serveur sont différentes de celles décrites dans cette rubrique. Par exemple, quand une date par défaut doit être fournie, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise 1900-1-1, plutôt que 1899-12-30.  
  
|Vers -><br /><br /> From|DATE|DBDATE|DBTIME|DBTIME2|DBTIMESTAMP|DBTIMESTAMPOFFSET|FILETIME|BYTES|VARIANT|SSVARIANT|BSTR|STR|WSTR|  
|----------------------|----------|------------|------------|-------------|-----------------|-----------------------|--------------|-----------|-------------|---------------|----------|---------|----------|  
|Date|1,7|OK|-|-|1|1,3|1,7|-|OK (VT_BSTR)|OK|OK|4|4|  
|Time|5,6,7|-|9|OK|6|3,6|5,6|-|OK (VT_BSTR)|OK|OK|4|4|  
|Smalldatetime|7|8|9,10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|DateTime|5,7|8|9,10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|Datetime2|5,7|8|9,10|10|7|3|5,7|-|OK (VT_BSTR)|OK|OK|4|4|  
|Datetimeoffset|5,7,11|8,11|9,10,11|10,11|7,11|OK|5,7,11|-|OK (VT_BSTR)|OK|OK|4|4|  
|Char, Varchar,<br /><br /> NVARCHAR2, NCHAR|7, 13|12|12,9|12|12|12|7,13|Néant|Néant|Néant|Néant|Néant|Néant|  
|Sql_variant<br /><br /> (datetime)|7|8|9,10|10|OK|3|7|-|7(VT_DATE)|OK|OK|4|4|  
|Sql_variant<br /><br /> (smalldatetime)|7|8|9,10|10|OK|3|7|-|7(VT_DATE)|OK|OK|4|4|  
|Sql_variant<br /><br /> (date)|1,7|OK|2|2|1|1,3|1,7|-|OK(VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (time)|5,6,7|2|6|OK|6|3,6|5,6|-|OK(VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (datetime2)|5,7|8|9,10|10|OK|3|5,7|-|OK(VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (datetimeoffset)|5,7,11|8,11|9,10,11|10,11|7,11|OK|5,7,11|-|OK(VT_BSTR)|OK|OK|4|4|  
  
## <a name="key-to-symbols"></a>Liste des symboles  
  
|Symbole|Signification|  
|------------|-------------|  
|OK|Aucune conversion nécessaire.|  
|-|Aucune conversion n'est prise en charge. Si la liaison est validée lorsque IAccessor::CreateAccessor est appelée, DBBINDSTATUS_UPSUPPORTEDCONVERSION est retourné dans *rgStatus*. Lorsque la validation pour l'accesseur est différée, DBSTATUS_E_BADACCESSOR est défini.|  
|1|Les champs heure sont définis avec la valeur zéro.|  
|2|DBSTATUS_E_CANTCONVERTVALUE est défini.|  
|3|Le décalage est défini avec la valeur zéro.|  
|4|Si la mémoire tampon du client n'est pas assez grande, DBSTATUS_S_TRUNCATED est défini. Lorsque le type de serveur inclut des fractions de seconde, le nombre de chiffres de la chaîne de résultats correspond exactement à l'échelle du type de serveur.|  
|5|La troncation de secondes ou de fractions de seconde est ignorée.|  
|6|La date est définie avec la date courante, à moins que la source ne soit un littéral de chaîne heure et que la destination soit DBTYPE_DATE. Dans ce cas, c'est 1899-12-30 qui est utilisé.|  
|7|En cas de dépassement de capacité de la valeur, DBSTATUS_E_DATAOVERFLOW est défini.|  
|8|Les champs time (heure) sont ignorés.|  
|9|Les champs de fractions de seconde sont ignorés.|  
|10|Le composant date est ignoré.|  
|11|L'heure est convertie en fuseau horaire du client. Si une erreur se produit pendant cette conversion, DBSTATUS_E_DATAOVERFLOW est défini.|  
|12|La chaîne est analysée comme littéral ISO et convertie dans le type cible. En cas d'échec, la chaîne est analysée comme littéral de date OLE (qui possède aussi des composants heure) et convertie d'une date OLE (DBTYPE_DATE) en type cible. La chaîne doit se conformer à la syntaxe des littéraux autorisés du type cible pour que l'analyse du format ISO réussisse. Pour que l'analyse OLE réussisse, la chaîne doit se conformer à la syntaxe reconnue par OLE. Si la chaîne ne peut pas être analysée, DBSTATUS_E_CANTCONVERTVALUE est défini. Si les valeurs d'un composant sont hors limites, DBSTATUS_E_DATAOVERFLOW est défini.|  
|13|La chaîne est analysée comme littéral ISO et convertie dans le type cible. En cas d'échec, la chaîne est analysée comme littéral de date OLE (qui possède aussi des composants heure) et convertie d'une date OLE (DBTYPE_DATE) en type cible. La chaîne doit se conformer à la syntaxe des littéraux datetime, à moins que la destination ne soit DBTYPE_DATE ou DBTYPE_DBTIMESTAMP. Si tel est le cas, un littéral datetime ou time est autorisé pour que l'analyse de format ISO réussisse. Pour que l'analyse OLE réussisse, la chaîne doit se conformer à la syntaxe reconnue par OLE. Si la chaîne ne peut pas être analysée, DBSTATUS_E_CANTCONVERTVALUE est défini. Si les valeurs d'un composant sont hors limites, DBSTATUS_E_DATAOVERFLOW est défini.|  
  
## <a name="see-also"></a>Voir aussi  
 [Liaisons et Conversions &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-date-time/conversions-ole-db.md)  
  
  
