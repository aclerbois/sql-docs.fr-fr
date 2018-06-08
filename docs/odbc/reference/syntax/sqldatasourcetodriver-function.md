---
title: Fonction de SQLDataSourceToDriver | Documents Microsoft
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
- SQLDataSourceToDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSourceToDriver
helpviewer_keywords:
- SQLDataSourceToDriver function [ODBC]
ms.assetid: 0d87fcac-30a0-4303-ad8f-a5b53f4b428d
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 742375c2ce4bfc9813c50d0584fe024e383ffd79
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqldatasourcetodriver-function"></a>SQLDataSourceToDriver (fonction)
**SQLDataSourceToDriver** supportstranslations pour les pilotes ODBC. Cette fonction n’est pas appelée par les applications ODBC compatibles ; applications de demander une traduction via **SQLSetConnectAttr**. Le pilote associé à la *handle de connexion* spécifié dans **SQLSetConnectAttr** appelle la DLL spécifiée pour effectuer des conversions de toutes les données circulent à partir de la source de données pour le pilote. Une DLL de traduction par défaut peut être spécifié dans le fichier d’initialisation ODBC.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL SQLDataSourceToDriver(  
     UDWORD     fOption,  
     SWORD      fSqlType,  
     PTR        rgbValueIn,  
     SDWORD     cbValueIn,  
     PTR        rgbValueOut,  
     SDWORD     cbValueOutMax,  
     SDWORD *   pcbValueOut,  
     UCHAR *    szErrorMsg,  
     SWORD      cbErrorMsgMax,  
     SWORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>Arguments  
 *fOption*  
 [Entrée] Valeur de l’option.  
  
 *fSqlType*  
 [Entrée] Le type de données SQL. Cet argument indique au pilote la conversion *rgbValueIn* dans un formulaire acceptable par l’application. Pour obtenir la liste des types de données SQL valides, consultez la [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) section annexe d : Types de données.  
  
 *rgbValueIn*  
 [Entrée] Valeur à convertir.  
  
 *cbValueIn*  
 [Entrée] Longueur de *rgbValueIn*.  
  
 *rgbValueOut*  
 [Sortie] Résultat de la traduction.  
  
> [!NOTE]  
>  La DLL de traduction n’arrête pas null cette valeur.  
  
 *cbValueOutMax*  
 [Entrée] Longueur de *rgbValueOut*.  
  
 *pcbValueOut*  
 [Sortie] Le nombre total d’octets (à l’exception de l’octet de valeur null) disponibles à renvoyer dans *rgbValueOut*.  
  
 Pour un caractère ou binaire, si cela est supérieur ou égal à *cbValueOutMax*, les données de *rgbValueOut* est tronqué à *cbValueOutMax* octets.  
  
 Pour tous les autres types de données, la valeur de *cbValueOutMax* est ignorée et la DLL de conversion suppose que la taille de *rgbValueOut* est la taille du type de données C par défaut du type de données SQL spécifié avec *fSqlType*.  
  
 Le *pcbValueOut* argument peut être un pointeur null.  
  
 *szErrorMsg*  
 [Sortie] Pointeur vers le stockage pour un message d’erreur. Il s’agit d’une chaîne vide, sauf si la conversion a échoué.  
  
 *cbErrorMsgMax*  
 [Entrée] Longueur de *szErrorMsg*.  
  
 *pcbErrorMsg*  
 [Sortie] Pointeur vers le nombre total d’octets (à l’exception de l’octet de valeur null) disponibles à renvoyer dans *szErrorMsg*. Si cela est supérieur ou égal à *cbErrorMsg*, les données de *szErrorMsg* est tronqué à *cbErrorMsgMax* moins le caractère de fin de la valeur null. Le *pcbErrorMsg* argument peut être un pointeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 TRUE si la conversion a réussi, FALSE en cas d’échec de la traduction.  
  
## <a name="comments"></a>Commentaires  
 Le pilote appelle **SQLDataSourceToDriver** traduire alldata (données de jeu de résultats, les noms de tables, des nombres de lignes, les messages d’erreur et ainsi de suite) de transmission à partir des données source au pilote. La DLL de conversion ne peut pas convertir certaines données, selon le type et l’objectif de la traduction de DLL ; par exemple, une DLL qui traduit les données de caractères à partir d’une page de codes à l’autre ignore toutes les données numériques et binaire.  
  
 La valeur de *fOption* est définie à la valeur de *vParam* spécifiée en appelant **SQLSetConnectAttr** avec l’attribut SQL_ATTR_TRANSLATE_OPTION. C’est une valeur 32 bits qui a une signification particulière pour une DLL de traduction donnée. Par exemple, il peut spécifier qu'un certain jeu de caractères traduction.  
  
 Si la même mémoire tampon est spécifiée pour *rgbValueIn* et *rgbValueOut*, la conversion des données dans la mémoire tampon sera effectuée en place.  
  
 Bien que *cbValueIn*, *cbValueOutMax*, et *pcbValueOut* sont du type des éléments SDWORD, **SQLDataSourceToDriver** ne prend en charge n’est pas nécessairement des pointeurs énormes.  
  
 Si **SQLDataSourceToDriver** renvoie la valeur FALSE, une troncation de données se sont produites lors de la traduction. Si *pcbValueOut* (le nombre d’octets disponibles à renvoyer dans le tampon de sortie) est supérieur à *cbValueOutMax* (la longueur de la mémoire tampon de sortie), puis de la troncation s’est produite. Le pilote doit déterminer si la troncation acceptable. Si la troncation ne prenaient pas le **SQLDataSourceToDriver** retourné FALSE en raison d’une autre erreur. Dans les deux cas, un message d’erreur spécifique est renvoyé dans *szErrorMsg*.  
  
 Pour plus d’informations sur la conversion de données, consultez [DLL de traduction](../../../odbc/reference/develop-app/translation-dlls.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Conversion des données envoyées à la source de données|[SQLDriverToDataSource](../../../odbc/reference/syntax/sqldrivertodatasource-function.md)|  
|Retourner la valeur de l’attribut de connexion|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Définition d’un attribut de connexion|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
