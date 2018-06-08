---
title: À l’aide de SQLConfigDatasource avec le pilote ODBC pour Oracle | Documents Microsoft
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
- SQLConfigDataSource function [ODBC], ODBC driver for Oracle
ms.assetid: e535d1ef-aff9-4ae7-a3ed-ef4ca2584289
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5859b6cc446e74c12b2fb09436246d693a3beea3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-sqlconfigdatasource-with-the-odbc-driver-for-oracle"></a>À l’aide de SQLConfigDatasource avec le pilote ODBC pour Oracle
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Le tableau suivant répertorie valide **SQLConfigDatasource** paramètres pour le pilote Microsoft ODBC pour Oracle, version 1.0 (Msorcl10.dll) et le pilote Microsoft ODBC pour Oracle, version 2.0 (Msorcl32.dll).  
  
> [!NOTE]  
>  Le pilote Msorcl10.dll (version 1.0) prend en charge tous les paramètres sauf **Server**. Le pilote Msorcl32.dll (version 2.0 et ultérieure) prend en charge tous les paramètres.  
  
 Certains paramètres sont ignorés par le pilote, mais sont acceptées par **SQLConfigDatasource**. Inclure ces paramètres dans la chaîne de connexion ODBC est la seule façon, qu'elles seront acceptés au moment de l’exécution. Un paramètre ignoré ne sera pas stocké dans le Registre lorsque **SQLConfigDatasource** crée la source de données.  
  
 Dans le tableau suivant, *A/N* signifie toute chaîne alphanumérique valide jusqu'à la longueur maximale autorisée. *Max Len* (longueur maximale) est la longueur maximale autorisée de la chaîne acceptée par le paramètre, y compris le caractère terminateur de chaîne.  
  
|Paramètre|Max Len|Valeur par défaut|Valeurs valides| Description|  
|-------------|-------------|-------------------|------------------|-----------------|  
|BufferSize|7|65535|1000|Jusqu'à 65 535 octets de taille de mémoire tampon d’extraction minimale|  
|CatalogCap|2|1|0 ou 1|Si 1, les identificateurs nonquoted sera converti en majuscules dans le catalogue des fonctions.|  
|ConnectString|128|""|A/N|Chaîne de connexion. Méthode nécessaire de spécifier le nom du serveur avec le pilote Msorcl10.dll.|  
| Description|256|""|A/N|Description.|  
|DSN|33|""|A/N|Nom de source de données.|  
|GuessTheColDef|4|0|A/N|Retourne une valeur différente de zéro pour les colonnes sans échelle définie par Oracle.|  
|NumberFloat|2|""|0 ou 1|Si 0, les colonnes de type FLOAT sont traitées comme SQL_FLOAT. La valeur 1, les colonnes de type FLOAT sont traitées comme SQL_DOUBLE.|  
|PWD|30|""|A/N|Mot de passe.|  
|RDOSupport|2|""|0 ou 1|Permet de RDO appeler des procédures d’Oracle.|  
|Notes|2|0|0 ou 1|Inclure des notes dans les fonctions de catalogue.|  
|RowLimit|4|""|comprise entre 0 et 99|Nombre maximal de lignes retournées par une instruction SELECT. Une chaîne de longueur zéro indique qu’aucune limite n’est appliquée.|  
|Server|128|""|A/N|Nom du serveur Oracle.|  
|SynonymColumns|2|1|0 ou 1|Inclure les synonymes dans SQLColumns.|  
|SystemTable|2|""|0 ou 1|Si 0, les tables système ne s’affichera pas. La valeur 1, les tables système seront affichera.|  
|TranslationDLL|33|""|A/N|Nom du fichier .dll de traduction.|  
|TranslationName|33|""|A/N|Nom de la traduction.|  
|TranslationOption|33|""|A/N|Option de traduction.|  
|TxnCap|2|""|A/N|Compatible avec la transaction. Si 0, le pilote signale qu’il ne prend pas en charge les transactions. La valeur 1, le pilote signale qu’il est capable d’effectuer des transactions.|  
|UID|30|""|A/N|Nom d’utilisateur.|
