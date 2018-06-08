---
title: Fonctions d’API de niveau 1 (le pilote ODBC pour Oracle) | Documents Microsoft
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
- functions [ODBC], ODBC driver for Oracle
- ODBC level 1 API functions [ODBC]
- ODBC driver for Oracle [ODBC], functions
- level 1 API functions [ODBC]
- API functions [ODBC]
ms.assetid: 98cced6f-41b8-43c1-a3cd-f4ea1615c0af
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e4329f1dc0a22c9bcea27df908bb0ce567fdca7c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="level-1-api-functions-odbc-driver-for-oracle"></a>Fonctions d’API de niveau 1 (le pilote ODBC pour Oracle)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Prend en charge les fonctions à cette fournir au niveau de conformité d’interface de base ainsi que des fonctionnalités supplémentaires telles que des transactions.  
  
|Fonction d’API|Remarques|  
|------------------|-----------|  
|**SQLColumns**|Crée un jeu de résultats pour une table, qui est la liste des colonnes pour la table spécifiée ou tables. Lorsque vous demandez des colonnes pour un synonyme PUBLIC, doit avoir l’attribut de connexion SYNONYMCOLUMNS et spécifié une chaîne vide comme le *szTableOwner* argument. Lors du renvoi des colonnes pour les synonymes PUBLIC, le pilote définit la colonne nom de la TABLE à une chaîne vide. Le jeu de résultats contient une colonne supplémentaire, ORDINAL POSITION, à la fin de chaque ligne. Cette valeur est la position ordinale de la colonne dans la table.|  
|**SQLDriverConnect**|Se connecte à une source de données existante. Pour plus d’informations, consultez [Format de chaîne de connexion et les attributs](../../odbc/microsoft/connection-string-format-and-attributes.md).|  
|**SQLGetConnectOption**|Retourne la valeur actuelle de l’option de connexion. Cette fonction est partiellement pris en charge. Le pilote prend en charge toutes les valeurs pour le *fOption* argument, mais ne prend ne pas en charge certaines *vParam* les valeurs pour le *fOption* argument [SQL_TXN_ISOLATION](../../odbc/microsoft/connect-options.md). Pour plus d’informations, consultez [connecter des Options](../../odbc/microsoft/connect-options.md).|  
|**SQLGetData**|Récupère la valeur d’un champ dans l’enregistrement actif du jeu de résultats donné.|  
|**SQLGetFunctions**|Renvoie la valeur TRUE pour toutes les fonctions prises en charge. Implémenté par le Gestionnaire de pilotes.|  
|**SQLGetInfo**|Retourne des informations, y compris SQLHDBC, SQLUSMALLINT, SQLPOINTER, SQLSMALLINT et SQLSMALLINT \*, sur le pilote ODBC pour Oracle et source de données associée à un handle de connexion, *pas*.|  
|**SQLGetStmtOption**|Retourne le paramètre actuel d’une option de l’instruction. Pour plus d’informations, consultez [Options d’instruction](../../odbc/microsoft/statement-options.md).|  
|**SQLGetTypeInfo**|Retourne des informations sur les types de données pris en charge par une source de données. Le pilote retourne les informations dans un jeu de résultats SQL.|  
|**SQLParamData**|Utilisée conjointement avec **SQLPutData** pour spécifier les données de paramètre au moment de l’exécution d’instruction.|  
|**SQLPutData**|Permet à une application envoyer des données d’un paramètre ou une colonne pour le pilote au moment de l’exécution d’instruction.|  
|**SQLSetConnectOption**|Fournit l’accès aux options qui régissent les aspects de la connexion. Cette fonction est partiellement disponible : le pilote prend en charge toutes les valeurs pour le *fOption* argument, mais ne prend ne pas en charge certaines *vParam* les valeurs pour le *fOption* argument [SQL_TXN_ISOLATION](../../odbc/microsoft/connect-options.md). Pour plus d’informations, consultez [connecter des Options](../../odbc/microsoft/connect-options.md).|  
|**SQLSetStmtOption**|Définit les options relatives à un descripteur d’instruction, *hstmt*. Pour plus d’informations, consultez [Options d’instruction](../../odbc/microsoft/statement-options.md).|  
|**SQLSpecialColumns**|Récupère le jeu optimal de colonnes qui identifie de façon unique une ligne dans la table.|  
|**SQLStatistics**|Récupère une liste des statistiques sur une seule table et les index ou les noms de balise associés à la table. Le pilote retourne les informations comme jeu de résultats.|  
|**SQLTables**|Retourne la liste des noms de table spécifié par le paramètre dans le **SQLTables** instruction. Si aucun paramètre n’est spécifié, retourne les noms de table stockées dans la source de données actuelle. Le pilote retourne les informations comme jeu de résultats.<br /><br /> Appels de type énumération ne reçoivent pas une entrée de jeu de résultats pour des vues paramétrées locales ou distant. Toutefois, un appel à **SQLTables** avec une table unique spécificateur de nom trouver une correspondance pour cette vue, le cas échéant, portant ce nom ; Cela permet à l’API de recherche de conflits de nom avant la création d’une nouvelle table.<br /><br /> Synonymes publiques sont retournées avec la valeur TABLE_OWNER « ».<br /><br /> VUES appartenant à SYS ou système sont identifiés en tant que vue système.|
