---
title: Caractères de données et chaînes C | Documents Microsoft
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
- data buffers [ODBC], length
- data buffers [ODBC], character data
- buffers [ODBC], C strings
- buffers [ODBC], character data
- character data and buffers [ODBC]
- length of data buffers [ODBC]
- data buffers [ODBC], C strings
- buffers [ODBC], length
- C strings and buffers [ODBC]
ms.assetid: 3a141cb4-229d-4027-9349-615cb2995e36
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c23f124aeba138e0ffecc432f28fde4c11c7d1b2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="character-data-and-c-strings"></a>Données de caractères et chaînes C
Les paramètres d’entrée qui font référence à des données de caractères de longueur variable (par exemple, les noms de colonnes, les paramètres dynamiques et valeurs d’attribut de chaîne) ont un paramètre de longueur. Si l’application termine par le caractère null, comme cela est courant en C, les chaînes, il fournit en tant qu’argument, la longueur en octets de la chaîne (ne pas y compris la marque de fin null) ou SQL_NTS (String Null-Terminated). Un argument de longueur non négative indique la longueur réelle de la chaîne associée. L’argument de longueur peut être 0 pour spécifier une chaîne de longueur nulle, ce qui est différent de celui à partir d’une valeur NULL. La valeur négative SQL_NTS dirige le pilote pour déterminer la longueur de la chaîne en recherchant le caractère de fin de la valeur null.  
  
 Lorsque les données de type caractère sont retournées à l’application à partir du pilote, le pilote doit toujours null-mettez-y fin. Cela donne la possibilité de gérer les données comme une chaîne ou un tableau de caractères à l’application. Si la mémoire tampon d’application n’est pas assez élevé pour renvoyer toutes les données caractères, le pilote tronque à la longueur d’octet de la mémoire tampon moins le nombre d’octets requis par le caractère de fin de la valeur null, les données tronquées sont terminées par null et la stocke dans la mémoire tampon. Par conséquent, les applications doivent toujours allouer un espace supplémentaire pour le caractère de fin de la valeur null dans les mémoires tampons utilisées pour récupérer des données caractères. Par exemple, une mémoire tampon 51-octets est nécessaire pour récupérer les 50 caractères de données.  
  
 Une attention particulière doit prendre par l’application et le pilote lors de l’envoi ou de récupération la durée pendant laquelle les données de caractères dans des parties avec **SQLPutData** ou **SQLGetData**. Si les données sont transmises sous forme de chaîne terminée par null, les caractères de fin de null avec ces chaînes doivent être supprimés avant que les données peuvent être rassemblées.  
  
 Un nombre de programmeurs ODBC ont confondu des données de caractères et chaînes C. De cet événement est un artefact de l’utilisation du langage C lors de la définition de fonctions ODBC. Si un pilote ODBC ou une application utilise une autre langue, n’oubliez pas que ODBC est indépendant du langage, cette confusion est moins susceptible de se produire.  
  
 Lorsque les chaînes C sont utilisés pour contenir les données de type caractère, le caractère de fin de la valeur null n’est pas considéré comme faisant partie des données et n’est pas compté dans le cadre de sa longueur en octets. Par exemple, le données de caractères « ABC » peut contenir en tant que la chaîne C « ABC\0 » ou le tableau de caractères {'A', 'B', 'Ce}. La longueur en octets des données est 3, il est traité comme une chaîne ou un tableau de caractères ou non.  
  
 Bien que les applications et les pilotes utilisent couramment des chaînes C (se terminant par null des tableaux de caractères) pour contenir les données de type caractère, il n’est pas nécessaire pour ce faire. En C, les données de caractères peuvent également être traitées comme un tableau de caractères (sans fin-null) et sa longueur en octets passé séparément dans la mémoire tampon de longueur / d’indicateur.  
  
 Car les données de caractères peuvent être contenues dans un tableau non – terminant par null et sa longueur en octets passé séparément, il est possible d’incorporer des caractères null dans les données de type caractère. Toutefois, le comportement des fonctions ODBC dans ce cas n’est pas défini et qu’il est spécifique au pilote si un pilote gère cela correctement. Par conséquent, les applications interopérables doivent gérer les données de type caractère qui peuvent contenir des caractères null incorporés en tant que données binaires.
