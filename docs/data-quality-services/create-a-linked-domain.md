---
title: Créer un domaine lié | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2011
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
- sql13.dqs.kb.linkeddomain.f1
ms.assetid: fd99d422-c53d-4d7c-9cdd-303c703683b6
caps.latest.revision: 20
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: bf2c8fa7c15539eedef198f9bac03d0b2d073bb4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-linked-domain"></a>Créer un domaine lié

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment créer un domaine lié dans une base de connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Un domaine lié est créé à partir d'un autre domaine existant, et hérite de toutes les valeurs, règles et propriétés du domaine auquel il est lié, à l'exception du nom et de la description. Vous pouvez gérer un ensemble de domaines liés comme un seul domaine. En liant un domaine à l'autre, vous créez un domaine qui hérite son contenu de l'autre domaine.  
  
## <a name="scenarios"></a>Scénarios  
 Les domaines liés sont particulièrement utiles dans les scénarios suivants.  
  
### <a name="mapping-multiple-fields-to-domains-that-share-values-rules-and-properties"></a>Mappage de plusieurs champs à des domaines qui partagent des valeurs, des règles et des propriétés  
 Vous ne pouvez pas mapper deux champs au même domaine, mais vous pouvez mapper un champ à un domaine, puis mapper un deuxième champ à un domaine lié au premier domaine. Les champs sont ainsi mappés à deux domaines différents qui ont le même contenu et les mêmes propriétés (à l'exception du nom et de la description). Pour plus d’informations, consultez [Map two fields to linked domains](#Map).  
  
### <a name="controlling-data-flow-to-composite-domains"></a>Contrôle du flux de données dans des domaines composites  
 Les domaines liés vous permettent de contrôler le flux de données entre les champs et les domaines composites. Vous pouvez distinguer le moment où des données d'un champ passent dans un domaine composite du moment où les données d'un autre champ très similaire ne passent pas dans un domaine composite. Pour ce faire, spécifiez qu'entre deux domaines liés, l'un fait partie d'un domaine composite, et l'autre non. Du point de vue des domaines, les domaines liés sont identiques. Ils contiennent les mêmes connaissances. Toutefois, du point de vue des domaines composites, les domaines liés sont différents. L'un participe au domaine composite, et l'autre non.  
  
 C'est le cas, par exemple, si un enregistrement contient les champs suivants : Prénom du client, Nom de famille du client et Prénom du parent. Supposons que vous mappez le prénom du client et le prénom du parent à un domaine Prénom, puis que vous définissiez le domaine Prénom et le domaine Nom de famille en tant que partie d'un domaine composite Nom complet. Le problème est que le prénom du parent sera ajouté au domaine composite sans nom de famille. Si, en revanche, vous liez chacun des deux champs de prénom à un domaine, puis que vous liez les deux domaines, vous pouvez alors ajouter le domaine Prénom du client au domaine composite Nom complet, et ne pas ajouter le champ Prénom du parent au domaine composite, ce qui évite que le Prénom du parent soit ajouté au domaine composite.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour créer un domaine lié, vous devez disposer d'une base de connaissances et d'un domaine existant auquel établir un lien.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer un domaine lié.  
  
##  <a name="Create"></a> Créer un domaine lié  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , ouvrez ou créez une base de connaissances. Sélectionnez **Gestion de l'arborescence du domaine** comme activité, puis cliquez sur **Ouvrir** ou **Créer**. Pour plus d’informations, consultez [Créer une base de connaissances](../data-quality-services/create-a-knowledge-base.md) ou [Ouvrir une base de connaissances](../data-quality-services/open-a-knowledge-base.md).  
  
3.  Dans la liste **Domaine** de la page **Gestion de l'arborescence du domaine** , cliquez avec le bouton droit sur le domaine auquel vous voulez lier un nouveau domaine, puis cliquez sur **Créer un domaine lié**.  
  
    > [!NOTE]  
    >  Aucune icône n'est dédiée à la création d'un domaine lié. Vous ne pouvez effectuer cette opération qu'à l'aide de la commande du menu contextuel.  
  
4.  Dans la boîte de dialogue **Créer un domaine** , entrez un nom qui est unique dans la base de connaissances et une description limitée à 256 caractères. Vérifiez que le nom du domaine avec lequel effectuer le lien est correct.  
  
5.  Cliquez sur **OK** pour terminer la création du domaine lié.  
  
6.  Si nécessaire, vous pouvez modifier le nom ou la description du domaine lié sous l'onglet Propriétés du domaine.  
  
7.  Cliquez sur **Terminer** pour terminer l'activité de gestion de l'arborescence du domaine, comme décrit dans [End the Domain Management Activity](http://msdn.microsoft.com/library/ab6505ad-3090-453b-bb01-58435e7fa7c0).  
  
##  <a name="Map"></a> Map two fields to linked domains  
  
1.  Ouvrez une base de connaissances dans l'activité de découverte des connaissances, puis mappez la base de connaissances à la base de données et à la table ou à la vue.  
  
2.  Mappez un champ à un domaine, puis essayez de mapper un deuxième champ au même domaine.  
  
3.  Dans la fenêtre indiquant que le domaine est déjà en cours d'utilisation, cliquez sur Oui pour créer un domaine lié.  
  
4.  Dans la boîte de dialogue Créer un domaine, entrez un nom de domaine et une description, puis cliquez sur OK.  
  
##  <a name="FollowUp"></a> Suivi : Après avoir créé un domaine lié  
 Après avoir créé un domaine lié, vous pouvez effectuer d'autres tâches de gestion de l'arborescence du domaine sur le domaine, effectuer une découverte des connaissances pour ajouter des connaissances au domaine ou ajouter une stratégie de correspondance au domaine. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="Behavior"></a> Comportement d'un domaine lié  
 Vous pouvez modifier les paramètres d'un domaine lié comme suit :  
  
-   Vous pouvez modifier le nom et la description d'un domaine lié.  
  
-   Pour modifier les propriétés **Type de données**, **Utiliser des valeurs par défaut**ou **Mettre en forme la sortie vers** du domaine, sélectionnez le domaine auquel vous avez établi le lien, puis modifiez ces paramètres sous l'onglet **Propriétés du domaine** pour ce domaine. Vous ne pouvez pas modifier ces paramètres dans les propriétés du domaine lié. Pour plus d’informations, consultez [Créer un domaine](../data-quality-services/create-a-domain.md).  
  
-   Les paramètres disponibles sous les onglets **Données de référence**, **Règles du domaine**, **Valeurs du domaine**et **Relations à base de termes** de la page Gestion de l'arborescence du domaine peuvent être modifiés pour le domaine lié ou pour le domaine auquel il a été lié, et les modifications apportées seront héritées par l'autre domaine.  
  
 Les domaines liés présentent les caractéristiques suivantes :  
  
-   Vous ne pouvez pas dissocier deux domaines. Pour supprimer le lien, supprimez l'un des domaines liés.  
  
-   Lorsque vous sélectionnez un domaine lié dans la liste des domaines de la page Gestion de l'arborescence du domaine, la chaîne qui identifie le domaine lié dans le volet contenant la table **Valeur** indique que le domaine est un domaine lié.  
  
-   Si vous supprimez un domaine auquel un autre domaine est lié, les deux domaines sont supprimés. Vous pouvez toutefois supprimer un domaine lié sans que le domaine auquel le lien est établi n'est pas supprimé.  
  
-   Un domaine lié ne peut pas être lié à un domaine qui est lui-même lié à un autre domaine.  
  
-   Vous ne pouvez pas créer un domaine lié ou un domaine composite lié à un domaine composite.  
  
-   Lorsque vous double-cliquez sur un domaine lié sous n'importe lequel des onglets Gestion de l'arborescence du domaine, le domaine s'ouvre pour modification, avec la chaîne de nom indiquant qu'il s'agit d'un domaine lié.  
  
  
