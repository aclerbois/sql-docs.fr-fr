---
title: Les Applications Unicode | Documents Microsoft
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
- Unicode [ODBC], compiling as Unicode application
- Unicode [ODBC], functions
- compiling Unicode applications [ODBC]
- functions [ODBC], Unicode functions
ms.assetid: 7986c623-2792-4e77-bfee-c86cbf84f08d
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5ff0858bbfe88ecc9f49306af2426d88a1db072f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unicode-applications"></a>Applications Unicode
Vous pouvez recompiler une application comme une application Unicode de deux manières :  
  
-   Inclure les Unicode **#define** contenus dans le fichier d’en-tête Sqlucode.h dans l’application.  
  
-   Compilez l’application avec l’option du compilateur Unicode. (Cette option sera différente pour différents compilateurs.)  
  
 Pour convertir une application ANSI vers une application Unicode, écrire l’application pour stocker et transmettre les données Unicode. En outre, les appels aux fonctions qui prennent en charge les arguments SQLPOINTER doivent être convertis pour utiliser le nombre d’octets.  
  
 Après la compilation d’une application comme une application Unicode, si l’application appelle une fonction d’API ODBC (sans suffixe), le Gestionnaire de pilotes reconnaît l’application comme une application Unicode et convertit l’appel de fonction à une fonction Unicode (avec la *W* suffixe) si le pilote sous-jacent prend en charge Unicode. Lorsqu’une application ANSI appelle une fonction sans un suffixe, le Gestionnaire de pilotes le convertit au format ANSI si le pilote sous-jacent prend en charge ANSI. Si l’application et le pilote prend en charge de l’encodage de caractères même, le Gestionnaire de pilote passe les appels par le biais du pilote (avec certaines exceptions pour les applications ANSI).  
  
 Une application peut appeler les deux fonctions Unicode (avec la *W* suffixe) et les fonctions ANSI (avec ou sans le *A* suffixe). Unicode et ANSI les appels de fonction peuvent être mélangés. Si la bibliothèque de curseurs doit être utilisé, toutefois, les appels de fonction Unicode et ANSI ne peuvent pas être mélangés. La bibliothèque de curseurs est au format Unicode ou ANSI, pas un mélange.  
  
 Une application peut être écrite tel qu’il peut être compilé en tant qu’une application Unicode ou d’une application ANSI. Dans ce cas, les types de données de caractères peuvent être déclarés en tant que SQL_C_TCHAR. Il s’agit d’une macro qui insère SQL_C_WCHAR si l’application est compilée dans une application Unicode ou insère SQL_C_CHAR s’il est compilé en tant qu’une application ANSI. Le programmeur doit être prudent de fonctions qui acceptent SQLPOINTER comme argument, car la taille de l’argument de longueur changera (pour les types de données de chaîne) selon que l’application est ANSI ou Unicode.  
  
 Une fonction peut être appelée de trois manières : en tant qu’un appel de fonction Unicode uniquement (avec la *W* suffixe), comme un appel de fonction ANSI uniquement (avec la *A* suffixe), ou en tant que l’appel de fonction ODBC sans suffixe. Les arguments pour les trois formes d’une fonction sont identiques. Seules les fonctions avec SQLCHAR \* arguments ou des arguments SQLPOINTER qui pointent vers les chaînes nécessitent des formulaires Unicode et ANSI. Pour les fonctions qui possèdent des arguments qui peuvent être déclarés comme type de caractère, tel que **SQLBindCol** ou **SQLGetData** (qui n’ont pas les formulaires Unicode et ANSI), l’argument peut être déclaré comme type Unicode, le type ANSI, ou dans le cas d’un argument de type C, la macro SQL_C_TCHAR. Pour plus d’informations, consultez [les données Unicode](../../../odbc/reference/develop-app/unicode-data.md).  
  
 Une application peut être écrite comme une application Unicode, même si aucun pilote Unicode n’est disponibles pour qu’il fonctionne avec. Le Gestionnaire de pilotes mappera les types de fonctions et les données Unicode en ANSI. Il existe certaines restrictions à Unicode aux mappages ANSI qui peuvent être effectuées. L’existence d’un pilote Unicode pour l’application Unicode pour travailler avec entraîne de meilleures performances et supprime les restrictions liées aux Unicode aux mappages d’ANSI.
