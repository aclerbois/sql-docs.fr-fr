---
title: Fonction SQLColAttribute | Documents Microsoft
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
- SQLColAttribute
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColAttribute
helpviewer_keywords:
- SQLColAttribute function [ODBC]
ms.assetid: 8c45c598-cb01-4789-a571-e93619a18ed9
caps.latest.revision: 42
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5afbe6bbea4e1c50e3b16742bf5d0fa1b3c16a9d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcolattribute-function"></a>Fonction SQLColAttribute
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLColAttribute** retourne des informations de descripteur pour une colonne dans un jeu de résultats. Informations de descripteur sont retournées comme une chaîne de caractères, une valeur de descripteur dépendant ou une valeur entière.  
  
> [!NOTE]  
>  Pour plus d’informations sur les le Gestionnaire de pilotes mappe cette fonction lorsqu’un ODBC 3. *x* application fonctionne avec une API ODBC 2. *x* pilote, consultez [mappage des fonctions de remplacement pour la compatibilité descendante des Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLColAttribute (  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ColumnNumber,  
      SQLUSMALLINT    FieldIdentifier,  
      SQLPOINTER      CharacterAttributePtr,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLLEN *        NumericAttributePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *ColumnNumber*  
 [Entrée] Le numéro de l’enregistrement dans l’IRD à partir de laquelle la valeur du champ doit être récupéré. Cet argument correspond au numéro de colonne de données de résultat, classés de manière séquentielle dans l’ordre croissant de colonne, en commençant à 1. Les colonnes peuvent être décrits dans n’importe quel ordre.  
  
 La colonne 0 peut être spécifiée dans cet argument, mais toutes les valeurs sauf SQL_DESC_TYPE et SQL_DESC_OCTET_LENGTH retournent des valeurs non définis.  
  
 *FieldIdentifier*  
 [Entrée] Le handle de descripteur. Ce handle définit quel champ de l’IRD doit être interrogée (par exemple, SQL_COLUMN_TABLE_NAME).  
  
 *CharacterAttributePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la valeur de la *FieldIdentifier* champ le *ColumnNumber* ligne de l’IRD, si le champ est une chaîne de caractères. Dans le cas contraire, le champ n’est pas utilisé.  
  
 Si *CharacterAttributePtr* est NULL, *StringLengthPtr* retourne toujours le nombre total d’octets (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *CharacterAttributePtr*.  
  
 *BufferLength*  
 [Entrée] Si *FieldIdentifier* est un champ défini par ODBC et *CharacterAttributePtr* pointe vers une chaîne de caractères ou de la mémoire tampon binaire, cet argument doit être la longueur de \* *CharacterAttributePtr*. Si *FieldIdentifier* est un champ défini par ODBC et \* *CharacterAttribute*Ptr est un entier, ce champ est ignoré. Si le  *\*CharacterAttributePtr* est une chaîne Unicode (lors de l’appel **SQLColAttributeW**), la *BufferLength* l’argument doit être un nombre pair. Si *FieldIdentifier* est un champ défini par le pilote, l’application indiquant la nature du champ au Gestionnaire de pilote en définissant le *BufferLength* argument. *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *CharacterAttributePtr* est un pointeur vers un pointeur, *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *CharacterAttributePtr* est un pointeur vers une chaîne de caractères, le *BufferLength* est la longueur de la mémoire tampon.  
  
-   Si *CharacterAttributePtr* est un pointeur vers une mémoire tampon binaire, les emplacements de l’application le résultat de la SQL_LEN_BINARY_ATTR (*longueur*) macro dans *BufferLength*. Il s’ensuit une valeur négative dans *BufferLength*.  
  
-   Si *CharacterAttributePtr* est un pointeur vers un type de données de longueur fixe *BufferLength* doit être une des opérations suivantes : SQL_IS_INTEGER, SQL_IS_UNINTEGER, SQL_SMALLINT ou SQLUSMALLINT.  
  
 *StringLengthPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre total d’octets (à l’exception de l’octet de valeur NULL pour les données de type caractère) disponibles à renvoyer dans **CharacterAttributePtr*.  
  
 Pour les données de caractères, si le nombre d’octets à retourner est supérieur ou égal à *BufferLength*, les informations de descripteur dans \* *CharacterAttributePtr* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null et se termine par null par le pilote.  
  
 Pour tous les autres types de données, la valeur de *BufferLength* est ignoré et le pilote suppose que la taille de **CharacterAttributePtr* est 32 bits.  
  
 *NumericAttributePtr*  
 [Sortie] Pointeur vers un mémoire tampon d’entier dans lequel retourner la valeur de la *FieldIdentifier* champ le *ColumnNumber* ligne de l’IRD, si le champ est un type de descripteur numérique, comme SQL_DESC_COLUMN_LENGTH. Dans le cas contraire, le champ n’est pas utilisé. Notez que certains pilotes peuvent écrire uniquement 32 bits inférieurs ou le bit d’ordre supérieur de 16 bits d’une mémoire tampon et la laisser inchangée. Par conséquent, les applications doivent initialiser la valeur 0 avant d’appeler cette fonction.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLColAttribute** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLColAttribute** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|La mémoire tampon \* *CharacterAttributePtr* n’est pas suffisamment grande pour retourner la valeur de la chaîne entière, donc la valeur de chaîne était tronquée. La longueur de la valeur de chaîne non tronqué est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07005|Instruction ne préparée pas une *spécification de curseur*|L’instruction associée le *au paramètre StatementHandle* n’a pas retourné un jeu de résultats et *FieldIdentifier* n’était pas SQL_DESC_COUNT. Il n’a aucune colonne pour décrire.|  
|07009|Index de descripteur non valide|(DM) la valeur spécifiée pour *ColumnNumber* était égale à 0, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS SQL_UB_OFF.<br /><br /> La valeur spécifiée pour l’argument *ColumnNumber* était supérieur au nombre de colonnes dans le jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagField** à partir des données de diagnostic structure décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction aynchronous toujours en cours d’exécution lors de l’appel de SQLColAttribute.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), la fonction a été appelée avant d’appeler **SQLPrepare**, **SQLExecDirect**, ou une fonction de catalogue pour la *au paramètre StatementHandle*.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM)  *\*CharacterAttributePtr* est une chaîne de caractères et *BufferLength* était inférieur à 0, mais pas égale à SQL_NTS.|  
|HY091|Identificateur de champ de descripteur non valide|La valeur spécifiée pour l’argument *FieldIdentifier* n’est pas une des valeurs définies et n’a pas une valeur définie par l’implémentation.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Pilote non compatibles avec|La valeur spécifiée pour l’argument *FieldIdentifier* n’était pas prise en charge par le pilote.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
 Lorsqu’elle est appelée après **SQLPrepare** et avant **SQLExecute**, **SQLColAttribute** peut retourner tout SQLSTATE qui peut être retournée par **SQLPrepare** ou **SQLExecute**, lorsque la source de données évalue l’instruction SQL associée selon le *au paramètre StatementHandle*.  
  
 Pour des raisons de performances, une application ne doit pas appeler **SQLColAttribute** avant d’exécuter une instruction.  
  
## <a name="comments"></a>Commentaires  
 Pour plus d’informations sur la façon dont les applications utilisent les informations retournées par **SQLColAttribute**, consultez [métadonnées de valeur de résultat](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLColAttribute** retourne les informations dans \* *NumericAttributePtr* ou dans \* *CharacterAttributePtr*. Informations de l’entier sont retournées dans \* *NumericAttributePtr* comme valeur SQLLEN ; tous les autres formats d’informations sont retournées dans \* *CharacterAttributePtr*. Lorsque les informations sont retournées dans \* *NumericAttributePtr*, le pilote ignore *CharacterAttributePtr*, *BufferLength*, et *StringLengthPtr*. Lorsque les informations sont retournées dans \* *CharacterAttributePtr*, le pilote ignore *NumericAttributePtr*.  
  
 **SQLColAttribute** retourne des valeurs dans les champs de descripteur de l’IRD. La fonction est appelée avec un descripteur d’instruction, plutôt qu’un handle de descripteur. Les valeurs retournées par **SQLColAttribute** pour le *FieldIdentifier* valeurs répertoriées plus loin dans cette section peuvent également être récupérées en appelant **SQLGetDescField** avec le descripteur IRD approprié.  
  
 Le descripteur actuellement défini des champs, la version d’ODBC dans lequel elles ont été ajoutées, et dans lequel les informations sont renvoyées pour les arguments sont présentés plus loin dans cette section. d’autres types de descripteur peuvent être définis par les pilotes pour tirer parti de différentes sources de données.  
  
 Une application ODBC 3. *x* pilote doit retourner une valeur pour chacun des champs de descripteur. Si un champ de descripteur ne s’applique pas à une source de données ou de pilote et, sauf indication contraire, le pilote retourne 0 dans \* *StringLengthPtr* ou une chaîne vide dans **CharacterAttributePtr*.  
  
## <a name="backward-compatibility"></a>Compatibilité descendante  
 ODBC 3. *x* fonction **SQLColAttribute** remplace déconseillées ODBC 2. *x* fonction **SQLColAttributes**. Lors du mappage **SQLColAttributes** à **SQLColAttribute** (quand un ODBC 2. *x* application fonctionne avec un ODBC 3. *x* pilote), ou d’un mappage **SQLColAttribute** à **SQLColAttributes** (quand un ODBC 3. *x* application fonctionne avec une API ODBC 2. *x* pilote), le Gestionnaire de pilotes passe soit la valeur de *FieldIdentifier* , mappe à une nouvelle valeur, ou retourne une erreur, comme suit :  
  
> [!NOTE]  
>  Le préfixe utilisé dans *FieldIdentifier* valeurs dans ODBC 3. *x* a été modifié depuis qu’utilisés dans ODBC 2. *x*. Le nouveau préfixe est « SQL_DESC » ; le préfixe ancien a été « SQL_COLUMN ».  
  
-   Si le **#define** valeur de l’API ODBC 2. *x* *FieldIdentifier* est le même que le **#define** valeur de ODBC 3. *x* *FieldIdentifier*, la valeur de l’appel de fonction est simplement passée.  
  
-   Le **#define** les valeurs de l’API ODBC 2. *x* *FieldIdentifiers* SQL_COLUMN_LENGTH, SQL_COLUMN_PRECISION et SQL_COLUMN_SCALE sont différentes de la **#define** valeurs de ODBC 3. *x* *FieldIdentifiers* SQL_DESC_PRECISION et SQL_DESC_LENGTH SQL_DESC_SCALE. Une application ODBC 2. *x* pilote doive prennent uniquement en charge l’API ODBC 2. *x* valeurs. Une application ODBC 3. *x* pilote doit prendre en charge les valeurs « SQL_COLUMN » et « SQL_DESC » pour ces trois *FieldIdentifiers*. Ces valeurs sont différentes, car la précision, échelle et longueur sont définis différemment dans ODBC 3. *x* qu’ils étaient dans ODBC 2. *x*. Pour plus d’informations, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
-   Si le **#define** valeur de l’API ODBC 2. *x* *FieldIdentifier* est différente de la **#define** valeur de ODBC 3. *x* *FieldIdentifier*, comme se produit avec le nombre, le nom, et les valeurs NULL, la valeur de l’appel de fonction est mappée à la valeur correspondante. Par exemple, SQL_COLUMN_COUNT est mappé à SQL_DESC_COUNT, et SQL_DESC_COUNT est mappé à SQL_COLUMN_COUNT, en fonction de la direction du mappage.  
  
-   Si *FieldIdentifier* est une nouvelle valeur dans ODBC 3. *x*, pour lesquels il n’a à aucune valeur correspondante dans ODBC 2. *x*, elle ne sera pas mappée lorsqu’un ODBC 3. *x* application l’utilise dans un appel à **SQLColAttribute** dans une API ODBC 2. *x* pilote et l’appel retourne SQLSTATE HY091 (identificateur de champ de descripteur non valide).  
  
 Le tableau suivant répertorie les types de descripteur retournés par **SQLColAttribute**. Le type de *NumericAttributePtr* valeurs est **SQLLEN \*** .  
  
|*FieldIdentifier*|Informations<br /><br /> retourné dans| Description|  
|-----------------------|---------------------------------|-----------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE (ODBC VERSION 1.0)|*NumericAttributePtr*|SQL_TRUE si la colonne est une colonne d’auto-incrémentation.<br /><br /> SQL_FALSE si la colonne n’est pas une colonne d’auto-incrémentation ou n’est pas numérique.<br /><br /> Ce champ est valide pour les colonnes de type de données numériques uniquement. Une application peut insérer des valeurs dans une ligne contenant une colonne autoincrement, mais en général, ne peut pas mettre à jour les valeurs de la colonne.<br /><br /> Lorsqu’une instruction insert est effectué dans une colonne autoincrement, une valeur unique est insérée dans la colonne au moment de l’insertion. L’incrément n’est pas défini, mais il est spécifique à la source de données. Une application ne doit pas supposer qu’une colonne autoincrement commence à tout moment donné par incréments par une valeur particulière.|  
|SQL_DESC_BASE_COLUMN_NAME (ODBC 3.0)|*CharacterAttributePtr*|Le nom de colonne de base pour le résultat de jeu de colonnes. Si un nom de colonne de base n’existe pas (comme dans le cas des colonnes qui sont des expressions), cette variable contient une chaîne vide.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_BASE_COLUMN_NAME de l’IRD qui est un champ en lecture seule.|  
|SQL_DESC_BASE_TABLE_NAME (ODBC 3.0)|*CharacterAttributePtr*|Le nom de la table de base qui contient la colonne. Si le nom de la table de base ne peut pas être défini ou n’est pas applicable, cette variable contient une chaîne vide.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_BASE_TABLE_NAME de l’IRD qui est un champ en lecture seule.|  
|SQL_DESC_CASE_SENSITIVE (ODBC VERSION 1.0)|*NumericAttributePtr*|SQL_TRUE si la colonne est considérée comme respectant la casse pour les classements et les comparaisons.<br /><br /> SQL_FALSE si la colonne n’est pas traitée comme respectant la casse pour les classements et les comparaisons ou est non caractère.|  
|SQL_DESC_CATALOG_NAME (ODBC VERSION 2.0)|*CharacterAttributePtr*|Le catalogue de la table qui contient la colonne. La valeur retournée est défini par l’implémentation si la colonne est une expression ou si la colonne fait partie d’une vue. Si la source de données ne prend pas en charge les catalogues ou le nom du catalogue ne peut pas être déterminé, une chaîne vide est retournée. Ce champ d’enregistrement VARCHAR n’est pas limité à 128 caractères.|  
|SQL_DESC_CONCISE_TYPE (ODBC VERSION 1.0)|*NumericAttributePtr*|Type de données précis.<br /><br /> Pour les types de données datetime et interval, ce champ retourne le type de données concis ; par exemple, SQL_TYPE_TIME ou SQL_INTERVAL_YEAR. (Pour plus d’informations, consultez [les identificateurs de Type de données et les descripteurs de](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) annexe d : Types de données.)<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_CONCISE_TYPE de l’IRD.|  
|SQL_DESC_COUNT (ODBC VERSION 1.0)|*NumericAttributePtr*|Le nombre de colonnes disponibles dans le jeu de résultats. Cela retourne 0 si il y a aucune colonne dans le jeu de résultats. La valeur de la *ColumnNumber* argument est ignoré.<br /><br /> Ces informations sont retournées à partir du champ d’en-tête SQL_DESC_COUNT de l’IRD.|  
|COLONNES SQL_DESC_DISPLAY_SIZE (ODBC 1.0)|*NumericAttributePtr*|Nombre maximal de caractères requis pour afficher des données à partir de la colonne. Pour plus d’informations sur la taille d’affichage, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et afficher la taille](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|SQL_DESC_FIXED_PREC_SCALE (ODBC VERSION 1.0)|*NumericAttributePtr*|SQL_TRUE si la colonne a une précision fixe et une échelle différente de zéro qui sont spécifiques à la source de données.<br /><br /> SQL_FALSE si la colonne n’a pas une précision fixe et une échelle différente de zéro qui sont spécifiques à la source de données.|  
|SQL_DESC_LABEL_NAME (ODBC VERSION 2.0)|*CharacterAttributePtr*|L’étiquette de colonne ou le titre. Par exemple, une colonne nommée EmpName doit être intitulée Nom de l’employé ou peut être étiquetée avec un alias.<br /><br /> Si une colonne n’ont pas d’étiquette, le nom de colonne est retourné. Si la colonne est sans titre et sans nom, une chaîne vide est retournée.|  
|SQL_DESC_LENGTH (ODBC 3.0)|*NumericAttributePtr*|Une valeur numérique qui est la longueur maximale ou réel une chaîne ou binaire des données de caractères de type. Il est le nombre maximal de caractères pour un type de données de longueur fixe ou la longueur de caractère pour un type de données de longueur variable. Sa valeur exclut toujours l’octet de fin null qui se termine par la chaîne de caractères.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_LENGTH de l’IRD.<br /><br /> Pour plus d’informations sur la longueur, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|SQL_DESC_LITERAL_PREFIX (ODBC 3.0)|*CharacterAttributePtr*|Ce champ d’enregistrement varchar (128) contient l’ou les caractères que le pilote ne reconnaît en tant que préfixe pour un littéral de ce type de données. Ce champ contient une chaîne vide pour un type de données pour laquelle un préfixe n’est pas applicable. Pour plus d’informations, consultez [littéral les préfixes et Suffixes](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LITERAL_SUFFIX (ODBC 3.0)|*CharacterAttributePtr*|Ce champ d’enregistrement varchar (128) contient l’ou les caractères que le pilote ne reconnaît comme suffixe pour un littéral de ce type de données. Ce champ contient une chaîne vide pour un type de données pour laquelle un suffixe littéral n’est pas applicable. Pour plus d’informations, consultez [littéral les préfixes et Suffixes](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LOCAL_TYPE_NAME (ODBC 3.0)|*CharacterAttributePtr*|Ce champ d’enregistrement varchar (128) contient n’importe quel nom localisé (langue) pour le type de données qui peut être différent du nom standard du type de données. S’il n’existe aucun nom localisé, une chaîne vide est retournée. Ce champ est uniquement à des fins d’affichage. Le jeu de caractères de la chaîne est dépendant des paramètres régionaux et est généralement le jeu de caractères par défaut du serveur.|  
|SQL_DESC_NAME (ODBC 3.0)|*CharacterAttributePtr*|L’alias de colonne, si elle s’applique. Si l’alias de colonne ne s’applique pas, le nom de colonne est retourné. Dans les deux cas, SQL_DESC_UNNAMED a pour valeur SQL_NAMED. S’il existe aucun nom de colonne ou un alias de colonne, une chaîne vide est retournée et SQL_DESC_UNNAMED est définie sur la valeur SQL_UNNAMED.<br /><br /> Ces informations sont retournées à partir du champ SQL_DESC_NAME d’enregistrement de l’IRD.|  
|SQL_DESC_NULLABLE (ODBC 3.0)|*NumericAttributePtr*|SQL_ autorisant la valeur null si la colonne peut avoir des valeurs NULL ; SQL_NO_NULLS si la colonne n’a pas de valeurs NULL ; ou SQL_NULLABLE_UNKNOWN si on ne sait pas si la colonne accepte les valeurs NULL.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_NULLABLE de l’IRD.|  
|SQL_DESC_NUM_PREC_RADIX (ODBC 3.0)|*NumericAttributePtr*|Si le type de données dans le champ SQL_DESC_TYPE est un type de données numériques approximatives, ce champ SQLINTEGER contient une valeur de 2, car le champ SQL_DESC_PRECISION contient le nombre de bits. Si le type de données dans le champ SQL_DESC_TYPE est un type de données numériques exactes, ce champ contient une valeur de 10, car le champ SQL_DESC_PRECISION contient le nombre de chiffres décimaux. Ce champ est défini sur 0 pour tous les types de données non numériques.|  
|SQL_DESC_OCTET_LENGTH (ODBC 3.0)|*NumericAttributePtr*|La longueur, en octets, d’un type de données binaire ou chaîne de caractères. Pour les caractères de longueur fixe ou des types de données binaires, il s’agit de la longueur réelle en octets. Pour la longueur de la variable de type caractère ou binaire, il s’agit de la longueur maximale en octets. Cette valeur n’inclut pas le terminateur null.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_OCTET_LENGTH de l’IRD.<br /><br /> Pour plus d’informations sur la longueur, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|SQL_DESC_PRECISION (ODBC 3.0)|*NumericAttributePtr*|Une valeur numérique pour un type de données numérique désigne la précision applicable. SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP, les types de données et tous les types de données de l’intervalle qui représentent un intervalle de temps, sa valeur correspond à la précision applicable du composant en fractions de seconde.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_PRECISION de l’IRD.|  
|SQL_DESC_SCALE (ODBC 3.0)|*NumericAttributePtr*|Valeur numérique correspondant à l’échelle applicable pour le type de données numérique. Pour les types de données DECIMAL et NUMERIC, il s’agit de l’échelle définie. Il n’est pas défini pour tous les autres types de données.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement de mise à l’échelle de l’IRD.|  
|SQL_DESC_SCHEMA_NAME (ODBC VERSION 2.0)|*CharacterAttributePtr*|Le schéma de la table qui contient la colonne. La valeur retournée est défini par l’implémentation si la colonne est une expression ou si la colonne fait partie d’une vue. Si la source de données ne prend pas en charge les schémas ou le nom de schéma ne peut pas être déterminé, une chaîne vide est retournée. Ce champ d’enregistrement VARCHAR n’est pas limité à 128 caractères.|  
|SQL_DESC_SEARCHABLE (ODBC VERSION 1.0)|*NumericAttributePtr*|SQL_PRED_NONE si la colonne ne peut pas être utilisée dans une clause WHERE. (Cela est identique à la valeur SQL_UNSEARCHABLE dans ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR si la colonne peut être utilisée dans une clause WHERE, mais uniquement avec le prédicat LIKE. (Cela est identique à la valeur SQL_LIKE_ONLY dans ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC si la colonne peut être utilisée dans une clause WHERE avec tous les opérateurs de comparaison à l’exception de type. (Cela est identique à la valeur SQL_EXCEPT_LIKE dans ODBC 2. *x*.)<br /><br /> SQL_PRED_SEARCHABLE si la colonne peut être utilisée dans une clause WHERE avec tout opérateur de comparaison.<br /><br /> Colonnes de type SQL_LONGVARCHAR et SQL_LONGVARBINARY les SQL_PRED_CHAR généralement retour.|  
|SQL_DESC_TABLE_NAME (ODBC VERSION 2.0)|*CharacterAttributePtr*|Nom de la table qui contient la colonne. La valeur retournée est défini par l’implémentation si la colonne est une expression ou si la colonne fait partie d’une vue.<br /><br /> Si le nom de table ne peut pas être déterminé, une chaîne vide est retournée.|  
|SQL_DESC_TYPE (ODBC 3.0)|*NumericAttributePtr*|Valeur numérique qui spécifie le type de données SQL.<br /><br /> Lorsque *ColumnNumber* est égal à 0, SQL_BINARY est retourné pour les signets de longueur variable et SQL_INTEGER est retournée pour les signets de longueur fixe.<br /><br /> Pour les types de données datetime et interval, ce champ retourne le type de données verbose : SQL_DATETIME ou SQL_INTERVAL. (Pour plus d’informations, consultez [les identificateurs de Type de données et les descripteurs de](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) annexe d : Types de données.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_TYPE de l’IRD. **Remarque :** pour travailler sur ODBC 2. *x* pilotes, utilisez SQL_DESC_CONCISE_TYPE à la place.|  
|SQL_DESC_TYPE_NAME (ODBC VERSION 1.0)|*CharacterAttributePtr*|Nom du type de données de dépend de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINARY » ou « () CHAR pour les données BIT ».<br /><br /> Si le type est inconnu, une chaîne vide est retournée.|  
|SQL_DESC_UNNAMED (ODBC 3.0)|*NumericAttributePtr*|Valeur SQL_NAMED ou valeur SQL_UNNAMED. Si le champ SQL_DESC_NAME de l’IRD contient un alias de colonne ou un nom de colonne, la valeur SQL_NAMED est retournée. S’il n’existe aucun nom de colonne ou un alias de colonne, la valeur SQL_UNNAMED est retournée.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_UNNAMED de l’IRD.|  
|SQL_DESC_UNSIGNED (ODBC VERSION 1.0)|*NumericAttributePtr*|SQL_TRUE si la colonne est non signé (ou non numérique).<br /><br /> SQL_FALSE si la colonne est signée.|  
|SQL_DESC_UPDATABLE (ODBC VERSION 1.0)|*NumericAttributePtr*|Colonne est décrite par les valeurs de l’une des constantes définies :<br /><br /> SQL_ATTR_READONLY SQL_ATTR_WRITE SQL_ATTR_READWRITE_UNKNOWN<br /><br /> SQL_DESC_UPDATABLE décrit la mise à jour de la colonne du jeu de résultats, pas la colonne dans la table de base. La mise à jour de la colonne de base sur laquelle repose la colonne du jeu de résultats peut être différente de la valeur de ce champ. Si une colonne doit être mise à jour peut être basée sur le type de données, des privilèges d’utilisateur et la définition du résultat se. S’il est difficile de savoir si une colonne est modifiable, SQL_ATTR_READWRITE_UNKNOWN doit être retourné.|  
  
 **SQLColAttribute** est une alternative extensible à **SQLDescribeCol**. **SQLDescribeCol** retourne un ensemble fixe d’informations de descripteur selon ANSI-89 SQL. **SQLColAttribute** autorise l’accès à l’ensemble plus vaste d’informations de descripteur disponibles dans ANSI SQL-92 et SGBD extensions de fournisseur.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLDescribeCol, fonction](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
  
## <a name="example"></a>Exemple  
 L’exemple de code suivant ne libère pas de handles et les connexions. Consultez [SQLFreeHandle, fonction](../../../odbc/reference/syntax/sqlfreehandle-function.md), [programme exemple ODBC](../../../odbc/reference/sample-odbc-program.md), et [SQLFreeStmt, fonction](../../../odbc/reference/syntax/sqlfreestmt-function.md) pour obtenir des exemples de code libérer les handles et les instructions.  
  
```  
// SQLColAttibute.cpp  
// compile with: user32.lib odbc32.lib  
  
#define UNICODE  
  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printStatementResult(SQLHSTMT hstmt) {  
   int bufferSize = 1024, i;  
   SQLRETURN retCode;  
   SQLSMALLINT numColumn = 0, bufferLenUsed;  
   SQLPOINTER* columnLabels = (SQLPOINTER *)malloc( numColumn * sizeof(SQLPOINTER*) );  
   struct DataBinding* columnData = (struct DataBinding*)malloc( numColumn * sizeof(struct DataBinding) );  
  
   retCode = SQLNumResultCols(hstmt, &numColumn);  
  
   printf( "Columns from that table:\n" );  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnLabels[i] = (SQLPOINTER)malloc( bufferSize*sizeof(char) );  
  
      retCode = SQLColAttribute(hstmt, (SQLUSMALLINT)i + 1, SQL_DESC_LABEL, columnLabels[i], (SQLSMALLINT)bufferSize, &bufferLenUsed, NULL);  
      wprintf( L"Column %d: %s\n", i, (wchar_t*)columnLabels[i] );  
   }  
  
   // allocate memory for the binding  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnData[i].TargetType = SQL_C_CHAR;  
      columnData[i].BufferLength = (bufferSize+1);  
      columnData[i].TargetValuePtr = malloc( sizeof(unsigned char)*columnData[i].BufferLength );  
   }  
  
   // setup the binding   
   for ( i = 0 ; i < numColumn ; i++ ) {  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, columnData[i].TargetType,   
         columnData[i].TargetValuePtr, columnData[i].BufferLength, &(columnData[i].StrLen_or_Ind));  
   }  
  
   printf( "Data from that table:\n" );  
   // fetch the data and print out the data  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt) ) {  
      int j;  
      for ( j = 0 ; j < numColumn ; j++ )  
         wprintf( L"%s: %hs\n", columnLabels[j], columnData[j].TargetValuePtr );  
      printf( "\n" );  
   }  
   printf( "\n" );   
}  
  
int main() {  
   int bufferSize = 1024, i, count = 1, numCols = 5;  
   wchar_t firstTableName[1024], * dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize ), * userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen, bufferLen;  
   SQLRETURN retCode;  
  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   SQLWCHAR* selectAllQuery = (SQLWCHAR *)malloc( sizeof(SQLWCHAR) * bufferSize );  
  
   // connect to database  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLCHAR *)(void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, L"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // display the database information  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferLen);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, &bufferLen);  
  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // Set up the binding. This can be used even if the statement is closed by closeStatementHandle  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   retCode = SQLTables( hstmt, (SQLWCHAR*)SQL_ALL_CATALOGS, SQL_NTS, L"", SQL_NTS, L"", SQL_NTS, L"", SQL_NTS );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   retCode = SQLTables( hstmt, dbName, SQL_NTS, userName, SQL_NTS, L"%", SQL_NTS, L"TABLE", SQL_NTS );  
  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt), ++count )  
      if ( count == 1 )  
         StringCchPrintfW( firstTableName, 1024, L"%hs", catalogResult[2].TargetValuePtr );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   wprintf( L"Select all data from the first table (%s)\n", firstTableName );  
   StringCchPrintfW( selectAllQuery, bufferSize, L"SELECT * FROM %s", firstTableName );  
  
   retCode = SQLExecDirect(hstmt, selectAllQuery, SQL_NTS);  
   printStatementResult(hstmt);  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md)
