---
title: Utilise des données de catalogue | Documents Microsoft
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
- catalog data [ODBC]
- functions [ODBC], catalog functions
- catalog functions [ODBC], using catalog data
ms.assetid: d5915d0c-eec3-4382-850e-bd863763c99a
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b4d510a0f7e0d0c401e0f8ea8a8c346a917f05c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="uses-of-catalog-data"></a>Utilise des données de catalogue
Applications utilisent les données du catalogue de plusieurs façons. Voici quelques utilisations courantes :  
  
-   **Construction d’instructions SQL en cours d’exécution.** Les applications verticales, comme une application de saisie de commandes, contiennent codé en dur des instructions SQL. Les tables et les colonnes qui sont utilisées par l’application sont résolus en avance, comme les instructions qui accèdent à ces tables. Par exemple, une application de saisie de commandes contient généralement un seul paramétrable **insérer** instruction pour l’ajout de nouvelles commandes au système.  
  
     Souvent, les applications génériques, tel qu’un programme de feuille de calcul qui utilise ODBC pour récupérer les données, construire des instructions SQL en cours d’exécution en fonction de l’entrée de l’utilisateur. Une telle application peut nécessiter l’utilisateur à taper les noms des tables et colonnes à utiliser. Toutefois, il est plus facile pour l’utilisateur si l’application affiche des listes de tables et colonnes à partir de laquelle l’utilisateur peut effectuer des sélections. Pour générer ces listes, l’application appelle la **SQLTables** et **SQLColumns** fonctions de catalogue.  
  
-   **Construction d’instructions SQL au cours du développement.** En règle générale, les environnements de développement permettant au programmeur de créer des requêtes de base de données lors du développement d’un programme. Les requêtes sont ensuite codé en dur dans l’application en cours de génération.  
  
     Ces environnements peuvent également utiliser **SQLTables** et **SQLColumns** pour créer des listes à partir de laquelle le programmeur peut effectuer des sélections. Ces environnements peuvent également utiliser **SQLPrimaryKeys** et **SQLForeignKeys** à déterminer automatiquement et afficher les relations entre les tables sélectionnées et utilisez **SQLStatistics** pour déterminer et mettre en surbrillance les champs indexés afin que le programmeur puisse créer des requêtes efficaces.  
  
-   **Les curseurs à construire.** Une application, un pilote ou un intergiciel (middleware) qui fournit un moteur de curseur de défilement utilise **SQLSpecialColumns** pour déterminer l’ou les colonnes qui identifient une ligne. Le programme peut générer un *keyset* contenant les valeurs de ces colonnes pour chaque ligne qui a été extrait. Lorsque l’application fait défiler vers la ligne, il est ensuite utiliser ces valeurs pour extraire les données les plus récentes pour la ligne. Pour plus d’informations sur les curseurs de défilement et des jeux de clés, consultez [curseurs permettant le défilement](../../../odbc/reference/develop-app/scrollable-cursors.md).
