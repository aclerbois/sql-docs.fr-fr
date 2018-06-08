---
title: Mise à jour d’une Application à partir de SQL Server 2005 Native Client | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client, updating applications
ms.assetid: 1e1e570c-7f14-4e16-beab-c328e3fbdaa8
caps.latest.revision: 42
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 32ebeaefe034128943f081251cc9c40e34e88ee7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-an-application-from-sql-server-2005-native-client"></a>Mise à jour d'une application depuis SQL Server 2005 Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Cette rubrique décrit les modifications essentielles apportées à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client depuis [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Lorsque vous effectuez une mise à niveau de MDAC (Microsoft Data Access Components) vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, vous pouvez constater également des différences de comportement. Pour plus d’informations, consultez [mise à jour d’une Application vers SQL Server Native Client à partir de MDAC](../../../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 livré avec [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 livré avec [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.5 livré avec [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 livré avec [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)].  
  
|Modification du comportement dans SQL Server Native Client depuis [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]| Description|  
|------------------------------------------------------------------------------------|-----------------|  
|OLE DB effectue un remplissage uniquement à l'échelle définie.|Pour les conversions où les données sont envoyées au serveur, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (à compter de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]) remplit les zéros dans les données uniquement jusqu'à la longueur maximale de **datetime** valeurs. SQL Server Native Client 9.0 effectuaient un remplissage à neuf chiffres.|  
|Valider DBTYPE_DBTIMESTAMP pour ICommandWithParameter::SetParameterInfo.|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (à compter de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]) implémente la spécification OLE DB pour *bScale* dans ICommandWithParameter::SetParameterInfo à définir à la précision de la fractions pour DBTYPE_DBTIMESTAMP.|  
|Le **sp_columns** procédure stockée maintenant renvoie **« Non »** au lieu de **« Non »** pour la colonne IS_NULLABLE.|À compter de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 ([!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]), **sp_columns** procédure stockée maintenant renvoie **« Non »** au lieu de **« Non »** pour une colonne IS_NULLABLE.|  
|SQLSetDescRec et SQLBindParameter SQLBindCol désormais effectuent une vérification de cohérence.|Antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, paramètre SQL_DESC_DATA_PTR n’a pas provoqué une vérification de cohérence pour les types de descripteurs dans SQLBindCol, SQLBindParameter ou SQLSetDescRec.|  
|SQLCopyDesc effectue désormais la vérification de cohérence de descripteur.|Antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, SQLCopyDesc n’avez pas une vérification de cohérence lorsque le champ SQL_DESC_DATA_PTR était défini sur un enregistrement particulier.|  
|SQLGetDescRec n’est plus n’une descripteur une vérification de cohérence.|Antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, SQLGetDescRec effectuée une vérification de cohérence de descripteur lorsque le champ SQL_DESC_DATA_PTR était défini. Cela n'était pas requis par la spécification ODBC et dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 ([!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]) et les versions ultérieures, cette vérification de cohérence n'est plus effectuée.|  
|Erreur différente retournée lorsque la date est hors limites.|Pour le **datetime** type, un numéro d’erreur différent est retourné par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (à compter de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]) pour un out-of-range date a été retournée dans les versions antérieures.<br /><br /> Plus précisément, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 retournait l’erreur 22007 pour toutes les valeurs hors limites année des conversions de chaîne pour **datetime**, et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] début Native Client avec la version 10.0 ([!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]) retourne l’erreur 22008 lorsque la date dans la plage prise en charge par **datetime2** mais en dehors de la plage prise en charge par **datetime** ou **smalldatetime**.|  
|**DateTime** valeur tronque les fractions de secondes et le pas round if arrondi modifie le jour.|Antérieures à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, le comportement client **datetime** valeurs envoyées au serveur consiste à les arrondir au 1/300e de seconde. Depuis [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, ce scénario provoque une troncation des fractions de seconde si l'arrondi modifie le jour.|  
|Trunction possible de secondes pour **datetime** valeur.|Une application générée avec [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] Native Client (ou ultérieure) qui se connecte à un serveur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2005 tronquera des secondes et des fractions de seconde pour la partie heure des données envoyées au serveur si vous créez une liaison avec une colonne datetime avec un identificateur de type de DBTYPE_DBTIMESTAMP (OLE DB) ou SQL_TIMESTAMP (ODBC) et une échelle de 0.<br /><br /> Par exemple :<br /><br /> Données d'entrée : 1994-08-21 21:21:36.000<br /><br /> Données insérées : 1994-08-21 21:21:00.000|  
|La conversion de données OLE DB de DBTYPE_DBTIME vers DBTYPE_DATE ne peut plus provoquer de changement de jour.|Avant [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, si la partie heure d'un DBTYPE_DATE était à moins d'une demi-seconde de minuit, le code de conversion OLE DB provoquait le changement de jour. Depuis [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, le jour ne change pas (les fractions de seconde sont tronquées et non arrondies).|  
|Modifications de conversion IBCPSession::BCColFmt.|À compter de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0, lorsque vous utilisez IBCPSession::BCOColFmt pour convertir SQLDATETIME ou SQLDATETIME en un type chaîne, une valeur fractionnaire est exportée. Par exemple, lors de la conversion du type SQLDATETIME vers le type SQLNVARCHARMAX, les versions antérieures de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client retournaient<br /><br /> 1989-02-01 00:00:00. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 et versions ultérieures retournent 1989-02-01 00:00:00.0000000.|  
|La taille des données envoyées doit correspondre à la longueur spécifiée dans SQL_LEN_DATA_AT_EXEC.|Lors de l'utilisation de SQL_LEN_DATA_AT_EXEC, la taille des données doit correspondre à la longueur que vous avez spécifiée avec SQL_LEN_DATA_AT_EXEC. Vous pouvez utiliser SQL_DATA_AT_EXEC, mais l'utilisation de SQL_LEN_DATA_AT_EXEC présente certains avantages en matière de performances.|  
|Les applications personnalisées qui utilisent l'API BCP peuvent maintenant afficher un avertissement.|L'API BCP génère un message d'avertissement si la longueur des données est supérieure à la longueur spécifiée pour un champ pour tous les types. Auparavant, cet avertissement était affiché uniquement pour les types caractère, et non pour tous les types.|  
|Insertion d’une chaîne vide dans un **sql_variant** liés comme un type date/heure génère une erreur.|Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0, insertion d’une chaîne vide dans un **sql_variant** liés comme un type date/heure n’a pas généré une erreur. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 (et versions ultérieures) génère correctement une erreur dans cette situation.|  
|Validation des paramètres _TIMESTAMP SQL_C_TYPE et DBTYPE_DBTIMESTAMP plus stricte.|Antérieures à [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] Native Client, **datetime** valeurs étaient arrondies selon l’échelle de **datetime** et **smalldatetime** les colonnes par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] Native Client (et versions ultérieures) applique maintenant les règles de validation plus strictes définies dans la spécification principale ODBC pour les fractions de seconde. Si une valeur de paramètre ne peut pas être convertie au type SQL à l'aide de l'échelle spécifiée ou déduite de la liaison de client sans troncation des chiffres de fin, une erreur est retournée.|  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut retourner des résultats différents lorsque le déclencheur s’exécute.|Modifications introduites dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] peut entraîner une application des résultats différents soient retournés par une instruction qui a provoqué un déclencheur à exécuter quand **NOCOUNT OFF** était en vigueur. Dans ce cas, votre application peut générer une erreur. Pour résoudre cette erreur, définissez **NOCOUNT ON** dans le déclencheur ou appelez SQLMoreResults à passer au résultat suivant.|  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation de SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md)  
  
  
