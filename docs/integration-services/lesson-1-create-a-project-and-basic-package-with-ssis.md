---
title: 'Leçon 1 : Créer un projet et un package de base avec SSIS | Microsoft Docs'
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: tutorial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: get-started-article
applies_to:
- SQL Server 2016
ms.assetid: 84d0b877-603f-4f8e-bb6b-671558ade5c2
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 84da6574d227b547606376e994188875e10254ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lesson-1-create-a-project-and-basic-package-with-ssis"></a>Leçon 1 : Créer un projet et un package de base avec SSIS

 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [Leçon 1 : Création du package de base et du package du projet](https://msdn.microsoft.com/library/ms170419(SQL.120).aspx).

Au cours de cette leçon, vous allez créer un package ETL simple qui extrait des données d'une seule source de fichier plat, transforme ces données en utilisant deux composants de transformation de recherche et les écrit dans la table de faits **FactCurrency** de la base de données **AdventureWorksDW2012**. Dans le cadre de cette leçon, vous allez apprendre à créer de nouveaux packages, ajouter et configurer des sources de données et des destinations et enfin, à utiliser le nouveau flux de contrôle et les composants de flux de données.  
  
> [!IMPORTANT]  
> Pour suivre ce didacticiel, vous devez disposer de l'exemple de base de données **AdventureWorksDW2012** . Pour plus d’informations sur l’installation et le déploiement **d’AdventureWorksDW2012**, consultez [Reporting Services Product Samples sur CodePlex](http://go.microsoft.com/fwlink/p/?LinkID=526910).  
  
## <a name="understanding-the-package-requirements"></a>Connaissances préalables à la création d'un package  
Ce didacticiel nécessite Microsoft SQL Server Data Tools.  
  
Pour plus d'informations sur l'installation de SQL Server Data Tools, consultez [Téléchargement de SQL Server Data Tools](http://msdn.microsoft.com/data/hh297027).  
  
Avant de créer un package, vous devez maîtriser les connaissances relatives au formatage utilisé pour les données sources et la destination. Une fois ces connaissances maîtrisées, vous êtes prêt à définir les transformations nécessaires pour mapper les données source avec les données de destination.  
  
### <a name="looking-at-the-source"></a>Étude de la source  
Dans le cadre de ce didacticiel, les données sources sont représentées par un ensemble de données monétaires d'historique contenu dans le fichier plat SampleCurrencyData.txt. Les données sources contiennent les quatre colonnes suivantes : le taux moyen de la devise, une clé de devise, une clé de date et le taux de clôture.  
  
Voici un exemple des données sources contenues dans le fichier SampleCurrencyData.txt :  
  
<pre>1.00070049USD9/3/05 0:001.001201442  
1.00020004USD9/4/05 0:001  
1.00020004USD9/5/05 0:001.001201442  
1.00020004USD9/6/05 0:001  
1.00020004USD9/7/05 0:001.00070049  
1.00070049USD9/8/05 0:000.99980004  
1.00070049USD9/9/05 0:001.001502253  
1.00070049USD9/10/05 0:000.99990001  
1.00020004USD9/11/05 0:001.001101211  
1.00020004USD9/12/05 0:000.99970009</pre>  
  
Pour bien utiliser des données sources issues d'un fichier plat, il est important de comprendre comment le Gestionnaire de connexions de fichiers plats interprète les données du fichier plat. Si la source du fichier plat est au format Unicode, le gestionnaire de connexions de fichiers plats définit toutes les colonnes avec le type [DT_WSTR] et une largeur par défaut égale à 50. Si la source du fichier plat est au format ANSI, les colonnes sont définies avec le type [DT_STR] et une largeur égale à 50. Il vous faudra probablement modifier ces valeurs par défaut pour affecter aux colonnes des types String plus appropriés à vos données. Pour cela, vous allez examiner le type de données de la destination dans laquelle les données seront enregistrées, puis choisir le type de données qui convient dans le Gestionnaire de connexions de fichiers plats.  
  
### <a name="looking-at-the-destination"></a>Étude de la destination  
La destination finale des données sources est la table de faits **FactCurrency** dans la base de données **AdventureWorksDW**. La table de faits **FactCurrency** contient quatre colonnes et des relations avec deux tables de dimension, comme illustré ci-après.  
  
|Nom de la colonne|Type de données|Table de recherche|Colonne de recherche|  
|---------------|-------------|----------------|-----------------|  
|AverageRate|FLOAT|None|None|  
|CurrencyKey|int (FK)|DimCurrency|CurrencyKey (PK)|  
|DateKey|int (FK)|DimDate|DateKey (PK)|  
|EndOfDayRate|FLOAT|None|None|  
  
### <a name="mapping-source-data-to-be-compatible-with-the-destination"></a>Mappage des données sources pour la compatibilité avec la destination  
L'analyse du format des données sources et de destination indique que des recherches seront nécessaires pour les valeurs **CurrencyKey** et **DateKey** . Les transformations qui effectueront ces recherches obtiendront les valeurs **CurrencyKey** et **DateKey** en utilisant les autres clés des tables de dimension **DimCurrency** et **DimDate** .  
  
|Colonne de fichier plat|Nom de la table|Nom de la colonne|Type de données|  
|--------------------|--------------|---------------|-------------|  
|0|FactCurrency|AverageRate|float|  
| 1|DimCurrency|CurrencyAlternateKey|nchar (3)|  
|2|DimDate|FullDateAlternateKey|Date|  
|3|FactCurrency|EndOfDayRate|FLOAT|  
  
## <a name="lesson-tasks"></a>Tâches de la leçon  
Cette leçon contient les tâches suivantes :  
  
-   [Étape 1 : Création d'un nouveau projet Integration Services](../integration-services/lesson-1-1-creating-a-new-integration-services-project.md)  
  
-   [Étape 2 : ajout et configuration d'un gestionnaire de connexions de fichiers plats](../integration-services/lesson-1-2-adding-and-configuring-a-flat-file-connection-manager.md)  
  
-   [Étape 3 : ajout et configuration d'un gestionnaire de connexions OLE DB](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
-   [Étape 4 : ajout d'une tâche de flux de données au package](../integration-services/lesson-1-4-adding-a-data-flow-task-to-the-package.md)  
  
-   [Étape 5 : Ajout et configuration de la source de fichier plat](../integration-services/lesson-1-5-adding-and-configuring-the-flat-file-source.md)  
  
-   [Étape 6 : Ajout et configuration des transformations de recherche](../integration-services/lesson-1-6-adding-and-configuring-the-lookup-transformations.md)  
  
-   [Étape 7 : ajout et configuration de la destination OLE DB](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
-   [Étape 8 : comment rendre le package de la leçon 1 plus facile à assimiler](../integration-services/lesson-1-8-making-the-lesson-1-package-easier-to-understand.md)  
  
-   [Étape 9 : Test de la leçon 1 du Package du didacticiel](../integration-services/lesson-1-9-testing-the-lesson-1-tutorial-package.md)  
  
## <a name="start-the-lesson"></a>Démarrer la leçon  
[Étape 1 : Création d'un nouveau projet Integration Services](../integration-services/lesson-1-1-creating-a-new-integration-services-project.md)  
  
