---
title: Extraction d’une ligne de données | Documents Microsoft
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
- SQLFetch function [ODBC], fetching a row of data
- cursors [ODBC], fetching rows
- result sets [ODBC], fetching
- fetches [ODBC], row of data
ms.assetid: 16d4a380-0d83-456b-aeee-f10738944e86
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3561212552b84e702319afb1d898fa354fca68b8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="fetching-a-row-of-data"></a>Extraction d’une ligne de données
Pour extraire une ligne de données, une application appelle **SQLFetch**. **SQLFetch** peut être appelé avec n’importe quel type de curseur, mais il ne déplace le curseur de l’ensemble de lignes dans une direction avant uniquement. **SQLFetch** avance le curseur à la ligne suivante et retourne les données pour toutes les colonnes qui ont été liés avec des appels à **SQLBindCol**. Lorsque le curseur atteint la fin du résultat défini, **SQLFetch** retourne SQL_NO_DATA. Pour obtenir des exemples de l’appel de **SQLFetch**, consultez [à l’aide de SQLBindCol](../../../odbc/reference/develop-app/using-sqlbindcol.md).  
  
 Exactement comment **SQLFetch** est implémentée est spécifique au pilote, mais le grand modèle est que le pilote à récupérer les données pour les colonnes liées à partir de la source de données, convertir selon les types de variables liées et placer les données converties dans ces variables. Si le pilote ne peut pas convertir des données, **SQLFetch** renvoie une erreur. L’application peut continuer l’extraction de lignes, mais les données de la ligne actuelle seront perdues. Que se passe-t-il pour les données pour les colonnes indépendantes varie selon le pilote, mais la plupart des pilotes récupérer et ignorer ou jamais l’extraire du tout.  
  
 Le pilote définit également les valeurs de toutes les mémoires tampons de longueur / d’indicateur qui ont été liées. Si la valeur des données pour une colonne est NULL, le pilote définit la mémoire tampon de longueur / d’indicateur correspondant avec la valeur SQL_NULL_DATA. Si la valeur de données n’est pas NULL, le pilote définit la mémoire tampon de longueur / d’indicateur pour la longueur en octets des données après la conversion. Si cette longueur ne peut pas être déterminée, comme cela est parfois le cas avec les données de type long sont récupérées par plus d’un appel de fonction, le pilote affecte la mémoire tampon de longueur / d’indicateur SQL_NO_TOTAL. Pour les types de données de longueur fixe, tels que les entiers et les structures de date, la longueur d’octet est la taille du type de données.  
  
 Pour les données de longueur variable, telles que des caractères ou binaire, le pilote vérifie la longueur en octets des données converties par rapport à la longueur d’octet de la mémoire tampon liée à la colonne ; la longueur de la mémoire tampon est spécifiée dans le *BufferLength* argument dans **SQLBindCol**. Si la longueur en octets des données converties est supérieure à la longueur d’octet de la mémoire tampon, le pilote tronque les données de la mémoire tampon, retourne la longueur non tronquée dans la mémoire tampon de longueur / d’indicateur retourne SQL_SUCCESS_WITH_INFO et place SQLSTATE 01004 (données tronquées) dans les tests de diagnostic. La seule exception à cela est que si un signet de longueur variable est tronqué lorsque retourné par **SQLFetch**, qui retourne SQLSTATE 22001 (données de chaîne tronquées à droite).  
  
 Données de longueur fixe ne sont jamais tronquées, car le pilote part du principe que la taille de la mémoire tampon liée est la taille du type de données. Une troncation de données a tendance à être rares, car l’application est généralement liée à une mémoire tampon suffisamment grande pour contenir la valeur entière de données. Il détermine la taille nécessaire à partir des métadonnées. Toutefois, l’application peut lier explicitement une mémoire tampon qu’il sache qu’il peut pour être trop petite. Par exemple, il peut récupérer et afficher les 20 premiers caractères d’une description de la partie ou les 100 premiers caractères d’une colonne de texte long.  
  
 Données de caractères doivent être terminée par le pilote avant d’être retournée à l’application, même si elle a été tronquée. Le caractère de fin de la valeur null n’est pas inclus dans la longueur d’octet retourné mais requiert d’espace dans la mémoire tampon liée. Par exemple, une application utilise des chaînes composées de données de caractères dans le jeu de caractères ASCII, un pilote a 50 caractères des données à renvoyer, et la mémoire tampon de l’application est 25 octets de long. Dans la mémoire tampon de l’application, le pilote retourne les premier 24 caractères suivis d’un caractère de fin de la valeur null. Dans la mémoire tampon de longueur / d’indicateur, il retourne une longueur d’octet de 50.  
  
 L’application peut restreindre le nombre de lignes du jeu de résultats en définissant l’attribut d’instruction SQL_ATTR_MAX_ROWS avant l’exécution de l’instruction qui crée le résultat. Par exemple, le mode Aperçu dans une application qui permet de formater des rapports doit uniquement suffisamment de données pour afficher la première page du rapport. En limitant la taille du jeu de résultats, une telle fonctionnalité s’exécute plus rapidement. Cet attribut d’instruction vise à réduire le trafic réseau et ne peut pas être pris en charge par tous les pilotes.
