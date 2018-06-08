---
title: Configurer le fuseau horaire - système de plateforme Analytique | Documents Microsoft
description: La page de fuseau horaire vous permet de définir le fuseau horaire pour tous les nœuds sur votre appliance Analytique plateforme système (APS).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 6a17ef4e77f9703a285f1e232077582e4441f293
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="appliance-time-zone-configuration---analytics-platform-system"></a>Configuration du fuseau horaire matériel - système de plateforme Analytique
Le **fuseau horaire** page vous permet de définir le fuseau horaire pour tous les nœuds sur votre appliance Analytique plateforme système (APS).  
  
## <a name="to-set-the-time-zone"></a>Pour définir le fuseau horaire  
  
1.  Lancez le Gestionnaire de Configuration. Pour plus d’informations, consultez [lancer le Gestionnaire de Configuration &#40;système de plateforme Analytique&#41;](launch-the-configuration-manager.md).  
  
2.  Arrêter les services d’application à l’aide de la **l’état des Services** page dans le Gestionnaire de Configuration. Consultez [l’état des Services PDW &#40;système de plateforme Analytique&#41; ](pdw-services-status.md) pour obtenir des instructions.  
  
3.  Dans le volet gauche du Gestionnaire de Configuration, cliquez sur **fuseau horaire**. Sélectionnez le fuseau horaire de votre choix à partir de la **fuseau horaire** menu déroulant. En fonction de votre emplacement, vous pouvez également choisir d’activer la case à côté **ajuster l’horloge pour l’heure d’été**.  
  
4.  Cliquez sur **appliquer** pour enregistrer vos modifications.  
  
5.  Redémarrez les services d’application à l’aide de la **l’état des Services** page dans le Gestionnaire de Configuration. Si vous souhaitez également modifier les privilèges, vous pouvez le faire avant de redémarrer l’application.  
  
![Temps des appliances DWConfig](./media/appliance-time-zone-configuration/SQL_Server_PDW_DWConfig_ApplTopTime.png "SQL_Server_PDW_DWConfig_ApplTopTime")  
  
## <a name="see-also"></a>Voir aussi  
[Lancez le Gestionnaire de Configuration &#40;Analytique plate-forme système&#41;](launch-the-configuration-manager.md)  
  
