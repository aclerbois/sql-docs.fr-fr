---
title: bcp_setcolfmt | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-extensions-bulk-copy-functions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- bcp_setcolfmt
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_setcolfmt function
ms.assetid: afb47987-39e7-4079-ad66-e0abf4d4c72b
caps.latest.revision: 36
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ad3993a22e0b2ec2091b0d37598f9cd6546d34aa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bcpsetcolfmt"></a>bcp_setcolfmt
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le **bcp_setcolfmt** fonction remplace la [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md). Lors de la spécification du classement des colonnes, le **bcp_setcolfmt** fonction doit être utilisée. [bcp_setbulkmode](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setbulkmode.md) peut être utilisé pour spécifier plusieurs formats de colonne.  
  
 Cette fonction propose une approche souple pour la définition du format des colonnes dans le cadre d'une opération de copie en bloc. Elle est employée pour définir des attributs de format de colonne individuels. Chaque appel à **bcp_setcolfmt** définit un attribut de format de colonne.  
  
 Le **bcp_setcolfmt** fonction spécifie le format source ou cible des données dans un fichier utilisateur. Lorsqu’il est utilisé comme format source, **bcp_setcolfmt** Spécifie le format d’un fichier de données existant utilisé comme source de données des données dans une copie en bloc dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsqu’il est utilisé comme format cible, le fichier de données est créé à l’aide des formats de colonne spécifiés avec **bcp_setcolfmt**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RETCODE bcp_setcolfmt (  
        HDBC hdbc,  
        INT field,  
        INT property,  
        void* pValue,  
        INT cbValue);  
