---
title: Exécution d’opérations de copie en bloc | Documents Microsoft
description: Exécution d’opérations de copie en bloc à l’aide du pilote OLE DB pour SQL Server
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
helpviewer_keywords:
- bulk copy [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], bulk copy operations
- OLE DB Driver for SQL Server, bulk copy operations
- MSOLEDBSQL, bulk copy operations
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 72b7fdaa9c221b46b05c6fc02d5f0b0121df4f0c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="performing-bulk-copy-operations"></a>Exécution d'opérations de copie en bloc
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  La fonction de copie en bloc de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge le transfert de quantités importantes de données vers ou depuis une table ou une vue [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les données peuvent également être transférées en spécifiant une instruction SELECT. Les données peuvent être déplacées entre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et un fichier de données du système d'exploitation, par exemple un fichier ASCII. Le fichier de données peut avoir différents formats ; le format est défini pour effectuer la copie en bloc dans un fichier de format. Facultativement, les données peuvent être chargées dans des variables de programme et transférées vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'aide de fonctions et de méthodes de copie en bloc.  
  
 Pour un exemple d’application qui illustre cette fonctionnalité, consultez [en bloc copie de données en bloc avec IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md).  
  
 Une application utilise en général la copie en bloc de l'une des manières suivantes :  
  
-   Effectuer une copie en bloc à partir d'une table, d'une vue ou du jeu de résultats d'une instruction Transact-SQL dans un fichier de données où les données sont stockées dans le même format que la table ou la vue.  
  
     On parle de « fichier de données en mode natif ».  
  
-   Effectuer une copie en bloc à partir d'une table, d'une vue ou du jeu de résultats d'une instruction Transact-SQL dans un fichier de données où les données sont stockées dans un format différent de celui de la table ou de la vue.  
  
     Dans ce cas, un fichier de format séparé est créé qui définit les caractéristiques (type de données, position, longueur, terminateur, etc.) de chaque colonne à mesure que les colonnes sont stockées dans le fichier de données. Si toutes les colonnes sont converties en format caractère, le fichier résultant est appelé un « fichier de données en mode caractère ».  
  
-   Effectuer une copie en bloc à partir d'un fichier de données dans une table ou une vue.  
  
     Si nécessaire, un fichier de format est utilisé pour déterminer la structure du fichier de données.  
  
-   Charger des données dans des variables de programme, puis importer les données dans une table ou une vue à l'aide des fonctions de copie en bloc pour effectuer la copie en bloc dans une ligne à la fois.  
  
 Il n'est pas nécessaire que les fichiers de données utilisés par les fonctions de copie en bloc soient créés par un autre programme de copie en bloc. Tout autre système peut générer un fichier de données et un fichier de format conformément aux définitions de la copie en bloc ; ces fichiers peuvent ensuite être utilisés avec un programme de copie en bloc [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour importer des données dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Par exemple, vous pouvez exporter des données à partir d'une feuille de calcul dans un fichier délimité par des tabulations, générer un fichier de format décrivant le fichier délimité par des tabulations, puis utilisez un programme de copie en bloc pour importer rapidement les données dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les fichiers de données générés par la copie en bloc peuvent également être importés dans d'autres applications. Par exemple, vous pouvez utiliser des fonctions de copie en bloc pour exporter les données d'une table ou d'une vue dans un fichier délimité par des tabulations que vous pouvez ensuite charger dans une feuille de calcul.  
  
 Les programmeurs qui codent des applications pour utiliser les fonctions de copie en bloc doivent respecter les règles générales pour obtenir de bonnes performances en matière de copie en bloc. Pour plus d’informations sur la prise en charge des opérations de copie en bloc dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [importation et exportation de données &#40;SQL Server&#41;](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Un type CLR défini par l'utilisateur (UDT) doit être lié en tant que données binaires. Même si un fichier de format spécifie SQLCHAR en tant que type de données pour une colonne UDT cible, l'utilitaire BCP traite les données en tant que données binaires.  
  
 N'utilisez pas SET FMTONLY OFF avec des opérations de copie en bloc. SET FMTONLY OFF peut entraîner l'échec de votre opération de copie en bloc ou générer des résultats inattendus.  
  
## <a name="ole-db-driver-for-sql-server"></a>Pilote d’OLE DB pour SQL Server 
 Le pilote OLE DB pour SQL Server implémente deux méthodes pour effectuer des opérations de copie en bloc avec un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] base de données. La première méthode implique l’utilisation de la [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md) interface pour les opérations de copie en bloc basées sur mémoire ; et la seconde implique l’utilisation de la [IBCPSession](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md) interface pour les opérations de copie en bloc basées sur le fichier.  
  
### <a name="using-memory-based-bulk-copy-operations"></a>Utilisation d'opérations de copie en bloc basées sur la mémoire  
 Le pilote OLE DB pour SQL Server implémente la **IRowsetFastLoad** interface à exposer la prise en charge de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les opérations de copie en bloc basées sur la mémoire. Le **IRowsetFastLoad** interface implémente le [IRowsetFastLoad::Commit](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md) et [IRowsetFastLoad::InsertRow](../../oledb/ole-db-interfaces/irowsetfastload-insertrow-ole-db.md) méthodes.  
  
#### <a name="enabling-a-session-for-irowsetfastload"></a>Activation d'une session pour IRowsetFastLoad  
 Le consommateur notifie le pilote OLE DB pour SQL Server de son besoin de copie en bloc en définissant le pilote OLE DB pour la propriété de source de données spécifiques à SQL Server SSPROP_ENABLEFASTLOAD avec la valeur VARIANT_TRUE. Avec la propriété définie sur la source de données, le consommateur crée un pilote OLE DB pour la session SQL Server. La nouvelle session permet au client d’accéder à la **IRowsetFastLoad** interface.  
  
> [!NOTE]  
>  Si le **IDataInitialize** interface est utilisée pour l’initialisation de la source de données, puis il est nécessaire de définir la propriété SSPROP_IRowsetFastLoad dans le *rgPropertySets* paramètre de la **IOpenRowset::OpenRowset** (méthode) ; sinon, l’appel à la **OpenRowset** retourne E_NOINTERFACE.  
  
 Activation d’une session de copie en bloc de contraint le pilote OLE DB pour la prise en charge de SQL Server pour les interfaces sur la session. Une session compatible avec la copie en bloc expose uniquement les interfaces suivantes :  
  
-   **IDBSchemaRowset**  
  
-   **IGetDataSource**  
  
-   **IOpenRowset**  
  
-   **ISupportErrorInfo**  
  
-   **ITransactionJoin**  
  
 Pour désactiver la création d’ensembles de lignes compatible avec la copie en bloc et provoquer le pilote OLE DB pour la session SQL Server rétablir le traitement standard, rétablir SSPROP_ENABLEFASTLOAD VARIANT_FALSE.  
  
#### <a name="irowsetfastload-rowsets"></a>Ensembles de lignes IRowsetFastLoad  
 Le pilote OLE DB pour SQL Server copie en bloc les ensembles de lignes sont en écriture seule, mais ils exposent des interfaces qui permettent au consommateur de déterminer la structure d’un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] table. Les interfaces suivantes sont exposées sur un bloc compatible avec la copie pilote OLE DB pour l’ensemble de lignes de SQL Server :  
  
