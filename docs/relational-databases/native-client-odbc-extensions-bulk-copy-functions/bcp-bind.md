---
title: bcp_bind | Documents Microsoft
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
- bcp_bind
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_bind function
ms.assetid: 6e335a5c-64b2-4bcf-a88f-35dc9393f329
caps.latest.revision: 47
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 31f50aa8c094ba983a8382379fd0d833edb0f9dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bcpbind"></a>bcp_bind
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Lie les données d'une variable de programme à une colonne de table pour une copie en bloc dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RETCODE bcp_bind (  
        HDBC hdbc,   
        LPCBYTE pData,  
        INT cbIndicator,  
        DBINT cbData,  
        LPCBYTE pTerm,  
        INT cbTerm,  
        INT eDataType,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Arguments  
 *pas*  
 Handle de connexion ODBC compatible avec la copie en bloc.  
  
 *pData*  
 Pointeur vers les données copiées. Si *eDataType* est SQLTEXT, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SQLBINARY, SQLNCHAR ou SQLIMAGE, *pData* peut être NULL. Une valeur NULL *pData* indique que les valeurs de données de type long sont envoyées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des segments à l’aide de [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md). L’utilisateur doit uniquement définir *pData* avec la valeur NULL si la colonne correspondant au champ utilisateur lié est une colonne BLOB dans le cas contraire **bcp_bind** échoue.  
  
 Si les indicateurs sont présents dans les données, ils apparaissent directement en mémoire avant les données. Le *pData* paramètre pointe vers la variable indicateur dans ce cas et la largeur de l’indicateur, la *cbIndicator* paramètre, est utilisé par la copie en bloc pour adresser des données utilisateur correctement.  
  
 *cbIndicator*  
 Longueur, en octets, d'un indicateur de longueur ou d'un indicateur null pour les données de la colonne. Les valeurs de longueur d'indicateur valides sont 0 (quand aucun indicateur n'est utilisé), 1, 2, 4 ou 8. Les indicateurs apparaissent directement en mémoire avant les données. Par exemple, la définition de type de structure suivante peut être utilisée pour insérer les valeurs entières dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de la copie en bloc :  
  
```  
typedef struct tagBCPBOUNDINT  
    {  
    int iIndicator;  
    int Value;  
    } BCPBOUNDINT;  
```  
  
 Dans le cas d’exemple, le *pData* paramètre est défini à l’adresse d’une instance déclarée de la structure, l’adresse de la BCPBOUNDINT *iIndicator* membre de structure. Le *cbIndicator* paramètre est défini à la taille d’un entier (sizeof et le *cbData* paramètre est défini à nouveau à la taille d’un entier (sizeof. Pour copier en bloc une ligne vers le serveur contenant une valeur NULL la valeur de la colonne liée, la valeur de l’instance *iIndicator* membre doit être défini à SQL_NULL_DATA.  
  
 *cbData*  
 Nombre d'octets de données dans la variable de programme, à l'exclusion de tout indicateur de longueur ou indicateur null ou terminateur.  
  
 Paramètre *cbData* avec la valeur SQL_NULL_DATA signifie que toutes les lignes copiées sur le serveur contiennent une valeur NULL pour la colonne.  
  
 Paramètre *cbData* sur SQL_VARLEN_DATA indique que le système utilisera une marque de fin de chaîne, ou autre méthode, afin de déterminer la longueur des données copiées.  
  
 Pour les types de données de longueur fixe, tels que les entiers, le type de données indique la longueur des données au système. Par conséquent, pour les types de données de longueur fixe, *cbData* peut sans risque avoir comme valeur SQL_VARLEN_DATA ou la longueur des données.  
  
 Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] caractères et les types de données binaires, *cbData* peut être SQL_VARLEN_DATA, SQL_NULL_DATA, une valeur positive ou 0. Si *cbData* a la valeur SQL_VARLEN_DATA, le système utilise un indicateur de longueur/null (le cas échéant) ou d’une séquence de terminaison pour déterminer la longueur des données. Si les deux sont fournis, le système utilise celui qui se traduit par la quantité de données à copier la moins élevée. Si *cbData* a la valeur SQL_VARLEN_DATA, le type de données de la colonne est une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] caractère ou type binaire et un indicateur de longueur ni une séquence de terminaison est spécifiée, le système retourne un message d’erreur.  
  
 Si *cbData* est égal à 0 ou une valeur positive, le système utilise *cbData* comme longueur de données. Toutefois, if, en plus d’un nombre positif *cbData* valeur, une séquence d’indicateur ou de marque de fin de la longueur est fournie, le système détermine la longueur des données à l’aide de la méthode qui entraîne le moins de données à copier.  
  
 Le *cbData* la valeur du paramètre représente le nombre d’octets de données. Si les données caractères sont représentées par des caractères Unicode étendus, un nombre positif *cbData* la valeur du paramètre représente le nombre de caractères multiplié par la taille en octets de chaque caractère.  
  
 *pTerm*  
 Pointeur vers le modèle d'octet, s'il existe, qui marque la fin de cette variable de programme. Par exemple, les chaines C ANSI et MBCS ont habituellement un terminateur d'1 octet (\0).  
  
 S’il n’existe aucune marque de fin de la variable, définissez *pTerm* avec la valeur NULL.  
  
 Vous pouvez utiliser une chaîne vide ("") pour désigner l'indicateur de fin C null comme terminateur de variable de programme. Étant donné que la chaîne vide se terminant par null constitue un seul octet (l’octet de terminateur lui-même), définissez *cbTerm* à 1. Par exemple, pour indiquer que la chaîne dans *szName* est terminée et que la marque de fin doit être utilisé pour indiquer la longueur :  
  