```  
  
## <a name="arguments"></a>Arguments  
 *pas*  
 Handle de connexion ODBC compatible avec la copie en bloc.  
  
 *field*  
 Numéro de colonne ordinal pour lequel la propriété est définie.  
  
 *propriété*  
 L'une des constantes de propriété. Les constantes de propriété sont définies dans le tableau qui suit.  
  
|Propriété|Valeur| Description|  
|--------------|-----------|-----------------|  
|BCP_FMT_TYPE|BYTE|Type de données de la colonne dans le fichier utilisateur. Si le type de données est différent de celui de la colonne correspondante dans la table de base de données, la copie en bloc convertit, si possible, les données.<br /><br /> Le paramètre BCP_FMT_TYPE est énuméré par les jetons de type de données SQL Server dans sqlncli.h, et non par les énumérateurs de type de données C ODBC. Par exemple, vous pouvez spécifier une chaîne de caractères (type de données ODBC SQL_C_CHAR) à l'aide du type SQLCHARACTER propre à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Pour spécifier la représentation des données par défaut pour le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], attribuez la valeur 0 à ce paramètre.<br /><br /> Pour une copie en bloc SQL Server dans un fichier, lorsque BCP_FMT_TYPE est SQLDECIMAL ou SQLNUMERIC, si la colonne source n’est pas **décimal** ou **numérique**, la précision par défaut et l’échelle sont utilisés. Sinon, si la colonne source est **décimal** ou **numérique**, la précision et l’échelle de la colonne source sont utilisées.|  
|BCP_FMT_INDICATOR_LEN|INT|Longueur, en octets, de l'indicateur (préfixe).<br /><br /> Longueur, en octets, d'un indicateur de longueur/null au sein des données de la colonne. Les valeurs de longueur d'indicateur valides sont 0 (quand aucun indicateur n'est utilisé), 1, 2, 4 ou 8.<br /><br /> Pour spécifier l'utilisation d'un indicateur de copie en bloc par défaut, définissez ce paramètre sur SQL_VARLEN_DATA.<br /><br /> Les indicateurs apparaissent directement en mémoire avant les autres données et, dans le fichier de données, juste avant les données auxquelles ils s'appliquent.<br /><br /> Si plusieurs méthodes de spécification de la longueur de colonne d'un fichier de données sont utilisées (par exemple, un indicateur et une longueur de colonne maximale, ou un indicateur et une séquence de terminaison), la copie en bloc choisit celle qui implique la quantité de données à copier la moins élevée.<br /><br /> Les fichiers de données générés par la copie en bloc lorsqu'aucune intervention de l'utilisateur n'ajuste le format des données contiennent des indicateurs si la longueur des données de la colonne est variable ou si la colonne peut accepter la valeur NULL.|  
|BCP_FMT_DATA_LEN|DBINT|Longueur, en octets, des données (longueur de colonne).<br /><br /> Longueur maximale, en octets, des données de cette colonne dans le fichier utilisateur, hormis la longueur d'un indicateur de longueur ou d'un terminateur.<br /><br /> La définition de BCP_FMT_DATA_LEN sur SQL_NULL_DATA indique que toutes les valeurs de la colonne du fichier de données ont ou doivent avoir la valeur NULL.<br /><br /> La définition de BCP_FMT_DATA_LEN sur SQL_VARLEN_DATA indique que le système doit déterminer la longueur des données dans chaque colonne. Pour certaines colonnes, cela peut signifier qu'un indicateur de longueur/null est généré pour précéder les données dans une copie à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou que l'indicateur est attendu dans les données copiées vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Pour les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] character et binary, BCP_FMT_DATA_LEN peut avoir comme valeur SQL_VARLEN_DATA, SQL_NULL_DATA, 0 ou une valeur positive quelconque. Si BCP_FMT_DATA_LEN a la valeur SQL_VARLEN_DATA, le système utilise l'indicateur de longueur (si disponible) ou une séquence de terminaison afin de déterminer la longueur des données. Si un indicateur de longueur et une séquence de terminaison sont fournis, la copie en bloc utilise celui qui implique le volume de données à copier le plus faible. Si BCP_FMT_DATA_LEN a la valeur SQL_VARLEN_DATA, si le type de données est un type [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] character ou binary et si aucun indicateur de longueur ou séquence de terminaison n'est spécifié, le système retourne un message d'erreur.<br /><br /> Si BCP_FMT_DATA_LEN affiche une valeur égale à 0 ou une valeur positive, le système utilise BCP_FMT_DATA_LEN comme longueur de données maximale. Toutefois, si un indicateur de longueur ou une séquence de terminaison est fourni en plus d'une valeur BCP_FMT_DATA_LEN positive, le système détermine la longueur de données en utilisant la méthode qui implique le moins de données à copier.<br /><br /> La valeur BCP_FMT_DATA_LEN représente le nombre d'octets de données. Si des données de caractères sont représentées par des caractères Unicode larges, une valeur de paramètre BCP_FMT_DATA_LEN positive représente le nombre de caractères multiplié par la taille, en octets, de chacun des caractères.|  
|BCP_FMT_TERMINATOR|LPCBYTE|Pointeur vers la séquence de terminaison (ANSI ou Unicode, selon les besoins) à utiliser pour cette colonne. Ce paramètre est utile surtout pour les types de données de caractères puisque tous les autres types sont de longueur fixe ou, dans le cas des données binaires, nécessitent un indicateur de longueur pour enregistrer avec précision le nombre d'octets présents.<br /><br /> Pour éviter de terminer des données extraites ou pour indiquer que les données dans un fichier utilisateur ne sont pas terminées, attribuez la valeur NULL à ce paramètre.<br /><br /> Si plusieurs méthodes sont employées pour définir la longueur des colonnes du fichier utilisateur (par exemple, un terminateur et un indicateur de longueur, ou un terminateur et une longueur de colonne maximale), la copie en bloc choisit celle qui implique le volume de données à copier le moins élevé.<br /><br /> L'interface de programmation d'applications (API) de la copie en bloc procède à la conversion des caractères Unicode vers MBCS en fonction des besoins. Prenez soin de définir comme il se doit la chaîne d'octets de terminaison et la longueur de cette même chaîne.|  
|BCP_FMT_SERVER_COL|INT|Position ordinale de la colonne dans la base de données.|  
|BCP_FMT_COLLATION|LPCSTR|Nom du classement.|  
  
 *pValue*  
 Pointeur vers la valeur à associer à la *propriété*. Il permet de définir individuellement chaque propriété de format de colonne.  
  
 *cbvalue*  
 Taille de la mémoire tampon de propriété, en octets.  
  
## <a name="returns"></a>Valeur renvoyée  
 SUCCEED ou FAIL.  
  
## <a name="remarks"></a>Notes  
 Cette fonction remplace la **bcp_colfmt** (fonction). Toutes les fonctionnalités de **bcp_colfmt** est fourni dans **bcp_setcolfmt** (fonction). Le classement des colonnes est également pris en charge. Il est préférable de définir les attributs de format de colonne suivants dans l'ordre précisé ci-dessous :  
  
 BCP_FMT_SERVER_COL  
  
 BCP_FMT_DATA_LEN  
  
 BCP_FMT_TYPE  
  
 Le **bcp_setcolfmt** fonction vous permet de spécifier le format de fichier utilisateur pour les copies en bloc. Pour la copie en bloc, un format se compose des éléments suivants :  
  
-   Mappage des colonnes du fichier utilisateur avec les colonnes de la base de données  
  
-   Type de données de chaque colonne du fichier utilisateur  
  
-   Longueur de l'indicateur facultatif pour chaque colonne  
  
-   Longueur maximale des données par colonne du fichier utilisateur  
  
-   Séquence d'octets de fin facultative pour chaque colonne  
  
-   Longueur de la séquence d'octets de fin facultative  
  
 Chaque appel à **bcp_setcolfmt** Spécifie le format d’une colonne du fichier de l’utilisateur. Par exemple, pour modifier les paramètres par défaut pour les trois colonnes dans un fichier de données utilisateur de cinq colonnes, appelez d’abord [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md)**(5)**, puis appelez **bcp_setcolfmt** cinq fois, avec trois de ces appels définissant votre format personnalisé. Les deux appels restants, définissez BCP_FMT_TYPE sur 0 et la valeur BCP_FMT_INDICATOR_LENGTH, BCP_FMT_DATA_LEN et *cbValue* à 0, SQL_VARLEN_DATA et 0 respectivement. Cette procédure copie les cinq colonnes, trois avec votre format personnalisé et deux avec le format par défaut.  
  
 Le **bcp_columns** fonction doit être appelée avant d’appeler **bcp_setcolfmt**.  
  
 Vous devez appeler **bcp_setcolfmt** une fois pour chaque propriété de chaque colonne dans le fichier utilisateur.  
  
 Vous n'avez pas besoin de copier toutes les données d'un fichier utilisateur dans la table SQL Server. Pour ignorer une colonne, spécifiez le format des données de la colonne en attribuant la valeur 0 au paramètre BCP_FMT_SERVER_COL. Pour ignorer une colonne, vous devez spécifier son type.  
  
 Le [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md) fonction peut être utilisée pour conserver la spécification de format.  
  
## <a name="bcpsetcolfmt-support-for-enhanced-date-and-time-features"></a>Prise en charge de la fonction bcp_setcolfmt pour les fonctionnalités de date et heure améliorées  
 Les types utilisés avec la propriété BCP_FMT_TYPE pour les types date/heure sont comme spécifié dans [modifications de copie en bloc pour les Types améliorées de Date et heure &#40;OLE DB et ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  
  
 Pour plus d’informations, consultez [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
