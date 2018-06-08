---
title: Fonction SQLGetDiagField | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLGetDiagField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagField
helpviewer_keywords:
- SQLGetDiagField function [ODBC]
ms.assetid: 1dbc4398-97a8-4585-bb77-1f7ea75e24c4
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 39ae1a58454a7ce70f5ae2a33d951a769223fdd1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetdiagfield-function"></a>Fonction SQLGetDiagField
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLGetDiagField** retourne la valeur actuelle d’un champ d’un enregistrement de la structure de données de diagnostic (associé à un handle spécifié) qui contient des informations d’erreur, d’avertissement et d’état.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetDiagField(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     DiagIdentifier,  
     SQLPOINTER      DiagInfoPtr,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 [Entrée] Identificateur de type handle qui décrit le type de handle pour lequel les tests de diagnostic sont requis. Doit prendre l'une des valeurs suivantes :  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 Handle SQL_HANDLE_DBC_INFO_TOKEN est utilisé uniquement par le Gestionnaire de pilotes et le pilote. Applications ne doivent pas utiliser ce type de handle. Pour plus d’informations sur SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement reconnaissance du Pool de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *Handle*  
 [Entrée] Un handle pour la structure de données de diagnostic, du type indiqué par *HandleType*. Si *HandleType* est SQL_HANDLE_ENV, *gérer* peut être partagé ou un handle d’environnement non partagé.  
  
 *RecNumber*  
 [Entrée] Indique que l’enregistrement de l’état à partir de laquelle l’application recherche des informations. Enregistrements d’état sont numérotées de 1. Si le *DiagIdentifier* argument indique n’importe quel champ de l’en-tête de diagnostics, *RecNumber* est ignoré. Si ce n’est pas le cas, il doit être supérieur à 0.  
  
 *DiagIdentifier*  
 [Entrée] Indique le champ du diagnostic dont la valeur doit être retournée. Pour plus d’informations, consultez le «*DiagIdentifier* Argument « section « Commentaires ».  
  
 *DiagInfoPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner les informations de diagnostic. Le type de données dépend de la valeur de *DiagIdentifier*. Si *DiagInfoPtr* est de type entier, les applications doivent utiliser une mémoire tampon de SQLULEN et initialiser la valeur 0 avant d’appeler cette fonction, en tant que certains pilotes peut uniquement écrire 32 bits inférieurs ou le bit d’ordre supérieur de 16 bits d’une mémoire tampon et la laisser inchangée.  
  
 Si *DiagInfoPtr* est NULL, *StringLengthPtr* retourne toujours le nombre total d’octets (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *DiagInfoPtr*.  
  
 *BufferLength*  
 [Entrée] Si *DiagIdentifier* est un diagnostic définis par ODBC et *DiagInfoPtr* pointe vers une chaîne de caractères ou d’un tampon binaire, cet argument doit être la longueur de \* *DiagInfoPtr*. Si *DiagIdentifier* est un champ défini par ODBC et \* *DiagInfoPtr* est un entier, *BufferLength* est ignoré. Si la valeur de  *\*DiagInfoPtr* est une chaîne Unicode (lors de l’appel **SQLGetDiagFieldW**), la *BufferLength* l’argument doit être un nombre pair.  
  
 Si *DiagIdentifier* est un champ défini par le pilote, l’application indiquant la nature du champ au Gestionnaire de pilote en définissant le *BufferLength* argument. *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *DiagInfoPtr* est un pointeur vers une chaîne de caractères *BufferLength* est la longueur de la chaîne ou le SQL_NTS.  
  
-   Si *DiagInfoPtr* est un pointeur vers une mémoire tampon binaire, les emplacements de l’application le résultat de la SQL_LEN_BINARY_ATTR (*longueur*) macro dans *BufferLength*. Il s’ensuit une valeur négative dans *BufferLength*.  
  
-   Si *DiagInfoPtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si  *\*DiagInfoPtr* contient un type de données de longueur fixe, *BufferLength* est SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT ou SQL_IS_USMALLINT, selon le cas.  
  
 *StringLengthPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total d’octets (autre que le nombre d’octets requis pour le caractère de fin de la valeur null) disponibles à renvoyer dans \* *DiagInfoPtr*, pour les données caractères. Si le nombre d’octets à retourner est supérieur ou égal à *BufferLength*, le texte de \* *DiagInfoPtr* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_NO_DATA.  
  
## <a name="diagnostics"></a>Diagnostics  
 **SQLGetDiagField** ne valide pas les enregistrements de diagnostic pour lui-même. Il utilise les valeurs de retournés suivantes pour signaler le résultat de l’exécution de son propre :  
  
-   SQL_SUCCESS : La fonction retournée avec succès les informations de diagnostic.  
  
-   SQL_SUCCESS_WITH_INFO : \* *DiagInfoPtr* était trop petite pour contenir le champ demandé de diagnostic. Par conséquent, les données dans le champ de diagnostic a été tronquées. Pour déterminer qu’une troncation s’est produite, l’application doit comparer *BufferLength* au nombre réel d’octets disponible, ce qui est écrite dans **StringLengthPtr*.  
  
-   SQL_INVALID_HANDLE : Le descripteur indiqué par *HandleType* et *gérer* n’était pas un handle valide.  
  
-   SQL_ERROR : Un des éléments suivants s’est produit :  
  
    -   *Le DiagIdentifier* argument n’est pas une des valeurs valides.  
  
    -   *Le DiagIdentifier* argument a été SQL_DIAG_CURSOR_ROW_COUNT, SQL_DIAG_DYNAMIC_FUNCTION, SQL_DIAG_DYNAMIC_FUNCTION_CODE ou SQL_DIAG_ROW_COUNT, mais *gérer* n’était pas un descripteur d’instruction. (Le Gestionnaire de pilotes retourne ce diagnostic.)  
  
    -   *Le RecNumber* argument était négatif ou égal à 0 lorsque *DiagIdentifier* indiqué un champ à partir d’un enregistrement de diagnostic. *RecNumber* est ignoré pour les champs d’en-tête.  
  
    -   La valeur demandée était une chaîne de caractères et *BufferLength* était inférieur à zéro.  
  
    -   Lorsque vous utilisez la notification asynchrone, l’opération asynchrone sur le handle n’était pas terminée.  
  
-   SQL_NO_DATA : *RecNumber* est supérieur au nombre d’enregistrements de diagnostic qui existaient pour le handle spécifié dans *gérer.* La fonction retourne également SQL_NO_DATA pour n’importe quel nombre positif *RecNumber* si aucun enregistrement de diagnostic pour *gérer*.  
  
## <a name="comments"></a>Commentaires  
 Une application appelle généralement **SQLGetDiagField** pour accomplir une des trois objectifs :  
  
1.  Pour obtenir des informations d’avertissement ou l’erreur spécifique lorsqu’un appel de fonction a renvoyé SQL_ERROR ou SQL_SUCCESS_WITH_INFO (ou SQL_NEED_DATA pour le **SQLBrowseConnect** fonction).  
  
2.  Pour déterminer le nombre de lignes dans la source de données ont été affectées lorsque les opérations d’insertion, suppression ou mise à jour a été effectuées avec un appel à **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** (dans le champ d’en-tête SQL_DIAG_ROW_COUNT), ou pour déterminer le nombre de lignes qui existent dans le curseur ouvert actuel, si le pilote peut fournir ces informations (à partir du champ d’en-tête SQL_DIAG_CURSOR_ROW_COUNT).  
  
3.  Pour déterminer quelle fonction a été exécutée par un appel à **SQLExecDirect** ou **SQLExecute** (à partir des champs d’en-tête SQL_DIAG_DYNAMIC_FUNCTION et SQL_DIAG_DYNAMIC_FUNCTION_CODE).  
  
 Toute fonction ODBC peut publier zéro ou plusieurs enregistrements de diagnostic chaque fois qu’elle est appelée, une application peut appeler **SQLGetDiagField** après un appel de fonction ODBC. Il n’existe aucune limite au nombre d’enregistrements de diagnostic qui peuvent être stockés à tout moment. **SQLGetDiagField** récupère uniquement les informations de diagnostic plus récemment associées à la structure de données de diagnostic spécifiée dans le *gérer* argument. Si l’application appelle une fonction ODBC autre que **SQLGetDiagField** ou **SQLGetDiagRec**, toutes les informations de diagnostic à partir d’un appel précédent avec le même handle sont perdues.  
  
 Une application peut analyser tous les enregistrements de diagnostic en incrémentant *RecNumber*, à condition que **SQLGetDiagField** retourne SQL_SUCCESS. Le nombre d’enregistrements d’état est indiqué dans le champ d’en-tête SQL_DIAG_NUMBER. Les appels à **SQLGetDiagField** sont non destructrice pour les champs d’en-tête et un enregistrement. L’application peut appeler **SQLGetDiagField** ultérieurement pour récupérer un champ d’un enregistrement, tant qu’une fonction autre que les fonctions de diagnostic n’a pas été appelée dans l’intervalle, ce qui est de valider des enregistrements sur le même handle.  
  
 Une application peut appeler **SQLGetDiagField** pour retourner des champs de diagnostic à tout moment, à l’exception SQL_DIAG_CURSOR_ROW_COUNT ou SQL_DIAG_ROW_COUNT, qui retourne SQL_ERROR si *gérer* n’est pas un descripteur d’instruction. Si d’autres champs de diagnostic n’est pas défini, l’appel à **SQLGetDiagField** retourne SQL_SUCCESS (fournie par aucun autre diagnostic n’est rencontré) et une valeur indéfinie est retournée pour le champ.  
  
 Pour plus d’informations, consultez [à l’aide de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) et [mise en œuvre de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md).  
  
 Appel d’une API que celle qui est exécutée de manière asynchrone génère HY010 « Erreur de séquence de fonction ». Toutefois, l’enregistrement d’erreur ne peut pas être récupérée avant la fin de l’opération asynchrone.  
  
## <a name="handletype-argument"></a>HandleType Argument  
 Chaque type de handle peut avoir des informations de diagnostic associées. Le *HandleType* argument indique le type de handle de *gérer*.  
  
 Certains champs d’en-tête et d’enregistrement ne peut pas retournés pour l’environnement, connexion, l’instruction et descripteur de handles. Ces poignées pour lesquels un champ n’est pas applicable sont indiquées dans les sections « Champs d’en-tête » et « Champs de l’enregistrement » suivant.  
  
 Si *HandleType* est SQL_HANDLE_ENV, *gérer* peut être soit un handle d’environnement partagé ou non partagé.  
  
 Aucun des champs de diagnostic spécifiques au pilote en-tête ne doivent être associés à un handle d’environnement.  
  
 Les champs d’en-tête uniquement de diagnostic qui sont définis pour un handle de descripteur sont SQL_DIAG_NUMBER et SQL_DIAG_RETURNCODE.  
  
## <a name="diagidentifier-argument"></a>Argument de DiagIdentifier  
 Cet argument indique l’identificateur de champ requis à partir de la structure de données de diagnostic. Si *RecNumber* est supérieur ou égal à 1, les données dans le champ décrivent les informations de diagnostic retournées par une fonction. Si *RecNumber* est 0, le champ est dans l’en-tête de la structure de données de diagnostic et par conséquent, contient les données relatives à l’appel de fonction qui a retourné les informations de diagnostic, pas à des informations spécifiques.  
  
 Pilotes peuvent définir en-tête spécifique au pilote et des champs d’enregistrement dans la structure de données de diagnostic.  
  
 Un ODBC 3 *.x* application utilisant une API ODBC 2 *.x* pilote sera en mesure d’appeler **SQLGetDiagField** uniquement avec un *DiagIdentifier* argument de SQL_DIAG_CLASS_ORIGIN, SQL_DIAG_CLASS_SUBCLASS_ORIGIN, SQL_DIAG_CONNECTION_NAME, SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_NUMBER, SQL_DIAG_RETURNCODE, SQL_DIAG_SERVER_NAME ou SQL_DIAG_SQLSTATE. Tous les autres champs de diagnostic retourne SQL_ERROR.  
  
## <a name="header-fields"></a>Champs d’en-tête  
 Les champs d’en-tête répertoriés dans le tableau suivant peuvent être inclus dans le *DiagIdentifier* argument.  
  
|DiagIdentifier|Type de retour|Valeur renvoyée|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CURSOR_ROW_COUNT|SQLLEN|Ce champ contient le nombre de lignes du curseur. Sa sémantique dépend de la **SQLGetInfo** SQL_DYNAMIC_CURSOR_ATTRIBUTES2, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2, SQL_KEYSET_CURSOR_ATTRIBUTES2 et SQL_STATIC_CURSOR_ATTRIBUTES2, qui indiquent le nombre de lignes est disponibles pour chaque type de curseur (dans les bits SQL_CA2_CRC_EXACT et SQL_CA2_CRC_APPROXIMATE) des types d’informations.<br /><br /> Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction et uniquement après avoir **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelée. Appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_CURSOR_ROW_COUNT sur autre qu’une instruction handle retourne SQL_ERROR.|  
|SQL_DIAG_DYNAMIC_FUNCTION|SQLCHAR *|Il s’agit d’une chaîne qui décrit l’instruction SQL exécutée par la fonction sous-jacente. (Consultez « Valeurs des champs, fonction dynamique » plus loin dans cette section, des valeurs spécifiques.) Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction et uniquement après un appel à **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults**. Appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_DYNAMIC_FUNCTION sur autre qu’une instruction handle retourne SQL_ERROR. La valeur de ce champ n’est pas définie avant un appel à **SQLExecute** ou **SQLExecDirect**.|  
|SQL_DIAG_DYNAMIC_FUNCTION_CODE|SQLINTEGER|Il s’agit d’un code numérique qui décrit l’instruction SQL qui a été exécutée par la fonction sous-jacente. (Consultez « Valeurs de la fonction champs dynamiques, » plus loin dans cette section, pour une valeur spécifique.) Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction et uniquement après un appel à **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults**. Appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_DYNAMIC_FUNCTION_CODE sur autre qu’une instruction handle retourne SQL_ERROR. La valeur de ce champ n’est pas définie avant un appel à **SQLExecute** ou **SQLExecDirect**.|  
|SQL_DIAG_NUMBER|SQLINTEGER|Le nombre d’enregistrements d’état qui sont disponibles pour le handle spécifié.|  
|SQL_DIAG_RETURNCODE|SQLRETURN|Code de retour renvoyé par la fonction. Pour obtenir la liste des codes de retour, consultez [Codes de retour](../../../odbc/reference/develop-app/return-codes-odbc.md). Le pilote n’a pas d’implémenter SQL_DIAG_RETURNCODE ; Il est toujours implémenté par le Gestionnaire de pilotes. Si aucune fonction n’a encore été appelée sur le *gérer*, SQL_SUCCESS est retournée pour SQL_DIAG_RETURNCODE.|  
|SQL_DIAG_ROW_COUNT|SQLLEN|Le nombre de lignes affectées par un insert, delete ou update effectuées par **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos**. Elle est définie par le pilote après un *spécification de curseur* a été exécutée. Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction. Appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_ROW_COUNT sur autre qu’une instruction handle retourne SQL_ERROR. Les données de ce champ sont également retournées dans le *RowCountPtr* argument de **SQLRowCount**. Les données de ce champ sont réinitialisées après chaque appel de fonction nondiagnostic, tandis que le nombre de lignes retournées par **SQLRowCount** reste la même jusqu'à ce que l’instruction est définie à l’état prêt ou alloué.|  
  
## <a name="record-fields"></a>Champs d’enregistrement  
 Les champs d’enregistrement répertoriés dans le tableau suivant peuvent être inclus dans le *DiagIdentifier* argument.  
  
|DiagIdentifier|Type de retour|Valeur renvoyée|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CLASS_ORIGIN|SQLCHAR *|Chaîne qui indique le document qui définit la partie de la classe de la valeur SQLSTATE de cet enregistrement. Sa valeur est « ISO 9075 » pour tous les SQLSTATE définies par Open Group et l’interface de niveau d’appel ISO. Pour SQLSTATE de ODBC spécifiques (tous ceux dont la classe SQLSTATE est « Mi »), sa valeur est « ODBC 3.0 ».|  
|SQL_DIAG_COLUMN_NUMBER|SQLINTEGER|Si le champ SQL_DIAG_ROW_NUMBER est un numéro de ligne valide dans un ensemble de lignes ou un ensemble de paramètres, ce champ contient la valeur qui représente le numéro de colonne dans le jeu de résultats ou le numéro de paramètre dans le jeu de paramètres. Résultat de jeu de colonnes, la numérotation commence toujours à 1 ; Si cet enregistrement de l’état correspond à une colonne de signet, le champ peut être zéro. Numéros de paramètres commencent à 1. Il a la valeur SQL_NO_COLUMN_NUMBER si l’état d’enregistrement n’est pas associé à un numéro de colonne ou paramètre. Si le pilote ne peut pas déterminer le numéro de colonne ou paramètre associé à cet enregistrement, ce champ a la valeur SQL_COLUMN_NUMBER_UNKNOWN.<br /><br /> Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction.|  
|SQL_DIAG_CONNECTION_NAME|SQLCHAR *|Chaîne qui indique le nom de la connexion correspondant à l’enregistrement de diagnostic. Ce champ est définie par le pilote. Pour les structures de données de diagnostic associés au handle d’environnement et pour les diagnostics ne sont pas liées à n’importe quelle connexion, ce champ est une chaîne de longueur nulle.|  
|SQL_DIAG_MESSAGE_TEXT|SQLCHAR *|Un message d’information sur l’erreur ou l’avertissement. Ce champ est mis en forme comme décrit dans [des Messages de Diagnostic](../../../odbc/reference/develop-app/diagnostic-messages.md). Il n’existe aucune longueur maximale pour le texte du message de diagnostic.|  
|SQL_DIAG_NATIVE|SQLINTEGER|Un code d’erreur native de spécifique à la source de données/pilote. S’il n’existe aucun code d’erreur natif, le pilote retourne 0.|  
|SQL_DIAG_ROW_NUMBER|SQLLEN|Ce champ contient le numéro de ligne dans l’ensemble de lignes, ou le numéro de paramètre dans le jeu de paramètres, à laquelle l’enregistrement de l’état est associé. Les numéros de ligne et de paramètre commencent à 1. Ce champ contient la valeur SQL_NO_ROW_NUMBER si cet enregistrement de l’état n’est pas associé à un numéro de ligne ou un numéro de paramètre. Si le pilote ne peut pas déterminer le numéro de ligne ou paramètre associé à cet enregistrement, ce champ a la valeur SQL_ROW_NUMBER_UNKNOWN.<br /><br /> Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction.|  
|SQL_DIAG_SERVER_NAME|SQLCHAR *|Chaîne qui indique le nom du serveur associé à l’enregistrement de diagnostic. Il est identique à la valeur retournée pour un appel à **SQLGetInfo** avec l’option SQL_DATA_SOURCE_NAME. Pour les structures de données de diagnostic associés au handle d’environnement et les diagnostics ne sont pas liées à n’importe quel serveur, ce champ est une chaîne de longueur nulle.|  
|SQL_DIAG_SQLSTATE|SQLCHAR *|Un code diagnostic SQLSTATE à cinq caractères. Pour plus d’informations, consultez [SQLSTATE](../../../odbc/reference/develop-app/sqlstates.md).|  
|SQL_DIAG_SUBCLASS_ORIGIN|SQLCHAR *|Chaîne avec le même format et les valeurs valides en tant que SQL_DIAG_CLASS_ORIGIN, qui identifie la partie de définition de la partie de la sous-classe de code SQLSTATE. Le SQLSTATE de ODBC spécifiques dont « ODBC 3.0 » est renvoyée est les suivantes :<br /><br /> 01 S 00, 01 S 01, 01 S 02, 01S06, 01 S 07, 07 S 01, 08 S 01, 21S01, 21S02, 25 S 01, 25S02, 25S03, 42S01, 42 S 02, 42S11, 42S12, 42S21, 42S22, HY095, HY097, HY098, HY099, HY100, HY101, HY105, HY107, HY109, HY110, HY111, HYT00, HYT01, IM001, IM002, IM003, IM004, IM005, IM006, IM007, IM008, IM010, IM011, IM012.|  
  
## <a name="values-of-the-dynamic-function-fields"></a>Valeurs des champs dynamiques (fonction)  
 Le tableau suivant décrit les valeurs de SQL_DIAG_DYNAMIC_FUNCTION et SQL_DIAG_DYNAMIC_FUNCTION_CODE qui s’appliquent à chaque type d’instruction SQL exécutée par un appel à **SQLExecute** ou **SQLExecDirect**. Le pilote peut ajouter des valeurs définies par le pilote à ceux répertoriés.  
  
|Instruction SQL<br /><br /> exécutée|Valeur de<br /><br /> SQL_DIAG_DYNAMIC_FUNCTION|Valeur de<br /><br /> SQL_DIAG_DYNAMIC_FUNCTION_CODE|  
|--------------------------------|-----------------------------------------------|-----------------------------------------------------|  
|*domaine-instruction ALTER*|« ALTER DOMAINE »|SQL_DIAG_ALTER_DOMAIN|  
|*instruction table ALTER*|« ALTER TABLE »|SQL_DIAG_ALTER_TABLE|  
|*définition de l’assertion*|« CRÉER ASSERTION »|SQL_DIAG_CREATE_ASSERTION|  
|*définition du jeu de caractères*|« CRÉER UN JEU DE CARACTÈRES »|SQL_DIAG_CREATE_CHARACTER_SET|  
|*définition du classement*|« CRÉER UN CLASSEMENT »|SQL_DIAG_CREATE_COLLATION|  
|*instruction-index créer*|« CRÉER UN INDEX »|SQL_DIAG_CREATE_INDEX|  
|*instruction de table créer*|« CREATE TABLE »|SQL_DIAG_CREATE_TABLE|  
|*instruction du mode Création*|« CRÉER UN AFFICHAGE »|SQL_DIAG_CREATE_VIEW|  
|*spécification de curseur*|« SÉLECTIONNEZ CURSEUR »|SQL_DIAG_SELECT_CURSOR|  
|*positionné-instruction de suppression*|« CURSEUR DYNAMIQUE DE SUPPRESSION »|SQL_DIAG_DYNAMIC_DELETE_CURSOR|  
|*recherche-instruction de suppression*|« DELETE WHERE »|SQL_DIAG_DELETE_WHERE|  
n-définition *|« CRÉER UN DOMAINE »|SQL_DIAG_CREATE_DOMAIN|  
|*instruction d’assertion DROP*|« DROP ASSERTION »|SQL_DIAG_DROP_ASSERTION|  
|*DROP-caractère-set-stmt*|« JEU DE CARACTÈRES DE DÉPÔT »|SQL_DIAG_DROP_CHARACTER_SET|  
|*instruction DROP-classement*|« DROP CLASSEMENT »|SQL_DIAG_DROP_COLLATION|  
|*instruction DROP-domaine*|« DROP DOMAINE »|SQL_DIAG_DROP_DOMAIN|  
|*instruction DROP-index*|« DROP INDEX »|SQL_DIAG_DROP_INDEX|  
|*instruction DROP-schéma*|« DROP SCHEMA »|SQL_DIAG_DROP_SCHEMA|  
|*instruction DROP-table*|« DROP TABLE »|SQL_DIAG_DROP_TABLE|  
|*instruction DROP-traduction*|« DROP TRADUCTION »|SQL_DIAG_DROP_TRANSLATION|  
|*instruction DROP-affichage*|« DROP VIEW »|SQL_DIAG_DROP_VIEW|  
-instruction *|« GRANT »|SQL_DIAG_GRANT|  
|*instruction INSERT*|« INSERTION »|SQL_DIAG_INSERT|  
|*Extension de procédure ODBC*|« APPELER »|APPEL DE SQL_DIAG_|  
|*instruction REVOKE*|« REVOKE »|SQL_DIAG_REVOKE|  
|*définition de schéma*|« CRÉER UN SCHÉMA »|SQL_DIAG_CREATE_SCHEMA|  
|*définition de la traduction*|« CRÉATION DE TRADUCTION »|SQL_DIAG_CREATE_TRANSLATION|  
|*positionné-instruction de mise à jour*|« MISE À JOUR DYNAMIQUE DE CURSEUR »|SQL_DIAG_DYNAMIC_UPDATE_CURSOR|  
|*recherche-instruction de mise à jour*|« MISE À JOUR DE WHERE »|SQL_DIAG_UPDATE_WHERE|  
|Unknown|*Chaîne vide*|SQL_DIAG_UNKNOWN_STATEMENT|  
  
## <a name="sequence-of-status-records"></a>Séquence d’enregistrements d’état  
 Enregistrements d’état sont placés dans une séquence en fonction du numéro de ligne et le type de diagnostic. Le Gestionnaire de pilotes détermine l’ordre final dans lequel retourner les enregistrements d’état qu’elle génère. Le pilote détermine l’ordre final dans lequel retourner les enregistrements d’état qu’elle génère.  
  
 Si les enregistrements de diagnostic sont validés par le Gestionnaire de pilotes et le pilote, le Gestionnaire de pilotes est responsable de leur classement.  
  
 S’il existe deux ou plusieurs enregistrements d’état, la séquence des enregistrements est déterminée par le numéro de ligne. Les règles suivantes s’appliquent à la détermination de la séquence des enregistrements de diagnostic par ligne :  
  
-   Les enregistrements qui ne correspondent pas à n’importe quelle ligne apparaissent devant les enregistrements qui correspondent à une ligne particulière, car SQL_NO_ROW_NUMBER est défini pour être -1.  
  
-   Enregistrements pour lesquels le numéro de ligne est inconnu apparaissent devant tous les autres enregistrements, étant donné que SQL_ROW_NUMBER_UNKNOWN est défini comme étant – 2.  
  
-   Pour tous les enregistrements qui se rapportent à des lignes spécifiques, les enregistrements sont triés par la valeur dans le champ SQL_DIAG_ROW_NUMBER. Toutes les erreurs et avertissements de la première ligne affectés sont répertoriés, puis toutes les erreurs et avertissements de la prochaine ligne affecté et ainsi de suite.  
  
> [!NOTE]  
>  Le ODBC 3 *.x* du Gestionnaire de pilotes ne trie pas les enregistrements d’état dans la file d’attente diagnostic si 01 s 01 SQLSTATE (erreur de ligne) est retourné par une API ODBC 2 *.x* pilote ou si 01 s 01 SQLSTATE (erreur de ligne) est retourné par une ODBC 3 *.x* pilote lorsque **SQLExtendedFetch** est appelée ou **SQLSetPos** est appelée sur un curseur qui a été positionné avec **SQLExtendedFetch**.  
  
 Dans chaque ligne, ou pour tous les enregistrements qui ne correspondent pas à une ligne ou pour lesquels le numéro de ligne est inconnu, ou pour tous les enregistrements ayant un nombre de lignes égal à SQL_NO_ROW_NUMBER, le premier enregistrement répertorié est déterminé à l’aide d’un ensemble de règles de tri. Après le premier enregistrement, l’ordre des enregistrements qui affectent une ligne n’est pas défini. Une application ne peut pas supposer que les erreurs précédent avertissements après le premier enregistrement. La structure de données de diagnostic complet pour obtenir des informations complètes sur un appel à une fonction doivent analyser les applications.  
  
 Les règles suivantes sont utilisées pour déterminer le premier enregistrement dans une ligne. L’enregistrement avec le rang le plus élevé est le premier enregistrement. La source d’un enregistrement (Gestionnaire de pilotes, pilotes, passerelle et ainsi de suite) n’est pas constitue lorsque les enregistrements de classement.  
  
-   **Erreurs** enregistrements d’état qui décrivent les erreurs ont le rang le plus élevé. Les règles suivantes sont appliquées pour trier les erreurs :  
  
    -   Les enregistrements qui indiquent un échec de la transaction ou l’échec de la transaction possible outrank tous les autres enregistrements.  
  
    -   Si deux ou plusieurs enregistrements décrivent la même condition d’erreur, SQLSTATE définis par la spécification CLI de groupe ouvert (classes 03 via HZ) outrank SQLSTATE de ODBC - et définies par le pilote.  
  
-   **Aucune des valeurs de données défini par l’implémentation** enregistrements d’état qui décrivent les valeurs d’absence de données définies par le pilote (classe 02) ont la deuxième rang le plus élevé.  
  
-   **Avertissements** enregistrements d’état qui décrivent les avertissements (classe 01) ont le rang le plus bas. Si deux ou plusieurs enregistrements décrivent la même condition d’avertissement, SQLSTATE définis par la spécification CLI de groupe ouvert, l’avertissement outrank SQLSTATE définies par ODBC et définies par le pilote.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention de plusieurs champs d’une structure de données de diagnostic|[SQLGetDiagRec, fonction](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
