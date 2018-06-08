---
title: Instructions et Limitations de SQLXML 4.0 | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQLXML, about SQLXML
ms.assetid: fe433d30-90a1-421e-85c6-af13294dc18d
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 407a3a50c6dfa6b88ddfb22b8c00b1b487607421
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="guidelines-and-limitations-of-sqlxml-40"></a>Recommandations et limitations de SQLXML 4.0
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Gardez en mémoire les points suivants lors de l'utilisation de SQLXML 4.0 :  
  
-   Le XML retourné en tant que résultat de requête n'est pas validé par rapport au schéma de mappage qui a généré le XML.  
  
-   SQLXML 4.0 inclut des PROGID dépendants et indépendants de la version. Il est recommandé que toutes les applications de production utilisent des PROGID dépendants de la version. Ceci est tout particulièrement important car SQLXML 4.0 n'a pas une compatibilité descendante complète. L'utilisation de PROGID dépendant de la version offre une protection contre les échecs de production éventuels lors de l'installation de versions plus récentes. De version en version, le comportement du programme peut changer pour de multiples raisons comme la résolution de bogues, les modifications de conception, etc. L'utilisation de PROGID dépendants de la version offre une protection contre les échecs inattendus de l'installation de versions plus récentes. Avec des PROGID dépendants de la version, lorsque vous installez une version plus récente, votre application continuera de fonctionner sans échec. Si vous décidez de modifier les précédents PROGID dépendants de la version pour en utiliser de plus récents dans une nouvelle version, vous devez tester votre application avant de la mettre en production. Par exemple, les applications qui utilisent des PROGID indépendants de la version peuvent échouer dans le scénario suivant :  
  
     Vous exécutez une application qui utilise SQLXML 4.0 et des PROGID indépendants de la version, et vous décidez d'installer un autre programme. Ce programme peut installer une version antérieure de SQLXML. Votre application risque d'échouer car les PROGID indépendants de la version de votre application pointent maintenant vers la version antérieure de SQLXML, qui peut ou non avoir la fonctionnalité SQLXML utilisée par votre application.  
  
-   Si pour une raison quelconque vous ne souhaitez utiliser le fournisseur SQLXMLOLEDB et plutôt que d’utiliser SQLOLEDB fournisseur pour les fonctionnalités SQLXML, définissez la **SQLXML Version** propriété valeur « SQLXML.4.0 ».  
  
  