```  
bcp_bind(hdbc, szName, 0,  
   SQL_VARLEN_DATA, "", 1,  
   SQLCHARACTER, 2)  
```  
  
 Un formulaire sans de cet exemple peut indiquer que 15 caractères copiés à partir de la *szName* variable à la deuxième colonne de la table liée :  
  
```  
bcp_bind(hdbc, szName, 0, 15,   
   NULL, 0, SQLCHARACTER, 2)  
```  
  
 L'interface de programmation d'applications (API) de la copie en bloc procède à la conversion des caractères Unicode vers MBCS en fonction des besoins. Veillez à définir correctement la chaîne d'octet de marque de fin et la longueur de la chaîne d'octets. Par exemple, pour indiquer que la chaîne dans *szName* est une chaîne de caractères larges Unicode terminée par la valeur de la marque de fin null Unicode :  
  
```  
bcp_bind(hdbc, szName, 0,   
   SQL_VARLEN_DATA, L"",  
   sizeof(WCHAR), SQLNCHAR, 2)  
```  
  
 Si la limite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonne est un caractère large, aucune conversion n’est effectuée sur [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md). Si la colonne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possède MBCS comme type de caractère, la conversion des caractères larges en caractères multioctets s'effectue quand les données sont envoyées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *cbTerm*  
 Nombre d'octets présents dans le terminateur de la variable de programme, s'il existe. S’il n’existe aucune marque de fin de la variable, définissez *cbTerm* à 0.  
  
 *eDataType*  
 Type de données C de la variable de programme. Les données de la variable de programme sont converties dans le type de la colonne de base de données. Si ce paramètre est égal à 0, aucune conversion n'est effectuée.  
  
 Le *eDataType* paramètre est énuméré par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des jetons de type de données dans sqlncli.h, pas les énumérateurs de type de données ODBC C. Par exemple, vous pouvez spécifier un entier à deux octets, type ODBC SQL_C_SHORT, à l'aide du type SQLINT2 propre à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a introduit la prise en charge des jetons de type de données SQLXML et SQLUDT dans le ***eDataType*** habituellement.  
 
 Le tableau suivant répertorie les types de données énumérées valides et les types de données ODBC C correspondants.
  
