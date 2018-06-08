---
title: Options de validation d’abonnement (abonnements transactionnels) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.validate.options.f1
helpviewer_keywords:
- Subscription Validation Options dialog box
ms.assetid: fd66ad1f-df01-4240-9e89-8f41bff12c1e
caps.latest.revision: 18
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 546a8f6e13cef68a25de66b1a9a75bb9d582a3d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="subscription-validation-options-transactional-subscriptions"></a>Options de validation d'abonnement (abonnements transactionnels)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Utilisez la boîte de dialogue **Options de validation d'abonnement** pour spécifier si la validation doit utiliser uniquement un nombre de lignes ou un nombre de lignes et une somme de contrôle binaire.  
  
## <a name="options"></a>Options  
 **Vérifiez si l'abonné possède le même nombre de lignes de données répliquées que le serveur de publication**  
 Sélectionnez le type de validation du nombre de lignes à effectuer. Pour les publications Oracle, la seule option proposée est **Calculer le nombre réel de lignes en interrogeant directement les tables**.  
  
 **Comparer les sommes de contrôle pour vérifier les données de ligne**  
 Outre le comptage des lignes sur le serveur de publication et sur l'Abonné, une somme de contrôle de toutes les données est calculée à l'aide de l'algorithme de somme de contrôle binaire. Si le nombre de lignes est erroné, la somme de contrôle n'est pas effectuée.  
  
 **Arrêter l'Agent de distribution une fois la validation terminée**  
 Par défaut, l'Agent de distribution s'exécute en permanence. Sélectionnez cette option pour arrêter l'agent lorsque la validation est terminée. Cela permet de vérifier que la validation a réussi avant de continuer à répliquer des données vers l'Abonné.  
  
## <a name="see-also"></a> Voir aussi  
 [Valider des données sur l’abonné](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [Valider des données répliquées](../../relational-databases/replication/validate-replicated-data.md)  
  
  
