---
title: Pilotes basés sur SGBD | Documents Microsoft
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
- drivers [ODBC], DBMS-based drivers
- DBMS-based drivers [ODBC]
- ODBC architecture [ODBC], drivers
ms.assetid: e2208ee0-4cd6-4f0d-bb71-a0b54f7d9330
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bb73a903d1e286e018c326b675a6bd914db0e367
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dbms-based-drivers"></a>Pilotes basés sur SGBD
Pilotes basés sur SGBD sont utilisées avec des sources de données telles que Oracle ou SQL Server qui fournissent un moteur de base de données autonome pour le pilote à utiliser. Ces pilotes accéder aux données physiques par le moteur autonome ; Autrement dit, ils soumettre des instructions SQL à et récupèrent les résultats à partir du moteur.  
  
 Pilotes basés sur SGBD utilisent un moteur de base de données existant, elles sont généralement plus faciles à écrire que les pilotes basés sur le fichier. Même si un pilote basés sur SGBD permettre être facilement implémenté en traduisant les appels à des appels d’API ODBC, cela entraîne un pilote plus lent. Un meilleur moyen d’implémenter un pilote basés sur SGBD est d’utiliser le protocole de flux de données sous-jacent, ce qui est généralement ce que fait l’API native. Par exemple, un pilote de SQL Server doit utiliser TDS (le flux de données protocole pour SQL Server) au lieu de la bibliothèque de base de données (l’API native pour SQL Server). Une exception à cette règle est lorsqu’ODBC est l’API native. Par exemple, Watcom SQL est un moteur autonome qui réside sur le même ordinateur que l’application est chargée directement en tant que le pilote.  
  
 Pilotes basés sur SGBD agissent en tant que le client dans une configuration de client/serveur sur lequel la source de données fait Office de serveur. Dans la plupart des cas, le client (pilote) et le serveur (source de données) résident sur des ordinateurs différents, bien que les deux peuvent résider sur le même ordinateur exécutant un système d’exploitation multitâche. Une troisième possibilité est un *passerelle,* qui se situe entre le pilote et de la source de données. Une passerelle est un logiciel qui provoque un SGBD ressemble à un autre. Par exemple, les applications écrites pour utiliser SQL Server peuvent également accéder aux données DB2 via la passerelle de DB2 Decisionware Micro ; Ce produit entraîne DB2 ressemble à SQL Server.  
  
 L’illustration suivante montre trois différentes configurations de pilotes basés sur SGBD. Dans la première configuration, le pilote et la source de données résident sur le même ordinateur. Dans la seconde, le pilote et la source de données résident sur des ordinateurs différents. Dans la troisième, le pilote et la source de données résident sur des ordinateurs différents et une passerelle se situe entre eux, résidant sur un autre ordinateur.  
  
 ![Trois configurations de SGBD&#45;pilotes](../../odbc/reference/media/pr07.gif "pr07")
