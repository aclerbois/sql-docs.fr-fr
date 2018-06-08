---
title: Utiliser les relations de valeur dans un domaine composite | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2011
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
- sql13.dqs.dm.cdvaluerelations.f1
ms.assetid: 5ee468f0-8538-4620-90e8-63f466c9000e
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4cbe8f78a5f6330a3c934b4097ebf0ae454fb592
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-value-relations-in-a-composite-domain"></a>Utiliser les relations de valeur dans un domaine composite

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment afficher les combinaisons de valeurs trouvées pour le domaine composite pendant le processus de découverte des connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Cette page affiche le nombre d'occurrences des combinaisons de valeur. La gestion de valeur n'est pas prise en charge pour les domaines composites, vous ne pouvez donc pas exécuter d'opérations sur ces valeurs.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour afficher les relations de valeur, vous devez avoir créé et ouvert un domaine composite.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour afficher les relations de valeur dans un domaine composite.  
  
##  <a name="Use"></a> Afficher les relations de valeur  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , ouvrez ou créez une base de connaissances. Sélectionnez **Gestion de l'arborescence du domaine** comme activité, puis cliquez sur **Ouvrir** ou **Créer**. Pour plus d’informations, consultez [Créer une base de connaissances](../data-quality-services/create-a-knowledge-base.md) ou [Ouvrir une base de connaissances](../data-quality-services/open-a-knowledge-base.md).  
  
3.  Dans **Liste des domaines** de la page **Gestion de l'arborescence du domaine** , sélectionnez le domaine composite pour lequel vous souhaitez créer une règle de domaine, ou créez un nouveau domaine composite. Si vous devez créer un domaine, consultez [Create a Composite Domain](../data-quality-services/create-a-composite-domain.md).  
  
4.  Cliquez sur l'onglet **Relations de valeurs** .  
  
5.  Affichez les fréquences affichées pour chaque combinaison de valeurs.  
  
    > [!NOTE]  
    >  La table **Valeur** affiche chaque combinaison de valeurs qui existe dans le domaine composite. Chaque valeur est affichée dans le domaine simple auquel elle s'applique. Le tri par défaut de la table de relations de valeur se fait sur la fréquence, mais vous pouvez cliquer sur une autre colonne pour trier sur cette colonne. Seules les valeurs ayant une fréquence supérieure ou égale à 20 sont affichées.  
  
6.  Vous ne pouvez pas modifier l'une des valeurs de la table. Si vous avez exécuté d'autres opérations, cliquez sur **Terminer** pour terminer l'activité de gestion de domaine. Sinon, cliquez sur **Annuler**.  
  
##  <a name="FollowUp"></a> Suivi : Après affichage des relations de valeur  
 Après avoir affiché les relations de valeur, vous pouvez effectuer d'autres tâches de gestion des domaines sur le domaine, effectuer une découverte des connaissances pour ajouter des connaissances au domaine ou ajouter une stratégie de correspondance au domaine. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
  
