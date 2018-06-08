---
title: Exécuter l’application Data Quality Client | Microsoft Docs
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
- sql13.dqs.browseforservers.f1
- sql13.dqs.connecttoserver.f1
ms.assetid: 0b2aa202-7ab2-4c9d-b0f1-802588053a1e
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: befb0baa5823fbfa025f1d2f007fab6f82fd4284
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="run-the-data-quality-client-application"></a>Exécuter l'application Data Quality Client

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Exécutez [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]et ouvrez une session sur un [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)].  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Vous devez avoir terminé l'installation du [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] en exécutant le fichier DQSInstaller.exe. Pour plus d’informations, consultez [Exécuter DQSInstaller.exe pour terminer l’installation du serveur DQS](../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Pour se connecter à [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)], un utilisateur doit disposer de l'un des trois rôles DQS suivants sur la base de données DQS_MAIN : dqs_administrator, dqs_kb_editor ou dqs_kb_operator.  
  
##  <a name="Run"></a> Exécuter Data Quality Client  
 Pour exécuter [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] sur l’ordinateur où vous l’avez installé :  
  
1.  Cliquez sur **Démarrer**, pointez sur **Tous les programmes**, cliquez sur **[!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]**, cliquez sur **Data Quality Services**, puis sur **Data Quality Client**.  
  
2.  Dans la boîte de dialogue **Se connecter au serveur** :  
  
    1.  Spécifiez le serveur auquel vous souhaitez connecter l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] . Sélectionnez **(LOCAL)** pour vous connecter à [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] sur l'ordinateur local. Vous pouvez également cliquer sur la flèche bas et sélectionner **\<Rechercher d’autres serveurs sur le réseau>** pour vous connecter à un serveur différent (ou pour vous connecter au serveur local par son nom). La boîte de dialogue **Parcourir les serveurs** s'affiche. Vous pouvez sélectionner un serveur sous l'onglet **Serveurs locaux** ou sous l'onglet **Serveurs réseau** .  
  
    2.  Pour chiffrer le transfert de données entre [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] et [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)], cliquez sur **Options**, puis activez la case à cocher **Chiffrer la connexion** .  
  
3.  Cliquez sur **Se connecter**.  
  
 L’écran d’accueil [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] s’affiche. Pour plus d’informations, consultez [Écran d’accueil de Data Quality Client](../data-quality-services/data-quality-client-home-screen.md).  
  
  
