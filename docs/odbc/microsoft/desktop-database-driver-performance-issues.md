---
title: Problèmes de performances du pilote bureau de base de données | Documents Microsoft
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
- ODBC desktop database drivers [ODBC], performance
- desktop database drivers [ODBC], performance
- Jet-based ODBC drivers [ODBC], performance
ms.assetid: 1a4c4b7e-9744-411f-9b6e-06dfdad92cf7
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 73261681421546dcec4cd37d73b1cdf55968038e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="desktop-database-driver-performance-issues"></a>Problèmes de performances du pilote bureau de base de données
Pour garantir la compatibilité avec les applications existantes ANSI, les types de données SQL_WCHAR, SQL_WVARCHAR et SQL_WLONGVARCHAR exposées en tant que SQL_CHAR, SQL_VARCHAR et SQL_LONGVARCHAR pour Microsoft Access 4.0 ou supérieure sources de données. Les sources de données ne retournent pas de types de données CHAR large, mais les données toujours doivent être envoyées à Jet sous forme de Char large. Il est important de comprendre que conversion a lieu si une colonne de paramètre ou le résultat SQL_C_CHAR est liée à un type de données SQL_CHAR dans une application ANSI.  
  
 Cette conversion peut être particulièrement inefficace en termes de mémoire lorsqu’un type SQL_C_CHAR est lié à un paramètre de type LONGVARCHAR. Étant donné que le moteur Jet 4.0 ne parvient pas à un flux de données du paramètre LONGTEXT, une mémoire tampon de conversion UNICODE qui est deux fois la taille de la mémoire tampon SQL_C_CHAR ANSI doit allouée. Le moyen le plus efficace est de l’application effectuer la conversion d’UNICODE et de lier le paramètre en tant que type SQL_C_WCHAR. Lorsqu’un paramètre est marqué en tant que données d’exécution et les données sont fournies dans plusieurs appels SQLPutData, un tampon de données longtext s’accroît de manière. Pour éviter le coût de cette croissance « Put » mémoire tampon consiste à fournir une longueur facultatif via SQL_DATA_AT_EXEC_LEN(x), où *x* est la longueur attendue d’octets. La taille d’un tampon interne de PlacerDonnées sera initialisé *x* octets.  
  
> [!NOTE]  
>  Un moyen efficace pour insérer ou mettre à jour des données de type long peut être effectué à l’aide de **SQLBulkOperations()** ou **SQLSetPos()** et la définition de données de type long à SQL_DATA_AT_EXEC. (EXEC_LEN est ignoré dans ce cas). Données peuvent être diffusées en segments en appelant **SQLPutData** plusieurs fois, ce qui en réalité ajoutera les données à la table.  
  
 Lorsqu’une application à l’aide d’une base de données Jet 3.5 via les pilotes de base de données Microsoft ODBC Desktop est mis à niveau vers la version 4.0, une baisse des performances et une taille de jeu de travail accrue peuvent se produire. C’est parce que lorsqu’une version 3. *x* base de données est ouverte à l’aide de la version 4.0 du pilote nouveau, il charge Jet 4.0. Lorsque Jet 4.0 s’ouvre à la base de données et constate que la base de données est un 3. *x* version, il charge un pilote ISAM Installable qui équivaut au chargement également le moteur Jet 3.5. Pour supprimer la pénalité de performances et la taille, la version 3 Jet. *x* base de données doit être compactée dans une base de données de format Jet 4.0. Cela élimine le chargement de deux moteurs Jet et réduire le chemin d’accès du code pour les données.  
  
 En outre, le moteur Jet 4.0 est un moteur de Unicode. Toutes les chaînes sont stockées et manipulées en Unicode. Lorsqu’une application ANSI accède à un Jet 3. *x* par le moteur Jet 4.0, les données de base de données est converti d’ANSI en Unicode et en ANSI. Si la base de données est mis à jour vers la version 4.0, les chaînes sont converties en Unicode, la suppression d’un niveau de conversion de chaîne ainsi que des minimiser le chemin d’accès du code aux données par l’intermédiaire d’un seul moteur Jet.
