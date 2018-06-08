---
title: Fournisseur Microsoft OLE DB pour SQL Server | Documents Microsoft
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
- providers [ADO], OLE DB provider for SQL Server
- OLE DB provider for SQL Server [ADO]
- SQLOLEDB [ADO]
ms.assetid: 99bc40c4-9181-4ca1-a06f-9a1a914a0b7b
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bbe50621dc248a3f11368717bbe9423b5a8b59e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-ole-db-provider-for-sql-server-overview"></a>Fournisseur Microsoft OLE DB pour la présentation de SQL Server
Le fournisseur Microsoft OLE DB pour SQL Server, SQLOLEDB, permet à ADO pour accéder à Microsoft SQL Server.

**Remarque :** il n’est pas recommandé d’utiliser de ce pilote pour tout nouveau développement. Le nouveau fournisseur OLE DB est appelé le [pilote Microsoft OLE DB pour SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL) qui sera mis à jour avec les fonctionnalités du serveur les plus récentes à l’avenir.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez la *fournisseur* l’argument de la [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) propriété :

```
SQLOLEDB
```

 Cette valeur peut également être définie ou lue à l’aide du [fournisseur](../../../ado/reference/ado-api/provider-property-ado.md) propriété.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion par défaut pour ce fournisseur est :

```
"Provider=SQLOLEDB;Data Source=serverName;"
Initial Catalog=databaseName;
User ID=MyUserID;Password=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé| Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur OLE DB pour SQL Server.|
|**Source de données** ou **Server**|Spécifie le nom d’un serveur.|
|**Catalogue initial** ou **base de données**|Spécifie le nom d’une base de données sur le serveur.|
|**ID d’utilisateur** ou **uid**|Spécifie le nom d’utilisateur (pour l’authentification SQL Server).|
|**Mot de passe** ou **pwd**|Spécifie le mot de passe (authentification SQL Server).|

> [!NOTE]
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = yes** ou **Integrated Security = SSPI** au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.

## <a name="provider-specific-connection-parameters"></a>Paramètres de connexion spécifique au fournisseur
 Le fournisseur prend en charge plusieurs paramètres de connexion spécifique au fournisseur en plus de ceux définis par ADO. Comme avec les propriétés de connexion ADO, ces propriétés spécifiques au fournisseur peuvent être définies la [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection d’un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) ou peut être définie dans le cadre de la **ConnectionString**.

|Paramètre| Description|
|---------------|-----------------|
|Trusted_Connection|Indique le mode d’authentification utilisateur. Cela peut être défini sur **Oui** ou **non**. La valeur par défaut est **non**. Si cette propriété est définie sur **Oui**, SQLOLEDB utilise le Mode d’authentification Microsoft Windows NT pour autoriser l’accès utilisateur à la base de données SQL Server spécifiée par le **emplacement** et [source de données ](../../../ado/reference/ado-api/datasource-property-ado.md) valeurs de propriété. Si cette propriété est définie sur **non**, SQLOLEDB utilise le Mode mixte pour autoriser l’accès utilisateur à la base de données SQL Server. Le compte de connexion SQL Server et le mot de passe sont spécifiés dans le **Id utilisateur** et **mot de passe** propriétés.|
|Langue actuelle|Indique un nom de langage SQL Server. Identifie la langue utilisée pour le choix et la mise en forme des messages système. La langue doit être installée sur le serveur SQL Server, la connexion échoue lors de l’ouverture dans le cas contraire.|
|Adresse réseau|Indique l’adresse réseau du serveur SQL spécifié par le **emplacement** propriété.|
|Bibliothèque réseau|Indique le nom de la bibliothèque réseau (DLL) utilisée pour communiquer avec le serveur SQL Server. Le nom ne doit pas inclure le chemin d'accès ni l'extension de nom de fichier .dll. La valeur par défaut est fournie par la configuration du client SQL Server.|
|Utiliser la procédure de préparation|Détermine si SQL Server crée des procédures stockées temporaires lors de la préparation de commandes (par le **Prepared** propriété).|
|Traduire automatiquement|Indique si les caractères OEM/ANSI sont convertis. Cette propriété peut être définie **True** ou **False**. La valeur par défaut est **True**. Si cette propriété est définie sur **True**, SQLOLEDB effectue la conversion de caractères OEM/ANSI lorsque les chaînes de caractères multioctets sont extraites ou envoyées à SQL Server. Si cette propriété est définie sur **False**, SQLOLEDB n’effectue pas de conversion de caractères OEM/ANSI sur les données de chaîne de caractères multioctets.|
|Taille du paquet|Indique la taille du paquet réseau en octets. La valeur de propriété de taille de paquet doit être comprise entre 512 et 32767. La taille du paquet réseau SQLOLEDB par défaut est 4096.|
|Application Name|Indique le nom de l’application cliente.|
|ID Station de travail|Chaîne identifiant la station de travail.|

## <a name="command-object-usage"></a>Utilisation de l’objet de commande
 SQLOLEDB accepte un amalgame d’ODBC, ANSI et spécifiques à SQL Server Transact-SQL comme une syntaxe valide. Par exemple, l'instruction SQL suivante utilise une séquence d'échappement ODBC SQL pour spécifier la fonction de chaîne LCASE :

```
SELECT customerid={fn LCASE(CustomerID)} FROM Customers

