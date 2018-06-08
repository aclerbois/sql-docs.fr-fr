---
title: État en lecture seule (pilote Excel) | Documents Microsoft
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
- read-only status for Excel driver [ODBC]
- Excel driver [ODBC], read-only status
ms.assetid: ef5d773b-4f8f-4005-b985-84b53d8e9f9b
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d885124c9e4274d402bfa504b975f089d333af95
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="read-only-status-excel-driver"></a>État en lecture seule (pilote Excel)
Lorsque le pilote Microsoft Excel est utilisé, les tables de source de données sont ouverts en lecture seule par défaut et peuvent être ouverte par un seul utilisateur à la fois. Même si les tables ont un état en lecture seule, toutefois, les applications peuvent effectuer des insertions et des mises à jour pour les tables de Microsoft Excel.  
  
 Lorsqu’une application exécute une commande Enregistrer en tant que données de Microsoft Excel via le pilote Microsoft Excel, l’application doit créer une table et insérez les données à enregistrer dans la nouvelle table. Insertions entraîner un ajout à la table. Aucune autre opération ne peut être effectuée sur la table jusqu'à ce qu’elle est fermée et rouverte. Une fois que la table est fermée, aucune insertion ultérieure ne peut être effectuée, car la table est ensuite une table en lecture seule.  
  
 Il est possible de mettre à jour les valeurs lorsque vous utilisez le pilote Microsoft Excel, mais une ligne ne peut pas être supprimée à partir d’une table basée sur une feuille de calcul Microsoft Excel, les mises à jour sont considérées comme pas officiellement pris en charge par le pilote Microsoft Excel.
