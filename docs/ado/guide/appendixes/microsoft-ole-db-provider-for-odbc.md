---
title: Fournisseur Microsoft OLE DB pour ODBC | Documents Microsoft
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
- OLE DB provider for ODBC [ADO]
- providers [ADO], OLE DB provider for ODBC
ms.assetid: 2dc0372d-e74d-4d0f-9c8c-04e5a168c148
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 612ca78e6af181aaf3e2d3b1eb16ae5fea7eec3c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-ole-db-provider-for-odbc-overview"></a>Fournisseur Microsoft OLE DB pour ODBC présentation
Pour un programmeur ADO ou RDS, l’idéal serait dans toutes les données source expose une interface OLE DB, afin que ADO peut appeler directement dans la source de données. Bien que les fournisseurs de base de données plus en plus implémentez les interfaces OLE DB, certaines sources de données ne sont pas encore exposées de cette manière. Toutefois, la plupart des systèmes SGBD utilisés aujourd'hui accessibles via ODBC.

 Pilotes ODBC sont disponibles pour les principaux SGBD utilisés aujourd'hui, y compris Microsoft SQL Server, Microsoft Access (moteur de base de données Microsoft Jet) et Microsoft FoxPro, en plus des produits non Microsoft de base de données comme Oracle.

 Le fournisseur Microsoft ODBC permet cependant ADO pour se connecter à n’importe quelle source de données ODBC. Le fournisseur est libre de threads et Unicode.

 Le fournisseur prend en charge les transactions, bien que les différents moteurs SGBD offrent différents types de prise en charge de la transaction. Par exemple, Microsoft Access prend en charge les transactions imbriquées jusqu'à cinq niveaux de profondeur.

 Il s’agit du fournisseur par défaut pour ADO et toutes les propriétés de ADO dépendant du fournisseur et les méthodes sont prises en charge.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez la **fournisseur =** argument de la [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) propriété :

```
MSDASQL
```

 La lecture de la [fournisseur](../../../ado/reference/ado-api/provider-property-ado.md) propriété retournera également cette chaîne.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion par défaut pour ce fournisseur est :

