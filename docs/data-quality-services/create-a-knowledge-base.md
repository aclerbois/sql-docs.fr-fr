---
title: Créer une base de connaissances | Microsoft Docs
ms.custom: ''
ms.date: 06/04/2013
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
- sql13.dqs.kb.selectkb.f1
- sql13.dqs.kb.newkb.f1
ms.assetid: 2733a284-975f-4650-abcc-cc2aad074cab
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1902c963b939987e10729b20a4d41d77e4574f48
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-knowledge-base"></a>Créer une base de connaissances

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment créer une base de connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) et la préparer pour la gestion de l'arborescence des domaines, la découverte de la connaissance ou l'ajout d'une stratégie correspondante.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour créer une base de connaissances, vous devez avoir installé [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] et [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)].  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer une base de connaissances.  
  
##  <a name="Createaknowledgebase"></a> Create a knowledge base  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Nouvelle base de connaissances**.  
  
3.  Entrez un nom et une description pour la nouvelle base de connaissances.  
  
4.  Dans **Créer la base de connaissances à partir de**, sélectionnez l'élément sur lequel fonder la base de connaissances :  
  
    -   Sélectionnez **Aucun** si vous ne souhaitez pas fonder la nouvelle base de connaissances sur une base de connaissances ou un fichier de données existants.  
  
    -   Sélectionnez **Base de connaissances existante** pour fonder la nouvelle base de connaissances sur une base de connaissances qui a déjà été créée sur [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]ou sur la base de connaissances par défaut. Sélectionnez la base de connaissances dans la liste déroulante **Sélectionner la Base de connaissances** ou cliquez sur **Parcourir** pour afficher la boîte de dialogue **Sélectionner une Base de connaissances** , sélectionnez une base de connaissances existante sur laquelle fonder la nouvelle base de connaissances, puis cliquez **OK**. Lorsque vous sélectionnez une base de connaissances à partir de la Tablet PC, les domaines et les règles correspondantes de la base de connaissances seront affichés dans le volet droit de la boîte de dialogue. Vous pouvez également sélectionner la base de connaissances **Données DQS** , qui est la base de connaissances par défaut contenant les domaines courants prêts à l’emploi et les connaissances relatives aux sociétés, adresses et données externes aux États-Unis.  
  
    -   Sélectionnez **Importer à partir d'un fichier DQS** pour fonder la nouvelle base de connaissances sur un fichier DQS de [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]. Cliquez **Parcourir**, sélectionnez un fichier de données de DQS avec une extension .dqs, puis cliquez **OK**.  
  
5.  Dans **Sélectionner une activité**, sélectionnez l'activité que vous souhaitez effectuer sur la nouvelle base de connaissances :  
  
    -   Sélectionnez **Gestion de l'arborescence du domaine** pour créer la base de connaissances et accédez aux écrans que vous utilisez pour modifier les domaines de la base de connaissances.  
  
    -   Sélectionnez **Découverte des connaissances** pour créer la base de connaissances et accédez à l'Assistant que vous utilisez pour analyser un échantillon de données et remplir les domaines de la base de connaissances avec les résultats.  
  
    -   Sélectionnez **Stratégie de correspondance** pour créer une stratégie de correspondance et l'ajouter à la base de connaissances.  
  
6.  Cliquez sur **Créer**.  
  
##  <a name="FollowUp"></a> Suivi : Après la création d'une base de connaissances  
 Après avoir créé une base de connaissances, vous voyez s'afficher un Assistant qui vous permet d'effectuer la découverte des connaissances, un Assistant qui vous permet de créer une stratégie de correspondance, ou des pages pour exécuter la gestion des domaines. Pour plus d’informations sur la découverte des connaissances, la gestion de domaine ou la stratégie de correspondance, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
  
