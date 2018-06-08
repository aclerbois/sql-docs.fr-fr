---
title: Activer l’option Verrouiller les pages en mémoire (Windows) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Lock Pages in Memory option
ms.assetid: cd581fbc-4747-439e-87f9-2f18e39c5bb9
caps.latest.revision: 35
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: aef0ca374aa9ccc08d50814c967a4fbee755ea64
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enable-the-lock-pages-in-memory-option-windows"></a>Activer l'option Verrouiller les pages en mémoire (Windows)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette stratégie Windows détermine quels comptes peuvent utiliser un processus destiné à conserver les données en mémoire physique pour éviter leur pagination en mémoire virtuelle sur le disque.  
  
> [!NOTE]  
>  Le verrouillage des pages en mémoire permet d'optimiser les performances lorsque la pagination de la mémoire sur disque est prévue.  
  
 L'outil Stratégie de groupe de Windows (gpedit.msc) vous permet d'activer cette stratégie pour le compte utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous devez être administrateur système pour modifier cette stratégie.  
  
### <a name="to-enable-the-lock-pages-in-memory-option"></a>Pour activer l'option Verrouiller les pages en mémoire  
  
1.  Dans le menu **Démarrer** , cliquez sur **Exécuter**. Dans la zone **Ouvrir** , tapez **gpedit.msc**.  
  
2.  Sur la console **Éditeur de stratégie de groupe locale** , développez **Configuration ordinateur**, puis **Paramètres Windows**.  
  
3.  Développez **Paramètres de sécurité**, puis **Stratégies locales**.  
  
4.  Sélectionnez le dossier **Attribution des droits utilisateur** .  
  
     Les stratégies s'affichent dans le volet Détails.  
  
5.  Dans le volet, double-cliquez sur **Verrouiller les pages en mémoire**.  
  
6.  Dans la boîte de dialogue **Paramètre de sécurité locale – Verrouiller les pages en mémoire** , cliquez sur **Ajouter un utilisateur ou un groupe**.  
  
7.  Dans la boîte de dialogue **Sélectionner des utilisateurs, comptes de service ou groupes**, sélectionnez le compte de service SQL Server.  
  
8.  Redémarrez le service SQL Server pour appliquer ce paramètre.
  
## <a name="see-also"></a> Voir aussi  
 [server memory (options de configuration du serveur)](../../database-engine/configure-windows/server-memory-server-configuration-options.md)  
  
  
