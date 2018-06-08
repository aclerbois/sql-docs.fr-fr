---
title: Liaisons et Conversions (OLE DB) | Documents Microsoft
description: Liaisons et conversions (OLE DB)
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: ole-db-date-time
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB]
- bindings [OLE DB]
- OLE DB, bindings and conversions
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 79887c62b3cd26c194bf140b29b836bf9140eb85
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="conversions-ole-db"></a>Conversions (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Cette section explique comment effectuer une conversion entre **datetime** et **datetimeoffset** valeurs. Les conversions décrites dans cette section sont soit déjà fournies par OLE DB, soit une extension cohérente de ce dernier.  
  
 Le format des littéraux et des chaînes pour les dates et les heures dans OLE DB suit généralement la norme ISO et ne dépend pas des paramètres régionaux du client. Une exception est DBTYPE_DATE où la norme est OLE Automation. Toutefois, parce que le pilote OLE DB pour SQL Server effectue uniquement des conversions entre types de données sont transmises vers ou à partir du client, il n’existe aucun moyen pour une application forcer le pilote OLE DB pour SQL Server effectuer des conversions entre DBTYPE_DATE et chaîne de formats. Sinon, les chaînes utilisent les formats suivants (le texte entre crochets indique un élément facultatif) :  
  
-   Le format de **datetime** et **datetimeoffset** chaînes est :  
  
     *aaaa*-*mm*-*jj*[ *hh*:*mm*:*ss*[. *9999999*] [± *hh*:*mm*]]  
  
-   Le format de **temps** chaînes est :  
  
     *hh*:*mm*:*ss*[. *9999999*]  
  
-   Le format de **date** chaînes est :  
  
     *aaaa*-*mm*-*dd*  
  
> [!NOTE]  
>  Les versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client et SQLOLEDB autorisaient l'implémentation de conversions OLE en cas d'échec des processus de conversion standard. Le pilote OLE DB pour SQL Server suit le même comportement que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Par conséquent, certaines conversions effectuées par le pilote OLE DB pour SQL Server diffèrent de la spécification OLE DB.  
  
 Les conversions de chaînes autorisent une souplesse en matière d'espace et de largeur de champ. Pour plus d’informations, consultez la section « Formats de données : chaînes et littéraux » dans [prise en charge du Type de données de Date OLE DB et les améliorations apportées au](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md).  
  
 Les règles suivantes sont les règles générales de conversion :  
  
-   Lorsqu'une chaîne est convertie en type date/heure, la chaîne est d'abord analysée en tant que littéral ISO. En cas d'échec, la chaîne est analysée comme un littéral de date OLE doté de composants heure.  
  
-   Si aucune heure n'est fournie mais le récepteur peut stocker l'heure, l'heure est définie avec la valeur zéro. Si aucune date n'est fournie mais le récepteur peut stocker une date, la date définie est la date actuelle qui utilise les conversions ISO ou bien 1899-12-30 qui utilise les conversions OLE.  
  
-   Si aucun fuseau horaire n'est fourni dans le type de données que le client utilise mais si le serveur peut stocker le fuseau, les données du client sont supposées apparaître dans le fuseau horaire du client.  
  
-   Si aucun fuseau horaire n'est fourni au serveur mais le client dispose d'informations sur les fuseaux, le fuseau horaire UTC est adopté. Ce comportement diffère de celui du serveur.  
  
-   Si l'heure est fournie mais le récepteur ne peut pas la stocker, le composant heure est ignoré.  
  
-   Si la date est fournie mais le récepteur ne peut pas la stocker, le composant date est ignoré.  
  
-   En cas de troncation des secondes ou des fractions de seconde lors de la conversion du client au serveur, DB_E_ERRORSOCCURRED est retourné et l'état DBSTATUS_E_DATAOVERFLOW est défini.  
  
-   En cas de troncation des secondes ou des fractions de seconde lors de la conversion du serveur au client, DBSTATUS_S_TRUNCATED est défini  
  
## <a name="in-this-section"></a>Dans cette section  
 [Conversions de client à serveur](../../oledb/ole-db-date-time/conversions-performed-from-client-to-server.md)  
 Décrit les conversions de date/heure effectuées entre une application cliente écrite avec le pilote OLE DB pour SQL Server et [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] (ou version ultérieure).  
  
 [Conversions de serveur à client](../../oledb/ole-db-date-time/conversions-performed-from-server-to-client.md)  
 Décrit les conversions de date/heure effectuées entre [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] (ou version ultérieure) et une application cliente écrite avec le pilote OLE DB pour SQL Server.  
  
## <a name="see-also"></a>Voir aussi  
 [Date et heure améliorations & #40 ; OLE DB & #41 ;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
