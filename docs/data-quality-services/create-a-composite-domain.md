---
title: Créer un domaine composite | Microsoft Docs
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
- sql13.dqs.kb.createcd.f1
- sql13.dqs.dm.cdproperties.f1
ms.assetid: c7f0bd84-a02e-4a81-885d-985e6415c499
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6a0bef49731f86f4d788884f545f944326b72357
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-composite-domain"></a>Créer un domaine composite

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment créer un domaine composite dans une base de connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Un domaine composite comprend un ou plusieurs domaines uniques qui s'appliquent à un seul champ de données. Pour plus d’informations sur les domaines composites, consultez [Gestion d’un domaine composite](../data-quality-services/managing-a-composite-domain.md).  
  
 Il existe deux méthodes pour créer un domaine composite. La première se situe pendant l'étape Mapper de l'activité de découverte des connaissances, lorsque vous êtes en cours d'analyse d'un exemple de données pour ajouter des connaissances à une base nouvelle ou existante. La deuxième se situe pendant l'activité de gestion de l'arborescence du domaine quand, au lieu de modifier un domaine existant, vous en créez un nouveau. Pour créer un domaine composite, vous devez avoir déjà créé au moins deux domaines uniques à ajouter au domaine composite. Seuls les domaines uniques qui ont déjà été créés et qui n'ont pas été ajoutés à un domaine composite existant sont disponibles lorsque vous créez un nouveau domaine composite. Un seul domaine ne peut pas être ajouté à plusieurs domaines composites et un domaine composite ne peut pas être ajouté à un autre domaine composite.  
  
 Après la création d'un domaine composite, vous pouvez modifier les propriétés du domaine composite, joindre un service de données de référence au domaine, créer des règles inter-domaines ou créer des relations de valeur. Pour ce faire, sélectionnez le domaine composite dans la liste **Domaine** de la page **Gestion de l'arborescence du domaine** , puis sélectionnez l'onglet approprié.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Pour créer un domaine composite, vous devez avoir créé et ouvert une base de connaissances, et vous devez avoir créé au moins deux domaines uniques à ajouter au domaine composite.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer un domaine composite.  
  
##  <a name="ParsingKnowledgeDiscoveryActivity"></a> Créer un domaine composite dans l'activité de découverte des connaissances  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Ouvrir la base de connaissances** , puis sélectionnez une base de connaissances ou cliquez sur **Nouvelle Base de connaissances** et entrez les propriétés de la base de connaissances.  
  
3.  Sélectionnez **Découverte des connaissances** comme activité, puis cliquez sur **Créer** pour créer la base de connaissances ou sur **Ouvrir** pour ouvrir une base de connaissances existante.  
  
4.  Dans la page **Mapper** , spécifiez une connexion à la source de données. Pour plus d'informations, consultez [Perform Knowledge Discovery](../data-quality-services/perform-knowledge-discovery.md).  
  
5.  Dans la table **Mappages** , sélectionnez une colonne source dans la liste déroulante de la colonne **Colonne source** d'une ligne vide. Assurez-vous que la colonne source contient le domaine composite pris en compte par deux domaines uniques existants. S'il n'existe aucun domaine unique correspondant, cliquez sur l'icône **Créer un domaine** .  
  
6.  Dans la table **Mappages** , sélectionnez une colonne source dans la liste déroulante de la colonne **Colonne source** d'une ligne vide. Assurez-vous que la colonne source contient des éléments de domaine composite qui sont pris en compte par deux domaines uniques existants. S'il n'existe aucun domaine unique correspondant, cliquez sur l'icône **Créer un domaine** pour les créer. Pour plus d’informations, consultez [Créer un domaine](../data-quality-services/create-a-domain.md).  
  
7.  Cliquez sur l'icône **Créer un domaine composite** .  
  
##  <a name="DomainManagementActivity"></a> Créer un domaine composite dans l'activité de gestion de l'arborescence du domaine  
  
1.  Dans la page d'accueil du client Data Quality Services, cliquez sur **Ouvrir la base de connaissances** , puis sélectionnez une base de connaissances, ou cliquez sur **Nouvelle Base de connaissances** et entrez les propriétés de la base de connaissances.  
  
2.  Sélectionnez **Gestion de l'arborescence du domaine** comme activité, puis cliquez sur **Créer** pour créer la base de connaissances ou sur **Ouvrir** pour ouvrir une base de connaissances existante.  
  
3.  Vérifiez que plusieurs domaines uniques requis par le domaine composite existent. Sinon, cliquez sur l'icône **Créer un domaine** pour les créer. Pour plus d’informations, consultez [Créer un domaine](../data-quality-services/create-a-domain.md).  
  
4.  Dans la page **Gestion de l'arborescence du domaine** , cliquez sur l'icône **Créer un domaine composite** au-dessus de la liste des domaines.  
  
5.  Entrez un nom qui est unique dans la base de connaissances et une description limitée à 256 caractères.  
  
6.  Dans la **Liste des domaines**, sélectionnez les domaines qui feront partie du domaine composite, puis cliquez sur la flèche droite pour les déplacer vers la table **Domaines du domaine composite** .  
  
7.  Cliquez sur **OK**.  
  
##  <a name="CompositeDomainProperties"></a> Définir les propriétés de domaine composite  
  
1.  Dans la boîte de dialogue **Créer un domaine composite** , entrez un nom qui est unique dans la base de connaissances et une description limitée à 256 caractères.  
  