```

 LCASE retourne une chaîne de caractères, en convertissant toutes les majuscules en leurs équivalents minuscules. La fonction de chaîne SQL ANSI LOWER effectue la même opération, par conséquent, l’instruction SQL suivante est ANSI équivalente à l’instruction ODBC présentée précédemment :

```
SELECT customerid=LOWER(CustomerID) FROM Customers

```

 SQLOLEDB traite correctement une forme quelconque de l’instruction lorsque spécifiés sous forme de texte pour une commande.

## <a name="stored-procedures"></a>Procédures stockées
 Lorsque l’exécution d’un serveur SQL Server stockées procédure à l’aide d’une commande SQLOLEDB, utilisez la séquence d’échappement d’appel ODBC procédure dans le texte de commande. SQLOLEDB utilise ensuite le mécanisme d’appel de procédure distante de SQL Server pour optimiser le traitement des commandes. Par exemple, l’instruction SQL ODBC suivante est le texte de commande par défaut sur le formulaire de Transact-SQL :

## <a name="odbc-sql"></a>ODBC SQL

```
{call SalesByCategory('Produce', '1995')}

```

## <a name="transact-sql"></a>Transact-SQL

```
EXECUTE SalesByCategory 'Produce', '1995'

```

## <a name="sql-server-features"></a>Fonctionnalités de SQL Server
 Avec SQL Server, ADO peut utiliser XML for **commande** d’entrée et de récupérer les résultats au format de flux de données XML plutôt que dans **Recordset** objets. Pour plus d’informations, consultez [à l’aide de flux de données pour l’entrée de commande](../../../ado/guide/data/command-streams.md) et [la récupération des jeux de résultats en flux](../../../ado/guide/data/retrieving-resultsets-into-streams.md).

### <a name="accessing-sqlvariant-data-using-mdac-27-mdac-28-or-windows-dac-60"></a>L’accès aux données sql_variant à l’aide de Windows DAC 6.0, MDAC 2.8 ou MDAC 2.7
 Microsoft SQL Server dispose d’un type de données appelé **sql_variant**. Similaire à OLE DB **DBTYPE_VARIANT**, le **sql_variant** type de données peut stocker des données de différents types. Toutefois, il existe quelques différences importantes entre **DBTYPE_VARIANT** et **sql_variant**. ADO gère également les données stockées en tant qu’un **sql_variant** valeur différemment de la façon dont il gère les autres types de données. La liste suivante décrit les problèmes à prendre en compte les quand vous accédez à des données SQL Server stockées dans des colonnes de type **sql_variant**.

-   MDAC 2.7, MDAC 2.8 et Windows Data Access Components (Windows DAC) 6.0, le fournisseur OLE DB pour SQL Server prend en charge la **sql_variant** type. Le fournisseur OLE DB pour ODBC ne fait pas.

-   Le **sql_variant** type ne correspond pas exactement le **DBTYPE_VARIANT** type de données.  Le **sql_variant** type prend en charge plusieurs nouveaux sous-types ne pas pris en charge par **DBTYPE_VARIANT,** notamment **GUID**, **ANSI** (non-UNICODE) chaînes, et **BIGINT**. À l’aide de sous-types autre que ceux répertoriés plus haut fonctionneront correctement.

-   Le **sql_variant** sous-type **numérique** ne correspond pas à la **DBTYPE_DECIMAL** taille.

-   Plusieurs conversions de types de données entraîne des types qui ne correspondent pas. Par exemple, la conversion forcée d’un **sql_variant** avec un sous-type de **GUID** à un **DBTYPE_VARIANT** un sous-type de **safearray**(octets) . Conversion de ce type de sauvegarde à un **sql_variant** un nouveau sous-type de **tableau**(octets).

-   **Jeu d’enregistrements** champs qui contiennent des **sql_variant** les données peuvent être exécutés à distance (marshaler) ou persistante uniquement si le **sql_variant** contient des sous-types spécifiques. Une tentative à distance ou de conserver les données avec les éléments suivants non pris en charge sous-types provoque une erreur d’exécution (non pris en charge la conversion) à partir du fournisseur de persistance Microsoft (MSPersist) : **VT_VARIANT**, **VT_RECORD**, **VT_ILLEGAL**, **VT_UNKNOWN**, **VT_BSTR**, et **VT_DISPATCH.**

-   Le fournisseur OLE DB pour SQL Server dans Windows DAC 6.0 MDAC 2.7 et MDAC 2.8 a une propriété dynamique appelée **autoriser les variantes Native** qui, comme son nom l’indique, permet aux développeurs d’accéder à la **sql_variant** dans son format natif, par opposition à un **DBTYPE_VARIANT**. Si cette propriété est définie et un **Recordset** est ouvert avec le moteur de curseur Client (**adUseClient**), la **Recordset.Open** appel échoue. Si cette propriété est définie et qu’une **Recordset** est ouvert avec les curseurs côté serveur (**adUseServer**), la **Recordset.Open** appel réussi, mais l’accès aux colonnes de type **sql_variant** génère une erreur.

-   Dans les applications clientes qui utilisent MDAC 2.5, **sql_variant** données peuvent être utilisées avec des requêtes par rapport à Microsoft SQL Server. Toutefois, les valeurs de la **sql_variant** les données sont traitées en tant que chaînes. Ces applications clientes doivent être mis à niveau Windows DAC 6.0, MDAC 2.8 ou MDAC 2.7.

## <a name="recordset-behavior"></a>Comportement du jeu d’enregistrements
 SQLOLEDB ne peut pas utiliser les curseurs SQL Server pour prendre en charge les multiples résultats générés par plusieurs commandes. Si un consommateur demande un jeu d’enregistrements nécessitant une prise en charge du curseur de SQL Server, une erreur se produit si le texte de commande utilisé génère plus d’un seul recordset comme résultat.

 Des jeux d’enregistrements SQLOLEDB permettant le défilement est pris en charge par les curseurs de SQL Server. SQL Server impose des limitations aux curseurs qui sont sensibles aux modifications apportées par d’autres utilisateurs de la base de données. Plus précisément, les lignes dans certains curseurs ne peuvent pas être triées, et tente de créer un jeu d’enregistrements à l’aide d’une commande contenant une clause SQL ORDER BY peut échouer.

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Le fournisseur Microsoft OLE DB pour SQL Server insère plusieurs propriétés dynamiques dans les **propriétés** collection de non ouverts [connexion](../../../ado/reference/ado-api/connection-object-ado.md), [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)et [ Commande](../../../ado/reference/ado-api/command-object-ado.md) objets.

 Les tableaux suivants sont un index croisé des noms ADO et OLE DB pour chaque propriété dynamique. Référence du programmeur OLE DB fait référence à un nom de la propriété ADO par le terme « Description ». Vous trouverez plus d’informations sur ces propriétés dans la référence du programmeur OLE DB. Recherchez le nom de propriété OLE DB dans l’Index ou consultez [propriétés OLE DB annexe c :](http://msdn.microsoft.com/en-us/deded3ff-f508-4e1b-b2b1-fd9afd3bd292).

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
|Connect Timeout|DBPROP_INIT_TIMEOUT|
|Catalogue actuel|DBPROP_CURRENTCATALOG|
|Source de données|DBPROP_INIT_DATASOURCE|
|Nom de la source de données|DBPROP_DATASOURCENAME|
|Objet de Source de données modèle de thread|DBPROP_DSOTHREADMODEL|
|Nom du SGBD|DBPROP_DBMSNAME|
|Version du SGBD|DBPROP_DBMSVER|
|Extended Properties|DBPROP_INIT_PROVIDERSTRING|
|Regrouper par prise en charge|DBPROP_GROUPBY|
|Prise en charge de Table hétérogène|DBPROP_HETEROGENEOUSTABLES|
|Identificateur de respect de la casse|DBPROP_IDENTIFIERCASE|
|Catalogue initial|DBPROP_INIT_CATALOG|
|Niveaux d’isolation|DBPROP_SUPPORTEDTXNISOLEVELS|
|Conservation d’isolement|DBPROP_SUPPORTEDTXNISORETAIN|
|Identificateur de paramètres régionaux|DBPROP_INIT_LCID|
|Taille d’Index maximale|DBPROP_MAXINDEXSIZE|
|Taille de ligne maximale|DBPROP_MAXROWSIZE|
|Taille de ligne maximale inclut les BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Nombre maximal de Tables dans SELECT|DBPROP_MAXTABLESINSELECT|
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
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|
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
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification de jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Délai de commande|DBPROP_COMMANDTIMEOUT|
|Différer la colonne|DBPROP_DEFERRED|
|Mises à jour des objets de stockage délai|DBPROP_DELAYSTORAGEOBJECTS|
|Récupérer vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Contenant des lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Signets littéral|DBPROP_LITERALBOOKMARKS|
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|
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
|Notification de modification de Position extraction de l’ensemble de lignes|DBPROP_NOTIFYROWSETFETCHPOSISIONCHANGE|
|Notification de mise en production de l’ensemble de lignes|DBPROP_NOTIFYROWSETRELEASE|
|Faites défiler vers l’arrière|DBPROP_CANSCROLLBACKWARDS|
|Curseur côté serveur|DBPROP_SERVERCURSOR|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIPPED|
|Forte identité de ligne|DBPROP_STRONGITDENTITY|
|Lignes uniques|DBPROP_UNIQUEROWS|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propriétés dynamiques de commande
 Les propriétés suivantes sont ajoutées à la **propriétés** collection de la **commande** objet.

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Chemin d’accès de base|SSPROP_STREAM_BASEPATH|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification de jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Type de contenu|SSPROP_STREAM_CONTENTTYPE|
|Extraction de curseur automatique|SSPROP_CURSORAUTOFETCH|
|Différer la colonne|DBPROP_DEFERRED|
|Préparation différée|SSPROP_DEFERPREPARE|
|Mises à jour des objets de stockage délai|DBPROP_DELAYSTORAGEOBJECTS|
|Récupérer vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Contenant des lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch|DBPROP_IRowsetResynch|
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
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
|Propriété encodage de sortie|DBPROP_OUTPUTENCODING|
|Propriété de flux de sortie|DBPROP_OUTPUTSTREAM|
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
|Curseur côté serveur|DBPROP_SERVERCURSOR|
|Données du serveur lors de l’insertion|DBPROP_SERVERDATAONINSERT|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIP|
|Forte identité de ligne|DBPROP_STRONGIDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|
|XML racine|SSPROP_STREAM_XMLROOT|
|XSL|SSPROP_STREAM_XSL|

 Pour les détails d’implémentation spécifiques et des informations fonctionnelles sur le fournisseur OLE DB Microsoft SQL Server, consultez le [fournisseur SQL Server](http://msdn.microsoft.com/en-us/adf1d6c4-5930-444a-9248-ff1979729635).

## <a name="see-also"></a>Voir aussi
 [ConnectionString, propriété (ADO)](../../../ado/reference/ado-api/connectionstring-property-ado.md) [fournisseur, propriété (ADO)](../../../ado/reference/ado-api/provider-property-ado.md) [l’objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
