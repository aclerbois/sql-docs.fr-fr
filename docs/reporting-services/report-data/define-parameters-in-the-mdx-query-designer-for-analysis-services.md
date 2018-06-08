---
title: Définir des paramètres dans le Concepteur de requêtes MDX pour Analysis Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- parameters [Reporting Services], MDX
- Multidimensional Expressions [Reporting Services]
- Data Mining Prediction [Reporting Services]
- MDX [Reporting Services], defining parameters
- DMX [Reporting Services]
ms.assetid: 4ad1e5bc-f510-4752-b4f6-589e55317a90
caps.latest.revision: 37
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: f8818e32f48d094d67da1ba828f5b5a5049cace0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="define-parameters-in-the-mdx-query-designer-for-analysis-services"></a>Définir des paramètres dans le Concepteur de requêtes MDX pour Analysis Services
  Pour paramétrer une requête MDX pour une source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous devez ajouter un paramètre de requête à la requête. Dans le Concepteur de requêtes MDX, vous pouvez ajouter un paramètre de requête à la fois en mode Création et en mode Requête en spécifiant un filtre. Après avoir défini la requête avec un paramètre de requête, Reporting Services crée automatiquement un paramètre de rapport et un dataset pour fournir la liste des valeurs valides. Cela permet à un utilisateur de spécifier une valeur transmise directement à la requête.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-define-a-query-parameter-in-mdx-in-design-mode"></a>Pour définir un paramètre de requête dans MDX en mode Création  
  
1.  Dans le volet Données du rapport, cliquez avec le bouton droit sur un dataset créé à partir d’un type de source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis cliquez sur **Requête**. Le Concepteur de requêtes MDX s'ouvre en mode Création.  
  
2.  Faites glisser une dimension vers la zone de filtre et placez-la sur la première cellule de la colonne **Dimension** .  
  
3.  Dans la colonne **Hiérarchie** , choisissez une valeur dans la liste déroulante.  
  
4.  Dans la colonne **Opérateur** , choisissez un opérateur pour la liste déroulante.  
  
5.  Dans la colonne **Expression de filtre** , sélectionnez des valeurs individuelles dans la liste déroulante ou cliquez sur le membre **All** pour choisir toutes les valeurs.  
  
6.  Dans la colonne **Paramètres** , activez la case à cocher pour créer un paramètre de rapport.  
  
7.  Cliquez sur **Exécuter**.  
  
     Après avoir exécuté la requête, cliquez sur **Création** dans la barre d'outils pour basculer en mode Requête et afficher la requête MDX créée. Ne modifiez pas le texte de la requête en mode Requête si vous souhaitez continuer à utiliser le mode Création pour développer la requête. Pour rebasculer en mode Création, cliquez sur **Création** .  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Dans le volet des données de rapport, développez le nœud Paramètres pour afficher le paramètre de rapport créé automatiquement pour le filtre.  
  
     Pour afficher le dataset qui fournit des valeurs disponibles pour le paramètre de rapport, cliquez avec le bouton droit sur tout espace vide dans le volet des données de rapport, puis cliquez sur **Afficher les datasets masqués**. Le volet des données de rapport affiche tous les datasets dans le rapport.  
  
### <a name="to-define-a-query-parameter-in-mdx-in-query-mode"></a>Pour définir un paramètre de requête dans MDX en mode Requête  
  
1.  Dans le volet Données du rapport, cliquez avec le bouton droit sur un dataset créé à partir d’un type de source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , puis cliquez sur **Requête**. Le Concepteur de requêtes MDX s'ouvre en mode Création.  
  
2.  Dans la barre d'outils, cliquez sur **Création** pour passer au mode Requête.  
  
3.  Dans la barre d’outils du Concepteur de requêtes MDX, cliquez sur **Paramètres de la requête** (![Icône de la boîte de dialogue Paramètres de la requête](../../reporting-services/report-data/media/iconqueryparameter.gif "Icône de la boîte de dialogue Paramètres de la requête")). La boîte de dialogue Paramètres de la requête s'ouvre.  
  
4.  Dans la colonne **Paramètre**, cliquez sur **\<Entrez le paramètre**, puis tapez le nom d’un paramètre.  
  
5.  Dans la colonne **Dimension** , choisissez une valeur dans la liste déroulante.  
  
6.  Dans la colonne **Hiérarchie** , choisissez une valeur dans la liste déroulante.  
  
7.  Dans la colonne **Valeurs multiples** , activez la case à cocher pour créer un paramètre à valeurs multiples.  
  
8.  Dans la colonne **Par défaut** , dans la liste déroulante, sélectionnez une valeur unique ou plusieurs valeurs selon votre choix de l’étape 5.  
  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
10. Dans la barre d'outils du Concepteur de requêtes, cliquez sur **Exécuter**.  
  
11. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Dans le volet des données de rapport, développez le nœud Paramètres pour afficher le paramètre de rapport créé automatiquement pour le filtre.  
  
     Pour afficher le dataset qui fournit des valeurs disponibles pour le paramètre de rapport, cliquez avec le bouton droit sur tout espace vide dans le volet des données de rapport, puis cliquez sur **Afficher les datasets masqués**. Le volet des données de rapport affiche tous les datasets dans le rapport.  
  
## <a name="see-also"></a> Voir aussi  
 [Type de connexion Analysis Services pour MDX (SSRS)](../../reporting-services/report-data/analysis-services-connection-type-for-mdx-ssrs.md)   
 [Interface utilisateur du concepteur de requêtes MDX Analysis Services](../../reporting-services/report-data/analysis-services-mdx-query-designer-user-interface.md)  
  
  
