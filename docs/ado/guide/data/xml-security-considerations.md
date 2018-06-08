---
title: Considérations de sécurité XML | Documents Microsoft
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
- XML security in ADO
ms.assetid: fadbd38e-6e7b-4b81-96ea-85169c664374
caps.latest.revision: 3
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 36394ea9806dc7345c391e3e5a6fd6733d08181d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-security-considerations"></a>Considérations de sécurité XML
Les méthodes Open sur l’objet Recordset et ADO enregistrer ne sont pas considérés comme des opérations sécurisées pour s’exécuter dans Internet Explorer. Par conséquent, si ces méthodes sont utilisées dans un code de script qui s’exécute dans une application ou un contrôle qui est hébergé dans un navigateur, la configuration de sécurité du navigateur aura un effet sur son comportement.  
  
 Internet Explorer 5 fournit des restrictions de sécurité pour ces opérations par défaut dans les zones Internet. Avec cette configuration, le jeu d’enregistrements ne peut pas rendre tout accès au système de fichiers local sur le client ou accéder aux sources de données en dehors du domaine du serveur à partir de laquelle la page a été téléchargée. Plus précisément, lors de l’exécution à l’intérieur de l’hôte du navigateur, un jeu d’enregistrements peut enregistré dans un fichier uniquement s’il est sur le même serveur à partir de laquelle la page a été téléchargée. De même, vous pouvez ouvrir un jeu d’enregistrements en le chargeant à partir d’un fichier uniquement si ce fichier se trouve sur le même serveur à partir de laquelle la page a été téléchargée.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