|eDataType|Type C|  
|-----------------------|------------|  
|SQLTEXT|char *|  
|SQLNTEXT|wchar_t *|  
|SQLCHARACTER|char *|  
|SQLBIGCHAR|char *|  
|SQLVARCHAR|char *|  
|SQLBIGVARCHAR|char *|  
|SQLNCHAR|wchar_t *|  
|SQLNVARCHAR|wchar_t *|  
|SQLBINARY|unsigned char *|  
|SQLBIGBINARY|unsigned char *|  
|SQLVARBINARY|unsigned char *|  
|SQLBIGVARBINARY|unsigned char *|  
|SQLBIT|char|  
|SQLBITN|char|  
|SQLINT1|char|  
|SQLINT2|short int|  
|SQLINT4|int|  
|SQLINT8|_int64|  
|SQLINTN|*cbIndicator*<br /> 1: SQLINT1<br /> 2: SQLINT2<br /> 4: SQLINT4<br /> 8: SQLINT8|  
|SQLFLT4|float|  
|SQLFLT8|float|  
|SQLFLTN|*cbIndicator*<br /> 4: SQLFLT4<br /> 8: SQLFLT8|  
|SQLDECIMALN|SQL_NUMERIC_STRUCT|  
|SQLNUMERICN|SQL_NUMERIC_STRUCT|  
|SQLMONEY|DBMONEY|  
|SQLMONEY4|DBMONEY4|  
|SQLMONEYN|*cbIndicator*<br /> 4: SQLMONEY4<br /> 8: SQLMONEY|  
|SQLTIMEN|SQL_SS_TIME2_STRUCT|  
|SQLDATEN|SQL_DATE_STRUCT|  
|SQLDATETIM4|DBDATETIM4|  
|SQLDATETIME|DBDATETIME|  
|SQLDATETIMN|*cbIndicator*<br /> 4: SQLDATETIM4<br /> 8: SQLDATETIME|  
|SQLDATETIME2N|SQL_TIMESTAMP_STRUCT|  
|SQLDATETIMEOFFSETN|SQL_SS_TIMESTAMPOFFSET_STRUCT|  
|SQLIMAGE|unsigned char *|  
|SQLUDT|unsigned char *|  
|SQLUNIQUEID|SQLGUID|  
|SQLVARIANT|*Tout type de données sauf :*<br />-   text<br />-   ntext<br />-   image<br />-   varchar(max)<br />-   varbinary(max)<br />-   nvarchar(max)<br />-   xml<br />-   timestamp|  
|SQLXML|*Types de données C pris en charge :*<br />-   char*<br />-   wchar_t *<br />-   unsigned char *|  
  
 *idxServerCol*  
 Position ordinale de la colonne dans la table de base de données vers laquelle les données sont copiées. La première colonne d'une table est la colonne 1. La position ordinale d'une colonne est indiquée par [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="returns"></a>Valeur renvoyée  
 SUCCEED ou FAIL.  
  
## <a name="remarks"></a>Notes  
 Utilisez **bcp_bind** un moyen rapide et efficace copier des données à partir d’une variable de programme dans une table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Appelez [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) avant d’appeler cette fonction ou toute autre fonction de copie en bloc. Appel de **bcp_init** définit le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la table cible pour la copie en bloc. Lors de l’appel **bcp_init** pour une utilisation avec **bcp_bind** et [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md), le **bcp_init** *szDataFile*paramètre, qui indique le fichier de données a la valeur NULL ; le **bcp_init *** eDirection* paramètre a la valeur DB_IN.  
  
 Rendre un distinct **bcp_bind** appeler pour chaque colonne dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table dans laquelle vous souhaitez copier. Après avoir nécessaires **bcp_bind** appels ont été apportées, puis appelez **bcp_sendrow** pour envoyer une ligne de données à partir de vos variables de programme à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La reliaison des colonnes n'est pas prise en charge.  
  
 Lorsque vous souhaitez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour valider les lignes déjà reçues, appelez [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md). Par exemple, appeler **bcp_batch** une fois toutes les 1000 lignes insérées ou à un autre intervalle.  
  
 Lorsqu’il n’y a plus aucune ligne à insérer, appelez [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md). L'échec de cette opération entraîne une erreur.  
  
 Contrôler les paramètres, spécifiés avec [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md), n’ont aucun effet **bcp_bind** transferts de ligne.  
  
 Si *pData* pour une colonne a la valeur NULL, car sa valeur sera fournie par les appels à [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md), toutes les colonnes suivantes avec *eDataType* valeur SQLTEXT, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SQLBINARY, SQLNCHAR ou SQLIMAGE doit également être liée avec *pData* la valeur NULL, et leurs valeurs doivent également être fournis par les appels à **bcp_moretext**.  
  
 Pour les nouveaux types de valeur élevée, telles que **varchar (max)**, **varbinary (max)**, ou **nvarchar (max)**, vous pouvez utiliser SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SQLBINARY et SQLNCHAR comme indicateurs de type dans le *eDataType* paramètre.  
  
 Si *cbTerm* est pas égal à 0, toute valeur (1, 2, 4 ou 8) est valide pour le préfixe (*cbIndicator*). Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sera recherche le terminateur, calculer la longueur des données par rapport au terminateur (*i*) et définissez la *cbData* à la plus petite valeur de i et la valeur de préfixe.  
  
 Si *cbTerm* est égal à 0 et *cbIndicator* (le préfixe) n’est pas 0, *cbIndicator* doit être de 8. Le préfixe de 8 octets peut prendre les valeurs suivantes :  
  
-   0xFFFFFFFFFFFFFFFF signifie une valeur NULL pour le champ  
  
-   0xFFFFFFFFFFFFFFFE est traité comme valeur de préfixe spéciale utilisée pour envoyer efficacement les données par segments au serveur. Le format des données avec ce préfixe particulier est :  
  
-   < SPECIAL_PREFIX > \<0 ou plusieurs segments de données >< ZERO_CHUNK > où :  
  
-   SPECIAL_PREFIX est 0xFFFFFFFFFFFFFFFE  
  
-   DATA_CHUNK est un préfixe de 4 octets qui contient longueur du segment, suivi par les données effectives dont la longueur est spécifiée dans le préfixe de 4 octets.  
  
-   ZERO_CHUNK est une valeur de 4 octets contenant tous les zéros (00000000) signifiant la fin des données.  
  
-   Toute autre longueur de 8 octets valide est traitée comme longueur de données normale.  
  
 Appel de [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) lors de l’utilisation **bcp_bind** génère une erreur.  
  
## <a name="bcpbind-support-for-enhanced-date-and-time-features"></a>Prise en charge de bcp_bind pour les fonctionnalités Date et Heure améliorées  
 Pour plus d’informations sur les types utilisés avec les *eDataType* paramètre pour les types date/heure, consultez [modifications de copie en bloc pour les Types améliorées de Date et heure &#40;OLE DB et ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  
  
 Pour plus d’informations, consultez [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="example"></a>Exemple  
  
```  
#include sql.h  
#include sqlext.h  
#include odbcss.h  
// Variables like henv not specified.  
HDBC      hdbc;  
char         szCompanyName[MAXNAME];  
DBINT      idCompany;  
DBINT      nRowsProcessed;  
DBBOOL      bMoreData;  
char*      pTerm = "\t\t";  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...   
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source; return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bcp.   
if (bcp_init(hdbc, "comdb..accounts_info", NULL, NULL  
   DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Bind program variables to table columns.   
if (bcp_bind(hdbc, (LPCBYTE) &idCompany, 0, sizeof(DBINT), NULL, 0,  
   SQLINT4, 1)    == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_bind(hdbc, (LPCBYTE) szCompanyName, 0, SQL_VARLEN_DATA,  
   (LPCBYTE) pTerm, strnlen(pTerm, sizeof(pTerm)), SQLCHARACTER, 2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
while (TRUE)  
   {  
   // Retrieve and process program data.   
   if ((bMoreData = getdata(&idCompany, szCompanyName)) == TRUE)  
      {  
      // Send the data.   
      if (bcp_sendrow(hdbc) == FAIL)  
         {  
         // Raise error and return.  
         return;  
         }  
      }  
   else  
      {  
      // Break out of loop.  
      break;  
      }  
   }  
  
// Terminate the bulk copy operation.  
if ((nRowsProcessed = bcp_done(hdbc)) == -1)  
   {  
   printf_s("Bulk-copy unsuccessful.\n");  
   return;  
   }  
  
printf_s("%ld rows copied.\n", nRowsProcessed);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de copie en bloc](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
