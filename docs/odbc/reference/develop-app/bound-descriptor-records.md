---
title: Limite les enregistrements de descripteurs | Documents Microsoft
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
- bound descriptor records [ODBC]
- descriptors [ODBC], bound descriptor records
ms.assetid: 55d09344-6682-40f6-b634-036b134ff650
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0ac161220673c32f13c08ab7588c269237b5aa6f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bound-descriptor-records"></a>Enregistrements de descripteurs liée
Lorsque l’application définit le champ SQL_DESC_DATA_PTR d’un enregistrement de descripteur pour qu’il contienne n’est plus une valeur null, l’enregistrement est dite *liés*.  
  
 Si le descripteur est un APD, chaque enregistrement lié constitue un paramètre dépendant. Pour les paramètres d’entrée, l’application doit lier un paramètre pour chaque marqueur de paramètre dynamique dans l’instruction SQL avant d’exécuter l’instruction. Pour les paramètres de sortie, l’application ne doive pas lier le paramètre.  
  
 Si le descripteur est un ARD, qui décrit une ligne de base de données, chaque enregistrement lié constitue une colonne dépendante.
