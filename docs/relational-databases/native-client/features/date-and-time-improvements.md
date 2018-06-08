---
title: Date et heure améliorations | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 9b1d0d9d-1f6e-4399-8f61-e23f9a486a7a
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e2077845a348243b8f2e68563580028f6d656354
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="date-and-time-improvements"></a>Date et heure améliorations
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Cette rubrique décrit la prise en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client des types de données de date et d'heure ajoutés à [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
 Pour plus d’informations sur les améliorations de date/heure, consultez [Date et heure améliorations &#40;OLE DB&#41; ](../../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md) et [Date et heure améliorations &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
 Pour plus d’informations sur les exemples d’applications qui illustrent cette fonctionnalité, consultez [exemples de programmation de données SQL Server](http://msftdpprodsamples.codeplex.com/).  
  
## <a name="usage"></a>Utilisation  
 Les sections suivantes décrivent les différentes façons d'utiliser les nouveaux types de date et d'heure.  
  
### <a name="use-date-as-a-distinct-data-type"></a>Utiliser le type Date comme type de données distinct  
 À partir de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], l'amélioration de la prise en charge des types date/heure rend plus efficace l'utilisation du type SQL_TYPE_DATE ODBC (SQL_DATE pour les applications ODBC 2.0) et du type DBTYPE_DBDATE OLE DB.  
  
### <a name="use-time-as-a-distinct-data-type"></a>Utiliser le type Time comme type de données distinct  
 OLE DB a déjà un type de données qui contient juste l'heure, DBTYPE_DBTIME, avec une précision de 1 seconde. Dans ODBC, le type équivalent est SQL_TYPE_TIME (SQL_TIME pour les applications ODBC 2.0).  
  
 La nouvelle [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données a la fraction de temps en secondes précision de 100 nanosecondes. Cela requiert de nouveaux types de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client : DBTYPE_DBTIME2 (OLE DB) et SQL_SS_TIME2 (ODBC). Les applications existantes écrites pour utiliser des heures sans fractions de seconde peuvent recourir à des colonnes time(0). Les types OLE DB DBTYPE_TIME et ODBC SQL_TYPE_TIME, ainsi que leurs structs correspondants, doivent fonctionner correctement, à moins que les applications ne reposent sur le type retourné dans les métadonnées.  
  
### <a name="use-time-as-a-distinct-data-type-with-extended-fractional-seconds-precision"></a>Utiliser le type Time comme type de données distinct avec l'extension de la précision en fractions de seconde  
 Certaines applications, telles que les applications de contrôle de processus et de fabrication, requièrent la possibilité de gérer les données d'heure avec une précision allant jusqu'à 100 nanosecondes. Les nouveaux types répondant à cet objectif sont DBTYPE_DBTIME2 (OLE DB) et SQL_SS_TIME2 (ODBC).  
  
### <a name="use-datetime-with-extended-fractional-seconds-precision"></a>Utiliser le type Datetime avec l'extension de la précision en fractions de seconde  
 OLE DB définit déjà un type avec une précision allant jusqu'à 1 nanoseconde. Toutefois, ce type est déjà utilisé par les applications [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existantes, lesquelles attendent une précision de 1/300 de seconde uniquement. La nouvelle **datetime2(3)** type n’est pas directement compatible avec le type datetime existant. S'il existe un risque que cela affecte le comportement des applications, celles-ci doivent utiliser un nouvel indicateur DBCOLUMN pour déterminer le type réel sur le serveur.  
  
 ODBC définit également un type avec une précision allant jusqu'à 1 nanoseconde. Toutefois, ce type est déjà utilisé par les applications [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existantes, lesquelles attendent une précision de 3 millisecondes uniquement. La nouvelle **datetime2(3)** type n’est pas directement compatible avec le **datetime** type. **datetime2(3)** a une précision d’une milliseconde et **datetime** a une précision de 1/300 de seconde. Dans ODBC, les applications peuvent déterminer le type serveur en cours d'utilisation avec le champ de descripteur SQL_DESC_TYPE_NAME. Par conséquent, le type existant SQL_TYPE_TIMESTAMP (SQL_TIMESTAMP pour les applications ODBC 2.0) peut être utilisé pour les deux types.  
  
### <a name="use-datetime-with-extended-fractional-seconds-precision-and-timezone"></a>Utiliser le type Datetime avec l'extension de la précision en fractions de seconde et le fuseau horaire  
 Certaines applications requièrent des valeurs datetime avec les informations de fuseau horaire. Cela est pris en charge par les nouveaux types DBTYPE_DBTIMESTAMPOFFSET (OLE DB) et SQL_SS_TIMESTAMPOFFSET (ODBC).  
  
### <a name="use-datetimedatetimedatetimeoffset-data-with-client-side-conversions-consistent-with-existing-conversions"></a>Utiliser les données de type Date/Time/Datetime/Datetimeoffset avec des conversions côté client cohérentes avec les conversions existantes  
 La norme ODBC décrit le fonctionnement des conversions entre les types date, time et timestamp existants. Ceux-ci sont étendus de manière cohérente afin d'inclure les conversions entre tous les types relatifs à la date et à l'heure introduits dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités de SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
