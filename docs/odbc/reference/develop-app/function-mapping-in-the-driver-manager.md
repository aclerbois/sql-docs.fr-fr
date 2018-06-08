---
title: Fonction de mappage dans le Gestionnaire de pilotes | Documents Microsoft
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
- Unicode [ODBC], functions
- driver manager [ODBC], function mapping
- functions [ODBC], Unicode functions
ms.assetid: ff093b29-671a-4fc0-86c9-08a311a98e54
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ee2e5d3e770ddd605618f6a71161f410dab7ab52
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="function-mapping-in-the-driver-manager"></a>Mappage de fonction dans le Gestionnaire de pilotes
Le Gestionnaire de pilote prend en charge les deux points d’entrée pour les fonctions qui acceptent des arguments de chaîne. La fonction non décorée (**SQLDriverConnect**) est au format ANSI de la fonction. Le format Unicode est décorée avec un *W* (**SQLDriverConnectW**.)  
  
 Le fichier d’en-tête ODBC prend également en charge les fonctions décorées avec un *A* (**SQLDriverConnectA**) pour la commodité d’applications de ANSI/Unicode mixtes. Les appels effectués à la **A** fonctions sont en fait des appels dans le point d’entrée non décoré (**SQLDriverConnect**.)  
  
 Si l’application est compilée avec le commutateur _UNICODE **#define**, le fichier d’en-tête ODBC mappera les appels de fonction non décoré (**SQLDriverConnect**) vers la version Unicode (**SQLDriverConnectW**.)  
  
 Le Gestionnaire de pilotes reconnaît un pilote en tant que Unicode pilote si **SQLConnectW** est pris en charge par le pilote.  
  
 Si le pilote est Unicode, le Gestionnaire de pilotes effectue des appels de fonction comme suit :  
  
-   Passe une fonction sans arguments de type chaîne ou des paramètres directement par le biais du pilote.  
  
-   Passe des fonctions Unicode (avec la *W* suffixe) directement par le biais du pilote.  
  
-   Convertit une fonction ANSI (avec la *A* suffixe) à une fonction Unicode (avec la *W* suffixe) en convertissant les arguments de chaîne Unicode des caractères et transmet la fonction Unicode au pilote.  
  
 Si le pilote est un pilote ANSI, le Gestionnaire de pilotes effectue des appels de fonction comme suit :  
  
-   Passe fonctions sans arguments de type chaîne ou de paramètres directement à l’aide du pilote.  
  
-   Convertit des fonctions Unicode (avec la *W* suffixe) à ANSI appel de fonction et le transmet au pilote.  
  
-   Passe une fonction ANSI directement dans le pilote.  
  
 Le Gestionnaire de pilote prend en charge Unicode en interne. Par conséquent, les performances optimales sont obtenu par une application Unicode opère avec un pilote Unicode, car le Gestionnaire de pilotes transmet simplement fonctions Unicode via le pilote. Lorsqu’une application ANSI fonctionne avec un pilote ANSI, le Gestionnaire de pilotes doit convertir des chaînes d’ANSI en Unicode lors du traitement de certaines fonctions, telles que **SQLDriverConnect**. Après le traitement de la fonction, le Gestionnaire de pilotes doit puis convertir la chaîne Unicode en ANSI avant l’envoi de la fonction au pilote ANSI.  
  
 Une application ne doit pas modifier ou lire ses tampons de paramètres liés, lorsque le pilote retourne SQL_NEED_DATA ou SQL_STILL_EXECUTING. Le Gestionnaire de pilotes laisse les mémoires tampons liés à la norme ANSI jusqu'à ce que le pilote retourne SQL_SUCCESS, SQL_SUCCESS_WITH_INFO ou SQL_ERROR. Une application multithread ne doit pas accéder à toutes les valeurs de paramètre lié un autre thread s’exécute une instruction SQL. Le Gestionnaire de pilote convertit les données Unicode en ANSI « sur place », et l’autre thread peut voir les données ANSI dans ces mémoires tampons pendant que le pilote traite toujours l’instruction SQL. Applications qui lient des données Unicode à un pilote ANSI ne doivent pas lier deux colonnes différentes à la même adresse.