2.  Dans la **Liste des domaines**, sélectionnez les domaines qui feront partie du domaine composite, puis cliquez sur la flèche droite pour les déplacer vers la table **Domaines du domaine composite** . Voici une liste de domaines uniques disponibles pour être ajoutés au domaine composite que vous créez. Seuls les domaines uniques qui ont déjà été créés et qui n'ont pas été ajoutés à un domaine composite existant sont disponibles. Un seul domaine ne peut pas être ajouté à plusieurs domaines composites dans la base de connaissances, et un domaine composite ne peut pas être ajouté à un autre domaine composite.  
  
3.  Cliquez sur **Avancé**.  
  
4.  Sélectionnez l'une des valeurs suivantes pour l'option **Méthode d'analyse**:  
  
    -   **Données de référence**: analysez les valeurs du champ selon la façon dont les données sont mises en forme par le service de données de référence (RDS). Data Quality Services envoie les valeurs du domaine composite au service RDS et celui-ci retourne les données corrigées et analysées en fonction du domaine dans le domaine composite.  
  
    -   **Dans l'ordre**: analysez les valeurs du champ en fonction de l'ordre des domaines dans le domaine composite. La première valeur sera incluse dans le premier domaine, la deuxième valeur dans le deuxième domaine, et ainsi de suite.  
  
    -   **Délimiteurs**: analysez les valeurs du champ en fonction du délimiteur sélectionné à partir des cases d'option affichées lorsque Délimiteurs est sélectionné. Il peut s'agir de **Tabulation**, **Point-virgule**, **Virgule**, **Espace**ou **Autre**. Si la valeur est **Autre**, entrez la valeur qui servira de délimiteur.  
  
5.  Si vous avez sélectionné **Délimiteurs** pour la méthode d'analyse, vous pouvez également sélectionner **Utiliser l'analyse de Base de connaissances**. Pour plus d’informations, consultez [Knowledge-Based Parsing](#KnowledgeBaseParsing).  
  
6.  Cliquez sur **Terminer** pour terminer l'activité de gestion de l'arborescence du domaine, comme décrit dans [End the Domain Management Activity](http://msdn.microsoft.com/library/ab6505ad-3090-453b-bb01-58435e7fa7c0).  
  
##  <a name="FollowUp"></a> Suivi : Après la création d'un domaine composite  
 Après avoir créé un domaine composite, vous pouvez effectuer d'autres tâches de gestion des domaines sur le domaine, effectuer une découverte des connaissances pour ajouter des connaissances au domaine ou ajouter une stratégie de correspondance au domaine. Pour plus d’informations, consultez [Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md), [Gestion d’un domaine](../data-quality-services/managing-a-domain.md) ou [Créer une stratégie de correspondance](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="KnowledgeBaseParsing"></a> Knowledge-Based Parsing  
 Data Quality Services vous permet d'analyser les données selon les connaissances, et pas simplement en fonction du délimiteur ou de l'ordre. L'analyse basée sur les connaissances est utilisée lorsque des données sources complexes sont mappées à un domaine composite et que vous n'utilisez pas les services de données de référence. Vous pouvez utiliser l'analyse basée sur les connaissances pour analyser les données de la source de données dans les domaines uniques appropriés. Avec l'analyse basée sur les connaissances, DQS tente d'abord d'utiliser les connaissances pour analyser des données complexes dans des domaines uniques. Si possible, il identifiera certaines parties de la chaîne comme étant dans un ou plusieurs domaines, et analysera la chaîne dans ses différents domaines. Par exemple, supposons que vous avez « John B.Doe » comme valeurs complexes dans un champ de nom complet représenté par un domaine composite Nom complet. Si DQS identifie « John » comme étant dans le domaine Prénom et « Doe » comme étant dans le domaine Nom, DQS ajoute alors « B. » au domaine Deuxième prénom selon la connaissance de domaine.  
  
 Vous pouvez utiliser l'analyse basée sur les connaissances uniquement si vous sélectionnez également l'analyse en fonction du délimiteur. L'analyse basée sur les connaissances ne remplace pas l'analyse de délimiteur, mais l'améliore. DQS utilisera un délimiteur pour effectuer l'analyse uniquement s'il n'existe aucune connaissance pour exécuter l'opération. Dans certains cas, DQS peut établir une analyse avec l'analyse basée sur les connaissances, puis une autre analyse avec l'analyse en fonction du délimiteur.  
  
 L'analyse basée sur les connaissances peut être utilisée lorsque le domaine composite est constitué de domaines de chaînes ou d'un mélange de différents types de domaines (int, date, heure, etc.). Si la source de données est composée de différents types de données, l'analyse doit être effectuée en premier pour les types de données qui ne sont pas des chaînes, puis comme décrit ci-dessus selon la connaissance de domaine pour le reste des données.  
  
 Lorsque vous utilisez l'analyse basée sur les connaissances et qu'il y a moins de valeurs dans les données sources que de domaines dans le domaine composite, DQS placera une valeur Null dans le domaine manquant. Lorsqu'il y a plus de valeurs dans les données sources que de domaines dans le domaine composite, DQS ajoutera les données supplémentaires à l'une des colonnes. Si plusieurs domaines ont les mêmes valeurs, la source de données sera analysée sur le premier domaine correspondant.  
  
  
