---
title: SQLSetStmtAttr | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetStmtAttr function
ms.assetid: 799c80fd-c561-4912-8562-9229076dfd19
caps.latest.revision: 52
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7074fcf6c6616c878b0cd4021aabb4359c2a40dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetstmtattr"></a>SQLSetStmtAttr
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  Native Client ne prend pas en charge le modèle de curseur mixte (jeu de clés/dynamique). Les tentatives de définir la taille du jeu de clés à l'aide de SQL_ATTR_KEYSET_SIZE échoue si la valeur définie n'est pas égale à 0.  
  
 L’application définit SQL_ATTR_ROW_ARRAY_SIZE sur toutes les instructions pour déclarer le nombre de lignes retournées sur une **SQLFetch** ou [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) l’appel de fonction. Sur les instructions qui indiquent un curseur côté serveur, le pilote utilise SQL_ATTR_ROW_ARRAY_SIZE pour déterminer la taille du bloc des lignes que le serveur génère pour satisfaire une demande d'extraction du curseur. Dans la taille des blocs d'un curseur dynamique, l'appartenance et l'ordre des lignes sont fixes si le niveau d'isolation de la transaction est suffisant pour garantir des lectures renouvelables des transactions validées. Le curseur est totalement dynamique en dehors du bloc indiqué par cette valeur. La taille des blocs de curseur côté serveur est complètement dynamique et peut être changée à n'importe quel stade du traitement d'extraction.  
  
