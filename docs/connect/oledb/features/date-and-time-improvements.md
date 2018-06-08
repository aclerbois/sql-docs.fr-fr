---
title: Date et heure améliorations | Documents Microsoft
description: Améliorations de date et d’heure dans le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|features
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: d39d7862dc470dfaada9b01f3717662dc2c9c94b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="date-and-time-improvements"></a>Date et heure améliorations
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Cette rubrique décrit le pilote OLE DB pour la prise en charge de SQL Server pour les types de données de date et d’heure qui ont été ajoutés dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
 Pour plus d’informations sur les améliorations de date/heure, consultez [Date et heure améliorations &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md).  
  
 Pour plus d’informations sur les exemples d’applications qui illustrent cette fonctionnalité, consultez [exemples de programmation de données SQL Server](http://msftdpprodsamples.codeplex.com/).  
  
## <a name="usage"></a>Utilisation  
 Les sections suivantes décrivent les différentes façons d'utiliser les nouveaux types de date et d'heure.  
  
### <a name="use-date-as-a-distinct-data-type"></a>Utiliser le type Date comme type de données distinct  
 À partir de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], prise en charge améliorée pour les types date/heure rend plus efficace d’utiliser le type DBTYPE_DBDATE OLE DB.  
  
### <a name="use-time-as-a-distinct-data-type"></a>Utiliser le type Time comme type de données distinct  
 OLE DB a déjà un type de données qui contient juste l'heure, DBTYPE_DBTIME, avec une précision de 1 seconde.
  
 La nouvelle [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] type de données a la fraction de temps en secondes précision de 100 nanosecondes. Cela nécessite un nouveau type dans le pilote OLE DB pour SQL Server : DBTYPE_DBTIME2. Les applications existantes écrites pour utiliser des heures sans fractions de seconde peuvent recourir à des colonnes time(0). Le type OLE DB DBTYPE_TIME existant et ses structures correspondantes doivent fonctionner correctement, sauf si les applications qui s’appuient sur le type retourné dans les métadonnées.  
  
### <a name="use-time-as-a-distinct-data-type-with-extended-fractional-seconds-precision"></a>Utiliser le type Time comme type de données distinct avec l'extension de la précision en fractions de seconde  
 Certaines applications, telles que les applications de contrôle de processus et de fabrication, requièrent la possibilité de gérer les données d'heure avec une précision allant jusqu'à 100 nanosecondes. Nouveau type à cet effet dans OLE DB est DBTYPE_DBTIME2.  
  
### <a name="use-datetime-with-extended-fractional-seconds-precision"></a>Utiliser le type Datetime avec l'extension de la précision en fractions de seconde  
 OLE DB définit déjà un type avec une précision allant jusqu'à 1 nanoseconde. Toutefois, ce type est déjà utilisé par les applications [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existantes, lesquelles attendent une précision de 1/300 de seconde uniquement. La nouvelle **datetime2(3)** type n’est pas directement compatible avec le type datetime existant. S'il existe un risque que cela affecte le comportement des applications, celles-ci doivent utiliser un nouvel indicateur DBCOLUMN pour déterminer le type réel sur le serveur.    
  
### <a name="use-datetime-with-extended-fractional-seconds-precision-and-timezone"></a>Utiliser le type Datetime avec l'extension de la précision en fractions de seconde et le fuseau horaire  
 Certaines applications requièrent des valeurs datetime avec les informations de fuseau horaire. Cela est pris en charge par le nouveau type DBTYPE_DBTIMESTAMPOFFSET.
  
### <a name="use-datetimedatetimedatetimeoffset-data-with-client-side-conversions-consistent-with-existing-conversions"></a>Utiliser les données de type Date/Time/Datetime/Datetimeoffset avec des conversions côté client cohérentes avec les conversions existantes  
 Les conversions sont étendues de manière cohérente pour inclure des conversions entre tous les types de date et d’heure introduits dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
