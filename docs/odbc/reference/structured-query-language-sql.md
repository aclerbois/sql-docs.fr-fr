---
title: Structured Query Language (SQL) | Documents Microsoft
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
- SQL [ODBC]
- SQL [ODBC], about SQL
- ODBC [ODBC], SQL
ms.assetid: bebfd93e-0dc0-46b3-a531-518beb7ea976
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e28770a1532d8d52ec8069d00154c4b377628d1f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="structured-query-language-sql"></a>SQL (Structured Query Language)
Un SGBD classique permet aux utilisateurs de stocker, accéder et modifier des données d’une manière organisée, efficace. À l’origine, les utilisateurs de SGBD étaient les programmeurs. L’accès aux données stockées requis de l’écriture d’un programme dans un langage de programmation tels que COBOL. Alors que ces programmes ont été écrits souvent pour présenter une interface conviviale pour un utilisateur non technique, l’accès aux données elles-mêmes nécessaire les services d’un programmeur expérimenté. Accès aisé aux données n’était pas pratique.  
  
 Les utilisateurs n’ont pas étaient entièrement satisfaits à cette situation. Pendant qu’ils peuvent accéder aux données, il est souvent nécessaire de convaincre un programmeur SGBD d’écrire un logiciel spécial. Par exemple, si un département des ventes pour voir le total des ventes du mois précédent en fonction de chacun de ses commerciaux et souhaitiez ces informations classées dans l’ordre par la longueur de chaque vendeur du service de l’entreprise, qu’elle avait deux possibilités : un programme existait déjà autorisés les informations accessibles exactement de cette manière, ou le service devait demander un programmeur pour écrire un programme de ce type. Dans de nombreux cas, il s’agissait de plus de travail qu’il était important, et il était toujours une solution coûteuse pour des requêtes à usage unique ou ad hoc. Comme le plus grand d’utilisateurs souhaitiez accéder facilement, ce problème a augmenté de plus en plus volumineuses.  
  
 Permettre aux utilisateurs d’accéder aux données sur une base ad hoc requis en leur donnant une langue dans laquelle exprimer leurs demandes. Une demande unique à une base de données est définie en tant que requête ; ces langages sont appelé un langage de requête. Nombreux langages de requête ont été développés à cet effet, mais un de ces est devenu le plus courant : Structured Query Language inventé IBM dans les années 1970. Il est plus généralement connue sous son un acronyme, SQL et est prononcé à la fois en tant que « ess-file d’attente-ell » et « suite » ; Ce manuel utilise la prononciation précédente. SQL est devenu ANSI standard en 1986 et un fichier ISO standard en 1987 ; Il est utilisé aujourd'hui dans une très nombreux systèmes de gestion de base de données.  
  
 Bien que SQL résolu les besoins des utilisateurs ad hoc, avoir accès aux données par les applications ne pas disparu. En fait, la plupart des accès de base de données toujours était (et est) programmes entrée de données par programmation, sous la forme de régulièrement des rapports et des analyses statistiques, telles que ceux utilisés pour l’entrée de commande et les données des programmes de manipulation, telles que celles utilisées pour rapprocher des comptes et générer des ordres de travail.  
  
 Ces programmes utilisent également SQL, à l’aide d’une des trois méthodes suivantes :  
  
-   **Embedded SQL**, dans SQL, les instructions sont incorporées dans un langage hôte tels que C ou COBOL.  
  
-   **Les modules SQL**, dans les instructions SQL sont compilées dans le SGBD et appelées à partir d’un langage hôte.  
  
-   **Interface de niveau d’appel**, ou l’interface CLI, qui se compose des fonctions appelées pour passer des instructions SQL au SGBD et récupérer les résultats à partir du SGBD.  
  
> [!NOTE]  
>  Il s’agit d’un incident historique qui créent une interface le terme interface au niveau de l’appel est utilisé au lieu de la programmation d’applications (API), un autre terme pour la même chose. Dans le monde de la base de données, l’API est utilisée pour décrire SQL lui-même : SQL est l’API à un SGBD.  
  
 De ces choix, embedded SQL est le plus couramment utilisées, bien que la plupart des principaux SGBD prennent en charge les propriétaires CLI.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Traitement d’une instruction SQL](../../odbc/reference/processing-a-sql-statement.md)  
  
-   [Embedded SQL](../../odbc/reference/embedded-sql.md)  
  
-   [Modules SQL](../../odbc/reference/sql-modules.md)  
  
-   [Interfaces de niveau d’appel](../../odbc/reference/call-level-interfaces.md)