## <a name="sqlsetstmtattr-and-table-valued-parameters"></a>SQLSetStmtAttr et paramètres table  
 SQLSetStmtAttr peut être utilisé pour définir SQL_SOPT_SS_PARAM_FOCUS dans le descripteur de paramètre d’application (APD) avant d’accéder aux champs de descripteur pour les colonnes de paramètre table.  
  
 Si une tentative est effectuée pour définir SQL_SOPT_SS_PARAM_FOCUS à l’ordinal d’un paramètre qui est pas un paramètre table, SQLSetStmtAttr retourne SQL_ERROR et un enregistrement de diagnostic est créé avec SQLSTATE = HY024 et le message « valeur d’attribut non valide ». SQL_SOPT_SS_PARAM_FOCUS n'est pas modifié quand SQL_ERROR est retourné.  
  
 La définition de SQL_SOPT_SS_PARAM_FOCUS sur 0 restaure l'accès aux enregistrements de descripteurs pour les paramètres.  
  
 SQLSetStmtAttr peut également être utilisé pour définir SQL_SOPT_SS_NAME_SCOPE. Pour plus d'informations, consultez la section SQL_SOPT_SS_NAME_SCOPE, plus loin dans cette rubrique.  
  
 Pour plus d’informations, consultez [des métadonnées de paramètre table pour des instructions préparées](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md).  
  
 Pour plus d’informations sur les paramètres table, consultez [paramètres table & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlsetstmtattr-support-for-sparse-columns"></a>Prise en charge par SQLSetStmtAttr des colonnes éparses  
 SQLSetStmtAttr peut être utilisé pour définir SQL_SOPT_SS_NAME_SCOPE. Pour plus d’informations, consultez la section SQL_SOPT_SS_NAME_SCOPE, plus loin dans cette rubrique. Pour plus d’informations sur les colonnes éparses, consultez [prise en charge des colonnes éparses &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="statement-attributes"></a>Attributs d'instruction  
 Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client prend aussi en charge les attributs d'instruction spécifiques au pilote suivants.  
  
### <a name="sqlsoptsscursoroptions"></a>SQL_SOPT_SS_CURSOR_OPTIONS  
 L'attribut SQL_SOPT_SS_CURSOR spécifie si le pilote utilise les options de performances spécifiques au pilote sur les curseurs. [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) n’est pas autorisée lorsque ces options sont définies. La valeur par défaut est SQL_CO_OFF. La valeur de *ValuePtr* est de type SQLLEN.  
  
|*ValuePtr* valeur| Description|  
|----------------------|-----------------|  
|SQL_CO_OFF|Valeur par défaut. Désactive les curseurs avant uniquement rapides, en lecture seule et l’auto-extraction, Active **SQLGetData** sur les curseurs avant uniquement, en lecture seule. Lorsque SQL_SOPT_SS_CURSOR_OPTIONS a la valeur SQL_CO_OFF, le type de curseur ne change pas. Autrement dit, le curseur avant uniquement rapide demeure un curseur avant uniquement rapide. Pour modifier le type de curseur, l’application doit maintenant définir un type de curseur différent à l’aide de **SQLSetStmtAttr**/SQL_ATTR_CURSOR_TYPE.|  
|SQL_CO_FFO|Active les curseurs avant uniquement rapides, en lecture seule, désactive **SQLGetData** sur les curseurs avant uniquement, en lecture seule.|  
|SQL_CO_AF|Active l'option d'auto-extraction sur tout type de curseur. Lorsque cette option est définie pour un descripteur d’instruction, **SQLExecute** ou **SQLExecDirect** génère implicite **SQLFetchScroll** (SQL_FIRST). Le curseur est ouvert et le premier lot de lignes est retourné en un seul aller-retour au serveur.|  
|SQL_CO_FFO_AF|Active les curseurs avant uniquement rapides avec l'option d'auto-extraction. La situation est la même que si SQL_CO_AF et SQL_CO_FFO étaient spécifiés.|  
  
 Lorsque ces options sont définies, le serveur ferme automatiquement le curseur lorsqu'il détecte que la dernière ligne a été extraite. L’application doit toujours appeler [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) (SQL_CLOSE) ou [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md), mais le pilote n’a pas envoyer la notification de fermeture au serveur.  
  
 Si la liste de sélection contient une **texte**, **ntext**, ou **image** colonne, le curseur avant uniquement rapide est converti en curseur dynamique et **SQLGetData** est autorisée.  
  
### <a name="sqlsoptssdeferprepare"></a>SQL_SOPT_SS_DEFER_PREPARE  
 L’attribut SQL_SOPT_SS_DEFER_PREPARE détermine si l’instruction est préparée immédiatement ou différée jusqu'à ce que **SQLExecute**, [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) ou [SQLDescribeParam](../../relational-databases/native-client-odbc-api/sqldescribeparam.md) est exécutée. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 et version antérieure, cette propriété est ignorée (aucune préparation différée). La valeur de *ValuePtr* est de type SQLLEN.  
  
|*ValuePtr* valeur| Description|  
|----------------------|-----------------|  
|SQL_DP_ON|Valeur par défaut. Après avoir appelé [fonction SQLPrepare](http://go.microsoft.com/fwlink/?LinkId=59360), la préparation de l’instruction est différée jusqu'à ce que **SQLExecute** est appelée ou opération de métapropriété (**SQLDescribeCol** ou **SQLDescribeParam**) est exécutée.|  
|SQL_DP_OFF|L’instruction est préparée dès que **SQLPrepare** est exécutée.|  
  
### <a name="sqlsoptssregionalize"></a>SQL_SOPT_SS_REGIONALIZE  
 L'attribut SQL_SOPT_SS_REGIONALIZE est utilisé pour déterminer la conversion des données au niveau de l'instruction. L'attribut oblige le pilote à respecter les paramètres régionaux du client lors de la conversion des valeurs date, heure et devise monétaire en chaînes de caractères. La conversion s'effectue uniquement des types de données natifs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers les chaînes de caractères.  
  
 La valeur de *ValuePtr* est de type SQLLEN.  
  
|*ValuePtr* valeur| Description|  
|----------------------|-----------------|  
|SQL_RE_OFF|Valeur par défaut. Le pilote ne convertit pas les valeurs date, heure et devise monétaire en données de chaînes de caractères à l'aide des paramètres régionaux du client.|  
|SQL_RE_ON|Le pilote utilise les paramètres régionaux du client lors de la conversion des données date, heure et devise monétaire en données de chaînes de caractères.|  
  
 Les paramètres de conversion régionaux s'appliquent aux types de données monétaire, numérique, date et heure. Le paramètre de conversion est applicable uniquement aux conversions sortantes lorsque des valeurs monétaire, numérique, de date ou d'heure sont converties en chaînes de caractères.  
  
> [!NOTE]  
>  Lorsque l'option d'instruction SQL_SOPT_SS_REGIONALIZE est activée, le pilote utilise les paramètres du Registre des paramètres régionaux de l'utilisateur en cours. Le pilote n’honore pas aux paramètres régionaux du thread actuel si l’application définit par, par exemple, l’appel **SetThreadLocale**.  
  
 La modification du comportement régional d'une source de données peut provoquer l'échec de l'application. Une application qui analyse les chaînes date et s'attend à ce qu'elles apparaissent comme définies par ODBC peut être affectée de manière négative par la modification de cette valeur.  
  
### <a name="sqlsoptsstextptrlogging"></a>SQL_SOPT_SS_TEXTPTR_LOGGING  
 L’attribut SQL_SOPT_SS_TEXTPTR_LOGGING bascule l’enregistrement des opérations sur les colonnes contenant des **texte** ou **image** données. La valeur de *ValuePtr* est de type SQLLEN.  
  
|*ValuePtr* valeur| Description|  
|----------------------|-----------------|  
|SQL_TL_OFF|Désactive la journalisation des opérations effectuées sur **texte** et **image** données.|  
|SQL_TL_ON|Valeur par défaut. Active la journalisation des opérations effectuées sur **texte** et **image** données.|  
  
### <a name="sqlsoptsshiddencolumns"></a>SQL_SOPT_SS_HIDDEN_COLUMNS  
 L'attribut SQL_SOPT_SS_HIDDEN_COLUMNS expose dans le jeu de résultats les colonnes masquées dans une instruction [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SELECT FOR BROWSE. Le pilote n'expose pas ces colonnes par défaut. La valeur de *ValuePtr* est de type SQLLEN.  
  
|*ValuePtr* valeur| Description|  
|----------------------|-----------------|  
|SQL_HC_OFF|Valeur par défaut. Les colonnes FOR BROWSE sont masquées dans le jeu de résultats.|  
|SQL_HC_ON|Expose les colonnes FOR BROWSE.|  
  
### <a name="sqlsoptssquerynotificationmsgtext"></a>SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT  
 L'attribut SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT retourne le texte du message pour la demande de notification de requête.  
  
### <a name="sqlsoptssquerynotificationoptions"></a>SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS  
 L'attribut SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS spécifie les options utilisées pour la demande de notification de requête. Ces options sont spécifiés dans une chaîne avec la syntaxe `name=value` comme spécifié ci-dessous. L'application est chargée de créer le service et de lire les notifications de la file d'attente.  
  
 La syntaxe de la chaîne des options de notifications de requêtes est :  
  
 `service=<service-name>[;(local database=<database>|broker instance=<broker instance>)]`  
  
 Par exemple :  
  
 `service=mySSBService;local database=mydb`  
  
### <a name="sqlsoptssquerynotificationtimeout"></a>SQL_SOPT_SS_QUERYNOTIFICATION_TIMEOUT  
 L'attribut SQL_SOPT_SS_QUERYNOTIFICATION_TIMEOUT spécifie le nombre de secondes pendant lesquelles la notification de requête demeure active. La valeur par défaut est de 432 000 secondes (5 jours). La valeur de *ValuePtr* est de type SQLLEN.  
  
### <a name="sqlsoptssparamfocus"></a>SQL_SOPT_SS_PARAM_FOCUS  
 L’attribut SQL_SOPT_SS_PARAM_FOCUS Spécifie le focus pour SQLBindParameter suivante, SQLGetDescField, SQLSetDescField, SQLGetDescRec et SQLSetDescRec appelle.  
  
 Le type de SQL_SOPT_SS_PARAM_FOCUS est SQLULEN.  
  
 La valeur par défaut est 0, ce qui signifie que ces appels adressent les paramètres qui correspondent aux marqueurs de paramètre dans l'instruction SQL. En cas de définition sur le numéro de paramètre d'un paramètre table, ces appels adressent les colonnes de ce paramètre table. En cas de définition sur une valeur qui n'est pas le numéro de paramètre d'un paramètre table, ces appels retournent l'erreur IM020 : « Le focus de paramètre ne fait pas référence à un paramètre table ».  
  
### <a name="sqlsoptssnamescope"></a>SQL_SOPT_SS_NAME_SCOPE  
 L'attribut SQL_SOPT_SS_NAME_SCOPE spécifie la portée de nom des appels de fonctions de catalogue suivants. Le jeu de résultats retourné par SQLColumns dépend de la valeur de SQL_SOPT_SS_NAME_SCOPE.  
  
 Le type de SQL_SOPT_SS_NAME_SCOPE est SQLULEN.  
  
|*ValuePtr* valeur| Description|  
|----------------------|-----------------|  
|SQL_SS_NAME_SCOPE_TABLE|Valeur par défaut.<br /><br /> Lors de l'utilisation de paramètres table, indique que les métadonnées des tables réelles doivent être retournées.<br /><br /> Lorsque vous utilisez la fonctionnalité des colonnes éparses, SQLColumns retourne uniquement les colonnes qui ne sont pas membres d’éparse **column_set**.|  
|SQL_SS_NAME_SCOPE_TABLE_TYPE|Indique que l'application requiert les métadonnées pour un type de table, plutôt qu'une table réelle (les fonctions de catalogue doivent retourner les métadonnées pour les types de table). L’application transmet ensuite le TYPE_NAME du paramètre table incluse en tant que le *TableName* paramètre.|  
|SQL_SS_NAME_SCOPE_EXTENDED|Lorsque vous utilisez la fonctionnalité des colonnes éparses, SQLColumns retourne toutes les colonnes, indépendamment du **column_set** l’appartenance.|  
|SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET|Lorsque vous utilisez la fonctionnalité des colonnes éparses, SQLColumns retourne uniquement les colonnes qui sont membres d’éparse **column_set**.|  
|SQL_SS_NAME_SCOPE_DEFAULT|Identique à SQL_SS_NAME_SCOPE_TABLE.|  
  
 SS_TYPE_CATALOG_NAME et SS_TYPE_SCHEMA_NAME sont utilisés avec la *CatalogName* et *SchemaName* paramètres, respectivement, pour identifier le catalogue et le schéma pour le paramètre table. Quand une application a fini d'extraire les métadonnées des paramètres table, elle doit redéfinir SQL_SOPT_SS_NAME_SCOPE avec sa valeur par défaut SQL_SS_NAME_SCOPE_TABLE.  
  
 Lorsque SQL_SOPT_SS_NAME_SCOPE est défini avec la valeur SQL_SS_NAME_SCOPE_TABLE, les requêtes adressées aux serveurs liés échouent. Les appels à SQLColumns ou SQLPrimaryKeys avec un catalogue qui contient un composant serveur échoue.  
  
 Si vous essayez de définir SQL_SOPT_SS_NAME_SCOPE avec une valeur non valide, SQL_ERROR est retourné et un enregistrement de diagnostic est généré avec SQLSTATE HY024 et le message « Valeur d'attribut non valide ».  
  
 Si une fonction de catalogue autre SQLTables, SQLColumns ou SQLPrimaryKeys est appelée lorsque SQL_SOPT_SS_NAME_SCOPE a une valeur autre que SQL_SS_NAME_SCOPE_TABLE, SQL_ERROR est retourné. Un enregistrement de diagnostic est généré avec SQLSTATE HY010 et le message « Erreur de séquence de fonction (SQL_SOPT_SS_NAME_SCOPE n'est pas défini avec la valeur SQL_SS_NAME_SCOPE_TABLE) ».  
  
## <a name="see-also"></a>Voir aussi  
 [SQLGetStmtAttr, fonction](http://go.microsoft.com/fwlink/?LinkId=59355)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
