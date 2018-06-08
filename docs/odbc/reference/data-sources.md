---
title: Sources de données | Microsoft Docs
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
- data sources [ODBC]
- data sources [ODBC], about data sources
ms.assetid: 4ae44fa2-0b9b-4e19-ab45-c1dc93b68406
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c3a57a40e301b565998a5eff86b7d631f1bc8e7a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-sources"></a>Sources de données
A *source de données* est simplement la source de données. Il peut être un fichier, une base de données sur un SGBD, ou même un flux de données actives. Les données peuvent être situées sur le même ordinateur que le programme, ou sur un autre ordinateur situé sur un réseau. Par exemple, une source de données peut être un SGBD Oracle s’exécutant sur un système d’exploitation OS/2®, accédé par Novell® Netware ; un SGBD de DB2 d’IBM accessibles via une passerelle ; une collection de fichiers Xbase dans un répertoire du serveur ; ou un fichier de base de données Microsoft® Access local.  
  
 L’objectif d’une source de données consiste à rassembler toutes les informations techniques nécessaires pour accéder aux données, le nom du pilote, adresse réseau, un logiciel réseau et ainsi de suite, en un seul placer et le masquer de l’utilisateur. L’utilisateur doit être en mesure de consulter la liste qui inclut les salaires, l’inventaire et Personnel, choisissez paie dans la liste et que l’application de se connecter aux données de la paie, tout cela sans savoir où résident les données de la paie ou comment l’application est parvenu à celui-ci.  
  
 Le terme *source de données* ne doivent pas être confondues avec les termes similaires. Dans ce manuel, *SGBD* ou *base de données* fait référence à un moteur de base de données ou. Une autre distinction entre *des bases de données bureautiques,* conçu pour s’exécuter sur les ordinateurs personnels et souvent manquantes dans SQL complet et la prise en charge de la transaction, et *bases de données de serveur,* conçu pour s’exécuter dans une situation de client/serveur et caractérisées par un moteur de base de données autonome, SQL riche et prise en charge de la transaction. *Base de données* fait également référence à une collection spécifique de données, comme une collection de fichiers Xbase dans un répertoire ou d’une base de données sur SQL Server. Il est généralement équivalent au terme de *catalogue,* utilisée ailleurs dans ce manuel ou le terme *qualificateur* dans les versions antérieures d’ODBC.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Types de sources de données](../../odbc/reference/types-of-data-sources.md)  
  
-   [Utilisation des sources de données](../../odbc/reference/using-data-sources.md)  
  
-   [Exemple de source de données](../../odbc/reference/data-source-example.md)
