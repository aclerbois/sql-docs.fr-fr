---
title: SQLFetchScroll (bibliothèque de curseurs) | Documents Microsoft
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
- SQLFetchScroll function [ODBC], Cursor Library
ms.assetid: 4417e57c-31dd-475e-8fe9-eab00a459c80
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b0c3e9709cd3aeced11116ab88c5323d4462abd2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlfetchscroll-cursor-library"></a>SQLFetchScroll (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 Cette rubrique décrit l’utilisation de la **SQLFetchScroll** fonction dans la bibliothèque de curseurs. Pour des informations générales sur **SQLFetchScroll**, consultez [fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 La bibliothèque de curseurs implémente **SQLFetchScroll** en appelant à plusieurs reprises **SQLFetch** dans le pilote. Il transfère les données qu'extraites à partir du pilote pour les tampons de l’ensemble de lignes fournis par l’application. Il met également en cache les données dans les fichiers de mémoire et de disque. Lorsqu’une application demande un nouvel ensemble de lignes, la bibliothèque de curseurs récupère comme nécessaire du pilote (si elle n'a pas été précédemment extrait) ou du cache (si elle a été extraite précédemment). Enfin, la bibliothèque de curseurs gère l’état des données mises en cache et retourne cette information à l’application dans le tableau d’état de ligne.  
  
 Lorsque la bibliothèque de curseurs est utilisée, les appels à **SQLFetchScroll** ne peut pas être combiné avec des appels vers **SQLFetch** ou **SQLExtendedFetch**.  
  
 Lorsque la bibliothèque de curseurs est utilisée, les appels à **SQLFetchScroll** sont pris en charge à la fois pour ODBC 2. *x* et pour ODBC 3. *x* pilotes.  
  
## <a name="rowset-buffers"></a>Tampons de l’ensemble de lignes  
 La bibliothèque de curseurs permet d’optimiser le transfert de données à partir du pilote dans le tampon de l’ensemble de lignes fourni par l’application si :  
  
-   L’application utilise la liaison.  
  
-   Il n’y a aucune octets inutilisés entre les champs dans la structure de que l’application déclare pour contenir une ligne de données.  
  
-   Les champs dans lesquels **SQLFetch** ou **SQLFetchScroll** retourne l’indicateur/longueur pour une colonne suit la mémoire tampon pour cette colonne et précède la mémoire tampon pour la colonne suivante. Ces champs sont facultatifs.  
  
 Lorsque l’application demande un nouvel ensemble de lignes, la bibliothèque de curseurs récupère les données à partir de sa mémoire cache et du pilote si nécessaire. Si les ensembles de lignes nouvelles et anciennes se chevauchent, la bibliothèque de curseurs peut optimiser ses performances en réutilisant les données dans les sections qui se chevauche de mémoires tampons de l’ensemble de lignes. Par conséquent, les tampons de l’ensemble de lignes des modifications non enregistrées sont perdues, sauf si les ensembles de lignes nouvelles et anciennes se chevauchent et que les modifications sont dans les sections qui se chevauche de mémoires tampons de l’ensemble de lignes. Pour enregistrer les modifications, une application soumet une instruction de mise à jour positionnée.  
  
 Notez que la bibliothèque de curseurs actualise toujours les tampons de l’ensemble de lignes avec des données à partir du cache lorsqu’une application appelle **SQLFetchScroll** avec la *FetchOrientation* argument a la valeur SQL_FETCH_RELATIVE et le *FetchOffset* argument défini à 0.  
  
 La bibliothèque de curseurs prend en charge l’appel **SQLSetStmtAttr** avec un *attribut* de SQL_ATTR_ROW_ARRAY_SIZE pour modifier la taille de l’ensemble de lignes tandis que le curseur est ouvert. La nouvelle taille de l’ensemble de lignes prendra effet lors du prochain **SQLFetchScroll** est appelée.  
  
## <a name="result-set-membership"></a>Appartenance au jeu de résultats  
 La bibliothèque de curseurs récupère les données à partir du pilote que l’application le demande. En fonction de la source de données et du paramètre de l’attribut d’instruction SQL_CONCURRENCY, cela entraîne les conséquences suivantes :  
  
-   Les données récupérées par la bibliothèque de curseurs peuvent différer de celles qui étaient disponibles au moment de que l’instruction a été exécutée. Par exemple, après l’ouverture du curseur, les lignes insérées après la position actuelle du curseur peuvent être récupérées par certains pilotes.  
  
