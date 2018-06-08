---
title: Importer une base de connaissances à partir d’un fichier .dqs | Microsoft Docs
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
ms.assetid: 9b9786fe-9e80-429a-afcb-dc3b3dd6f0b0
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2e1c3c9856aa67672b9bb586cf67ca05a5494220
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-a-knowledge-base-from-a-dqs-file"></a>Importer une base de connaissances à partir d'un fichier .dqs

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment importer une base de connaissances entière à partir d'un fichier de données .dqs dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Vous créez le fichier de données en exportant une base de connaissances existante de l’application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] (consultez [Exporter une base de connaissances vers un fichier .dqs](../data-quality-services/export-a-knowledge-base-to-a-dqs-file.md)).  
  
 L'utilisation d'un fichier de données .dqs pour exporter le contenu d'une base de connaissances, puis importer le contenu dans une autre base de connaissances sur le même [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] ou un [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] différent simplifie le processus de génération de la connaissance, en permettant de gagner du temps et d'économiser les efforts. Il vous permet de partager une base de connaissances et ses connaissances avec d'autres, ce qui leur fait gagner du temps. Le fichier .dqs contient toutes les informations de la base de connaissances, y compris les domaines et la stratégie de correspondance, à l'exception des informations de données de référence jointes. Les données publiées et non publiées seront importées.  
  
 Un fichier de données .dqs est chiffré, et ne peut pas être affiché.  
  
 Lorsque vous importez une base de connaissances, vous pouvez utiliser le même nom, à moins que le nom de la base de connaissances existe déjà dans l'application cliente, auquel cas vous devez la renommer.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour importer une base de connaissances à partir d'un fichier .dqs, vous devez avoir déjà exporté la base de connaissances dans le fichier .dqs.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour importer une base de connaissances à partir d'un fichier de données .dqs.  
  
##  <a name="Import"></a> Importer une base de connaissances à partir d'un fichier .dqs  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Nouvelle base de connaissances**.  
  
3.  Entrez un nom pour la base de connaissances.  
  
4.  Cliquez sur la flèche bas pour **Créer la base de connaissances à partir de**, puis sélectionnez **Importer à partir d'un fichier DQS**.  
  
5.  Pour **Sélectionnez le fichier de données**, cliquez sur **Parcourir**.  
  
6.  Dans la boîte de dialogue **Importer à partir d'un fichier de données** , accédez au dossier qui contient le fichier .dqs à importer, puis cliquez sur le nom du fichier. Cliquez sur **Ouvrir**.  
  
7.  Vérifiez que la base de connaissances et les domaines corrects sont affichés dans la liste **Domaine** .  
  
8.  Sélectionnez l'activité que vous souhaitez effectuer, puis cliquez sur **Créer**.  
  
9. Dans la boîte de dialogue **Importer la base de connaissances** , vérifiez que la ligne d'état indique que l'importation est terminée. Cliquez sur **OK**.  
  
10. Effectuez les tâches de découverte des connaissances, de gestion des domaines ou de stratégie de correspondance que vous devez effectuer, puis cliquez sur **Terminer**.  
  
11. Cliquez sur **Publier** pour publier la connaissance dans la base de connaissances ou sur **Non** dans le cas contraire.  
  
12. Si vous avez publié la base de connaissances, cliquez sur **OK**.  
  
13. Dans la page d'accueil de Data Quality Services, vérifiez que la base de connaissances est répertoriée sous **Base de connaissances récentes**.  
  
##  <a name="FollowUp"></a> Suivi : Après l'importation d'une base de connaissances à partir d'un fichier .dqs  
 Après avoir importé une base de connaissances à partir d'un fichier .dqs, vous pouvez ajouter la connaissance à la base de connaissances ou utiliser la base de connaissances dans un projet de nettoyage ou de correspondance, selon le contenu de la base de connaissances. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md), [Gestion d’un domaine composite](../data-quality-services/managing-a-composite-domain.md), [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md), [Nettoyage des données](../data-quality-services/data-cleansing.md) ou [Correspondance de données](../data-quality-services/data-matching.md).  
  
  
