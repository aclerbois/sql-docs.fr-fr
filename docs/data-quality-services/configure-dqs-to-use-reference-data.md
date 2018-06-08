---
title: Configurer DQS pour utiliser des données de référence | Microsoft Docs
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
- sql13.dqs.administration.rdsconfiguration.f1
- sql13.dqs.administration.configuration.createDirectRDS.f1
- sql13.dqs.admin.config.rds.f1
ms.assetid: fae745e7-57a7-4cbc-8979-56ea8e392e4e
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 86d1f6f79aebb7cb75a0c8b361f4be8137a282ae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-dqs-to-use-reference-data"></a>Configurer DQS pour utiliser des données de référence

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment configurer [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) afin d'utiliser des données de référence pour le nettoyage de vos données. Vous pouvez utiliser des données de référence provenant de Windows Azure Marketplace ou de fournisseurs de données de référence tiers en ligne directs.  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour utiliser des données de référence de Marketplace, vous devez disposer d'une clé de compte Marketplace valide. Pour obtenir des informations détaillées sur la création d’une clé de compte Place de marché, consultez [Créer votre compte](http://go.microsoft.com/fwlink/?LinkId=212936) (http://go.microsoft.com/fwlink/?LinkId=212936). Vous pouvez également créer une clé de compte Marketplace à partir de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] . Pour ce faire, cliquez sur **Configuration** sous **Administration** dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , puis cliquez sur **Créer un ID de compte DataMarket** sous l'onglet **Données de référence** .  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_administrator sur la base de données DQS_MAIN pour configurer les paramètres du service de données de référence dans DQS.  
  
##  <a name="Marketplace"></a> Configurer DQS pour utiliser des données de référence de Marketplace  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , sous **Administration**, cliquez sur **Configuration**.  
  
3.  Sous l'onglet **Données de référence** , sous la zone **Paramètres réseau** , tapez les valeurs appropriées dans les zones **Serveur proxy** et **Port** si vous ou votre organisation utilisez un serveur proxy pour vous connecter à Internet.  
  
4.  Spécifiez la clé de compte Marketplace dans la zone **ID de compte DataMarket** , puis cliquez sur l'icône **Valider l'ID de compte DataMarket** pour valider la clé de compte. Un message indiquant si la clé de compte Marketplace spécifiée est valide s'affiche.  
  
 Vous êtes maintenant prêt à utiliser, dans DQS, les services de données de référence Marketplace faisant l'objet d'un abonnement pour la clé de compte Marketplace spécifiée.  
  
##  <a name="ThirdParty"></a> Configurer DQS pour utiliser des données de référence de fournisseurs de données de référence tiers en ligne directs  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , sous **Administration**, cliquez sur **Configuration**.  
  
3.  Sous l'onglet **Données de référence** , sous la zone **Paramètres réseau** , tapez les valeurs appropriées dans les zones **Serveur proxy** et **Port** si vous ou votre organisation utilisez un serveur proxy pour vous connecter à Internet.  
  
4.  Dans la zone **Paramètres du service de données de référence tiers en ligne direct** , cliquez sur l'icône **Ajouter un nouveau fournisseur de services de données de référence** .  
  
5.  Dans la boîte de dialogue **Créer un fournisseur de services de données de référence tiers en ligne direct** , spécifiez les détails suivants :  
  
    1.  Dans la zone **Nom** , tapez le nom du nouveau fournisseur de services de données de référence direct.  
  
    2.  (Facultatif) Dans la zone **Description** , tapez la description du nouveau fournisseur de services de données de référence direct.  
  
    3.  Dans la zone **Catégorie** , tapez la catégorie des données fournies par le nouveau fournisseur de services de données de référence direct.  
  
    4.  Dans la zone Schéma, spécifiez le schéma qui définit la chaîne des champs (noms de colonnes) à utiliser à partir du fournisseur de services de données de référence direct. Un nom de champ ne doit pas contenir d'espace, et les champs doivent être séparés par des virgules. Par exemple : `FirstName, LastName, City, State`.  
  
    5.  Dans la zone **URI** , tapez l'URI du fournisseur de services de données de référence direct. Seuls les URI sécurisés (adresse commençant par « https:// ») sont autorisés dans DQS.  
  
    6.  Dans la zone **Taille de lot maximale** , tapez le nombre maximal d'enregistrements par lot qui seront envoyés au fournisseur de services de données de référence pour être nettoyés. Il est possible de spécifier au maximum 100 enregistrements par lot pour l'activité de nettoyage.  
  
    7.  Dans la zone **ID de compte** , tapez l'ID de compte de l'abonné avec le fournisseur de services de données de référence.  
  
6.  Cliquez sur **OK** pour enregistrer les données, puis fermez la boîte de dialogue **Créer un fournisseur de services de données de référence tiers en ligne direct** . Le fournisseur de données de référence tiers en ligne direct que vous venez d'ajouter devient disponible dans la grille des **Fournisseurs de services de données de référence** dans DQS.  
  
 Vous êtes maintenant prêt à utiliser, dans DQS, les services de données de référence à partir du fournisseur de données de référence tiers en ligne direct que vous venez de configurer.  
  
##  <a name="FollowUp"></a> Suivi : Après la configuration de DQS pour utiliser des données de référence  
 Vous devez maintenant mapper les domaines de base de connaissances requis aux données de référence disponibles auprès des fournisseurs de données que vous venez de configurer. Pour ce faire, consultez [Attacher un domaine ou un domaine composite à des données de référence](../data-quality-services/attach-domain-or-composite-domain-to-reference-data.md).  
  
  
