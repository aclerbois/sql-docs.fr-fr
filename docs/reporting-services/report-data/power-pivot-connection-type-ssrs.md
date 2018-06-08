---
title: Type de connexion PowerPivot (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a104c3c7-f118-4d02-9a0f-6859f1469d11
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: abc0e7f2d8ded8d69f758059b5ce85d34b080ebb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="power-pivot-connection-type-ssrs"></a>Type de connexion PowerPivot (SSRS)
  Vous pouvez utiliser l’extension de traitement des données SQL Server Analysis Services pour récupérer des données d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] publié dans une Galerie SharePoint [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Utilisez les informations de cette rubrique pour générer une source de données. Pour obtenir des instructions détaillées, consultez [Ajouter et vérifier une connexion de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 La source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] doit être publiée dans une Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur un site SharePoint.  
  
 Pour prendre en charge les connexions du Générateur de rapports à un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , vous devez disposer de SQL Server 2008 R2 ADOMD.NET sur votre station de travail. Cette bibliothèque cliente est installée avec [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel, mais si vous utilisez un ordinateur qui n’a pas cette application, vous devez télécharger et installer ADOMD.NET à partir de la page [SQL Server 2008 R2 Feature Pack](http://go.microsoft.com/fwlink/?LinkId=192565).  
  
## <a name="data-source-type"></a>Type de source de données  
 Utilisez le type de source de données de rapport **Microsoft SQL Server Analysis Services**.  
  
## <a name="connection-string"></a>Chaîne de connexion  
 La chaîne de connexion est l’URL du classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] publié sur SharePoint dans la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou une autre bibliothèque, par exemple `http://contoso-srv/subsite/PowerPivotLibrary/ContosoSales.xlsx`.  
  
## <a name="credentials"></a>Informations d'identification  
 Spécifiez les informations d’identification dont vous avez besoin pour accéder au classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et au site SharePoint, par exemple l’authentification Windows (sécurité intégrée). Pour plus d’informations, consultez [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) ou [Spécifier des informations d’identification dans le Générateur de rapports](http://msdn.microsoft.com/library/7412ce68-aece-41c0-8c37-76a0e54b6b53).  
  
## <a name="queries"></a>Requêtes  
 Lorsque vous êtes connecté à la source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , utilisez la requête graphique MDX pour créer une requête en explorant et en sélectionnant des données dans les structures de données sous-jacentes. Après avoir créé une requête, exécutez-la pour visualiser des exemples de données dans le volet Résultats.  
  
 Le Concepteur de requêtes analyse la requête pour déterminer les champs du dataset. Vous pouvez aussi modifier manuellement la collection de champs du dataset dans le volet **Données du rapport** . Pour plus d’informations, consultez [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
## <a name="filters"></a>Filtres  
 Dans le volet Filtres, spécifiez les dimensions et les membres à exclure par filtrage ou à inclure dans les résultats de la requête.  
  
## <a name="parameters"></a>Paramètres  
 Dans le volet Filtres, sélectionnez l'option **Paramètres** pour qu'un filtre crée automatiquement un paramètre de rapport avec les valeurs disponibles qui correspondent aux sélections de filtre.  
  
## <a name="remarks"></a>Notes   
 Si vous ouvrez le Générateur de rapports à partir du classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans une Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , les tableaux croisés dynamiques, les graphiques croisés dynamiques, les segments et autres mises en page et fonctionnalités analytiques du classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne sont pas recréés dans le rapport. Le rapport vide inclut une source de données préconfigurée qui pointe vers les données du classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . La conception de rapports basés sur un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] peut être fastidieuse et chronophage selon le nombre de segments, de filtres et de tables ou de graphiques que vous voulez recréer dans le rapport. Pour être plus efficace, séparez la présentation des données que vous souhaitez dans un rapport, de la conception [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Les données d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont très compressées, ce qui n’est pas le cas des données récupérées du classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] d’un rapport. Utilisez le concepteur de requêtes pour spécifier des filtres et des paramètres afin de limiter les données à ce qui est absolument nécessaire dans le rapport.  
  
 Contrairement à la connexion à un cube Analysis Services, un modèle [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’a pas de hiérarchies. Pour fournir des fonctionnalités semblables aux segments associés dans le classeur, vous devez créer des paramètres en cascade dans le rapport. Pour plus d’informations, consultez [Ajouter des paramètres en cascade à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-cascading-parameters-to-a-report-report-builder-and-ssrs.md).  
  
 Parfois, vous devrez ajuster les expressions en fonction des valeurs des données sous-jacentes du modèle [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Vous devrez peut-être modifier des expressions pour convertir les données dans le type de données approprié, ou ajouter ou supprimer une fonction d'agrégation. Par exemple, pour convertir le type de données de Chaîne en Entier, utilisez `=CInt`. Vérifiez toujours, avant de publier le rapport, que celui-ci affiche les valeurs attendues des données dans le modèle [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Les images d’aperçu d’un rapport dans une Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne sont générées que si les conditions suivantes sont remplies :  
  
-   Le rapport et le classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui fournissent les données doivent être stockés dans la même Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
-   Le rapport ne contient que les données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] d’une source de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
## <a name="see-also"></a> Voir aussi  
 [Interface utilisateur du Concepteur de requêtes MDX Analysis Services &#40;Générateur de rapports&#41;](http://msdn.microsoft.com/library/7e288eee-2d37-485e-a6a0-dbba5e041e26)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  
