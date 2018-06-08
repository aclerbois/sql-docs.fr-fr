---
title: Prise en charge internationale (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- double-byte character sets [ODBC]
- FoxPro ODBC driver [ODBC], international support
- DBCS [ODBC]
- international support [ODBC]
- sort order [ODBC]
- collating sequences [ODBC]
- Visual FoxPro ODBC driver [ODBC], international support
ms.assetid: cd3fab32-13f1-4a86-abc4-5e18667669fc
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 151d3989737d221e46f6771055d0775c69f7e576
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="international-support-visual-foxpro-odbc-driver"></a>Prise en charge internationale (le pilote ODBC Visual FoxPro)
Le pilote ODBC Microsoft Visual FoxPro prend en charge :  
  
-   Les jeux de caractères de codés sur deux octets (DBCS)  
  
-   Plusieurs séquences de classement  
  
 Une séquence de classement définit le *l’ordre de tri* pour les données stockées dans une table Visual FoxPro ou d’une base de données. Par défaut, le pilote est configuré pour utiliser les séquences de classement qui prennent en charge la version linguistique de votre système d’exploitation.  
  
 Pour obtenir la liste des séquences de classement pris en charge, consultez [définir COLLATE](../../odbc/microsoft/set-collate-command.md).  
  
## <a name="locale"></a>paramètres régionaux  
 Le jeu d’informations qui correspond à une langue donnée et le pays/région. Paramètres régionaux indique des paramètres spécifiques comme séparateurs décimaux, date et formats d’heure et l’ordre de tri des caractères.  
  
## <a name="sort-order"></a>ordre de tri  
 Ordres de tri intègrent les règles de tri de différentes *paramètres régionaux*s, ce qui vous permet de trier correctement les données dans ces langues. Dans Visual FoxPro, l’ordre de tri actuelle détermine les résultats des comparaisons d’expression de caractères et l’ordre dans lequel les enregistrements apparaissent dans indexée ou du tri des tables.
