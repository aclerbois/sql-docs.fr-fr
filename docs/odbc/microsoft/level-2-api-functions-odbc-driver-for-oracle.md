---
title: Fonctions d’API de niveau 2 (le pilote ODBC pour Oracle) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ODBC level 2 API functions [ODBC]
- functions [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], functions
- ODBC API functions [ODBC]
- API functions [ODBC]
- level 2 API functions [ODBC]
ms.assetid: d9f49520-72d7-4234-8635-260d0ce4199c
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a3209e07c2601de33b9d83a422ca8f986fb9326b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="level-2-api-functions-odbc-driver-for-oracle"></a>Fonctions d’API de niveau 2 (le pilote ODBC pour Oracle)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Fonctions à ce niveau fournissent la conformité d’interface de niveau 1 ainsi que des fonctionnalités supplémentaires telles que la prise en charge les signets, les paramètres dynamiques et exécution asynchrone de fonctions ODBC.  
  
|Fonction d’API|Remarques|  
|------------------|-----------|  
|**SQLBindParameter**|Associe une mémoire tampon à un marqueur de paramètre dans une instruction SQL.|  
|**SQLBrowseConnect**|Retourne les niveaux successifs d’attributs et valeurs d’attribut.|  
|**SQLDataSources**|Répertorie les noms de source de données. Implémenté par le Gestionnaire de pilotes.|  
|**SQLDescribeParam**|Retourne la description d’un marqueur de paramètre associé à une instruction SQL préparée.<br /><br /> Retourne une estimation de quel paramètre est, en fonction de l’analyse de l’instruction. Si le type de paramètre ne peut pas être déterminé, SQL_VARCHAR retourne avec la longueur (2000).|  
|**SQLDrivers**|Implémenté par le Gestionnaire de pilotes.|  
|**SQLExtendedFetch**|Semblable à **SQLFetch** mais retourne plusieurs lignes à l’aide d’un tableau pour chaque colonne. Le jeu de résultats avant-autorise le défilement et peut être effectué descendante déroulable si le curseur est défini comme étant statique, pas de type avant uniquement. Pour les curseurs avant uniquement avec la liaison de colonne par défaut, les données de colonne à partir de jeux de données supérieure à l’attribut de connexion BUFFERSIZE sont extraites directement dans les mémoires tampons de données. Ne prend pas en charge les signets de longueur variable et ne prend pas en charge l’extraction d’un ensemble de lignes à un offset (autre que 0) à partir d’un signet.|  
|**SQLForeignKeys**|Retourne une liste de clés étrangères dans une seule table ou une liste de clés étrangères dans d’autres tables qui font référence à une table unique.|  
|**SQLMoreResults**|Détermine si plusieurs résultats sont en attente sur un descripteur d’instruction, hstmt, qui contient des instructions SELECT, UPDATE, INSERT ou DELETE et si tel est le cas, initialise le traitement de ces résultats.<br /><br /> Oracle prend en charge plusieurs jeux de résultats uniquement à partir de procédures stockées, lorsque vous utilisez les séquences d’échappement {resultset...}.|  
|**SQLNativeSql**|Pour plus d’informations sur l’utilisation, consultez [retournant des paramètres de tableau à partir de procédures stockées](../../odbc/microsoft/returning-array-parameters-from-stored-procedures.md).|  
|**SQLNumParams**|Retourne le nombre de paramètres dans une instruction SQL. Le nombre de paramètres doit égal au nombre de points d’interrogation dans l’instruction SQL transmise à **SQLPrepare**.|  
|**SQLPrimaryKeys**|Retourne les noms de colonnes qui composent la clé primaire pour une table.|  
|**SQLProcedureColumns**|Retourne une liste de paramètres d’entrée et sortie, la valeur de retour, les colonnes du jeu de résultats d’une procédure unique et deux colonnes supplémentaires, la surcharge et la position ordinale. La surcharge est la colonne de la surcharge de la table ALL_ARGUMENTS de la vue de dictionnaire de données Oracle. Position ordinale est la colonne de séquence de la table ALL_ARGUMENTS de la vue de dictionnaire de données Oracle. Pour les procédures de packages, la colonne nom de la procédure est en *packagename.procedurename* format. Ne retourne pas les colonnes de la procédure d’un synonyme créé qui fait référence à une procédure ou fonction.|  
|**SQLProcedures**|Retourne une liste des procédures dans la source de données. Pour les procédures de packages, la colonne nom de la procédure est en *packagename.procedurename* format.<br /><br /> Étant donné que Oracle ne fournit pas de permet de distinguer les procédures de packages à partir de fonctions empaquetées, le pilote retourne SQL_PT_UNKNOWN pour la colonne PROCEDURE_TYPE.|  
|**SQLSetPos**|Définit la position du curseur dans un ensemble de lignes. Vous pouvez utiliser **SQLSetPos** avec **SQLGetData** pour extraire des lignes à partir des colonnes indépendantes après le curseur sur une ligne spécifique dans l’ensemble de lignes. Lignes ajoutées dans le jeu de résultats *fOption* SQL_ADD sont ajoutés après la dernière ligne du jeu de résultats.|  
|**SQLSetScrollOptions**|Définit les options qui contrôlent le comportement des curseurs associé à un descripteur d’instruction, hstmt. Pour plus d’informations, consultez [Type de curseur et les combinaisons d’accès concurrentiel](../../odbc/microsoft/cursor-type-and-concurrency-combinations.md).|
