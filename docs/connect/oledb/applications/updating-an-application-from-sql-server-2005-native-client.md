---
title: Mise à jour d’une Application à partir de SQL Server 2005 Native Client | Documents Microsoft
description: Mise à jour d’une application à partir de SQL Server 2005 Native Client
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, updating applications
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: faea11704538aea426d5c1c6fdbe8018893dcdc4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-an-application-from-sql-server-2005-native-client"></a>Mise à jour d'une application depuis SQL Server 2005 Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Cette rubrique décrit les modifications avec rupture dans le pilote OLE DB pour SQL Server depuis [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  

 Lorsque vous mettez à niveau à partir de Microsoft Data Access composants (MDAC) pour le pilote OLE DB pour SQL Server, vous verrez également des différences de comportement. Pour plus d’informations, consultez [mise à jour d’une Application de pilote OLE DB pour SQL Server à partir de MDAC](../../oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md).  

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 livré avec [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 livré avec [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.5 livré avec [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 livré avec [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)].  

|Modification de comportement dans le pilote OLE DB pour SQL Server par rapport à [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] Native Client| Description|  
|------------------------------------------------------------------------------------|-----------------|  
|OLE DB effectue un remplissage uniquement à l'échelle définie.|Pour les conversions où les données sont envoyées au serveur, pilote OLE DB pour SQL Server remplit les zéros à droite dans les données uniquement jusqu'à la longueur maximale de **datetime** valeurs. SQL Server Native Client 9.0 effectuaient un remplissage à neuf chiffres.|  
|Valider DBTYPE_DBTIMESTAMP pour ICommandWithParameter::SetParameterInfo.|Pilote OLE DB pour SQL Server implémente la spécification OLE DB pour *bScale* dans ICommandWithParameter::SetParameterInfo à définir à la précision de la fractions pour DBTYPE_DBTIMESTAMP.|  
|Le **sp_columns** procédure stockée maintenant renvoie **« Non »** au lieu de **« Non »** pour la colonne IS_NULLABLE.|Dans le pilote OLE DB pour SQL Server, **sp_columns** procédure stockée maintenant renvoie **« Non »** au lieu de **« Non »** pour une colonne IS_NULLABLE.|  
|Erreur différente retournée lorsque la date est hors limites.|Pour le **datetime** type, un numéro d’erreur différent est retourné par le pilote OLE DB pour SQL Server pour une date hors des limites que vous a été retourné dans les versions antérieures.<br /><br /> Plus précisément, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 retournait l’erreur 22007 pour toutes les valeurs hors limites année des conversions de chaîne pour **datetime**, et le pilote OLE DB pour SQL Server retourne l’erreur 22008 lorsque la date dans la plage prise en charge par **datetime2** mais en dehors de la plage prise en charge par **datetime** ou **smalldatetime**.|  
|**DateTime** valeur tronque les fractions de secondes et le pas round if arrondi modifie le jour.|Antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, le comportement client **datetime** valeurs envoyées au serveur consiste à les arrondir au 1/300e de seconde. Dans le pilote OLE DB pour SQL Server, ce scénario provoque une troncation des fractions de seconde si l’arrondi modifie le jour.|  
|Trunction possible de secondes pour **datetime** valeur.|Une application générée avec le pilote OLE DB pour SQL Server qui se connecte à un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] serveur 2005 tronquera des secondes et fractions de secondes pour la partie heure des données envoyées au serveur si vous liez à une colonne datetime avec un identificateur de type de DBTYPE_DBTIMESTAMP ( OLE DB) ou SQL_TIMESTAMP (ODBC) et une échelle de 0.<br /><br /> Par exemple :<br /><br /> Données d'entrée : 1994-08-21 21:21:36.000<br /><br /> Données insérées : 1994-08-21 21:21:00.000|  
|La conversion de données OLE DB de DBTYPE_DBTIME vers DBTYPE_DATE ne peut plus provoquer de changement de jour.|Avant [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, si la partie heure d'un DBTYPE_DATE était à moins d'une demi-seconde de minuit, le code de conversion OLE DB provoquait le changement de jour. Dans le pilote OLE DB pour SQL Server, le jour ne change pas (en fractions de seconde est tronquées et non arrondies).|  
|Modifications de conversion IBCPSession::BCColFmt.|Dans le pilote OLE DB pour SQL Server, lorsque vous utilisez IBCPSession::BCOColFmt pour convertir SQLDATETIME ou SQLDATETIME en un type chaîne, une valeur fractionnaire est exportée. Par exemple, lorsque conversion du type SQLDATETIME vers le type SQLNVARCHARMAX, les versions antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 retourné<br /> 1989-02-01 00:00:00.<br />Retourne de pilote OLE DB pour SQL Server <br />00:00:00.0000000 de 1989-02-01.|  
|Les applications personnalisées qui utilisent l'API BCP peuvent maintenant afficher un avertissement.|L'API BCP génère un message d'avertissement si la longueur des données est supérieure à la longueur spécifiée pour un champ pour tous les types. Auparavant, cet avertissement était affiché uniquement pour les types caractère, et non pour tous les types.|  
|Insertion d’une chaîne vide dans un **sql_variant** liés comme un type date/heure génère une erreur.|Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0, insertion d’une chaîne vide dans un **sql_variant** liés comme un type date/heure n’a pas généré une erreur. Pilote OLE DB pour SQL Server génère correctement une erreur dans cette situation.|  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut retourner des résultats différents lorsque le déclencheur s’exécute.|Modifications introduites dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] peut entraîner une application des résultats différents soient retournés par une instruction qui a provoqué un déclencheur à exécuter quand **NOCOUNT OFF** était en vigueur. Dans ce cas, votre application peut générer une erreur. Pour résoudre cette erreur, définissez **NOCOUNT ON** dans le déclencheur.|  

## <a name="see-also"></a>Voir aussi   
 [OLE DB Driver pour SQL Server](../../oledb/oledb-driver-for-sql-server.md)
