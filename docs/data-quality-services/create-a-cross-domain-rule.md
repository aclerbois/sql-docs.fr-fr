---
title: Créer une règle inter-domaines | Microsoft Docs
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
- sql13.dqs.dm.testcdrule.f1
- sql13.dqs.dm.cdrules.f1
ms.assetid: 0f3f5ba4-cc47-4d66-866e-371a042d1f21
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 3fcfb54a15a5d756ef2ca0be7ab142bbae508fdf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-cross-domain-rule"></a>Créer une règle inter-domaines

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment créer une règle inter-domaines pour un domaine composite d'une base de connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Une règle inter-domaines teste la relation entre les valeurs des domaines uniques inclus dans un domaine composite. Une règle inter-domaines doit contenir true sur un domaine composite pour que les valeurs de domaine soient considérées comme exactes et conformes aux besoins de l'entreprise. Une règle inter-domaines est utilisée pour valider, corriger et normaliser les valeurs de domaine.  
  
 La clause If et la clause Then d'une règle inter-domaines sont définies chacune pour l'un des domaines uniques du domaine composite. Chaque clause doit être définie pour un seul domaine. Une règle inter-domaines doit être en relation avec plusieurs domaines uniques ; vous ne pouvez pas définir une règle simple de domaine (pour un seul domaine) pour un domaine composite. Pour cela, définissez une règle de domaine pour un seul domaine. La clause If et la clause Then peuvent chacune contenir une ou plusieurs conditions.  
  
 Une règle inter-domaines ayant des conditions définitives applique la logique de règles aux synonymes de la valeur des conditions, ainsi qu'aux valeurs elles-mêmes. Les conditions définitives pour les clauses If et Then sont La valeur est égale à, La valeur n'est pas égale à, La valeur se trouve dans, ou La valeur ne se trouve pas dans. Par exemple, imaginons que vous ayez la règle inter-domaines suivante pour un domaine composite : « Pour « Ville », si la valeur est égale « à Los Angeles », alors pour « État », la valeur est égale à « CA ». « Si « Los Angeles » et « LA » sont synonymes, cette règle retourne comme corrects « Los Angeles CA » et « LA CA », et comme erronés « Los Angeles WA » et « LA WA ».  
  
 En dehors de vous permettre de connaître la validité d'une règle inter-domaines, la clause finale *Then* d'une règle inter-domaines, **La valeur est égale à**, corrige également les données pendant l'activité de nettoyage. Pour plus d'informations, consultez [Data Correction using Definitive Cross-Domain Rules](../data-quality-services/cleanse-data-in-a-composite-domain.md#CDCorrection) dans [Cleanse Data in a Composite Domain](../data-quality-services/cleanse-data-in-a-composite-domain.md).  
  
 Les règles inter-domaines sont prises en compte après toutes les règles simples qui affectent seulement un domaine. La règle inter-domaines n'est appliquée que si une valeur franchit les règles de domaine simple (si elles existent). Le domaine composite et les domaines uniques sur lesquels une règle s'exécute doivent tous être définis avant que la règle ne puisse être exécutée.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour créer une règle inter-domaines, vous devez avoir créé et ouvert un domaine composite.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer une règle inter-domaines.  
  
##  <a name="Create"></a> Créer les règles inter-domaines  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , ouvrez ou créez une base de connaissances. Sélectionnez **Gestion de l'arborescence du domaine** comme activité, puis cliquez sur **Ouvrir** ou **Créer**. Pour plus d’informations, consultez [Créer une base de connaissances](../data-quality-services/create-a-knowledge-base.md) ou [Ouvrir une base de connaissances](../data-quality-services/open-a-knowledge-base.md).  
  
    > [!NOTE]  
    >  La gestion de domaine est exécutée dans une page du client Data Quality Service qui contient cinq onglets pour les opérations distinctes de gestion de domaine. Ce n'est pas un processus piloté par l'Assistant ; toute opération de gestion peut être exécutée séparément.  
  
3.  Dans **Liste des domaines** de la page **Gestion de l'arborescence du domaine** , sélectionnez le domaine composite pour lequel vous souhaitez créer une règle de domaine, ou créez un nouveau domaine composite. Si vous devez créer un domaine, consultez [Create a Composite Domain](../data-quality-services/create-a-composite-domain.md).  
  
4.  Cliquez sur l'onglet **Règles de domaine composite** .  
  
5.  Dans la page **Ajouter une nouvelle règle de domaine**, entrez le nom et la description de la règle.  
  
6.  Sélectionnez **Active** pour spécifier que la règle sera exécutée (valeur par défaut), et désactivez l'option pour empêcher la règle de s'exécuter.  
  
7.  Créez la clause If comme suit :  
  
    1.  Dans la liste des domaines du volet de la clause If, sélectionnez l'un des domaines uniques inclus dans le domaine composite comme objet de la clause If. Vous en pouvez sélectionner un seul domaine du domaine composite.  
  
    2.  Sélectionnez une condition dans la liste déroulante pour la première condition de la clause.  
  
    3.  Si la condition requiert une valeur, entrez la valeur dans la zone de texte associée à la condition.  
  
    4.  Si la clause If requiert une autre condition, cliquez sur **Ajoute une nouvelle condition à la clause sélectionnée.** Sélectionnez l'opérateur, sélectionnez une condition, puis entrez une valeur pour la condition, si nécessaire.  
  
    5.  Pour modifier l'ordre des conditions, sélectionnez une condition en cliquant à sa gauche, puis cliquez sur la flèche haut ou bas.  
  
    6.  Pour masquer les conditions, cliquez sur le signe moins à gauche du nom de domaine. Cliquez sur le signe plus pour afficher les conditions.  
  
8.  Créez la clause Then en sélectionnant un seul domaine, autre que l'objet de la clause If, dans la liste des domaines du volet de la clause Then. Puis, générez la clause Then puis en utilisant les mêmes étapes que celles de la construction de la clause If.  
  
9. Poursuivez jusqu'à la procédure de test ci-dessous.  
  
##  <a name="Test"></a> Tester les règles inter-domaines  
  
1.  Testez la règle interdomaines comme suit :  
  
    1.  Cliquez sur l'icône **Exécuter la règle de domaine sélectionnée sur des données de test** dans l'angle supérieur droit du volet du domaine composite.  
  
    2.  Dans la boîte de dialogue **Tester une règle de domaine** , cliquez sur l'icône **Ajoute un nouveau terme de test pour la règle de domaine** .  
  
    3.  Entrez les valeurs de test pour le domaine unique associé à la clause If et le domaine unique associé à la clause Then. Les valeurs de test écrites dans la clause If doivent répondre aux conditions de cette clause, ou un point d'interrogation sera saisi dans la colonne **Validité** indiquant que la règle inter-domaines ne s'applique pas aux données de test.  
  
    4.  Cliquez à nouveau sur l'icône **Ajoute un nouveau terme de test pour la règle de domaine** pour ajouter un autre ensemble de valeurs de test.  
  
    5.  Cliquez sur l'icône **Tester la règle de domaine sur tous les termes** . Si un jeu de valeurs de test est valide, DQS écrit un contrôle dans la colonne **Validité** de la ligne. Si l'ensemble de valeurs de test n'est pas valide, DQS écrit un triangle avec un point d'exclamation dans la colonne Validité de la ligne.  
  
    6.  Une fois le test terminé, cliquez sur **Fermer** dans la boîte de dialogue **Tester la règle de domaine composite** .  
  
2.  Lorsque vous avez terminé vos règles inter-domaines, cliquez sur **Terminer** pour terminer l'activité de gestion de domaine, comme décrit dans [End the Domain Management Activity](http://msdn.microsoft.com/library/ab6505ad-3090-453b-bb01-58435e7fa7c0).  
  
##  <a name="FollowUp"></a> Suivi : Après la création d'une règle interdomaines  
 Après avoir créé une règle inter-domaines, vous pouvez effectuer d'autres tâches de gestion de domaine sur le domaine, vous pouvez exécuter la découverte de connaissances pour ajouter des connaissances au domaine ou vous pouvez ajouter une stratégie correspondante au domaine. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
  
