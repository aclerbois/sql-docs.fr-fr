---
title: Ouvrir, déverrouiller, renommer et supprimer un projet de qualité des données | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dqs.dqproject.opendqproject.f1
helpviewer_keywords:
- data quality project,delete
- data quality project,rename
- data quality project,unlock
- data quality project,open
ms.assetid: de8a2b04-4673-4beb-b4cf-96a28cdf3a93
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e0c3a8c66481b3c87ea263d4362335cedd452b83
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="open-unlock-rename-and-delete-a-data-quality-project"></a>Ouvrir, déverrouiller, renommer et supprimer un projet de qualité des données

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment gérer un projet de qualité des données à l'aide de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , par exemple ouvrir, déverrouiller, renommer et supprimer un projet de qualité des données.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="LimitationsRestrictions"></a> Limitations et restrictions  
  
-   Vous ne pouvez pas ouvrir un projet verrouillé créé par un autre utilisateur.  
  
-   Vous ne pouvez pas déverrouiller, renommer ou supprimer un projet de qualité des données créé par un autre utilisateur.  
  
-   Vous ne pouvez pas supprimer un projet de qualité des données verrouillé. Vous devez d'abord le déverrouiller.  
  
-   Vous pouvez uniquement déverrouiller un projet de qualité des données que vous avez vous-même créé.  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Vous devez disposer d'au moins un projet de qualité des données à gérer.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_kb_operator sur la base de données DQS_MAIN pour gérer un projet de qualité des données.  
  
##  <a name="Open"></a> Ouvrir un projet de qualité des données  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir le projet de qualité des données**. L'écran **Ouvrir le projet** s'affiche.  
  
     Vous pouvez également ouvrir directement un projet de qualité des données répertorié sous la zone **Projet de qualité des données récent** en cliquant dessus.  
  
3.  Dans l'écran **Ouvrir le projet** , cliquez pour sélectionner le projet de qualité des données que vous voulez ouvrir, puis cliquez sur **Suivant**.  
  
4.  Le projet de qualité des données s'ouvre dans le même état d'activité que celui dans lequel il se trouvait lorsqu'il a été fermé pour la dernière fois. Un projet de qualité des données peut prendre les états suivants :  
  
    -   Pour l'activité **Nettoyage** , un projet de qualité des données peut prendre les états suivants : **Nettoyage - Mapper**, **Nettoyage - Nettoyer**, **Nettoyage – Gérer et afficher les résultats**et **Nettoyage – Exporter**.  
  
    -   Pour l'activité **Correspondance** , un projet de qualité des données peut prendre les états suivants : **Correspondance - Mapper**, **Correspondance - Correspondance**, **Correspondance – Survivance**et **Correspondance – Exporter**.  
  
##  <a name="Unlock"></a> Déverrouiller un projet de qualité des données  
 Lorsque vous créez un projet de qualité des données, il se trouve dans un état verrouillé pour empêcher son utilisation ou sa modification par d'autres utilisateurs. Vous devez déverrouiller le projet de qualité des données une fois votre travail terminé si vous voulez que d'autres utilisateurs travaillent dessus. Un symbole de verrou est affiché pour les projets verrouillés.  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir le projet de qualité des données**. L'écran **Ouvrir le projet** s'affiche.  
  
3.  Dans l'écran **Ouvrir le projet** , cliquez avec le bouton droit sur un projet de qualité des données verrouillé que vous avez créé, puis cliquez sur **Déverrouiller** dans le menu contextuel. Une coche verte s'affiche pour le projet, indiquant qu'il est déverrouillé.  
  
##  <a name="Rename"></a> Renommer un projet de qualité des données  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir le projet de qualité des données**. L'écran **Ouvrir le projet** s'affiche.  
  
3.  Dans l'écran **Ouvrir le projet** , cliquez avec le bouton droit sur un projet de qualité des données verrouillé que vous avez créé, puis cliquez sur **Renommer** dans le menu contextuel.  
  
4.  Le nom du projet de qualité des données peut être modifié dans la colonne **Nom** . Tapez un nouveau nom, puis appuyez sur Entrée.  
  
##  <a name="Delete"></a> Supprimer un projet de qualité des données  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir le projet de qualité des données**. L'écran **Ouvrir le projet** s'affiche.  
  
3.  Dans l'écran **Ouvrir le projet** , cliquez avec le bouton droit sur un projet de qualité des données déverrouillé que vous avez créé, puis cliquez sur **Supprimer** dans le menu contextuel.  
  
4.  Un message de confirmation s'affiche. Cliquez sur **Oui**.  
  
  
