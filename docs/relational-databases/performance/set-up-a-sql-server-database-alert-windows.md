---
title: Configurer une alerte de base de données SQL Server (Windows) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server], creating
ms.assetid: 65d2c5c1-921f-4eff-9ef7-149170ab61e8
caps.latest.revision: 23
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 0aefc9bac8fc8ca26b759f1229bbe98cbb20ba93
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="set-up-a-sql-server-database-alert-windows"></a>Configurer une alerte de base de données SQL Server (Windows)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez utiliser le Moniteur système pour créer une alerte devant être déclenchée quand la valeur seuil est atteinte par le compteur du Moniteur système. En réponse à l'alerte, le Moniteur système peut lancer une application, telle qu'une application personnalisée écrite pour prendre en charge la condition d'alerte. Par exemple, vous pouvez créer une alerte déclenchée quand le nombre de blocages dépasse une valeur spécifique. 
  
 Vous pouvez également définir des alertes à l’aide de Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Alertes](http://msdn.microsoft.com/library/3f57d0f0-4781-46ec-82cd-b751dc5affef).  
  
## <a name="set-up-a-sql-server-database-alert"></a>Configurer une alerte de base de données SQL Server  
  
1. Dans l’arborescence de navigation de la fenêtre **Performances**, développez **Journaux et alertes de performance**.  
  
2. Cliquez avec le bouton droit sur **Alertes**, puis sélectionnez **Nouveaux paramètres d’alerte**.
  
3. Dans la boîte de dialogue **Nouveaux paramètres d’alerte**, tapez un nom pour la nouvelle alerte, puis sélectionnez **OK**.  
  
4. Sous l’onglet **Général** de la boîte de dialogue de la nouvelle alerte, ajoutez un **commentaire**. Sélectionnez **Ajouter** pour ajouter un compteur à l’alerte.  
  
     Chaque alerte doit avoir au moins un compteur.  
  
5. Dans la boîte de dialogue **Ajouter des compteurs**, sélectionnez un objet SQL Server dans la liste **Objet de performance**. Sélectionnez un compteur dans la liste **Sélectionner des compteurs dans**.  
  
6. Pour ajouter le compteur à l’alerte, sélectionnez **Ajouter**. Vous pouvez continuer à ajouter des compteurs, ou bien sélectionnez **Fermer** pour revenir à la boîte de dialogue de la nouvelle alerte.  
  
7. Dans la boîte de dialogue de la nouvelle alerte, sélectionnez **Supérieur à** ou **Inférieur à** dans la liste **Avertir si la valeur est**. Entrez une valeur de seuil dans **Limite**.  
  
     L’alerte est générée lorsque la valeur du compteur est supérieure ou inférieure à la valeur du seuil (selon que vous avez sélectionné **Supérieur à** ou **Inférieur à**).  
  
8. Dans les zones **Période d’échantillonnage des données** , définissez la fréquence d’échantillonnage.  
  
9. Sous l’onglet **Action** , définissez les actions qui doivent se produire lorsque l’alerte est déclenchée.  
  
10. Sous l’onglet **Planification** , définissez la planification de début et de fin de l’analyse d’alerte.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une alerte de base de données SQL Server](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md)  
  
  
