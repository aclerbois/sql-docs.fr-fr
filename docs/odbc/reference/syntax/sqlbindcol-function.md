---
title: Fonction SQLBindCol | Documents Microsoft
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
- SQLBindCol
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLBindCol
helpviewer_keywords:
- SQLBindCol function [ODBC]
ms.assetid: 41a37655-84cd-423f-9daa-e0b47b88dc54
caps.latest.revision: 37
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f51f8da589db04b44ab31a493d97fe3d94b3b6d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlbindcol-function"></a>Fonction SQLBindCol
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLBindCol** lie des tampons de données d’application aux colonnes du jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLBindCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_Ind);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *ColumnNumber*  
 [Entrée] Nombre du résultat défini de colonne à lier. Les colonnes sont numérotées dans l’ordre croissant des colonnes à partir de 0, où la colonne 0 est la colonne de signet. Si les signets ne sont pas utilisés, autrement dit, l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a la valeur SQL_UB_OFF, puis les numéros de colonne commencent à 1.  
  
 *TargetType*  
 [Entrée] L’identificateur du type de données C de la \* *TargetValuePtr* mémoire tampon. Lorsqu’il récupère des données à partir de la source de données avec **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations**, ou **SQLSetPos**, le pilote convertit les données pour ce type ; lorsqu’il envoie des données à la source de données avec **SQLBulkOperations** ou **SQLSetPos**, le pilote convertit les données de ce type. Pour obtenir la liste des types de données C valides et les identificateurs de type, consultez la [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md) section annexe d : Types de données.  
  
 Si le *TargetType* argument est un type de données d’intervalle, l’intervalle par défaut de début précision (2) et la précision de secondes de l’intervalle par défaut (6), comme défini dans les champs SQL_DESC_DATETIME_INTERVAL_PRECISION et SQL_DESC_PRECISION de la ARD, respectivement, sont utilisés pour les données. Si le *TargetType* argument est SQL_C_NUMERIC, la précision par défaut (définies par le pilote) et par défaut de l’échelle (0), comme défini dans les champs SQL_DESC_PRECISION et SQL_DESC_SCALE de la ARD, sont utilisé pour les données. Si l’échelle ni précision par défaut ne c'est-à-dire pas, l’application doit définir explicitement le champ de descripteur approprié par un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
 Vous pouvez également spécifier un type de données C étendu. Pour plus d’informations, consultez [des Types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 [Entrée/sortie différée] Pointeur vers le tampon de données à lier à la colonne. **SQLFetch** et **SQLFetchScroll** retourner des données dans cette mémoire tampon. **SQLBulkOperations** retourne des données dans cette lorsque la mémoire tampon *opération* est SQL_FETCH_BY_BOOKMARK ; il récupère des données à partir de ce lorsque la mémoire tampon *opération* est SQL_ADD ou SQL_UPDATE_BY_BOOKMARK. **SQLSetPos** retourne des données dans cette lorsque la mémoire tampon *opération* est SQL_REFRESH ; il récupère des données à partir de ce lorsque la mémoire tampon *opération* est SQL_UPDATE.  
  
 Si *TargetValuePtr* est un pointeur null, le pilote dissocie le tampon de données pour la colonne. Une application peut annuler la liaison de toutes les colonnes en appelant **SQLFreeStmt** avec l’option SQL_UNBIND. Une application peut séparer le tampon de données pour une colonne, tout en ayant une mémoire tampon de longueur / d’indicateur liée pour la colonne, si le *TargetValuePtr* argument dans l’appel à **SQLBindCol** est un pointeur null, mais la *StrLen_or_IndPtr* argument est une valeur valide.  
  
 *BufferLength*  
 [Entrée] Longueur de la **TargetValuePtr* mémoire tampon en octets.  
  
 Le pilote utilise *BufferLength* pour éviter d’écrire au-delà de la fin de la \* *TargetValuePtr* en mémoire tampon lorsqu’il retourne les données de longueur variable, telles que binaire ou caractère. Notez que le pilote compte le caractère de fin de la valeur null lorsqu’il retourne des données caractères \* *TargetValuePtr*. **TargetValuePtr* doit donc comporter d’espace pour le caractère de fin de la valeur null ou le pilote tronque les données.  
  
 Lorsque le pilote retourne les données de longueur fixe, par exemple un entier ou une structure de date, le pilote ignore *BufferLength* et suppose que la mémoire tampon est suffisamment grande pour contenir les données. Par conséquent, il est important de l’application pour allouer une mémoire tampon suffisamment grand pour les données de longueur fixe ou le pilote écrira au-delà de la fin de la mémoire tampon.  
  
 **SQLBindCol** retourne SQLSTATE HY090 (longueur de chaîne ou une mémoire tampon non valide) lorsque *BufferLength* est inférieure à 0 mais pas lorsque *BufferLength* est 0. Toutefois, si *TargetType* spécifie un type de caractère, une application ne doit pas affecter *BufferLength* à 0, car les pilotes compatibles ISO CLI retournent SQLSTATE HY090 (longueur de chaîne ou une mémoire tampon non valide) dans ce cas.  
  
 *StrLen_or_IndPtr*  
 [Entrée/sortie différée] Pointeur vers la mémoire tampon de longueur / d’indicateur à lier à la colonne. **SQLFetch** et **SQLFetchScroll** retournent une valeur dans cette mémoire tampon. **SQLBulkOperations** récupère une valeur à partir de cette mémoire tampon quand *opération* est SQL_ADD, SQL_UPDATE_BY_BOOKMARK ou SQL_DELETE_BY_BOOKMARK. **SQLBulkOperations** retourne une valeur dans cette mémoire tampon quand *opération* est SQL_FETCH_BY_BOOKMARK. **SQLSetPos** retourne une valeur dans cette mémoire tampon quand *opération* est SQL_REFRESH ; il récupère une valeur à partir de ce lorsque la mémoire tampon *opération* est SQL_UPDATE.  
  
 **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations**, et **SQLSetPos** peut renvoyer les valeurs suivantes dans la mémoire tampon de longueur / d’indicateur :  
  
-   La longueur des données available retourner  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 L’application peut placer les valeurs suivantes dans la mémoire tampon de longueur / d’indicateur pour une utilisation avec **SQLBulkOperations** ou **SQLSetPos**:  
  
-   La longueur des données envoyées  
  
-   SQL_NTS  
  
-   SQL_NULL_DATA  
  
-   SQL_DATA_AT_EXEC  
  
-   Le résultat de la macro SQL_LEN_DATA_AT_EXEC  
  
-   SQL_COLUMN_IGNORE  
  
 Si la mémoire tampon d’indicateur et la mémoire tampon de longueur sont distincts de mémoires tampons, la mémoire tampon d’indicateur peut retourner uniquement SQL_NULL_DATA, alors que la mémoire tampon de longueur peut renvoyer toutes les autres valeurs.  
  
 Pour plus d’informations, consultez [fonction SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md), [SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md), et [à l’aide des valeurs de longueur / d’indicateur](../../../odbc/reference/develop-app/using-length-and-indicator-values.md).  
  
 Si *StrLen_or_IndPtr* est une valeur de pointeur, aucune longueur ou un indicateur null est utilisée. Il s’agit d’une erreur lors de l’extraction de données et les données est NULL.  
  
 Consultez [informations ODBC 64 bits](../../../odbc/reference/odbc-64-bit-information.md), si votre application s’exécutera sur un système d’exploitation de 64 bits.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLBindCol** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLBindCol** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07006|Violation de l’attribut de type de données restreint|(DM) le *ColumnNumber* argument était 0 et le *TargetType* argument n’était pas SQL_C_BOOKMARK ou SQL_C_VARBOOKMARK.|  
|07009|Index de descripteur non valide|La valeur spécifiée pour l’argument *ColumnNumber* a dépassé le nombre maximal de colonnes dans le jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire qui est requis pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY003|Type de tampon d’application non valide|L’argument *TargetType* n’est ni un type de données valide ni SQL_C_DEFAULT.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque **SQLBindCol** a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength* était inférieure à 0.<br /><br /> (DM) le pilote a été un ODBC 2. *x* pilote, le *ColumnNumber* argument a été défini sur 0 et la valeur spécifiée pour l’argument *BufferLength* n’est pas égale à 4.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La source de données ou le pilote ne prend pas en charge la conversion spécifiée par la combinaison de la *TargetType* argument et le type de données spécifique au pilote SQL de la colonne correspondante.<br /><br /> L’argument *ColumnNumber* était 0 et le pilote ne prend pas en charge les signets.<br /><br /> Le pilote prend en charge uniquement ODBC 2. *x* et l’argument *TargetType* est une des opérations suivantes :<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> et les types de données d’intervalle C répertoriés dans [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md) annexe d : Types de données.<br /><br /> Le pilote prend uniquement en charge les versions ODBC avant 3.50 et l’argument *TargetType* a été SQL_C_GUID.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 **SQLBindCol** est utilisé pour associer, ou *lier,* colonnes dans le résultat de la valeur des tampons de données et les mémoires tampons de longueur / d’indicateur dans l’application. Lorsque l’application appelle **SQLFetch**, **SQLFetchScroll**, ou **SQLSetPos** pour extraire des données, le pilote retourne les données pour les colonnes liées dans les mémoires tampons spécifiées ; pour plus d’informations, consultez [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md). Lorsque l’application appelle **SQLBulkOperations** mise à jour ou insérer une ligne ou **SQLSetPos** pour mettre à jour une ligne, le pilote récupère les données pour les colonnes liées à partir de mémoires tampons spécifiés ; pour plus d’informations, consultez [fonction SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) ou [SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md). Pour plus d’informations sur la liaison, consultez [la récupération des résultats (Basic)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 Notez que les colonnes n’ont pas à respecter pour récupérer des données à partir de celles-ci. Une application peut également appeler **SQLGetData** pour récupérer des données à partir de colonnes. Bien qu’il soit possible de lier des colonnes dans une ligne et l’appel **SQLGetData** pour d’autres, il est soumis à certaines restrictions. Pour plus d’informations, consultez [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
## <a name="binding-unbinding-and-rebinding-columns"></a>Liaison et annulation de la liaison de reliaison des colonnes  
 Une colonne peut être liée, indépendante ou reliée à tout moment, même après que les données ont été récupérées à partir de l’ensemble de résultats. La nouvelle liaison en vigueur la prochaine fois qu’est appelée une fonction qui utilise des liaisons. Par exemple, une application lie les colonnes dans un jeu de résultats et les appels **SQLFetch**. Le pilote retourne les données dans les mémoires tampons liées. Supposons que l’application lie les colonnes à un autre jeu de tampons. Le pilote ne met pas les données de la ligne juste-extraites dans les mémoires tampons qui vient d’être liés. Au lieu de cela, il attend **SQLFetch** est de nouveau appelée, puis placer les données de la ligne suivante dans les mémoires tampons qui vient d’être liés.  
  
> [!NOTE]  
>  L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS doit toujours être défini avant de lier la colonne à 0. Cela n’est pas obligatoire, mais est fortement recommandé.  
  
## <a name="binding-columns"></a>Liaison de colonnes  
 Pour lier une colonne, une application appelle **SQLBindCol** et transmet le numéro de colonne, type, adresse et la longueur d’une mémoire tampon de données et l’adresse d’une mémoire tampon de longueur / d’indicateur. Pour plus d’informations sur la façon dont ces adresses sont utilisées, consultez « Adresses mémoire tampon », plus loin dans cette section. Pour plus d’informations sur les colonnes de la liaison, consultez [à l’aide de SQLBindCol](../../../odbc/reference/develop-app/using-sqlbindcol.md).  
  
 L’utilisation de ces mémoires tampons est différée ; Autrement dit, l’application lie les **SQLBindCol** , mais le pilote y accède à partir d’autres fonctions, à savoir **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll**, ou **SQLSetPos**. Il est responsable de l’application pour vous assurer que les pointeurs spécifié dans **SQLBindCol** reste valide tant que la liaison reste en vigueur. Si l’application autorise ces pointeurs deviennent non valides, par exemple, il libère une mémoire tampon :, puis appelle une fonction qui attend les soit valide, les conséquences sont non définis. Pour plus d’informations, consultez [différée de mémoires tampons](../../../odbc/reference/develop-app/deferred-buffers.md).  
  
 La liaison reste en vigueur jusqu'à ce qu’elle est remplacée par une nouvelle liaison, la colonne est indépendante ou l’instruction est libérée.  
  
## <a name="unbinding-columns"></a>Annulation de la liaison des colonnes  
 Pour dissocier une seule colonne, une application appelle **SQLBindCol** avec *ColumnNumber* défini sur le nombre de cette colonne et *TargetValuePtr* défini sur un pointeur null. Si *ColumnNumber* fait référence à une colonne indépendante, **SQLBindCol** toujours retourne SQL_SUCCESS.  
  
 Pour dissocier toutes les colonnes, une application appelle **SQLFreeStmt** avec *fOption* défini sur SQL_UNBIND. Cela peut également être accompli en définissant le champ SQL_DESC_COUNT de la ARD à zéro.  
  
## <a name="rebinding-columns"></a>Nouvelle liaison de colonnes  
 Une application peut effectuer une des deux opérations pour modifier une liaison :  
  
-   Appelez **SQLBindCol** pour spécifier une liaison pour une colonne qui est déjà liée. Le pilote remplace l’ancienne liaison avec une nouvelle.  
  
-   Spécifier un décalage à ajouter à l’adresse de la mémoire tampon qui a été spécifié par l’appel de la liaison à **SQLBindCol**. Pour plus d’informations, consultez la section suivante, « Les décalages de liaison ».  
  
## <a name="binding-offsets"></a>Les décalages de liaison  
 Un décalage de la liaison est une valeur qui est ajoutée aux adresses des mémoires tampons de données et l’indicateur/longueur (tel que spécifié dans le *TargetValuePtr* et *StrLen_or_IndPtr* argument) avant qu’ils sont déréférencés. Lorsque les décalages sont utilisés, les liaisons sont un « modèle » de la disposition des mémoires tampons de l’application et l’application peut passer cet « modèle » à différentes zones de mémoire en modifiant le décalage. Étant donné que le même décalage est ajouté à chaque adresse dans chaque liaison, le décalage relatif entre les mémoires tampons pour différentes colonnes doit être identique au sein de chaque jeu de mémoires tampons. Cela est vrai lors de la liaison selon les lignes est utilisée ; l’application doit disposer avec soin ses mémoires tampons pour cette option pour avoir la valeur true lorsque la liaison est utilisée.  
  
 À l’aide d’un décalage de la liaison est essentiellement le même effet que la reliaison des colonnes en appelant **SQLBindCol**. La différence est qu’un nouvel appel à **SQLBindCol** spécifie de nouvelles adresses de la mémoire tampon de données et de la mémoire tampon de longueur / d’indicateur, tandis que l’utilisation d’un décalage de liaison ne modifie pas les adresses mais simplement leur ajoute un décalage. L’application peut spécifier un décalage de nouveau chaque fois qu’il le souhaite, et ce décalage est toujours ajouté aux adresses liées à l’origine. En particulier, si le décalage est défini sur 0 ou si l’attribut d’instruction est défini à un pointeur null, le pilote utilise les adresses liées à l’origine.  
  
 Pour spécifier un décalage de la liaison, l’application définit l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR à l’adresse d’une mémoire tampon SQLINTEGER. Avant que l’application appelle une fonction qui utilise des liaisons, il place un offset en octets dans cette mémoire tampon. Pour déterminer l’adresse de la mémoire tampon à utiliser, le pilote ajoute le décalage à l’adresse dans la liaison. La somme de l’adresse et le décalage doit être une adresse valide, mais l’adresse à laquelle le décalage est ajouté ne doit pas être valide. Pour plus d’informations sur l’utilisation des décalages de liaison, consultez « Adresses mémoire tampon », plus loin dans cette section.  
  
## <a name="binding-arrays"></a>Tableaux de liaison  
 Si la taille de l’ensemble de lignes (la valeur de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE) est supérieure à 1, l’application lie les tableaux de mémoires tampons au lieu des mémoires tampons uniques. Pour plus d’informations, consultez [curseurs de bloc](../../../odbc/reference/develop-app/block-cursors.md).  
  
 L’application peut lier les tableaux de deux manières :  
  
-   Lier un tableau à chaque colonne. Cela est appelé *liaison selon les colonnes* , car chaque structure de données (tableau) contient des données pour une colonne unique.  
  
-   Définissez une structure pour contenir les données pour une ligne entière et de lier un tableau de ces structures. Cela est appelé *liaison selon les lignes* , car chaque structure de données contient les données d’une seule ligne.  
  
 Chaque tableau de mémoires tampons doit avoir au moins autant d’éléments que la taille de l’ensemble de lignes.  
  
> [!NOTE]  
>  Une application doit vérifier que l’alignement est valide. Pour plus d’informations sur les considérations d’alignement, consultez [alignement](../../../odbc/reference/develop-app/alignment.md).  
  
## <a name="column-wise-binding"></a>Liaison selon les colonnes  
 Dans la liaison, l’application lie les données et les tableaux de longueur / d’indicateur à chaque colonne.  
  
 Pour utiliser la liaison, l’application affecte d’abord l’attribut d’instruction SQL_ATTR_ROW_BIND_TYPE sur SQL_BIND_BY_COLUMN. (Il s’agit de la valeur par défaut). Pour chaque colonne à lier, l’application effectue les étapes suivantes :  
  
1.  Alloue un tableau de tampons de données.  
  
2.  Alloue un tableau de mémoires tampons de longueur / d’indicateur.  
  
    > [!NOTE]  
    >  Si l’application écrit directement aux descripteurs lorsque la liaison est utilisée, les tableaux distincts peuvent être utilisés pour les données de longueur et l’indicateur.  
  
3.  Appels **SQLBindCol** avec les arguments suivants :  
  
    -   *TargetType* est le type d’un élément unique dans le tableau de tampons de données.  
  
    -   *TargetValuePtr* est l’adresse du tableau de mémoire tampon de données.  
  
    -   *BufferLength* est la taille d’un seul élément dans le tableau de tampons de données. Le *BufferLength* argument est ignoré lorsque les données sont des données de longueur fixe.  
  
    -   *StrLen_or_IndPtr* est l’adresse du tableau de longueur / d’indicateur.  
  
 Pour plus d’informations sur l’utilisation de ces informations, consultez « Adresses mémoire tampon », plus loin dans cette section. Pour plus d’informations sur la liaison, consultez [liaison selon les colonnes](../../../odbc/reference/develop-app/column-wise-binding.md).  
  
## <a name="row-wise-binding"></a>Liaison selon les lignes  
 Dans la liaison selon les lignes, l’application définit une structure qui contient les mémoires tampons de données et de longueur / d’indicateur pour chaque colonne à lier.  
  
 Pour utiliser la liaison, l’application effectue les étapes suivantes :  
  
1.  Définit une structure pour contenir une seule ligne de données (y compris les mémoires tampons de données et de longueur / d’indicateur) et alloue un tableau de ces structures.  
  
    > [!NOTE]  
    >  Si l’application écrit directement aux descripteurs lorsque la liaison est utilisée, les champs distincts peuvent être utilisés pour les données de longueur et l’indicateur.  
  
2.  Définit l’attribut d’instruction SQL_ATTR_ROW_BIND_TYPE à la taille de la structure qui contient une ligne unique de données ou à la taille d’une instance d’une mémoire tampon dans laquelle les colonnes de résultats seront liés. La longueur doit inclure l’espace pour toutes les colonnes liées et tout remplissage de la structure ou de la mémoire tampon, pour vous assurer que lorsque l’adresse d’une colonne dépendante est incrémentée de la longueur spécifiée, le résultat pointera vers le début de la même colonne dans la ligne suivante. Lorsque vous utilisez la *sizeof* opérateur en C ANSI, ce comportement est garanti.  
  
3.  Appels **SQLBindCol** avec les arguments suivants pour chaque colonne à lier :  
  
    -   *TargetType* est le type du membre de mémoire tampon de données pour être lié à la colonne.  
  
    -   *TargetValuePtr* est l’adresse du membre de mémoire tampon de données dans le premier élément du tableau.  
  
    -   *BufferLength* est la taille du membre de mémoire tampon de données.  
  
    -   *StrLen_or_IndPtr* est l’adresse du membre de longueur / d’indicateur à lier.  
  
 Pour plus d’informations sur l’utilisation de ces informations, consultez « Adresses mémoire tampon », plus loin dans cette section. Pour plus d’informations sur la liaison, consultez [liaison selon les lignes](../../../odbc/reference/develop-app/row-wise-binding.md).  
  
## <a name="buffer-addresses"></a>Adresses de mémoire tampon  
 Le *adresse de la mémoire tampon* est l’adresse réelle de la mémoire tampon de données ou de longueur / d’indicateur. Le pilote calcule l’adresse de la mémoire tampon juste avant qu’elle écrit dans les mémoires tampons (par exemple, pendant le temps de récupération). Il est calculé à partir de la formule suivante, qui utilise les adresses spécifiées dans le *TargetValuePtr* et *StrLen_or_IndPtr* arguments, le décalage de la liaison et le numéro de ligne :  
  
 *Lié adresse* + *liaison décalage* + ((*numéro de ligne* – 1) x *taille de l’élément*)  
  
 où des variables de la formule sont définies comme décrit dans le tableau suivant.  
  
|Variable| Description|  
|--------------|-----------------|  
|*Adresse de lié*|Pour les mémoires tampons de données, l’adresse spécifiée avec la *TargetValuePtr* argument dans **SQLBindCol**.<br /><br /> Pour les mémoires tampons de longueur / d’indicateur, l’adresse spécifiée avec la *StrLen_or_IndPtr* argument dans **SQLBindCol**. Pour plus d’informations, consultez « Commentaires supplémentaires » dans la section « Descripteurs et SQLBindCol ».<br /><br /> Si l’adresse associée est 0, aucune valeur de données est retournée, même si l’adresse telle que calculée par la formule précédente est différente de zéro.|  
|*Décalage de la liaison*|Si la liaison est utilisée, la valeur stockée à l’adresse spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR.<br /><br /> Si la liaison est utilisée ou si la valeur de l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR est un pointeur null, *liaison décalage* est 0.|  
|*Numéro de ligne*|Le nombre de base 1 de la ligne dans l’ensemble de lignes. Pour les extractions de ligne unique, qui sont la valeur par défaut, il s’agit de 1.|  
|*Taille de l’élément*|La taille d’un élément dans le tableau lié.<br /><br /> Si la liaison est utilisée, il s’agit **sizeof(SQLINTEGER)** pour les mémoires tampons de longueur / d’indicateur. Pour les mémoires tampons de données, c’est la valeur de la *BufferLength* argument dans **SQLBindCol** si le type de données est de longueur variable et la taille du type de données si le type de données est de longueur fixe.<br /><br /> Si la liaison est utilisée, cela est la valeur de l’attribut d’instruction SQL_ATTR_ROW_BIND_TYPE pour les mémoires tampons de données et de longueur / d’indicateur.|  
  
## <a name="descriptors-and-sqlbindcol"></a>Descripteurs et SQLBindCol  
 Les sections suivantes décrivent comment **SQLBindCol** interagit avec des descripteurs.  
  
> [!CAUTION]  
>  Appel de **SQLBindCol** pour une seule instruction peut affecter les autres instructions. Cela se produit lorsque le ARD associé à l’instruction est explicitement alloué et est également associé avec d’autres instructions. Étant donné que **SQLBindCol** modifie le descripteur, les modifications s’appliquent à toutes les instructions à laquelle ce descripteur est associé. Si ce comportement n’est pas obligatoire, l’application doit dissocier ce descripteur à partir d’autres instructions avant d’appeler **SQLBindCol**.  
  
## <a name="argument-mappings"></a>Mappages d’argument  
 Point de vue conceptuel, **SQLBindCol** exécute les étapes suivantes dans l’ordre :  
  
1.  Appels **SQLGetStmtAttr** pour obtenir le handle ARD.  
  
2.  Appels **SQLGetDescField** pour obtenir SQL_DESC_COUNT champ de ce descripteur et si la valeur de la *ColumnNumber* argument dépasse la valeur de SQL_DESC_COUNT, appels **SQLSetDescField** pour augmenter la valeur de SQL_DESC_COUNT à *ColumnNumber*.  
  
3.  Appels **SQLSetDescField** plusieurs fois pour assigner des valeurs aux champs suivants de la ARD :  
  
    -   Définit la valeur de SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE *TargetType*, sauf que si *TargetType* est un des identificateurs d’un sous-type datetime ou interval concis, il définit SQL_DESC_TYPE SQL_DATETIME ou SQL_INTERVAL, respectivement ; définit l’identificateur concis ; SQL_DESC_CONCISE_TYPE et affecte SQL_DESC_DATETIME_INTERVAL_CODE le correspondant sous-code datetime ou interval.  
  
    -   Définit une ou plusieurs des SQL_DESC_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE et SQL_DESC_DATETIME_INTERVAL_PRECISION, en fonction de *TargetType*.  
  
    -   Définit la valeur du champ SQL_DESC_OCTET_LENGTH *BufferLength*.  
  
    -   Définit le champ SQL_DESC_DATA_PTR à la valeur de *TargetValue*.  
  
    -   Définit la valeur du champ SQL_DESC_INDICATOR_PTR *StrLen_or_Ind*. (Consultez le paragraphe suivant.)  
  
    -   Définit la valeur du champ SQL_DESC_OCTET_LENGTH_PTR *StrLen_or_Ind*. (Consultez le paragraphe suivant.)  
  
 La variable qui la *StrLen_or_Ind* argument fait référence à utilisé pour les informations d’indicateur et la longueur. Si une instruction fetch rencontre une valeur null pour la colonne, il stocke SQL_NULL_DATA dans cette variable. dans le cas contraire, elle stocke la longueur des données dans cette variable. En passant un pointeur null en tant que *StrLen_or_Ind* effectue l’opération d’extraction de retourner la longueur des données, mais rend l’extraction échoue si elle rencontre une valeur null et n’a aucun moyen pour retourner la valeur SQL_NULL_DATA.  
  
 Si l’appel à **SQLBindCol** échoue, le contenu des champs de descripteur qui il aurait défini dans le ARD ne sont pas définis, et la valeur du champ de la ARD SQL_DESC_COUNT est inchangée.  
  
## <a name="implicit-resetting-of-count-field"></a>La réinitialisation implicite du nombre de champ  
 **SQLBindCol** définit SQL_DESC_COUNT à la valeur de la *ColumnNumber* argument uniquement lorsque cela augmenterait la valeur de SQL_DESC_COUNT. Si la valeur dans la *TargetValuePtr* argument est un pointeur null et la valeur dans la *ColumnNumber* argument est égal à SQL_DESC_COUNT (autrement dit, lorsque la séparation de la plus élevée colonne dépendante), puis SQL_DESC_COUNT est défini sur le numéro de la colonne liée restante la plus élevée.  
  
## <a name="cautions-regarding-sqldefault"></a>Précautions concernant SQL_DEFAULT  
 Pour récupérer les données de la colonne avec succès, l’application doit déterminer correctement la longueur et le point de départ des données dans la mémoire tampon d’application. Lorsque l’application spécifie explicite *TargetType*, reçues de l’application sont facilement détectées. Toutefois, lorsque l’application spécifie un *TargetType* de SQL_DEFAULT, **SQLBindCol** peut être appliqué à une colonne de type de données différent de celle prévue par l’application, soit des modifications apportées aux métadonnées ou en appliquant le code à une autre colonne. Dans ce cas, l’application ne peut toujours pas déterminer le début ou la longueur des données de la colonne d’extraction. Cela peut entraîner des erreurs de données ou des violations de mémoire.  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application exécute un **sélectionnez** instruction sur la table Customers pour retourner un jeu de résultats du client ID, les noms et numéros de téléphone, triés par nom. Il appelle ensuite **SQLBindCol** pour lier les colonnes de données aux mémoires tampons locales. Enfin, l’application extrait chaque ligne de données avec **SQLFetch** et imprime le nom de chaque client, ID et numéro de téléphone.  
  
 Pour plus d’exemples de code, consultez [fonction SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [fonction SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md), et [SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```  
// SQLBindCol_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <stdio.h>  
  
#define UNICODE  
#include <sqlext.h>  
  
#define NAME_LEN 50  
#define PHONE_LEN 20  
  
void show_error() {  
   printf("error\n");  
}  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
   SQLWCHAR szName[NAME_LEN], szPhone[PHONE_LEN], sCustID[NAME_LEN];  
   SQLLEN cbName = 0, cbCustID = 0, cbPhone = 0;  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLWCHAR*) L"NorthWind", SQL_NTS, (SQLWCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {   
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               retcode = SQLExecDirect(hstmt, (SQLWCHAR *) L"SELECT CustomerID, ContactName, Phone FROM CUSTOMERS ORDER BY 2, 1, 3", SQL_NTS);  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
                  // Bind columns 1, 2, and 3  
                  retcode = SQLBindCol(hstmt, 1, SQL_C_CHAR, &sCustID, 100, &cbCustID);  
                  retcode = SQLBindCol(hstmt, 2, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
                  retcode = SQLBindCol(hstmt, 3, SQL_C_CHAR, szPhone, PHONE_LEN, &cbPhone);   
  
                  // Fetch and print each row of data. On an error, display a message and exit.  
                  for (i ; ; i++) {  
                     retcode = SQLFetch(hstmt);  
                     if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
                        show_error();  
                     if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                        wprintf(L"%d: %S %S %S\n", i + 1, sCustID, szName, szPhone);  
                     else  
                        break;  
                  }  
               }  
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLCancel(hstmt);  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
 Voir aussi, [programme exemple ODBC](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLDescribeCol, fonction](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Libération des tampons de colonnes pour l’instruction|[SQLFreeStmt, fonction](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|L’extraction de tout ou partie d’une colonne de données|[SQLGetData, fonction](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Définie les colonnes de retour du nombre de résultats|[SQLNumResultCols, fonction](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