-   Les données dans le jeu de résultats peuvent être verrouillées par la source de données pour la bibliothèque de curseurs et par conséquent pas disponible pour les autres utilisateurs.  
  
 Une fois que la bibliothèque de curseurs a mis en cache d’une ligne de données, il ne peut pas détecter les modifications apportées à cette ligne dans la source de données sous-jacente (à l’exception des mises à jour positionnées et des suppressions d’exploitation sur le cache du même curseur). Cela se produit parce que, pour les appels à **SQLFetchScroll**, la bibliothèque de curseurs réextrait jamais des données à partir de la source de données. Au lieu de cela, il réextrait les données à partir de son cache.  
  
## <a name="scrolling"></a>Le défilement  
 La bibliothèque de curseurs prend en charge les types suivants de fetch dans **SQLFetchScroll**.  
  
|Type de curseur|Types d’extraction|  
|-----------------|-----------------|  
|Curseur avant uniquement|SQL_FETCH_NEXT|  
|Statique|SQL_FETCH_NEXT<br /><br /> SQL_FETCH_PRIOR<br /><br /> SQL_FETCH_FIRST<br /><br /> SQL_FETCH_LAST<br /><br /> SQL_FETCH_RELATIVE<br /><br /> SQL_FETCH_ABSOLUTE<br /><br /> SQL_FETCH_BOOKMARK|  
  
## <a name="errors"></a>Erreurs  
 Lorsque **SQLFetchScroll** est appelée et l’autre des appels à **SQLFetch** retourne SQL_ERROR, la bibliothèque de curseur se poursuit comme suit. Après avoir terminé ces étapes, la bibliothèque de curseurs continue le traitement.  
  
1.  Appels **SQLGetDiagRec** pour obtenir des informations d’erreur à partir du pilote et enregistre ceci comme un enregistrement de diagnostic dans le Gestionnaire de pilotes.  
  
2.  Définit le champ SQL_DIAG_ROW_NUMBER dans l’enregistrement de diagnostic sur la valeur appropriée.  
  
3.  Définit le champ SQL_DIAG_COLUMN_NUMBER dans l’enregistrement de diagnostic sur la valeur appropriée, le cas échéant ; dans le cas contraire, il le définit sur 0.  
  
4.  Définit la valeur de la ligne de l’erreur dans le tableau d’état de ligne à SQL_ROW_ERROR.  
  
 Après le curseur est appelée bibliothèque **SQLFetch** plusieurs fois dans son implémentation de **SQLFetchScroll**, toute erreur ou un avertissement retourné par un des appels à **SQLFetch** seront dans un enregistrement de diagnostic et peuvent être récupérés par un appel à **SQLGetDiagRec**. Si les données ont été tronquées lorsqu’il a été extraite, les données tronquées réside désormais dans le cache de la bibliothèque de curseurs. Les appels suivants à **SQLFetchScroll** pour accéder à une ligne avec des données tronquées renvoie les données tronquées et aucun avertissement n’est générée, car les données sont extraites à partir du cache de la bibliothèque de curseurs. Pour effectuer le suivi de la longueur des données retournées afin qu’elle peut déterminer si les données retournées dans une mémoire tampon a été tronquées, une application doit lier la mémoire tampon de longueur / d’indicateur.  
  
## <a name="bookmark-operations"></a>Opérations de signet  
 La bibliothèque de curseurs prend en charge l’appel **SQLFetchScroll** avec un *FetchOrientation* de SQL_FETCH_BOOKMARK. Il prend également en charge en spécifiant un décalage dans le *FetchOffset* argument peut être utilisé dans l’opération de signet. Il s’agit de la seule opération de signet que prend en charge de la bibliothèque de curseurs. La bibliothèque de curseurs ne prend pas en charge l’appel **SQLBulkOperations**.  
  
 Si l’application a défini l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS et est liée à la colonne de signet, la bibliothèque de curseurs génère un signet de longueur fixe et le retourne à l’application. La bibliothèque de curseurs crée et gère les signets pour qu’il utilise ; Il n’utilise pas les signets gérées au niveau de la source de données. Lorsque **SQLFetchScroll** est appelée pour extraire un bloc de données qui ont déjà été récupérées à partir de la source de données, il récupère les données à partir du cache de bibliothèque de curseur. Par conséquent, le signet est utilisé dans un appel à **SQLFetchScroll** avec un *FetchOrientation* de SQL_FETCH_BOOKMARK doivent être créées et gérées par la bibliothèque de curseurs.  
  
## <a name="interaction-with-other-functions"></a>Interaction avec d’autres fonctions  
 Une application doit appeler **SQLFetch** ou **SQLFetchScroll** avant prépare ou exécute une positionné instructions update ou delete.
