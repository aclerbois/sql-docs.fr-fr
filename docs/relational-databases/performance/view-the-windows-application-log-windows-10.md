---
title: Afficher le journal des applications Windows (Windows) | Microsoft Docs
ms.custom: ''
ms.date: 02/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing logs
- application logs [SQL Server]
- logs [SQL Server], application
- monitoring Windows NT application log [SQL Server]
- Windows NT application logs [SQL Server]
- displaying logs
- monitoring [SQL Server], events
- logs [SQL Server], viewing
ms.assetid: 168a6c6e-12df-46a9-9904-55d63ca8fe14
caps.latest.revision: 25
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2a1154f65a9fa5cc03f9fe9d964d864092250370
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="view-the-windows-application-log-windows-10"></a>Afficher le journal des applications Windows (Windows 10)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré pour vous permettre d'utiliser le journal des applications Windows, chaque session [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y enregistre des nouveaux événements. Contrairement au journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un nouveau journal des applications n'est pas créé chaque fois que vous démarrez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="view-the-windows-application-log"></a>Consulter le journal des applications Windows  
  
1. Dans la **barre de recherche**, tapez **Observateur d’événements**, puis sélectionnez l’application de bureau **Observateur d’événements**.
  
2. Dans **l’Observateur d’événements**, ouvrez **Journaux des applications et des services**.

3. Les événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont identifiés par l’entrée **MSSQLSERVER** (les instances nommées sont identifiées par **MSSQL$<nom_instance>*) dans la colonne **Source**. Les événements de SQL Server Agent sont identifiés par l’entrée SQLSERVERAGENT (pour les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommées, les événements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sont identifiés par **SQLAgent$**\<*nom_instance*>). Les événements du service Microsoft Search sont identifiés par l'entrée **Microsoft Search**.  
  
4. Pour afficher le journal d’un autre ordinateur, cliquez avec le bouton droit sur **Observateur d’événements (local)**. Sélectionnez **Se connecter à un autre ordinateur**, puis renseignez les champs de la boîte de dialogue **Sélectionner un ordinateur**.  
  
5. Si vous le souhaitez, pour afficher uniquement les événements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dans le menu **Afficher**, sélectionnez **Filtrer**. Dans la liste **Source de l’événement**, sélectionnez **MSSQLSERVER**. Pour afficher uniquement les événements de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionnez **SQLSERVERAGENT** dans la liste **Source de l'événement** .  
  
6. Pour obtenir plus de détails sur un événement particulier, double-cliquez sur cet événement.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher le journal des erreurs SQL Server &#40;SQL Server Management Studio&#41;](../../relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md)  
  
  
