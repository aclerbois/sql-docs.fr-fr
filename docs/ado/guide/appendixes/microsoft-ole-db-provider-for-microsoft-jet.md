---
title: Fournisseur Microsoft OLE DB pour Microsoft Jet | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Jet provider for OLE DB [ADO]
- providers [ADO], OLE DB provider for Microsoft Jet
- OLE DB provider for Microsoft Jet [ADO]
ms.assetid: fd956da1-5203-40af-aa7e-fc13a6c6581f
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c0046abe221607ff85b237c1b15ad331ba09c4e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-ole-db-provider-for-microsoft-jet-overview"></a>Fournisseur Microsoft OLE DB pour Microsoft Jet présentation
Le fournisseur OLE DB pour Microsoft Jet permet à ADO accéder aux bases de données Microsoft Jet.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez la *fournisseur* argument de la [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) suivante à la propriété :

```
Microsoft.Jet.OLEDB.4.0
```

 La lecture de la [fournisseur](../../../ado/reference/ado-api/provider-property-ado.md) propriété renvoie également cette chaîne.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion par défaut pour ce fournisseur est :

```
"Provider=Microsoft.Jet.OLEDB.4.0;Data Source=databaseName;User ID=MyUserID;Password=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé| Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur OLE DB pour Microsoft Jet.|
|**Source de données**|Spécifie le chemin d’accès et le nom de base de données (par exemple, `c:\Northwind.mdb`).|
|**ID d'utilisateur**|Spécifie le nom d’utilisateur. Si ce mot clé n’est pas spécifié, la chaîne «`admin`», est utilisé par défaut.|
|**Mot de passe**|Spécifie le mot de passe. Si ce mot clé n’est pas spécifié, la chaîne vide (« »), est utilisé par défaut.|

> [!NOTE]
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = yes** ou **Integrated Security = SSPI** au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.

## <a name="provider-specific-connection-parameters"></a>Paramètres de connexion spécifique au fournisseur
 Le fournisseur OLE DB pour Microsoft Jet prend en charge plusieurs propriétés dynamiques spécifiques au fournisseur en plus de ceux qui sont définis par ADO. Comme avec tous les autres **connexion** paramètres, ils peuvent être définis à l’aide de la **propriétés** collection de la **connexion** objet ou dans le cadre de la chaîne de connexion.

 Le tableau suivant répertorie ces propriétés avec le nom de propriété OLE DB correspondant entre parenthèses.

|Paramètre| Description|
|---------------|-----------------|
|Jet OLEDB:Compact espace récupéré quantité (DBPROP_JETOLEDB_COMPACTFREESPACESIZE)|Indique une estimation de la quantité d’espace, en octets, qui peut être récupéré lors du compactage de la base de données. Cette valeur est uniquement valide après avoir établi une connexion de base de données.|
|Contrôle de OLEDB:Connection Jet (DBPROP_JETOLEDB_CONNECTIONCONTROL)|Indique si les utilisateurs peuvent se connecter à la base de données.|
|Jet OLEDB : créer la base de données système (DBPROP_JETOLEDB_CREATESYSTEMDATABASE)|Indique si une base de données système doit être créé lors de la création d’une source de données.|
|Mode de verrouillage Jet OLEDB : Database (DBPROP_JETOLEDB_DATABASELOCKMODE)|Indique le mode de verrouillage pour cette base de données. Le premier utilisateur pour ouvrir la base de données détermine le mode utilisé lorsque la base de données est ouverte.|
|Jet OLEDB : Database Password (DBPROP_JETOLEDB_DATABASEPASSWORD)|Indique le mot de passe de base de données.|
|Jet OLEDB : ne pas copier les paramètres régionaux sur Compact (DBPROP_JETOLEDB_COMPACT_DONTCOPYLOCALE)|Indique si Jet doit copier les informations de paramètres régionaux lorsque vous compactez une base de données.|
|Jet OLEDB : chiffrer la base de données (DBPROP_JETOLEDB_ENCRYPTDATABASE)|Indique si une base de données compactée doit être chiffrée. Si cette propriété n’est pas définie, la base de données compactée est chiffrée si la base de données d’origine a également été chiffrée.|
|Type de OLEDB:Engine Jet (DBPROP_JETOLEDB_ENGINE)|Indique le moteur de stockage utilisé pour accéder au magasin de données en cours.|
|Jet définies Async Delay (DBPROP_JETOLEDB_EXCLUSIVEASYNCDELAY)|Indique la longueur maximale de temps, en millisecondes, Jet susceptibles de retarder les écritures asynchrones sur le disque lorsque la base de données est ouverte en mode exclusif.<br /><br /> Cette propriété est ignorée sauf si **Jet OLEDB : Flush Transaction Timeout** est définie sur 0.|
|Jet OLEDB : Flush Transaction Timeout (DBPROP_JETOLEDB_FLUSHTRANSACTIONTIMEOUT)|Indique la quantité de temps à attendre avant que les données stockées dans un cache en écriture asynchrone sont écrite sur le disque. Ce paramètre remplace les valeurs pour **Jet OLEDB : Shared Async Delay** et **Jet définies Async Delay**.|
|Jet OLEDB : les Transactions globales en bloc (DBPROP_JETOLEDB_GLOBALBULKNOTRANSACTIONS)|Indique si les transactions SQL en bloc sont traitées.|
|Jet OLEDB : Simplifiez les opérations en bloc de partielle Global (DBPROP_JETOLEDB_GLOBALBULKPARTIAL)|Indique le mot de passe utilisé pour ouvrir la base de données.|
|Jet OLEDB : synchronisation de Commit implicite (DBPROP_JETOLEDB_IMPLICITCOMMITSYNC)|Indique si les modifications apportées dans les transactions internes implicites sont écrites en mode synchrone ou asynchrone.|
|Délai de OLEDB:Lock Jet (DBPROP_JETOLEDB_LOCKDELAY)|Indique le nombre de millisecondes à attendre avant d’essayer d’acquérir un verrou après qu’une tentative précédente a échoué.|
|Jet OLEDB:Lock Réessayer (DBPROP_JETOLEDB_LOCKRETRY)|Indique le nombre de répétitions une tentative d’accès à une page verrouillée.|
|Taille de mémoire tampon Jet OLEDB:Max (DBPROP_JETOLEDB_MAXBUFFERSIZE)|Indique la quantité maximale de mémoire, en kilo-octets, Jet peut utiliser avant de commencer le vidage des modifications sur le disque.|
|Verrous de OLEDB:Max Jet par fichier (DBPROP_JETOLEDB_MAXLOCKSPERFILE)|Indique le nombre maximal de verrous que Jet peut placer sur une base de données. La valeur par défaut est 9500.|
|Jet OLEDB : nouveau mot de passe de base de données (DBPROP_JETOLEDB_NEWDATABASEPASSWORD)|Indique le nouveau mot de passe à définir pour cette base de données. L’ancien mot de passe est stocké dans **Jet OLEDB : Database Password**.|
|Jet OLEDB : ODBC commande délai d’expiration (DBPROP_JETOLEDB_ODBCCOMMANDTIMEOUT)|Indique le nombre de millisecondes avant une requête ODBC distante à partir de Jet va expirer.|
|Jet OLEDB:Page verrouille à un verrou de Table (DBPROP_JETOLEDB_PAGELOCKSTOTABLELOCK)|Indique le nombre de pages doit être verrouillé dans une transaction avant que Jet tente de promouvoir le verrou en verrou de table. Si cette valeur est 0, le verrou n’est jamais promu.|
|Délai d’attente OLEDB:Page Jet (DBPROP_JETOLEDB_PAGETIMEOUT)|Indique le nombre de millisecondes de qu'attente avant de vérifier si son cache est obsolète avec le fichier de base de données Jet.|
|Pages de table Long OLEDB:Recycle Jet (DBPROP_JETOLEDB_RECYCLELONGVALUEPAGES)|Indique si Jet doit glissent récupérer les pages BLOB lorsqu’elles sont libérées.|
|Chemin d’accès OLEDB:Registry Jet (DBPROP_JETOLEDB_REGPATH)|Indique la clé de Registre Windows qui contient des valeurs pour le moteur de base de données Jet.|
|Jet OLEDB:Reset ISAM Stats (DBPROP_JETOLEDB_RESETISAMSTATS)|Indique si le schéma **Recordset** DBSCHEMA_JETOLEDB_ISAMSTATS doit réinitialiser ses compteurs de performances après avoir retourné les informations de performances.|
|Jet OLEDB : Shared Async Delay (DBPROP_JETOLEDB_SHAREDASYNCDELAY)|Indique la quantité maximale de temps, en millisecondes, Jet peut retarder les écritures asynchrones sur le disque lorsque la base de données est ouverte en mode multi-utilisateur.|
|Base de données Jet OLEDB : System (DBPROP_JETOLEDB_SYSDBPATH)|Indique le chemin d’accès et le nom pour le fichier d’informations de groupe de travail (base de données système).|
|Mode de validation Jet OLEDB:Transaction (DBPROP_JETOLEDB_TXNCOMMITMODE)|Indique si Jet écrit les données sur le disque de façon synchrone ou asynchrone lorsqu’une transaction est validée.|
|Synchronisation de validation Jet OLEDB:User (DBPROP_JETOLEDB_USERCOMMITSYNC)|Indique si les modifications apportées dans les transactions sont écrites en mode synchrone ou asynchrone.|

## <a name="provider-specific-recordset-and-command-properties"></a>Jeu d’enregistrements spécifiques au fournisseur et les propriétés de commande
 Le fournisseur Jet prend également en charge plusieurs spécifique au fournisseur **Recordset** et **commande** propriétés. Ces propriétés sont accessibles et définies par le **propriétés** collection de la **Recordset** ou **commande** objet. Le tableau répertorie le nom de la propriété ADO et de son nom de propriété OLE DB correspondant entre parenthèses.

|Nom de la propriété| Description|
|-------------------|-----------------|
|Jet OLEDB:Bulk Transactions (DBPROP_JETOLEDB_BULKNOTRANSACTIONS)|Indique si les opérations en bloc SQL sont traitées. Grandes opérations en bloc peuvent échouer lorsque traitée en raison des retards de ressource.|
|Jet Enable Fat Cursors (DBPROP_JETOLEDB_ENABLEFATCURSOR)|Indique si Jet doit mettre en cache plusieurs lignes lors du remplissage d’un jeu d’enregistrements pour les sources de la ligne à distance.|
|Taille du cache du curseur Jet OLEDB:Fat (DBPROP_JETOLEDB_FATCURSORMAXROWS)|Indique le nombre de lignes à mettre en cache lorsque vous utilisez la mise en cache de ligne de magasin de données à distance. Cette valeur est ignorée sauf si **Jet OLEDB : Enable Fat Cursors** a la valeur True.|
|Jet OLEDB : incohérent (DBPROP_JETOLEDB_INCONSISTENT)|Indique si les résultats de requête autorisent les mises à jour incohérentes.|
|Jet OLEDB : granularité (DBPROP_JETOLEDB_LOCKGRANULARITY)|Indique si une table est ouverte à l’aide du verrouillage au niveau de la ligne.|
|Jet OLEDB : ODBC Pass-Through Statement (DBPROP_JETOLEDB_ODBCPASSTHROUGH)|Indique que Jet doit transmettre le texte SQL dans un **commande** objet vers le serveur principal sans altération.|
|Jet OLEDB:Partial Bulk Operations (DBPROP_JETOLEDB_BULKPARTIAL)|Indique le comportement de Jet en cas d’échouent des opérations DML SQL.|
|OLEDB:Pass Jet via une requête d’opération en bloc (DBPROP_JETOLEDB_PASSTHROUGHBULKOP)|Indique si les requêtes qui ne retournent un **Recordset** sont transmises sans modification à la source de données.|
|Chaîne (DBPROP_JETOLEDB_ODBCPASSTHROUGHCONNECTSTRING) de connexion de OLEDB:Pass Jet via la requête|Indique la chaîne de connexion Jet utilisée pour se connecter à un magasin de données à distance. Cette valeur est ignorée sauf si **Jet OLEDB : ODBC Pass-Through Statement** a la valeur True.|
|Jet OLEDB : stockées requête (DBPROP_JETOLEDB_STOREDQUERY)|Indique si le texte de commande doit être interprété comme une requête stockée au lieu d’une commande SQL.|
|Jet OLEDB : valider des règles de jeu (DBPROP_JETOLEDB_VALIDATEONSET)|Indique si les règles de validation Jet sont évaluées lorsque les données de la colonne sont définies ou lorsque les modifications sont validées dans la base de données.|

 Par défaut, le fournisseur OLE DB pour Microsoft Jet ouvre les bases de données Microsoft Jet en mode lecture/écriture. Pour ouvrir une base de données en mode lecture seule, définissez la [Mode](../../../ado/reference/ado-api/mode-property-ado.md) propriété ADO **connexion** objet **adModeRead**.

## <a name="command-object-usage"></a>Utilisation de l’objet de commande
 Texte de commande de la [commande](../../../ado/reference/ado-api/command-object-ado.md) objet utilise le dialecte SQL Microsoft Jet. Vous pouvez spécifier des requêtes de renvoi à la ligne, les requêtes d’action et les noms de tables dans le texte de commande ; Toutefois, les procédures stockées ne sont pas prises en charge et ne doivent pas être spécifiés.

## <a name="recordset-behavior"></a>Comportement du jeu d’enregistrements
 Le moteur de base de données Microsoft Jet ne prend pas en charge les curseurs dynamiques. Par conséquent, le fournisseur OLE DB pour Microsoft Jet ne prend pas en charge la **adLockDynamic** type de curseur. Lorsqu’un curseur dynamique est demandé, le fournisseur renvoie un curseur keyset et réinitialiser la [CursorType](../../../ado/reference/ado-api/cursortype-property-ado.md) pour indiquer le type de propriété [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) retourné. En outre, si un actualisable **Recordset** est demandée (**LockType** est **adLockOptimistic**, **adLockBatchOptimistic**, ou **adLockPessimistic**) le fournisseur renvoie un curseur keyset et également réinitialiser le **CursorType** propriété.

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Le fournisseur OLE DB pour Microsoft Jet insère plusieurs propriétés dynamiques dans les **propriétés** collection de non ouverts [connexion](../../../ado/reference/ado-api/connection-object-ado.md), [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)et [Commande](../../../ado/reference/ado-api/command-object-ado.md) objets.

 Les tableaux suivants sont un index croisé des noms ADO et OLE DB pour chaque propriété dynamique. Référence du programmeur OLE DB fait référence à un nom de la propriété ADO sous le terme « Description ». Vous trouverez plus d’informations sur ces propriétés dans la référence du programmeur OLE DB.

## <a name="connection-dynamic-properties"></a>Propriétés dynamiques de connexion
 Les propriétés suivantes sont ajoutées à la **propriétés** collection de la **connexion** objet.

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Sessions actives|DBPROP_ACTIVESESSIONS|
|Annulation gérable en asynchrone|DBPROP_ASYNCTXNABORT|
|Validation gérable en asynchrone|DBPROP_ASYNCTNXCOMMIT|
|Niveaux d’Isolation de validation automatique|DBPROP_SESS_AUTOCOMMITISOLEVELS|
|Emplacement du catalogue|DBPROP_CATALOGLOCATION|
|Terme du catalogue|DBPROP_CATALOGTERM|
|Définition de colonne|DBPROP_COLUMNDEFINITION|
|Catalogue actuel|DBPROP_CURRENTCATALOG|
|Source de données|DBPROP_INIT_DATASOURCE|
|Nom de la source de données|DBPROP_DATASOURCENAME|
|Objet de Source de données modèle de thread|DBPROP_DSOTHREADMODEL|
|Nom du SGBD|DBPROP_DBMSNAME|
|Version du SGBD|DBPROP_DBMSVER|
|Regrouper par prise en charge|DBPROP_GROUPBY|
|Prise en charge de Table hétérogène|DBPROP_HETEROGENEOUSTABLES|
|Identificateur de respect de la casse|DBPROP_IDENTIFIERCASE|
|Niveaux d’isolation|DBPROP_SUPPORTEDTXNISOLEVELS|
|Conservation d’isolement|DBPROP_SUPPORTEDTXNISORETAIN|
|Identificateur de paramètres régionaux|DBPROP_INIT_LCID|
|Taille d’Index maximale|DBPROP_MAXINDEXSIZE|
|Taille de ligne maximale|DBPROP_MAXROWSIZE|
|Taille de ligne maximale inclut les BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Nombre maximal de Tables dans SELECT|DBPROP_MAXTABLESINSELECT|
|Mode|DBPROP_INIT_MODE|
|Plusieurs jeux de paramètres|DBPROP_MULTIPLEPARAMSETS|
|Plusieurs résultats|DBPROP_MULTIPLERESULTS|
|Plusieurs objets de stockage|DBPROP_MULTIPLESTORAGEOBJECTS|
|Mise à jour de plusieurs Table|DBPROP_MULTITABLEUPDATE|
|Ordre de classement NULL|DBPROP_NULLCOLLATION|
|Comportement de la concaténation des valeurs NULL|DBPROP_CONCATNULLBEHAVIOR|
|OLE DB Version|DBPROP_PROVIDEROLEDBVER|
|Prise en charge des objets OLE|DBPROP_OLEOBJECTS|
|Ouvrez la prise en charge de l’ensemble de lignes|DBPROP_OPENROWSETSUPPORT|
|Colonnes ORDER BY dans la liste de sélection|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilité du paramètre de sortie|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Passer par les accesseurs Ref|DBPROP_BYREFACCESSORS|
|Mot de passe|DBPROP_AUTH_PASSWORD|
|Type d’identificateur persistant|DBPROP_PERSISTENTIDTYPE|
|Préparation d’annulation|DBPROP_PREPAREABORTBEHAVIOR|
|Préparation de validation|DBPROP_PREPARECOMMITBEHAVIOR|
|Terme de procédure|DBPROP_PROCEDURETERM|
|Demander|DBPROP_INIT_PROMPT|
|Nom convivial du fournisseur|DBPROP_PROVIDERFRIENDLYNAME|
|Provider Name|DBPROP_PROVIDERFILENAME|
|Version du fournisseur|DBPROP_PROVIDERVER|
|Source de données en lecture seule|DBPROP_DATASOURCEREADONLY|
|Conversions d’ensemble de lignes de commande|DBPROP_ROWSETCONVERSIONSONCOMMAND|
|Terme du schéma|DBPROP_SCHEMATERM|
|Utilisation de schéma|DBPROP_SCHEMAUSAGE|
|Prise en charge SQL|DBPROP_SQLSUPPORT|
|Stockage structuré|DBPROP_STRUCTUREDSTORAGE|
|Prise en charge de sous-requête|DBPROP_SUBQUERIES|
|Terme de table|DBPROP_TABLETERM|
|DDL de transaction|DBPROP_SUPPORTEDTXNDDL|
|ID d'utilisateur|DBPROP_AUTH_USERID|
|Nom d'utilisateur|DBPROP_USERNAME|
|Handle de fenêtre|DBPROP_INIT_HWND|

## <a name="recordset-dynamic-properties"></a>Propriétés dynamiques du jeu d’enregistrements
 Les propriétés suivantes sont ajoutées à la **propriétés** collection de la **Recordset** objet.

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Ensemble de lignes en mode Append-Only|DBPROP_APPENDONLY|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Signet|DBPROP_IROWSETLOCATE|
|Signets ordonnés|DBPROP_ORDEREDBOOKMARKS|
|Mettre en cache les colonnes différées|DBPROP_CACHEDEFERRED|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification de jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Colonne accessible en écriture|DBPROP_MAYWRITECOLUMN|
|Différer la colonne|DBPROP_DEFERRED|
|Mises à jour des objets de stockage délai|DBPROP_DELAYSTORAGEOBJECTS|
|Récupérer vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Contenant des lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Signets littéral|DBPROP_LITERALBOOKMARKS|
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|
|Nombre maximal de lignes ouvrir|DBPROP_MAXOPENROWS|
|Nombre maximal de lignes en attente|DBPROP_MAXPENDINGROWS|
|Nombre maximal de lignes|DBPROP_MAXROWS|
|Utilisation de la mémoire|DBPROP_MEMORYUSAGE|
|Granularité de notification|DBPROP_NOTIFICATIONGRANULARITY|
|Phases de notification|DBPROP_NOTIFICATIONPHASES|
|Objets traités|DBPROP_TRANSACTEDOBJECT|
|Autres modifications visibles|DBPROP_OTHERUPDATEDELETE|
|Autres Inserts Visible|DBPROP_OTHERINSERT|
|Propres Changes visibles|DBPROP_OWNUPDATEDELETE|
|Propres Inserts visibles|DBPROP_OWNINSERT|
|Conserver sur Abandon|DBPROP_ABORTPRESERVE|
|Conserver lors de la validation|DBPROP_COMMITPRESERVE|
|Redémarrage rapide|DBPROP_QUICKRESTART|
|Événements réentrants|DBPROP_REENTRANTEVENTS|
|Supprimer les lignes supprimées|DBPROP_REMOVEDELETED|
|Signaler les changements multiples|DBPROP_REPORTMULTIPLECHANGES|
|Retourner les insertions en attente|DBPROP_RETURNPENDINGINSERTS|
|Notification de suppression de ligne|DBPROP_NOTIFYROWDELETE|
|Notification de modification de première ligne|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notification d’insertion de ligne|DBPROP_NOTIFYROWINSERT|
|Privilèges de ligne|DBPROP_ROWRESTRICT|
|Notification de resynchronisation de ligne|DBPROP_NOTIFYROWRESYNCH|
|Modèle de thread de ligne|DBPROP_ROWTHREADMODEL|
|Notification de modification de ligne Annuler|DBPROP_NOTIFYROWUNDOCHANGE|
|Notification de suppression de ligne Annuler|DBPROP_NOTIFYROWUNDODELETE|
|Notification d’annulation Insert ligne|DBPROP_NOTIFYROWUNDOINSERT|
|Notification de mise à jour de ligne|DBPROP_NOTIFYROWUPDATE|
|Notification de modification de Position extraction de l’ensemble de lignes|DBPROP_NOTIFYROWSETFETCHPOSISIONCHANGE|
|Notification de mise en production de l’ensemble de lignes|DBPROP_NOTIFYROWSETRELEASE|
|Faites défiler vers l’arrière|DBPROP_CANSCROLLBACKWARDS|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIPPED|
|Forte identité de ligne|DBPROP_STRONGITDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propriétés dynamiques de commande
 Les propriétés suivantes sont ajoutées à la **propriétés** collection de la **commande** objet.

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Ensemble de lignes en mode Append-Only|DBPROP_APPENDONLY|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification de jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Différer la colonne|DBPROP_DEFERRED|
|Mises à jour des objets de stockage délai|DBPROP_DELAYSTORAGEOBJECTS|
|Récupérer vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Contenant des lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Signets littéral|DBPROP_LITERALBOOKMARKS|
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|
|Mode de verrouillage|DBPROP_LOCKMODE|
|Nombre maximal de lignes ouvrir|DBPROP_MAXOPENROWS|
|Nombre maximal de lignes en attente|DBPROP_MAXPENDINGROWS|
|Nombre maximal de lignes|DBPROP_MAXROWS|
|Granularité de notification|DBPROP_NOTIFICATIONGRANULARITY|
|Phases de notification|DBPROP_NOTIFICATIONPHASES|
|Objets traités|DBPROP_TRANSACTEDOBJECT|
|Autres modifications visibles|DBPROP_OTHERUPDATEDELETE|
|Autres Inserts Visible|DBPROP_OTHERINSERT|
|Propres Changes visibles|DBPROP_OWNUPDATEDELETE|
|Propres Inserts visibles|DBPROP_OWNINSERT|
|Conserver sur Abandon|DBPROP_ABORTPRESERVE|
|Conserver lors de la validation|DBPROP_COMMITPRESERVE|
|Redémarrage rapide|DBPROP_QUICKRESTART|
|Événements réentrants|DBPROP_REENTRANTEVENTS|
|Supprimer les lignes supprimées|DBPROP_REMOVEDELETED|
|Signaler les changements multiples|DBPROP_REPORTMULTIPLECHANGES|
|Retourner les insertions en attente|DBPROP_RETURNPENDINGINSERTS|
|Notification de suppression de ligne|DBPROP_NOTIFYROWDELETE|
|Notification de modification de première ligne|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notification d’insertion de ligne|DBPROP_NOTIFYROWINSERT|
|Privilèges de ligne|DBPROP_ROWRESTRICT|
|Notification de resynchronisation de ligne|DBPROP_NOTIFYROWRESYNCH|
|Modèle de thread de ligne|DBPROP_ROWTHREADMODEL|
|Notification de modification de ligne Annuler|DBPROP_NOTIFYROWUNDOCHANGE|
|Notification de suppression de ligne Annuler|DBPROP_NOTIFYROWUNDODELETE|
|Notification d’annulation Insert ligne|DBPROP_NOTIFYROWUNDOINSERT|
|Notification de mise à jour de ligne|DBPROP_NOTIFYROWUPDATE|
|Notification de modification de Position extraction de l’ensemble de lignes|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|
|Notification de mise en production de l’ensemble de lignes|DBPROP_NOTIFYROWSETRELEASE|
|Faites défiler vers l’arrière|DBPROP_CANSCROLLBACKWARDS|
|Données du serveur lors de l’insertion|DBPROP_SERVERDATAONINSERT|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIP|
|Forte identité de ligne|DBPROP_STRONGIDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

 Pour les détails d’implémentation spécifiques et des informations fonctionnelles sur le fournisseur OLE DB pour Microsoft Jet, consultez [fournisseur Jet](https://msdn.microsoft.com/library/windows/desktop/ms722791.aspx) dans la documentation OLE DB.
