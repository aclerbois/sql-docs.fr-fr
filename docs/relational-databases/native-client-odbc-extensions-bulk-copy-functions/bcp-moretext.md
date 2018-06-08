---
title: bcp_moretext | Documents Microsoft
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
- bcp_moretext
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_moretext function
ms.assetid: 23e98015-a8e4-4434-9b3f-9c7350cf965f
caps.latest.revision: 39
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e64f445788e35563120c357b93395fcfede50410
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bcpmoretext"></a>bcp_moretext
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Envoie une partie d'une valeur de type de données longue et de longueur variable à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RETCODE bcp_moretext (  
        HDBC hdbc,  
        DBINT cbData,  
        LPCBYTE pData);  
```  
  
## <a name="arguments"></a>Arguments  
 *pas*  
 Handle de connexion ODBC compatible avec la copie en bloc.  
  
 *cbData*  
 Est le nombre d’octets de données sont copiées vers SQL Server à partir de données référencées par *pData*. Une valeur de SQL_NULL_DATA indique NULL.  
  
 *pData*  
 Pointeur vers le segment de données long, de longueur variable, pris en charge à envoyer à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="returns"></a>Valeur renvoyée  
 SUCCEED ou FAIL.  
  
## <a name="remarks"></a>Notes  
 Cette fonction peut être utilisée conjointement avec [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) et [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) copier long, les valeurs de données de longueur variable vers SQL Server dans un nombre de segments plus petits. **bcp_moretext** peut être utilisé avec des colonnes qui ont des types de données SQL Server suivants : **texte**, **ntext**, **image**, **varchar (max)** , **nvarchar (max)**, **varbinary (max)**, type défini par l’utilisateur (UDT) et XML. **bcp_moretext** ne pas les conversions de données de prise en charge, les données fournies doivent correspondre au type de données de la colonne cible.  
  
 Si **bcp_bind** est appelée avec une valeur non NULL *pData* paramètre pour les types de données qui sont pris en charge par **bcp_moretext**, **bcp_sendrow** envoie la valeur de données entière, quelle que soit la longueur. If, toutefois, **bcp_bind** a une valeur NULL *pData* paramètre pour les types de données pris en charge, **bcp_moretext** peut être utilisé pour copier les données immédiatement après un retour réussi de **bcp_sendrow** indiquant que toutes les colonnes liées avec des données présentes ont été traitées.  
  
 Si vous utilisez **bcp_moretext** pour envoyer une colonne de type de données pris en charge dans une ligne, vous devez également l’utiliser pour envoyer toutes les autres colonnes de type de données pris en charge dans la ligne. Aucune colonne ne peut être ignorée. Les types de données pris en charge sont SQLTEXT, SQLNTEXT, SQLIMAGE, SQLUDT et SQLXML. SQLCHARACTER, SQLVARCHAR, SQNCHAR, SQLBINARY et SQLVARBINARY appartiennent également à cette catégorie si la colonne est un varchar (max), nvarchar (max) ou varbinary (max), respectivement.  
  
 Appelant **bcp_bind** ou [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md) définit la longueur totale de toutes les parties de données doit être copié vers la colonne SQL Server. Une tentative d’envoi SQL Server plus d’octets que le nombre spécifié dans l’appel à **bcp_bind** ou **bcp_collen** génère une erreur. Cette erreur surviendrait, par exemple, dans une application ayant utilisé **bcp_collen** pour définir la longueur des données disponibles pour un serveur SQL Server **texte** colonne 4500, puis appelé **bcp_moretext** cinq reprises en indiquant à chaque appel que les données de mémoire tampon de longueur de 1 000 octets.  
  
 Si une ligne copiée contient plusieurs colonnes longues, de longueur variable, **bcp_moretext** envoie d’abord ses données à la plus petite de colonne, suivi par le prochain numéro inférieur plus petit numéro de colonne et ainsi de suite. Il est important de définir correctement la longueur totale des données attendues. Il n'existe aucun moyen de signaler, en dehors du paramètre de longueur, que toutes les données pour une colonne ont été reçues par copie en bloc.  
  
 Lorsque **var(max)** les valeurs sont envoyées au serveur à l’aide de bcp_sendrow et bcp_moretext, il n’est pas nécessaire d’appeler bcp_collen pour définir la longueur de colonne. Au lieu de cela, pour ces types uniquement, la valeur est terminée en appelant bcp_sendrow avec une longueur de zéro.  
  
 Une application appelle normalement **bcp_sendrow** et **bcp_moretext** dans les boucles pour envoyer un nombre de lignes de données. Voici comment procéder pour une table qui contient deux **texte** colonnes :  
  
```  
while (there are still rows to send)  
{  
bcp_sendrow(...);  
  
for (all the data in the first varbinary(max) column)  
bcp_moretext(...);  
bcp_moretext(hdbc, 0, NULL);  
  
for (all the data in the second varbinary(max) column)  
bcp_moretext(...);  
bcp_moretext(hdbc, 0, NULL);  
  
}  
  
```  
  
## <a name="example"></a>Exemple  
 Cet exemple montre comment utiliser **bcp_moretext** avec **bcp_bind** et **bcp_sendrow**:  
  
```  
// Variables like henv not specified.  
HDBC      hdbc;  
DBINT      idRow = 5;  
char*      pPart1 = "This text value isn't very long,";  
char*      pPart2 = " but it's broken into three parts";  
char*      pPart3 = " anyhow.";  
DBINT      cbAllParts;  
DBINT      nRowsProcessed;  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...   
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source, return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bulk copy.   
if (bcp_init(hdbc, "comdb..articles", NULL, NULL, DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Bind program variables to table columns.   
if (bcp_bind(hdbc, (LPCBYTE) &idRow, 0, SQL_VARLEN_DATA, NULL, 0,  
   SQLINT4, 1)    == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
cbAllParts = (DBINT) (strnlen(pPart1, sizeof(pPart1) + 1) + strnlen(pPart2, sizeof(pPart2) + 1) + strnlen(pPart3, sizeof(pPart3) + 1));  
if (bcp_bind(hdbc, NULL, 0, cbAllParts, NULL, 0, SQLTEXT, 2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Send this row, with the text value broken into three chunks.   
if (bcp_sendrow(hdbc) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart1, sizeof(pPart1) + 1), pPart1) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart2, sizeof(pPart2) + 1), pPart2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart3, sizeof(pPart3) + 1), pPart3) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// All done. Get the number of rows processed (should be one).  
nRowsProcessed = bcp_done(hdbc);  
  
// Carry on.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
