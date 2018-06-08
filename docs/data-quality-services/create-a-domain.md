---
title: Créer une règle de domaine | Microsoft Docs
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
- sql13.dqs.kb.createdomain.f1
ms.assetid: 5c4828f5-bd51-4c29-b3de-87b7d2f2d3e5
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d537cfab052e9b569d88600eadf9c74d3df7d846
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-domain"></a>Créer un domaine

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment créer un domaine dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Les valeurs du domaine sont une représentation sémantique des données d'un champ. Pour plus d’informations sur les domaines, consultez [Gestion d’un domaine](../data-quality-services/managing-a-domain.md).  
  
 Il existe deux méthodes pour créer un domaine : La première se situe pendant l'étape Mapper de l'activité de découverte des connaissances, lorsque vous êtes en cours d'analyse d'un exemple de données pour ajouter des connaissances à une base nouvelle ou existante. La deuxième se situe pendant l'activité de gestion de l'arborescence du domaine quand, au lieu de modifier un domaine existant, vous en créez un nouveau.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour créer un domaine, vous devez avoir créé et ouvert une base de connaissances.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer un domaine.  
  
##  <a name="Discovery"></a> Créer un domaine dans l'activité de découverte des connaissances  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir la base de connaissances** , puis sélectionnez une base de connaissances ou cliquez sur **Nouvelle Base de connaissances** et entrez les propriétés de la base de connaissances.  
  
3.  Sélectionnez **Découverte des connaissances** comme activité, puis cliquez sur **Créer** pour créer la base de connaissances ou sur **Ouvrir** pour ouvrir une base de connaissances existante.  
  
4.  Dans la page **Mapper** , spécifiez une connexion à la source de données. Pour plus d'informations, consultez [Perform Knowledge Discovery](../data-quality-services/perform-knowledge-discovery.md).  
  
5.  Dans la table **Mappages** , sélectionnez une colonne source dans la liste déroulante de la colonne **Colonne source** d'une ligne vide. Si aucun domaine correspondant n'existe, cliquez sur l'icône **Créer un domaine** .  
  
##  <a name="DomainManagement"></a> Créer un domaine dans l'activité de gestion de l'arborescence du domaine  
  
1.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir la base de connaissances** , puis sélectionnez une base de connaissances ou cliquez sur **Nouvelle Base de connaissances** et entrez les propriétés de la base de connaissances.  
  
2.  Sélectionnez **Gestion de l'arborescence du domaine** comme activité, puis cliquez sur **Créer** pour créer la base de connaissances ou sur **Ouvrir** pour ouvrir une base de connaissances existante.  
  
3.  Dans la page **Gestion de l'arborescence du domaine** , cliquez sur l'icône **Créer un domaine** au-dessus de la liste des domaines.  
  
##  <a name="Properties"></a> Définir des propriétés de domaine  
  
1.  Dans la boîte de dialogue **Créer un domaine** , entrez un nom qui est unique dans la base de connaissances et une description limitée à 256 caractères.  
  
    > [!NOTE]  
    >  Pour plus d'informations sur les propriétés des domaines, consultez [Set Domain Properties](../data-quality-services/set-domain-properties.md).  
  
2.  Dans la liste **Type de données** , sélectionnez un type de données pour les valeurs dans le domaine. Le type de données peut être **String** (valeur par défaut), **Date**, **Integer**ou **Decimal**.  
  
3.  Sélectionnez **Utiliser des valeurs de début** pour spécifier que la valeur de début d'un groupe de synonymes sera sortie au lieu d'une valeur qui lui est synonyme. Désélectionnez **Utiliser des valeurs de début** pour spécifier que chaque valeur de synonyme est générée sous sa forme correcte ou corrigée, et n'est pas remplacée par la valeur de début de son groupe.  
  
4.  Si le type de données est **String**, sélectionnez **Normaliser la chaîne** pour supprimer les caractères spéciaux dans les valeurs de domaine, ce qui peut améliorer la probabilité des correspondances.  
  
5.  Dans la liste déroulante de **Mettre en forme la sortie vers** , sélectionnez la mise en forme qui sera appliqué lors de la sortie des valeurs de données du domaine. La mise en forme est spécifique au type de données sélectionné à l'étape 2, comme indiqué dans la liste suivante :  
  
    -   Pour une valeur de chaîne, vous pouvez spécifier que la chaîne soit générée en majuscules, en minuscules, ou avec la première lettre en majuscule.  
  
    -   Pour une valeur de date, vous pouvez spécifier le format du jour, du mois et de l'année.  
  
    -   Pour une valeur entière, vous pouvez spécifier le type de masque de format à appliquer.  
  
    -   Pour une valeur décimale, vous pouvez spécifier la précision et le type de masque de format à appliquer.  
  
     Si vous sélectionnez **Aucun** dans la liste déroulante du **Mettre en forme la sortie vers** aucun des formats dans la liste ne sera appliqué.  
  
6.  Si le type de données est **String**, dans la liste déroulante **Langue** , sélectionnez la version linguistique du vérificateur d'orthographe que vous souhaitez appliquer si vous activez ce dernier.  
  
7.  Si le type de données est **String**, sélectionnez **Activer le vérificateur d'orthographe** pour exécuter le vérificateur orthographique sur toutes les valeurs de chaîne en remplissant le domaine.  
  
8.  Si le type de données est **String**, sélectionnez **Désactiver les algorithmes d'erreur de syntaxe** pour remplir le domaine sans vérifier les valeurs de chaîne pour les erreurs de syntaxe.  
  
9. Cliquez sur **OK**.  
  
10. Cliquez sur **Terminer** pour terminer l'activité de gestion de l'arborescence du domaine, comme décrit dans [End the Domain Management Activity](http://msdn.microsoft.com/library/ab6505ad-3090-453b-bb01-58435e7fa7c0).  
  
##  <a name="FollowUp"></a> Suivi : Après la création d'un domaine  
 Après avoir créé un domaine, vous pouvez effectuer d'autres tâches de gestion des domaines sur le domaine, effectuer une découverte des connaissances pour ajouter des connaissances au domaine ou ajouter une stratégie de correspondance au domaine. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
  
