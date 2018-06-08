---
title: Ajout de connaissances à une base de connaissances | Microsoft Docs
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
ms.assetid: da148a7f-55bc-4990-a157-e61968b831d7
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 012d198b941623bb93b660f1a0e5ce310905914e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="adding-knowledge-to-a-knowledge-base"></a>Ajout de connaissances à une base de connaissances

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit les différentes façons d'ajouter des connaissances à une base de connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Avant de pouvoir effectuer des opérations de qualité des données, vous devez disposer de connaissances sur les données. Pour acquérir ces connaissances, vous devez créer et maintenir une base de connaissances de qualité des données, et y ajouter des connaissances liées à un type spécifique de source de données. La base de connaissances est un référentiel de connaissances sur vos données qui vous permet de comprendre vos données et de préserver leur intégrité.  
  
 La base de connaissances contient des domaines de données en rapport avec la source de données. Pour chaque domaine de données, la base de données DQKB stocke l'ensemble des termes, fautes d'orthographe, règles de validation et d'entreprise, et données de référence identifiés qui peuvent être utilisés pour effectuer des actions de qualité des données sur la source de données. DQS utilise ces connaissances pour identifier les données incorrectes ou non valides, ou pour effectuer des mises en correspondance.  
  
 Vous pouvez ajouter des connaissances à une base de connaissances via les méthodes assistées par ordinateur ou interactives suivantes.  
  
-   [Effectuer une découverte des connaissances](#Discovery)  
  
-   [Gérer des valeurs de données dans un domaine](#ManageDomain)  
  
-   [Importer des connaissances à partir d'un fichier .dqs](#DQSFile)  
  
-   [Importer des connaissances à partir d'un fichier Excel](#Excel)  
  
-   [Réimporter des connaissances d'un projet dans la base de connaissances](#Project)  
  
-   [Utiliser la base de connaissances DQS par défaut](#Default)  
  
##  <a name="Discovery"></a> Effectuer une découverte des connaissances  
 La découverte des connaissances analyse un exemple des données en fonction de critères de qualité des données, puis ajoute les connaissances qu'il a acquises à la base de connaissances. Il s'agit d'un processus assisté par ordinateur qui identifie les incohérences et les erreurs de syntaxe dans les données, et qui suggère des modifications à apporter aux données. L'activité de découverte des connaissances est un Assistant qui comprend une page vous permettant de gérer de façon interactive les valeurs de domaine.  
  
-   Pour plus d'informations dans la documentation, consultez [Perform Knowledge Discovery](../data-quality-services/perform-knowledge-discovery.md).  
  
-   Pour obtenir une vidéo qui montre comment effectuer une découverte des connaissances, cliquez [ici](http://msdn.microsoft.com/sqlserver/hh323825.aspx).  
  
##  <a name="ManageDomain"></a> Gérer des valeurs de données dans un domaine  
 DQS vous permet de modifier et d'augmenter de façon interactive les métadonnées qui sont générées par l'activité de découverte des connaissances assistée par ordinateur. Vous effectuez ces opérations dans l'activité Gestion de l'arborescence du domaine, où vous pouvez appliquer une modification à une valeur de données spécifique.  
  
-   Pour plus d'informations dans la documentation, consultez [Change Domain Values](../data-quality-services/change-domain-values.md).  
  
-   Pour obtenir une vidéo qui montre comment effectuer la gestion de l'arborescence du domaine, cliquez [ici](http://msdn.microsoft.com/sqlserver/hh323825.aspx). Notez que dans cette vidéo, vous modifiez des valeurs de domaine dans la page Gestion de l'arborescence du domaine de l'Assistant de découverte des connaissances. Vous pouvez également effectuer ces étapes dans la page Valeurs du domaine de l'activité Gestion de l'arborescence du domaine.  
  
##  <a name="DQSFile"></a> Importer des connaissances à partir d'un fichier .dqs  
 Vous pouvez importer un domaine à partir d'un fichier de données .dqs vers une base de connaissances existante, ou importer la totalité d'une base de connaissances d'un fichier .dqs vers une nouvelle base de connaissances. Pour ce faire, vous devez d'abord exporter un domaine ou une base de connaissances existant vers un fichier .dqs. Un fichier .dqs contenant un domaine inclut toutes les données du domaine ; un fichier .dqs contenant une base de connaissances contient toutes les informations de la base de connaissances, y compris les domaines et la stratégie de correspondance.  
  
-   Pour plus d’informations dans la documentation, consultez [Importer un domaine à partir d’un fichier .dqs](../data-quality-services/import-a-domain-from-a-dqs-file.md) ou [Importer une base de connaissances à partir d’un fichier .dqs](../data-quality-services/import-a-knowledge-base-from-a-dqs-file.md).  
  
##  <a name="Excel"></a> Importer des connaissances à partir d'un fichier Excel  
 Vous pouvez importer des valeurs de domaine à partir d'un fichier de feuille de calcul Excel vers un domaine ou une base de connaissances existant. Pour ce faire, vous devez d'abord créer une feuille de calcul Excel avec les valeurs du domaine que vous voulez importer, puis vérifier qu'Excel est installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] afin de pouvoir importer les valeurs à l'aide de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]. Vous ne pouvez pas exporter des valeurs de domaine d'un domaine ou d'une base de connaissances vers un fichier Excel.  
  
-   Pour plus d’informations dans la documentation, consultez [Importer les valeurs d’un fichier Excel dans un domaine](../data-quality-services/import-values-from-an-excel-file-into-a-domain.md) ou [Importer les domaines d’un fichier Excel dans la découverte des connaissances](../data-quality-services/import-domains-from-an-excel-file-in-knowledge-discovery.md).  
  
##  <a name="Project"></a> Réimporter des connaissances d'un projet dans la base de connaissances  
 Après avoir exécuté un projet de nettoyage ou de qualité des données de correspondance à l'aide d'une base de connaissances, vous pouvez réimporter les connaissances créées pendant le nettoyage ou la correspondance dans cette base de connaissances. Cela vous permet de conserver les connaissances générées pendant le projet, et de générer en permanence les connaissances dans la base de connaissances.  
  
-   Pour plus d’informations dans la documentation, consultez [Importer des valeurs de projet de nettoyage dans un domaine](../data-quality-services/import-cleansing-project-values-into-a-domain.md).  
  
##  <a name="Default"></a> Utiliser la base de connaissances DQS par défaut  
 DQS est fourni avec une base de connaissances prégénérée appelée Données DQS, qui contient des domaines pour des données relatives à des sociétés et adresses des États-Unis. Cette base de connaissances peut être utilisée pour démarrer rapidement un projet sans créer une nouvelle base de connaissances. La base de connaissances Données DQS est en lecture seule, mais le gestionnaire de données peut créer une base de connaissances à partir de celle-ci.  
  
-   Pour plus d'informations dans la documentation, consultez [Using the DQS Default Knowledge Base](../data-quality-services/using-the-dqs-default-knowledge-base.md).  
  
  