-   **IAccessor**  
  
-   **IColumnsInfo**  
  
-   **IColumnsRowset**  
  
-   **IConvertType**  
  
-   **IRowsetFastLoad**  
  
-   **IRowsetInfo**  
  
-   **ISupportErrorInfo**  
  
 Les propriétés spécifiques au fournisseur SSPROP_FASTLOADOPTIONS, SSPROP_FASTLOADKEEPNULLS et SSPROP_FASTLOADKEEPIDENTITY contrôlent les comportements d’un pilote OLE DB pour l’ensemble de lignes de copie en bloc SQL Server. Les propriétés sont spécifiées dans le *rgProperties* membre d’un *rgPropertySets* **IOpenRowset** membre de paramètre.  
  
|ID de propriété| Description|  
|-----------------|-----------------|  
|SSPROP_FASTLOADKEEPIDENTITY|Colonne : non<br /><br /> R/W : lecture/écriture<br /><br /> Type : VT_BOOL<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : Maintient les valeurs d'identité fournies par le consommateur.<br /><br /> VARIANT_FALSE : les valeurs pour une colonne d'identité dans la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont générées par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Toute valeur liée pour la colonne est ignorée par le pilote OLE DB pour SQL Server.<br /><br /> VARIANT_TRUE : le consommateur lie un accesseur qui fournit une valeur pour une colonne d'identité [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La propriété identity n’est pas disponible sur les colonnes qui acceptent NULL, le consommateur fournit une valeur unique sur chaque **IRowsetFastLoad::Insert** appeler.|  
|SSPROP_FASTLOADKEEPNULLS|Colonne : non<br /><br /> R/W : lecture/écriture<br /><br /> Type : VT_BOOL<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : maintient la valeur NULL pour les colonnes avec une contrainte DEFAULT. Affecte uniquement les colonnes [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui acceptent NULL et auxquelles une contrainte DEFAULT est appliquée.<br /><br /> VARIANT_FALSE : [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] insère la valeur par défaut pour la colonne lorsque le pilote OLE DB pour le consommateur SQL Server insère une ligne contenant la valeur NULL pour la colonne.<br /><br /> VARIANT_TRUE : [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] insère la valeur NULL pour la valeur de colonne lorsque le pilote OLE DB pour le consommateur SQL Server insère une ligne contenant la valeur NULL pour la colonne.|  
|SSPROP_FASTLOADOPTIONS|Colonne : non<br /><br /> R/W : lecture/écriture<br /><br /> Type : VT_BSTR<br /><br /> Valeur par défaut : aucune<br /><br /> Description : Cette propriété est identique à la **-h** »*indicateur*[,... *n*] « l’option de le **bcp** utilitaire. La ou les chaînes suivantes peuvent être utilisées en tant qu'options pour la copie en bloc de données dans une table.<br /><br /> **ORDER**(*column*[**ASC** &#124; **DESC**][,... *n*]) : ordre de tri des données dans le fichier de données. Les performances de la copie en bloc peuvent être améliorées si le fichier de données à charger est trié conformément à l'index cluster de la table.<br /><br /> **ROWS_PER_BATCH** = *bb*: nombre de lignes de données par lot (en tant que *bb*). Le serveur optimise le chargement en masse en fonction de la valeur de *bb*. Par défaut, **ROWS_PER_BATCH** est inconnu.<br /><br /> **KILOBYTES_PER_BATCH** = *cc*: nombre de kilo-octets (Ko) de données par lot (cc). Par défaut, **KILOBYTES_PER_BATCH** est inconnu.<br /><br /> **TABLOCK**: un verrou de niveau table est acquis pour la durée de l’opération de copie en bloc. Cette option augmente sensiblement les performances car le maintien d'un verrou uniquement pour la durée de la seule opération de copie en bloc réduit la contention de verrou sur la table. Une table peut être chargée par plusieurs clients simultanément si la table ne comporte pas d’index et **TABLOCK** est spécifié. Par défaut, le comportement de verrouillage est déterminé par l’option de table **un verrou sur le chargement en masse de table**.<br /><br /> **CHECK_CONSTRAINTS**: les contraintes sur les *table_name* sont vérifiées pendant l’opération de copie en bloc. Par défaut, les contraintes sont ignorées.<br /><br /> **FIRE_TRIGGER**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise la version de la ligne pour les déclencheurs et stocke les versions de ligne dans le magasin de versions **tempdb**. Par conséquent, les optimisations de journalisation en bloc sont disponibles même lorsque les déclencheurs sont activés. Avant l’importation en bloc d’un lot avec un grand nombre de lignes avec les déclencheurs activés, vous devrez peut-être augmenter la taille de **tempdb**.|  
  
### <a name="using-file-based-bulk-copy-operations"></a>Utilisation d'opérations de copie en bloc basées sur le fichier  
 Le pilote OLE DB pour SQL Server implémente la **IBCPSession** interface à exposer la prise en charge de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les opérations de copie en bloc basées sur le fichier. Le **IBCPSession** interface implémente le [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md), [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md), [IBCPSession::BCPControl](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md), [IBCPSession::BCPDone](../../oledb/ole-db-interfaces/ibcpsession-bcpdone-ole-db.md), [IBCPSession::BCPExec](../../oledb/ole-db-interfaces/ibcpsession-bcpexec-ole-db.md), [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md), [IBCPSession::BCPReadFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md), et [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md)méthodes.  
  
  
## <a name="see-also"></a>Voir aussi  
 [Pilote de base de données OLE pour les fonctionnalités SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [Propriétés de Source de données &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)   
 [Importation et exportation de données &#40; en bloc SQL Server &#41;](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)   
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Optimisation des performances de l’importation en bloc](http://msdn.microsoft.com/library/ms190421\(SQL.105\).aspx)  

