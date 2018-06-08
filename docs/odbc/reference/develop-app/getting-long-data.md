---
title: Obtention de données de type Long | Documents Microsoft
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
- long data [ODBC]
- fetches [ODBC], long data
- result sets [ODBC], fetching
- SQLGetData function [ODBC], getting long data
- retrieving long data [ODBC]
ms.assetid: 6ccb44bc-8695-4bad-91af-363ef22bdb85
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a465c8200181c87caa49d6e36df50cb5c158cdbf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-long-data"></a>Obtention de données de type Long
Définir des SGBD *données longues* sous forme de n’importe quel caractère ou binaire sur une certaine taille, tels que 255 caractères. Ces données peuvent être assez petites pour être stocké dans une mémoire tampon unique, telle qu’une description de partie de plusieurs caractères de milliers. Toutefois, il peut être trop long pour stocker en mémoire, telles que des documents de texte long ou de bitmaps. Étant donné que ces données ne peut pas être stockées dans un tampon unique, il est extraite du pilote dans des parties avec **SQLGetData** après les autres données de la ligne a été extraite.  
  
> [!NOTE]  
>  Une application peut récupérer tout type de données avec **SQLGetData**, pas seulement données de type long, bien que les caractères et les données binaires peuvent être récupérées parties. Toutefois, si les données sont assez petites pour tenir dans un tampon unique, il est généralement inutile d’utiliser **SQLGetData**. Il est beaucoup plus facile de lier une mémoire tampon pour la colonne et laisser le pilote de renvoyer les données de la mémoire tampon.  
  
 Pour récupérer des données de type long à partir d’une colonne, une application appelle d’abord **SQLFetchScroll** ou **SQLFetch** pour déplacer vers une ligne et extraire les données pour les colonnes liées. L’application appelle ensuite **SQLGetData**. **SQLGetData** a les mêmes arguments que **SQLBindCol**: un descripteur d’instruction ; un numéro de colonne ; le C octets, l’adresse et type de longueur des données d’une variable d’application et l’adresse d’une mémoire tampon de longueur / d’indicateur. Les deux fonctions ont les mêmes arguments, car ils effectuent essentiellement la même tâche : elles décrivent une variable d’application pour le pilote et spécifier que les données d’une colonne particulière doivent être renvoyées dans cette variable. Les principales différences sont qui **SQLGetData** est appelée après qu’une ligne est extraite (et est parfois appelé *liaison tardive* pour cette raison) et que la liaison spécifiée par **SQLGetData** uniquement pendant toute la durée de l’appel.  
  
 En ce qui concerne une seule colonne, **SQLGetData** se comporte comme **SQLFetch**: il récupère les données pour la colonne, il convertit le type de la variable d’application et le retourne dans cette variable. Il retourne également la longueur en octets des données dans la mémoire tampon de longueur / d’indicateur. Pour plus d’informations sur la façon **SQLFetch** renvoie des données, consultez [l’extraction d’une ligne de données](../../../odbc/reference/develop-app/fetching-a-row-of-data.md).  
  
 **SQLGetData** diffère **SQLFetch** un aspect important. Si elle est appelée plusieurs fois de suite pour la même colonne, chaque appel renvoie une successive partie des données. Chaque appel à l’exception du dernier appel retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01004 (chaîne de données, à droite tronquées) ; le dernier appel retourne SQL_SUCCESS. Voici comment **SQLGetData** est utilisée pour récupérer des données de type long dans les parties. Lorsqu’il n’existe aucune donnée supplémentaire à renvoyer, **SQLGetData** retourne SQL_NO_DATA. L’application est chargée de rassembler des données de type long, ce qui peut signifier que la concaténation les parties des données. Chaque partie se termine par null ; l’application doit supprimer le caractère de fin de la valeur null si la concaténation de parties. Est possible de récupérer des données dans les parties de signets de longueur variable, ainsi que pour d’autres données de type long. La valeur retournée dans la baisse de la mémoire tampon/indicateur de longueur dans chaque appel par le nombre d’octets retournés dans l’appel précédent, bien qu’il soit courant pour le pilote d’être impossible de détecter la quantité de données disponibles et de retourner une longueur d’octet de SQL_NO_TOTAL. Par exemple :  
  
