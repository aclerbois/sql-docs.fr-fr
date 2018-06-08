---
title: SQLPutData | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLPutData function
ms.assetid: d39aaa5b-7fbc-4315-a7f2-5a7787e04f25
caps.latest.revision: 49
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: cfb11999229c383fab06415cb06a07f555dbccb7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlputdata"></a>SQLPutData
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Les restrictions suivantes s’appliquent lors de l’utilisation de SQLPutData pour envoyer plus de 65 535 octets de données (pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 4. 21 a) ou 400 Ko de données (pour SQL Server version 6.0 ou ultérieure) pour un SQL_LONGVARCHAR (**texte**), SQL_WLONGVARCHAR (**ntext**) ou SQL_LONGVARBINARY (**image**) colonne :  
  
-   Le paramètre référencé peut être le *insert_value* dans une instruction INSERT.  
  
-   Le paramètre référencé peut être un *expression* dans la clause SET d’une instruction UPDATE.  
  
 L’annulation d’une séquence d’appels SQLPutData qui fournissent des données dans des blocs à un serveur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provoque une mise à jour partielle de la valeur de colonne lorsque vous utilisez la version 6.5 ou antérieure. Le **texte**, **ntext**, ou **image** colonne qui a été référencé lors de l’appel de SQLCancel est définie sur une valeur d’espace réservé intermédiaire.  
  
> [!NOTE]  
>  Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ne prend pas en charge la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 6.5 ou antérieure.  
  
## <a name="diagnostics"></a>Diagnostics  
 Il y a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQLSTATE spécifique de Native Client pour SQLPutData :  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|22026|Chaîne de données ou longueur non correspondante|Si la longueur des données en octets à envoyer a été spécifiée par une application, par exemple, avec SQL_LEN_DATA_AT_EXEC (*n*) où *n* est supérieur à 0, le nombre total d’octets donné par l’application via SQLPutData doit correspondre à la longueur spécifiée.|  
  
## <a name="sqlputdata-and-table-valued-parameters"></a>SQLPutData et paramètres table  
 SQLPutData est utilisé par une application lors de l’utilisation de la liaison de ligne variable avec des paramètres table. Le *StrLen_Or_Ind* paramètre indique qu’il est prêt pour le pilote collecter des données pour l’ou les lignes de données de paramètre table suivante, ou que plus aucune ligne n’est disponibles :  
  
-   Une valeur supérieure à 0 indique que le jeu suivant de valeurs de ligne est disponible.  
  
-   La valeur 0 indique qu'il ne reste plus de lignes à envoyer.  
  
-   Toute valeur inférieure à 0 est une erreur et entraîne la consignation d'un enregistrement de diagnostic avec SQLState HY090 et le message « Longueur de chaîne ou de mémoire tampon non valide ».  
  
 Le *DataPtr* paramètre est ignoré, mais doit être définie sur une valeur non NULL. Pour plus d’informations, consultez la section sur la liaison de ligne Variable TVP dans [liaison et les valeurs des colonnes et les paramètres Data Transfer of Table-Valued](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md).  
  
 Si *StrLen_Or_Ind* a une valeur autre que SQL_DEFAULT_PARAM ou un nombre compris entre 0 et SQL_PARAMSET_SIZE (c'est-à-dire, le *ColumnSize* paramètre de SQLBindParameter), il s’agit d’une erreur. Cette erreur conduit SQLPutData à retourner SQL_ERROR: SQLSTATE=HY090, « Longueur de chaîne ou de mémoire tampon non valide ».  
  
 Pour plus d’informations sur les paramètres table, consultez [paramètres table & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlputdata-support-for-enhanced-date-and-time-features"></a>Prise en charge de SQLPutData pour les fonctionnalités Date et Heure améliorées  
 Les valeurs de paramètre des types date/heure sont converties comme décrit dans [Conversions de C en SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md).  
  
 Pour plus d’informations, consultez [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlputdata-support-for-large-clr-udts"></a>Prise en charge des grands types définis par l'utilisateur CLR par SQLPutData  
 **SQLPutData** prend en charge les types CLR volumineux définis par l’utilisateur (UDT). Pour plus d’informations, consultez [Large CLR User-Defined Types & #40 ; ODBC & #41 ;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLPutData, fonction](http://go.microsoft.com/fwlink/?LinkId=59365)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
