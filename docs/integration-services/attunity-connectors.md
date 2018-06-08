---
title: Connecteurs Microsoft pour Oracle et Teradata par Attunity (SSIS) | Microsoft Docs
ms.date: 05/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: ''
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9a0ee34589b277c8a5cb765d0fc71e480dfd525e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-connectors-for-oracle-and-teradata-by-attunity-for-integration-services-ssis"></a>Connecteurs Microsoft pour Oracle et Teradata par Attunity pour Integration Services (SSIS)

Vous pouvez télécharger des connecteurs pour Integration Services par Attunity afin d’optimiser les performances lors du chargement des données vers ou à partir d’Oracle ou Teradata dans un package SSIS.

## <a name="download-the-latest-attunity-connectors"></a>Télécharger les connecteurs Attunity les plus récents

Vous trouverez la dernière version des connecteurs ici :  
[Connecteurs Microsoft v5.0 pour Oracle et Teradata](https://www.microsoft.com/download/details.aspx?id=55179)

## <a name="issue---the-attunity-connectors-arent-visible-in-the-ssis-toolbox"></a>Problème - Les connecteurs Attunity ne sont pas visibles dans la boîte à outils SSIS

Pour voir les connecteurs Attunity dans la boîte à outils SSIS, vous devez toujours installer la version des connecteurs qui cible la même version de SQL Server que celle de SQL Server Data Tools (SSDT) installée sur votre ordinateur. (Il se peut que des versions antérieures des connecteurs soient également installées.) Cette condition requise est indépendante de la version de SQL Server que vous souhaitez cibler dans vos projets et vos packages SSIS.

Par exemple, si vous avez installé la version la plus récente de SSDT, vous avez la version 17 de SSDT avec un numéro de build qui commence par 14. Cette version de SSDT ajoute la prise en charge de SQL Server 2017. Pour voir et utiliser les connecteurs Attunity lors du développement de package SSIS (même si vous souhaitez cibler une version antérieure de SQL Server), vous devez également installer la dernière version des connecteurs Attunity, la version 5.0. Cette version des connecteurs ajoute également la prise en charge de SQL Server 2017.

Vous pouvez vérifier la version installée de SSDT dans Visual Studio à partir de **Aide** | **À propos de Microsoft Visual Studio**, ou dans **Programmes et fonctionnalités** dans le Panneau de configuration. Ensuite, installez la version correspondante des connecteurs Attunity d’après le tableau suivant.

|Version de SSDT|Numéro de build de SSDT|Version cible de SQL Server|Version requise des connecteurs|
|---------|---------|---------|---------|
|17|Commence par 14|SQL Server 2017|[Connecteurs Microsoft v5.0 pour Oracle et Teradata](https://www.microsoft.com/download/details.aspx?id=55179)|
|16|Commence par 13|SQL Server 2016|[Connecteurs Microsoft v4.0 pour Oracle et Teradata](https://www.microsoft.com/download/details.aspx?id=52950)|
||||

## <a name="download-the-latest-sql-server-data-tools-ssdt"></a>Télécharger la dernière version de SQL Server Data Tools (SSDT)

Vous pouvez obtenir la version la plus récente de SSDT ici :  
[Télécharger SSDT (SQL Server Data Tools)](..//ssdt/download-sql-server-data-tools-ssdt.md)
