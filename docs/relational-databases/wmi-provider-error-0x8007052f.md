---
title: Erreur WMI 0x8007052f | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.service: ''
ms.component: relational-databases-misc
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- 0x8007052f (WMI error)
ms.assetid: a33f12bd-15c4-42a8-b343-de44c3e87729
caps.latest.revision: 10
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 06f5b8000dcd43b12ed5d25ddc733fb892d385f3
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="wmi-provider-error-0x8007052f"></a>Erreur du fournisseur WMI 0x8007052f
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|0x8007052f|  
|Source de l'événement|Erreur du fournisseur WMI|  
|Composant|Gestionnaire de configuration SQL Server|  
|Nom symbolique|N/A|  
|Texte du message|Échec d'ouverture de session : restriction de compte d'utilisateur. Parmi les explications possibles figurent les mots de passe vides qui ne sont pas autorisés, les restrictions liées aux heures de connexion ou l'application d'une restriction de stratégie.|  
  
## <a name="explanation"></a>Explication  
 Cette erreur peut se produire lors de l'utilisation du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour basculer vers les comptes intégrés (Service réseau, Service local ou Système Local) lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est exécuté sur un cluster de serveurs Windows Server ou un contrôleur de domaine Windows Server. N'exécutez pas de services sous les comptes intégrés sur un cluster de serveurs Windows Server ou un contrôleur de domaine Windows Server. Pour obtenir des recommandations sur les comptes de service, consultez la rubrique « Configuration des comptes de service Windows » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Configurez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour utiliser un compte de domaine.  
  
  
