---
title: Configurer l’ensemble de champs par défaut pour les rapports Power View | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 32d8d9c4acbc1c5eae47e90709c4ffdedd269f11
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="power-view---configure-default-field-set-for-reports"></a>Power View - configurer l’ensemble de champs par défaut pour les rapports
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Un ensemble de champs par défaut est une liste prédéfinie de colonnes et de mesures qui sont automatiquement ajoutées à une zone de dessin de rapport [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] quand vous sélectionnez la table dans la liste des champs du rapport. Les auteurs de modèles tabulaires peuvent créer un ensemble de champs par défaut pour éliminer les étapes redondantes pour les auteurs de rapport qui utilisent le modèle pour leurs rapports. Par exemple, si vous savez que la plupart des auteurs de rapport qui utilisent des coordonnées de clients souhaitent toujours voir un nom de contact, un numéro de téléphone principal, une adresse de messagerie et un nom de société, vous pouvez présélectionner ces colonnes afin qu'elles soient toujours ajoutées à la zone de dessin du rapport lorsque l'auteur clique sur la table Customer Contact.  
  
> [!NOTE]  
>  Un ensemble de champs par défaut s'applique uniquement à un modèle tabulaire utilisé comme modèle de données dans [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]. Les ensembles de champs par défaut ne sont pas pris en charge dans les rapports de tableau croisé dynamique Excel.  
  
## <a name="creating-a-default-field-set"></a>Création d'un ensemble de champs par défaut  
 Vous pouvez déterminer les champs, le cas échéant, qui sont inclus par défaut chaque fois qu’une table spécifique est sélectionnée dans [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]. Vous pouvez également déterminer l'ordre dans lequel les champs s'affichent dans la liste. Pour spécifier un ensemble de champs par défaut, vous définissez des propriétés de rapport dans le projet de modèle tabulaire.  
  
#### <a name="to-add-a-default-field-set"></a>Pour ajouter un ensemble de champs par défaut  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur la table (l’onglet) pour laquelle vous configurez une liste de champs par défaut.  
  
2.  Dans la fenêtre **Propriétés** , dans la propriété **Ensemble de champs par défaut** , cliquez sur **Cliquer pour modifier**.  
  
3.  Dans la boîte de dialogue Ensemble de champs par défaut, sélectionnez un ou plusieurs champs. Vous pouvez choisir n'importe quel champ de la table, y compris des mesures. Maintenez la touche Maj enfoncée pour sélectionner une plage, ou la touche Ctrl pour sélectionner des champs individuels.  
  
4.  Cliquez sur **Ajouter** pour les ajouter à l’ensemble de champs par défaut.  
  
5.  Utilisez les boutons Monter et Descendre pour indiquer l'ordre dans la liste de champs. Les champs sont ajoutés au rapport dans l'ordre défini pour l'ensemble de champs.  
  
6.  Répétez ces étapes pour les autres tables de votre classeur.  
  
## <a name="next-step"></a>Étape suivante  
 Après avoir créé un ensemble de champs par défaut, vous pouvez encore influencer l'expérience de conception de rapports en spécifiant les étiquettes par défaut, les images par défaut, le comportement de groupe par défaut, ou si les lignes qui contiennent la même valeur sont regroupées dans une ligne ou répertoriées individuellement. Pour plus d’informations, consultez [configurer des propriétés de comportement de Table pour les rapports Power View](../../analysis-services/tabular-models/power-view-configure-table-behavior-properties-for-reports.md).  
  
  
