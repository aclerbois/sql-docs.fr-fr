---
title: Codes de retour ODBC | Documents Microsoft
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
- return codes [ODBC]
- diagnostic information [ODBC], return codes
ms.assetid: e893b719-4392-476f-911a-5ed6da6f7e94
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 12751f87c9f9832567dc04ba7df7659e80e66897
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="return-codes-odbc"></a>Codes de retour ODBC
Chaque fonction dans ODBC retourne un code appelé son *code de retour,* qui indique la réussite ou l’échec de la fonction globale. La logique du programme repose en général sur des codes de retour.  
  
 Par exemple, les éléments suivants code appelle **SQLFetch** pour récupérer les lignes dans un jeu de résultats. Il vérifie le code de retour de la fonction pour déterminer si la fin du jeu de résultats a été atteinte (SQL_NO_DATA), si toutes les informations d’avertissement a été retournées (SQL_SUCCESS_WITH_INFO) ou si une erreur s’est produite (SQL_ERROR).  
  
```  
SQLRETURN   rc;  
SQLHSTMT    hstmt;  
  
while ((rc=SQLFetch(hstmt)) != SQL_NO_DATA) {  
   if (rc == SQL_SUCCESS_WITH_INFO) {  
      // Call function to display warning information.  
   } else if (rc == SQL_ERROR) {  
      // Call function to display error information.  
      break;  
   }  
   // Process row.  
}  
```  
  
 Le code de retour SQL_INVALID_HANDLE indique toujours une erreur de programmation et ne doit jamais se rencontrer au moment de l'exécution. Tous les autres codes de retour fournissent des informations d'exécution, bien que SQL_ERROR puisse indiquer une erreur de programmation.  
  
 Le tableau suivant définit les codes de retour.  
  
|Code de retour| Description|  
|-----------------|-----------------|  
|SQL_SUCCESS|Fonction s’est terminée correctement. L’application appelle **SQLGetDiagField** pour récupérer des informations supplémentaires à partir de l’enregistrement d’en-tête.|  
|SQL_SUCCESS_WITH_INFO|Fonction s’est terminée avec succès, éventuellement avec une erreur non fatale (avertissement). L’application appelle **SQLGetDiagRec** ou **SQLGetDiagField** pour récupérer des informations supplémentaires.|  
|SQL_ERROR|Échoué de la fonction. L’application appelle **SQLGetDiagRec** ou **SQLGetDiagField** pour récupérer des informations supplémentaires. Le contenu de tous les arguments de sortie à la fonction n’est pas défini.|  
|SQL_INVALID_HANDLE|Échec de la fonction en raison d’un handle d’environnement, connexion, instruction ou descripteur non valide. Cela indique une erreur de programmation. Aucune information supplémentaire n’est disponible à partir de **SQLGetDiagRec** ou **SQLGetDiagField**. Ce code est renvoyé uniquement lorsque le handle est un pointeur null ou le type est incorrect, par exemple lorsqu’un descripteur d’instruction est passé à un argument qui requiert un handle de connexion.|  
|SQL_NO_DATA|Aucune donnée n’était disponible. L’application appelle **SQLGetDiagRec** ou **SQLGetDiagField** pour récupérer des informations supplémentaires. Un ou plusieurs enregistrements de statut définies par le pilote de classe 02xxx peuvent être renvoyés. **Remarque :** dans ODBC 2. *x*, ce code a été nommé SQL_NO_DATA_FOUND de retour.|  
|SQL_NEED_DATA|Plus de données est nécessaire, par exemple lorsque les données du paramètre sont envoyées au moment de l’exécution, ou les informations de connexion supplémentaires sont requises. L’application appelle **SQLGetDiagRec** ou **SQLGetDiagField** pour récupérer des informations supplémentaires, le cas échéant.|  
|SQL_STILL_EXECUTING|Une fonction qui a été démarrée en mode asynchrone est en cours d’exécution. L’application appelle **SQLGetDiagRec** ou **SQLGetDiagField** pour récupérer des informations supplémentaires, le cas échéant.|
