---
title: Mise à jour des lignes par le signet avec SQLBulkOperations | Documents Microsoft
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
- data updates [ODBC], bookmarks
- SQLBulkOperations function [ODBC], updating rows
- data updates [ODBC], SQLBulkOperations
- bookmarks [ODBC]
- updating data [ODBC], bookmarks
- updating data [ODBC], SQLBulkOperations
ms.assetid: c9ad82b7-8dba-45b0-bdb9-f4668b37c0d6
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4e69a8a2da8b4d246ab99217af23737d7b10eb1b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-rows-by-bookmark-with-sqlbulkoperations"></a>Mise à jour des lignes par le signet avec SQLBulkOperations
Lors de la mise à jour d’une ligne par un signet, **SQLBulkOperations** rend la source de données à mettre à jour une ou plusieurs lignes de la table. Les lignes sont identifiées par le signet dans une colonne liée de signet. La ligne est mise à jour à l’aide des données dans les mémoires tampons d’application pour chaque colonne dépendante (sauf si la valeur dans la mémoire tampon de longueur / d’indicateur pour une colonne est SQL_COLUMN_IGNORE). Colonnes indépendantes ne seront pas mis à jour.  
  
 Pour mettre à jour des lignes par le signet avec **SQLBulkOperations**, l’application :  
  
1.  Récupère et met en cache les signets de toutes les lignes à mettre à jour. S’il existe plusieurs signets et la liaison est utilisée, les signets sont stockés dans un tableau ; s’il existe plusieurs signets et la liaison est utilisée, les signets sont stockés dans un tableau de structures de ligne.  
  
2.  Définit l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE au nombre de signets et lie la mémoire tampon qui contient la valeur du signet, ou le tableau de signets, à la colonne 0.  
  
3.  Place les nouvelles valeurs de données dans les tampons de l’ensemble de lignes. Pour plus d’informations sur l’envoi des données de type long avec **SQLBulkOperations**, consultez [données de type Long et SQLSetPos et SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
4.  Définit la valeur dans la mémoire tampon de longueur / d’indicateur de chaque colonne si nécessaire. Il s’agit de la longueur d’octet des données ou SQL_NTS pour les colonnes liées aux mémoires tampons de chaîne, la longueur en octets des données pour les colonnes liées aux mémoires tampons de binaire et SQL_NULL_DATA pour toutes les colonnes à la valeur NULL.  
  
5.  Définit la valeur de la mémoire tampon de longueur / d’indicateur de ces colonnes ne doivent ne pas être mis à jour vers SQL_COLUMN_IGNORE. Bien que l’application peut ignorer cette étape et renvoyer les données existantes, il est inefficace et risque d’envoyer des valeurs dans la source de données qui ont été tronquées lors de leur lecture.  
  
6.  Appels **SQLBulkOperations** avec la *opération* argument a la valeur SQL_UPDATE_BY_BOOKMARK.  
  
 Pour chaque ligne est envoyée à la source de données en tant qu’une mise à jour, les tampons de l’application doivent avoir des données de ligne valide. Si les mémoires tampons d’application ont été remplies par extraction, si un tableau d’état de ligne a été maintenu, et si la valeur d’état d’une ligne est SQL_ROW_DELETED, SQL_ROW_ERROR ou SQL_ROW_NOROW, les données non valides par inadvertance pu être envoyées que pour la source de données.
