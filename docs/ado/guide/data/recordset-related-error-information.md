---
title: "Informations sur l’erreur Recordset | Documents Microsoft"
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: ado
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: sql
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- Recordset-related errors [ADO]
- errors [ADO], Recordset-related
ms.assetid: 7e103574-59ad-4790-b5f9-fa8d715e711e
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: b1ad53c42fa3da686de10e3254c6fa7e3916d806
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="recordset-related-error-information"></a>Informations sur l’erreur Recordset
Au cours de traitement par lots, le **état** propriété de la **Recordset** objet fournit des informations sur les enregistrements individuels dans le **Recordset**. Avant une mise à jour par lots a lieu, le **état** propriété de la **Recordset** reflète les informations concernant les enregistrements à ajouter, modifier et supprimer. Après avoir **UpdateBatch** a été appelée, le **état** propriété indique la réussite ou l’échec de l’opération. Lorsque vous passez d’un enregistrement à l’autre dans le **Recordset**, la valeur de la **état** des modifications de propriété pour décrire l’état de l’enregistrement actif.
