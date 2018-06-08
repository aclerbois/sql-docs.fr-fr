---
title: Wide World Importers - base de données exemple SQL | Documents Microsoft
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 5a1eef0650c520ff4757de627633d096ffa46ec8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wide-world-importers-sample-databases-for-microsoft-sql"></a>Wide World Importers exemples de bases de données pour Microsoft SQL
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
Il s’agit d’une vue d’ensemble de la société fictive Wide World Importers et les flux de travail qui est abordés dans les bases de données exemple WideWorldImporters pour SQL Server et la base de données SQL Azure.  

Wide World Importers (WWI) est un importateur de biens de gros nouveauté et d’un serveur de distribution d’exploitation à partir de la zone de la baie de San Francisco.

Comme un grossistes, les clients de WWI sont principalement les sociétés qui les revendent à des personnes. WWI vend au détail entre les États américains, y compris les magasins spécialisés, grande distribution, calcul de magasins, les magasins d’attraction tourisme et certaines personnes. WWI vend également aux autres grossistes via un réseau des agents qui promouvoir les produits procuration de WWI. Actuellement, tous les clients de WWI sont basés aux États-Unis, la société projette à pousser l’expansion dans d’autres pays.

WWI achète des marchandises auprès des fournisseurs, y compris la nouveauté et les fabricants de jouet et autres grossistes nouveauté. Ils stock marchandises dans leur entrepôt de WWI et de la commande à partir des fournisseurs en fonction des besoins de satisfaire aux demandes du client. Ils également acheter des volumes importants d’emballages et vendent ces en plus petites quantités de commodité pour les clients.

A récemment commencé à WWI de vendre une variété d’alimentaires attrapes telles que piment chocolat.  Précédemment, la société n’avait pas gérer des éléments réfrigérées. À présent, pour répondre à la gestion des spécifications de produits alimentaires, ils doivent surveiller la température dans leur espace de refroidissement et de leurs camions qui ont des sections de refroidissement.

## <a name="workflow-for-warehouse-stock-items"></a>Flux de travail pour les éléments de stock de l’entrepôt

Le flux général de la façon dont les éléments sont stockés et distribués est la suivante :
- WWI crée des bons de commande et envoie les commandes aux fournisseurs.
- Les éléments d’envoi de fournisseurs, WWI les reçoit et les stocks dans leur magasin.
- Facturation des clients à partir de WWI
- WWI remplit la commande du client avec des éléments de stock de l’entrepôt, et lorsqu’ils ne disposent pas de stock suffisante, ils commandent le stock supplémentaire des fournisseurs.
- Certains clients ne souhaitent pas attendre que les éléments qui ne sont pas en stock. Si suivant leur ordre que cinq éléments stocks et quatre sont disponibles, ils souhaitent recevoir quatre éléments et en souffrance de l’élément. L’élément seraient les envoyées ultérieurement dans une expédition distincte.
- WWI factures clients pour les articles en stock, en règle générale en convertissant la commande en facture.
- Les clients peuvent commander des éléments qui ne sont pas en stock. Ces éléments sont en souffrance.
- WWI offre des éléments de stock via leurs propres RVA de remise, ou autres courriers ou les méthodes de transport.
- Clients payer les factures pour WWI.
- Périodiquement, WWI paie des fournisseurs pour les éléments qui étaient présents sur les bons de commande. Il s’agit souvent un certain temps après avoir reçu les marchandises.

## <a name="data-warehouse-and-analysis-workflow"></a>Workflow de l’entrepôt et d’analyse de données

Tandis que l’équipe de WWI utilisent SQL Server Reporting Services pour générer des rapports opérationnels à partir de la base de données WideWorldImporters, ils doivent également exécuter l’analytique sur leurs données et avez besoin générer des rapports stratégiques. L’équipe a créé un modèle de données de dimension dans une base de données WideWorldImportersDW. Cette base de données est remplie par un package Integration Services.

SQL Server Analysis Services est utilisé pour créer des modèles de données analytiques à partir des données dans le modèle de données de dimension. SQL Server Reporting Services est utilisé pour générer des rapports stratégiques directement dans le modèle de données de dimension et également à partir du modèle d’analyse. Power BI permet de créer des tableaux de bord à partir des mêmes données. Les tableaux de bord est utilisés sur les sites Web et sur les téléphones et tablettes. *Remarque : ces modèles de données et les rapports ne sont pas encore disponibles*

## <a name="additional-workflows"></a>Flux de travail supplémentaires

Voici d’autres workflows.
- Notes de crédit WWI problèmes lorsqu’un client ne reçoit pas la bonne pour une raison quelconque, ou lorsque les marchandises sont défectueux. Ceux-ci sont traités comme des factures négatifs.
- WWI compte régulièrement des quantités disponibles des articles de stock pour vous assurer que les quantités en stock indiquées comme étant disponibles sur le système sont précises. (Le processus d’effectuer cette opération est appelé un d’inventaire).
- Températures ambiantes à froid. Périssables sont stockés dans des locaux frigorifiques. Données de capteur à partir de ces espaces sont intégrées dans la base de données à des fins d’analyse et analytique.
- Emplacement du véhicule de suivi. Véhicules qui transportent des marchandises pour WWI incluent les capteurs qui effectuent le suivi de l’emplacement. Cet emplacement est intégré à nouveau dans la base de données pour l’analyse et plus analytique.

## <a name="fiscal-year"></a>Année fiscale

La société opère avec un exercice qui commence le 1er novembre.

## <a name="terms-of-use"></a>Conditions d’utilisation

La licence de la base de données et de l’exemple de code est décrit ici : [license.txt](https://github.com/Microsoft/sql-server-samples/blob/master/license.txt)

La base de données inclut des données publiques qui a été chargées à partir de data.gov et EarthData naturelle. Les conditions d’utilisation sont ici : [http://www.naturalearthdata.com/about/terms-of-use/](http://www.naturalearthdata.com/about/terms-of-use/)
