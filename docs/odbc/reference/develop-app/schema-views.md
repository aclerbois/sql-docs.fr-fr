---
title: Vues de schémas | Documents Microsoft
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
- schema views [ODBC]
- functions [ODBC], catalog functions
- catalog functions [ODBC], schema views
ms.assetid: f1dfb3e8-a7bd-46c3-92b6-c19531e8409d
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 686b13ff9b480a1e2e96a175fe546d064079d871
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="schema-views"></a>Vues de schémas
Une application peut récupérer les informations de métadonnées à partir du SGBD en appelant les fonctions de catalogue ODBC ou à l’aide de vues INFORMATION_SCHEMA. Les vues sont définies par la norme ANSI SQL-92.  
  
 Si la prise en charge par le SGBD et du pilote, les vues INFORMATION_SCHEMA fournissent un moyen plus puissant et complet de la récupération de métadonnées que les fonctions de catalogue ODBC fournissent. Une application peut exécuter son propre personnalisé **sélectionnez** instruction par rapport à un de ces affichages, peuvent joindre des vues, ou peut effectuer une union sur des vues. Tout en offrant l’utilitaire supérieure et une plus large gamme de métadonnées, les vues INFORMATION_SCHEMA ne sont pas souvent pris en charge par le SGBD. Cela peut modifier que des pilotes et des SGBD plus mettre en conformité avec SQL-92.  
  
 Pour déterminer les modes d’affichage sont pris en charge, une application appelle **SQLGetInfo** avec l’option SQL_INFO_SCHEMA_VIEWS. Pour récupérer les métadonnées à partir d’une vue pris en charge, l’application exécute une **sélectionnez** instruction qui spécifie les informations de schéma requises.
