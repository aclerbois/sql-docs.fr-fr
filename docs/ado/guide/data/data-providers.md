---
title: Fournisseurs de données | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data providers [ADO]
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 877b9f25-60c4-4ab6-8052-2c28a3849e89
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ddd360c49c3cbd10b76dc39e6c161a523a93b1bb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-providers"></a>Fournisseurs de données
Fournisseurs de données représentent diverses sources de données telles que les bases de données SQL, fichiers séquentiels indexés, des feuilles de calcul, banques de documents et fichiers de la messagerie. Fournisseurs présentent les données de façon uniforme en utilisant une abstraction commune appelée l’ensemble de lignes.  
  
 ADO est puissant et flexible, car il peut se connecter à plusieurs fournisseurs de données et exposent toujours le même modèle de programmation, indépendamment des fonctionnalités spécifiques de n’importe quel fournisseur donné. Toutefois, étant donné que chaque fournisseur de données est unique, comment votre application interagit avec ADO varie par fournisseur de données.  
  
 Par exemple, les fonctionnalités du fournisseur OLE DB pour SQL Server, ce qui permet d’accéder aux bases de données Microsoft SQL Server, sont très différentes de ceux du fournisseur Microsoft OLE DB pour Internet Publishing, qui est utilisé pour accéder au fichier magasins de sur un serveur Web.