```
"Provider=MSDASQL;DSN=dsnName;UID=MyUserID;PWD=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé| Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur OLE DB pour ODBC.|
|**DSN**|Spécifie le nom de source de données.|
|**UID**|Spécifie le nom d’utilisateur.|
|**PWD**|Spécifie le mot de passe.|
|**URL**|Spécifie l’URL d’un fichier ou un répertoire publié dans un dossier Web.|

 Comme il s’agit du fournisseur par défaut pour ADO, si vous omettez le **fournisseur =** paramètre à partir de la chaîne de connexion ADO va tenter d’établir une connexion à ce fournisseur.

> [!NOTE]
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = yes** ou **Integrated Security = SSPI** au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.

 Le fournisseur ne prend pas en charge tous les paramètres de connexion spécifique en plus de ceux définis par ADO. Toutefois, le fournisseur passe tous les paramètres de connexion non-ADO pour le Gestionnaire de pilotes ODBC.

 Étant donné que vous pouvez omettre le **fournisseur** paramètre, vous pouvez composer une chaîne de connexion ADO qui est identique à une chaîne de connexion ODBC pour la même source de données. Utilisez les mêmes noms de paramètre (**pilote =**, **base de données =**, **DSN =**, et ainsi de suite), valeurs et la syntaxe que vous feriez lors de la composition d’une chaîne de connexion ODBC. Vous pouvez vous connecter avec ou sans un nom de source de données prédéfini (DSN) ou un FileDSN.

## <a name="syntax-with-a-dsn-or-filedsn"></a>Syntaxe avec un DSN ou un FileDSN :

```
"[Provider=MSDASQL;] { DSN=name | FileDSN=filename } ;
[DATABASE=database;] UID=user; PWD=password"
```

## <a name="syntax-without-a-dsn-dsn-less-connection"></a>Syntaxe sans DSN (connexion sans DSN) :

```
"[Provider=MSDASQL;] DRIVER=driver; SERVER=server;
DATABASE=database; UID=MyUserID; PWD=MyPassword"
```

## <a name="remarks"></a>Notes
 Si vous utilisez un **DSN** ou **FileDSN**, il doit être défini via l’administrateur de Source de données ODBC dans le panneau de configuration Windows. Dans Microsoft Windows 2000, l’administrateur ODBC se trouve sous Outils d’administration. Dans les versions antérieures de Windows, l’icône de l’administrateur ODBC est appelée **ODBC 32 bits** ou simplement **ODBC**.

 Comme alternative au paramètre un **DSN**, vous pouvez spécifier le pilote ODBC (**pilote =**), tels que « SQL Server » ; le nom du serveur (**SERVER =**) ; et le nom de la base de données (**Base de données =**).

 Vous pouvez également spécifier un nom de compte d’utilisateur (**UID =**) et le mot de passe du compte d’utilisateur (**PWD =**) dans les paramètres spécifiques à ODBC ou dans la norme définis par ADO *utilisateur* et *mot de passe* paramètres.

 Bien qu’un **DSN** définition spécifie déjà une base de données, vous pouvez spécifier *un* *base de données* paramètre à un **DSN** pour se connecter pour une base de données. Il est conseillé de toujours inclure *le* *base de données* paramètre lorsque vous utilisez un **DSN**. Cela permet de garantir que vous vous connectez à la base de données si un autre utilisateur a modifié le paramètre de base de données de valeur par défaut depuis votre dernière vérification le **DSN** définition.

## <a name="provider-specific-connection-properties"></a>Propriétés de connexion spécifique au fournisseur
 Le fournisseur OLE DB pour ODBC ajoute plusieurs propriétés pour le [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection de la **connexion** objet. Le tableau suivant répertorie ces propriétés avec le nom de propriété OLE DB correspondant entre parenthèses.

|Nom de la propriété| Description|
|-------------------|-----------------|
|Procédures accessibles (KAGPROP_ACCESSIBLEPROCEDURES)|Indique si l’utilisateur a accès à des procédures stockées.|
|Table accessible (KAGPROP_ACCESSIBLETABLES)|Indique si l’utilisateur a l’autorisation d’exécuter des instructions SELECT sur les tables de base de données.|
|Instructions actives (KAGPROP_ACTIVESTATEMENTS)|Indique le nombre de handles qu'un pilote ODBC peut prendre en charge sur une connexion.|
|Nom du pilote (KAGPROP_DRIVERNAME)|Indique le nom de fichier du pilote ODBC.|
|Pilote ODBC Version (KAGPROP_DRIVERODBCVER)|Indique la version d’ODBC qui prend en charge par ce pilote.|
|Utilisation du fichier (KAGPROP_FILEUSAGE)|Indique comment le pilote traite un fichier dans la source de données. sous la forme d’une table ou un catalogue.|
|Comme la Clause d’échappement (KAGPROP_LIKEESCAPECLAUSE)|Indique si le pilote prend en charge la définition et l’utilisation d’un caractère d’échappement de pour le caractère de pourcentage (%) et le caractère de soulignement (_) dans le prédicat LIKE d’une clause WHERE.|
|Nombre maximal de colonnes Group By (KAGPROP_MAXCOLUMNSINGROUPBY)|Indique le nombre maximal de colonnes qui peuvent être répertoriées dans la clause GROUP BY d’une instruction SELECT.|
|Nombre maximal de colonnes dans l’Index (KAGPROP_MAXCOLUMNSININDEX)|Indique le nombre maximal de colonnes qui peuvent être inclus dans un index.|
|Nombre maximal de colonnes dans la clause Order By (KAGPROP_MAXCOLUMNSINORDERBY)|Indique le nombre maximal de colonnes qui peuvent être répertoriées dans la clause ORDER BY d’une instruction SELECT.|
|Nombre maximal de colonnes dans Select (KAGPROP_MAXCOLUMNSINSELECT)|Indique le nombre maximal de colonnes qui peuvent être répertoriées dans la partie SELECT d’une instruction SELECT.|
|Nombre maximal de colonnes dans la Table (KAGPROP_MAXCOLUMNSINTABLE)|Indique le nombre maximal de colonnes autorisé dans une table.|
|Fonctions numériques (KAGPROP_NUMERICFUNCTIONS)|Indique les fonctions numériques sont pris en charge par le pilote ODBC. Pour une liste des noms de fonction et valeurs associées utilisés dans ce masque de bits, consultez [les fonctions scalaires annexe e :](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), dans la documentation ODBC.|
|Fonctionnalités de jointure externe (KAGPROP_OJCAPABILITY)|Indique les types de jointures externes pris en charge par le fournisseur.|
|Jointures externes (KAGPROP_OUTERJOINS)|Indique si le fournisseur prend en charge les jointures externes.|
|Caractères spéciaux (KAGPROP_SPECIALCHARACTERS)|Indique les caractères ayant une signification particulière pour le pilote ODBC.|
|Procédures stockées (KAGPROP_PROCEDURES)|Indique si les procédures stockées sont disponibles pour une utilisation avec ce pilote ODBC.|
|Fonctions de chaîne (KAGPROP_STRINGFUNCTIONS)|Indique les fonctions de chaîne sont pris en charge par le pilote ODBC. Pour une liste des noms de fonction et valeurs associées utilisés dans ce masque de bits, consultez [les fonctions scalaires annexe e :](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), dans la documentation ODBC.|
|Fonctions système (KAGPROP_SYSTEMFUNCTIONS)|Indique les fonctions système sont prises en charge par le pilote ODBC. Pour une liste des noms de fonction et valeurs associées utilisés dans ce masque de bits, consultez [les fonctions scalaires annexe e :](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), dans la documentation ODBC.|
|Fonctions de Date/heure (KAGPROP_TIMEDATEFUNCTIONS)|Indique les fonctions de date et heure sont pris en charge par le pilote ODBC. Pour une liste des noms de fonction et valeurs associées utilisés dans ce masque de bits, consultez [les fonctions scalaires annexe e :](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), dans la documentation ODBC.|
|Prise en charge de grammaire SQL (KAGPROP_ODBCSQLCONFORMANCE)|Indique la grammaire SQL qui prend en charge par le pilote ODBC.|

## <a name="provider-specific-recordset-and-command-properties"></a>Jeu d’enregistrements spécifiques au fournisseur et les propriétés de commande
 Le fournisseur OLE DB pour ODBC ajoute plusieurs propriétés pour le **propriétés** collection de la **Recordset** et **commande** objets. Le tableau suivant répertorie ces propriétés avec le nom de propriété OLE DB correspondant entre parenthèses.

|Nom de la propriété| Description|
|-------------------|-----------------|
|Requête en fonction des mises à jour/supprime/insertions (KAGPROP_QUERYBASEDUPDATES)|Indique si les insertions, suppressions et mises à jour peuvent être effectuées à l’aide de requêtes SQL.|
|Type de concurrence ODBC (KAGPROP_CONCURRENCY)|Indique la méthode utilisée pour réduire les problèmes éventuels engendrés par deux utilisateurs qui tentent d’accéder simultanément aux mêmes données à partir de la source de données.|
|Accessibilité des objets BLOB sur un curseur avant uniquement (KAGPROP_BLOBSONFOCURSOR)|Indique si le BLOB **champs** sont accessibles lors de l’utilisation d’un curseur avant uniquement.|
|Inclure les SQL_FLOAT, SQL_DOUBLE et SQL_REAL dans les clauses QBU WHERE (KAGPROP_INCLUDENONEXACT)|Indique si les valeurs SQL_FLOAT, SQL_DOUBLE et SQL_REAL peuvent être inclus dans une clause QBU WHERE.|
|Position sur la dernière ligne après une insertion (KAGPROP_POSITIONONNEWROW)|Indique qu’après l’insertion d’un nouvel enregistrement dans une table, la dernière ligne dans la table proviennent de la ligne actuelle.|
|IRowsetChangeExtInfo (KAGPROP_IROWSETCHANGEEXTINFO)|Indique si le **IRowsetChange** interface prend en charge les informations étendues.|
|Type de curseur ODBC (KAGPROP_CURSOR)|Indique le type de curseur utilisé par le **Recordset**.|
|Générer un ensemble de lignes qui peut être marshalée (KAGPROP_MARSHALLABLE)|Indique que le pilote ODBC génère un jeu d’enregistrements qui peut être marshalé.|

## <a name="command-text"></a>Texte de la commande
 Utilisation de la [commande](../../../ado/reference/ado-api/command-object-ado.md) objet dépend en grande partie de la source de données, et le type d’instruction de requête ou une commande qu’elle accepte.

 ODBC fournit une syntaxe spécifique pour appeler les procédures stockées. Pour le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) propriété d’un **commande** objet, le *CommandText* l’argument de la **Execute** méthode sur un [ Connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet, ou le *Source* l’argument de la **ouvrir** méthode sur un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet, passe dans une chaîne avec cette syntaxe :

```
"{ [ ? = ] call procedure [ ( ? [, ? [ , … ]] ) ] }"
```

 Chaque **?** fait référence à un objet dans le [paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md) collection. Le premier **?** références **paramètres**(0), la prochaine **?** références **paramètres**(1), et ainsi de suite.

 Les références de paramètre sont facultatives et dépendent de la structure de la procédure stockée. Si vous souhaitez appeler une procédure stockée qui ne définit aucun paramètre, votre chaîne ressemble à ce qui suit :

```
"{ call procedure }"
```

 Si vous avez deux paramètres de requête, votre chaîne se présente comme suit :

```
"{ call procedure ( ?, ? ) }"
```

 Si la procédure stockée renvoie une valeur, la valeur de retour est traitée comme un autre paramètre. Si vous ne disposez d’aucun paramètre de requête, mais vous n’avez pas une valeur de retour, votre chaîne se présente comme suit :

```
"{ ? = call procedure }"
```

 Enfin, si vous avez une valeur de retour et les deux paramètres de requête, votre chaîne se présente comme suit :

```
"{ ? = call procedure ( ?, ? ) }"
```

## <a name="recordset-behavior"></a>Comportement du jeu d’enregistrements
 Les tableaux suivants répertorient les méthodes ADO standard et les propriétés disponibles sur un **Recordset** objet ouvert avec ce fournisseur.

 Pour plus d’informations sur les **Recordset** comportement de configuration de votre fournisseur, exécutez le [prend en charge](../../../ado/reference/ado-api/supports-method.md) (méthode) et énumérer le **propriétés** collection de la **Recordset** pour déterminer si les propriétés dynamiques spécifiques au fournisseur sont présentes.

 Disponibilité de l’objet ADO standard **Recordset** propriétés :

|Propriété|ForwardOnly|Dynamique|Keyset|Statique|
|--------------|-----------------|-------------|------------|------------|
|[AbsolutePage](../../../ado/reference/ado-api/absolutepage-property-ado.md)|non disponible|non disponible|lecture/écriture|lecture/écriture|
|[AbsolutePosition](../../../ado/reference/ado-api/absoluteposition-property-ado.md)|non disponible|non disponible|lecture/écriture|lecture/écriture|
|[ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[BOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md)|Lecture seule|Lecture seule|Lecture seule|Lecture seule|
|[Signet](../../../ado/reference/ado-api/bookmark-property-ado.md)|non disponible|non disponible|lecture/écriture|lecture/écriture|
|[CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[CursorType](../../../ado/reference/ado-api/cursortype-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[EditMode](../../../ado/reference/ado-api/editmode-property.md)|Lecture seule|Lecture seule|Lecture seule|Lecture seule|
|[Filtre](../../../ado/reference/ado-api/filter-property.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[LockType](../../../ado/reference/ado-api/locktype-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[MarshalOptions](../../../ado/reference/ado-api/marshaloptions-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[MaxRecords](../../../ado/reference/ado-api/maxrecords-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[PageCount](../../../ado/reference/ado-api/pagecount-property-ado.md)|lecture/écriture|non disponible|Lecture seule|Lecture seule|
|[PageSize](../../../ado/reference/ado-api/pagesize-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md)|lecture/écriture|non disponible|Lecture seule|Lecture seule|
|[Source](../../../ado/reference/ado-api/source-property-ado-recordset.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[État](../../../ado/reference/ado-api/state-property-ado.md)|Lecture seule|Lecture seule|Lecture seule|Lecture seule|
|[État](../../../ado/reference/ado-api/status-property-ado-recordset.md)|Lecture seule|Lecture seule|Lecture seule|Lecture seule|

 Le [AbsolutePosition](../../../ado/reference/ado-api/absoluteposition-property-ado.md) et [AbsolutePage](../../../ado/reference/ado-api/absolutepage-property-ado.md) propriétés sont en écriture seule lorsqu’ADO est utilisé avec la version 1.0 du fournisseur Microsoft OLE DB pour ODBC.

 Disponibilité de l’objet ADO standard **Recordset** méthodes :

|Méthode|ForwardOnly|Dynamique|Keyset|Statique|
|------------|-----------------|-------------|------------|------------|
|[AddNew](../../../ado/reference/ado-api/addnew-method-ado.md)|Oui|Oui|Oui|Oui|
|[Annuler](../../../ado/reference/ado-api/cancel-method-ado.md)|Oui|Oui|Oui|Oui|
|[CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md)|Oui|Oui|Oui|Oui|
|[CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md)|Oui|Oui|Oui|Oui|
|[Clone](../../../ado/reference/ado-api/clone-method-ado.md)|non|Non|Oui|Oui|
|[Fermer](../../../ado/reference/ado-api/close-method-ado.md)|Oui|Oui|Oui|Oui|
|[Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md)|Oui|Oui|Oui|Oui|
|[GetRows](../../../ado/reference/ado-api/getrows-method-ado.md)|Oui|Oui|Oui|Oui|
|[Déplacer](../../../ado/reference/ado-api/move-method-ado.md)|Oui|Oui|Oui|Oui|
|[MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Oui|Oui|Oui|Oui|
|[MoveLast](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|non|Oui|Oui|Oui|
|[MoveNext](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Oui|Oui|Oui|Oui|
|[MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|non|Oui|Oui|Oui|
|[NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)*|Oui|Oui|Oui|Oui|
|[Ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md)|Oui|Oui|Oui|Oui|
|[Requery)](../../../ado/reference/ado-api/requery-method.md)|Oui|Oui|Oui|Oui|
|[Resynchronisation](../../../ado/reference/ado-api/resync-method.md)|non|Non|Oui|Oui|
|[Prise en charge](../../../ado/reference/ado-api/supports-method.md)|Oui|Oui|Oui|Oui|
|[Update](../../../ado/reference/ado-api/update-method.md)|Oui|Oui|Oui|Oui|
|[UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)|Oui|Oui|Oui|Oui|

 * N’est pas pris en charge pour les bases de données Microsoft Access.

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Le fournisseur Microsoft OLE DB pour ODBC insère plusieurs propriétés dynamiques dans les **propriétés** collection de non ouverts [connexion](../../../ado/reference/ado-api/connection-object-ado.md), [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)et [Commande](../../../ado/reference/ado-api/command-object-ado.md) objets.

 Les tableaux suivants sont un index croisé des noms ADO et OLE DB pour chaque propriété dynamique. Référence du programmeur OLE DB fait référence à un nom de la propriété ADO sous le terme « Description ». Vous trouverez plus d’informations sur ces propriétés dans la référence du programmeur OLE DB. Recherchez le nom de propriété OLE DB dans l’Index ou consultez [propriétés OLE DB annexe c :](http://msdn.microsoft.com/en-us/deded3ff-f508-4e1b-b2b1-fd9afd3bd292).

## <a name="connection-dynamic-properties"></a>Propriétés dynamiques de connexion
 Les propriétés suivantes sont ajoutées à la **connexion** l’objet **propriétés** collection.

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
|Emplacement|DBPROP_INIT_LOCATION|
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
|Services OLE DB|DBPROP_INIT_OLEDBSERVICES|
|OLE DB Version|DBPROP_PROVIDEROLEDBVER|
|Prise en charge des objets OLE|DBPROP_OLEOBJECTS|
|Ouvrez la prise en charge de l’ensemble de lignes|DBPROP_OPENROWSETSUPPORT|
|Colonnes ORDER BY dans la liste de sélection|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilité du paramètre de sortie|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Mot de passe|DBPROP_AUTH_PASSWORD|
|Passer par les accesseurs Ref|DBPROP_BYREFACCESSORS|
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
 Les propriétés suivantes sont ajoutées à la **Recordset** l’objet **propriétés** collection.

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification de jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
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
|IRowsetResynch||
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
|Lignes uniques|DBPROP_UNIQUEROWS|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propriétés dynamiques de commande
 Les propriétés suivantes sont ajoutées à la **commande** l’objet **propriétés** collection.

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification de jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
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
|IRowsetResynch||
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
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIP|
|Forte identité de ligne|DBPROP_STRONGIDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

 Pour plus d’informations concernant l’implémentation spécifique et des informations fonctionnelles sur le fournisseur Microsoft OLE DB pour ODBC, consultez le [de référence du programmeur OLE DB](http://msdn.microsoft.com/en-us/3c5e2dd5-35e5-4a93-ac3a-3818bb43bbf8) ou visitez le site d’accès aux données et stockage Web du centre de développement sur MSDN.

## <a name="see-also"></a>Voir aussi
 [Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md) [CommandText, propriété (ADO)](../../../ado/reference/ado-api/commandtext-property-ado.md) [objet de connexion (ADO)](../../../ado/reference/ado-api/connection-object-ado.md) [ConnectionString, propriété (ADO)](../../../ado/reference/ado-api/connectionstring-property-ado.md) [exécuter Méthode (commande ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md) [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md) [Collection de paramètres (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md) [Collection de propriétés (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md) [Fournisseur, propriété (ADO)](../../../ado/reference/ado-api/provider-property-ado.md) [l’objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md) [prend en charge (méthode)](../../../ado/reference/ado-api/supports-method.md)