```  
// Declare a binary buffer to retrieve 5000 bytes of data at a time.  
SQLCHAR       BinaryPtr[5000];  
SQLUINTEGER   PartID;  
SQLINTEGER    PartIDInd, BinaryLenOrInd, NumBytes;  
SQLRETURN     rc;   
SQLHSTMT      hstmt;  
  
// Create a result set containing the ID and picture of each part.  
SQLExecDirect(hstmt, "SELECT PartID, Picture FROM Pictures", SQL_NTS);  
  
// Bind PartID to the PartID column.  
SQLBindCol(hstmt, 1, SQL_C_ULONG, &PartID, 0, &PartIDInd);  
  
// Retrieve and display each row of data.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {  
   // Display the part ID and initialize the picture.  
   DisplayID(PartID, PartIDInd);  
   InitPicture();  
  
   // Retrieve the picture data in parts. Send each part and the number   
   // of bytes in each part to a function that displays it. The number   
   // of bytes is always 5000 if there were more than 5000 bytes   
   // available to return (cbBinaryBuffer > 5000). Code to check if   
   // rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
   while ((rc = SQLGetData(hstmt, 2, SQL_C_BINARY, BinaryPtr, sizeof(BinaryPtr),  
                           &BinaryLenOrInd)) != SQL_NO_DATA) {  
      NumBytes = (BinaryLenOrInd > 5000) || (BinaryLenOrInd == SQL_NO_TOTAL) ?  
                  5000 : BinaryLenOrInd;  
      DisplayNextPictPart(BinaryPtr, NumBytes);  
   }  
}  
  
// Close the cursor.  
SQLCloseCursor(hstmt);  
```  
  
 Il existe plusieurs restrictions sur l’utilisation de **SQLGetData**. En règle générale, les colonnes accessibles avec **SQLGetData**:  
  
-   Doit être accessible dans l’ordre de l’augmentation du nombre de colonnes (en raison de la façon des que colonnes d’un jeu de résultats sont lus à partir de la source de données). Par exemple, il est incorrect pour appeler **SQLGetData** pour la colonne 5, puis l’appelez pour la colonne 4.  
  
-   Ne peut pas être lié.  
  
-   Doit avoir un numéro de colonne supérieur à la dernière colonne dépendante. Par exemple, si la dernière colonne dépendante est la colonne 3, il est incorrect pour appeler **SQLGetData** pour la colonne 2. Pour cette raison, les applications doivent veillez à placer les colonnes de données de type long à la fin de la liste de sélection.  
  
-   Ne peut pas être utilisé si **SQLFetch** ou **SQLFetchScroll** a été appelée pour récupérer plusieurs lignes. Pour plus d’informations, consultez [à l’aide de curseurs de bloc](../../../odbc/reference/develop-app/using-block-cursors.md).  
  
 Certains pilotes n’appliquent pas ces restrictions. Applications interopérables soit partez du principe qu’ils existent ou déterminent les restrictions ne sont pas appliquées en appelant **SQLGetInfo** avec l’option SQL_GETDATA_EXTENSIONS.  
  
 Si l’application ne doit pas toutes les données dans un caractère ou d’une colonne de données binary, elle peut réduire le trafic réseau dans les pilotes basés sur SGBD en définissant l’attribut d’instruction SQL_ATTR_MAX_LENGTH avant d’exécuter l’instruction. Cela limite le nombre d’octets de données qui seront retournées pour tout caractère ou d’une colonne binaire. Par exemple, qu'une colonne contient des documents de texte long. Une application qui parcourt la table contenant cette colonne peut avoir afficher uniquement la première page de chaque document. Bien que cet attribut d’instruction peut être simulé dans le pilote, il n’existe aucune raison à cela. En particulier, si une application souhaite tronquer binaire ou caractère, il doit lier une petite mémoire tampon à la colonne avec **SQLBindCol** et laisser le pilote tronquer les données.
