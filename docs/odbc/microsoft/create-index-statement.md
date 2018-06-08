---
title: CRÉER des INDEX (instruction) | Documents Microsoft
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
- CREATE INDEX [ODBC]
- SQL grammar [ODBC], create index
ms.assetid: 69438247-eef3-44c5-bef2-acef4e146f41
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 86d4cf1bb047cd475b86b9a58c37f3268c07c91d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-index-statement"></a>CRÉER des INDEX (instruction)
La syntaxe de l’instruction CREATE INDEX est :  
  
 CRÉER des INDEX [UNIQUE] *-nom de l’index* ON *-nom de la table* (*identificateur de la colonne* [ASC] [Description] [, *identificateur de la colonne* [ASC][DESC]...]) AVEC \< *liste option d’index*>  
  
 où \< *liste option d’index*> peut être : principal &#124; DISALLOW NULL &#124; IGNORE NULL  
  
 Uniquement le pilote Microsoft Access utilise les options d’index DISALLOW NULL et IGNORE NULL. Les pilotes de Corel dBASE acceptent la syntaxe, mais ignore la présence de l’option.  
  
 Lorsque le pilote Paradox est utilisé, l’instruction CREATE INDEX crée les fichiers de clé primaires de Corel et les fichiers secondaires.  
  
 Cette instruction n’est pas prise en charge par les pilotes Microsoft Excel ou texte.
