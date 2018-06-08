---
title: Boîte de dialogue Configurer | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: service
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configure.f1
- sql13.SSIS.SSMS.ISPROJECTPROP.REFERENCES.F1
- sql13.SSIS.SSMS.ISPROJECTPROP.PARAMETERS.F1
ms.assetid: 10183c8d-b1be-420f-972a-96ea97d4f4d8
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 751a027e0b07de1ce451cd77b0e8b05d38f14591
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-dialog-box"></a>Boîte de dialogue Configurer
  Utilisez la boîte de dialogue **Configurer** pour configurer les paramètres, les gestionnaires de connexions, ainsi que les références aux environnements, pour les packages et les projets.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir la boîte de dialogue Configurer](#open_dialog)  
  
-   [Définir les options de la page Paramètres](#parameter)  
  
-   [Définir les options de la page Références](#references)  
  
##  <a name="open_dialog"></a> Ouvrir la boîte de dialogue Configurer  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     Vous vous connectez à l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données SSISDB.  
  
2.  Dans l'Explorateur d'objets, développez l'arborescence pour afficher le nœud **Integration Services Catalogues** .  
  
3.  Développez le nœud **SSISDB** .  
  
4.  Développez le dossier qui contient le package ou le projet à configurer.  
  
5.  Cliquez avec le bouton droit sur le package ou le projet, puis cliquez sur **Configurer**.  
  
##  <a name="parameter"></a> Définir les options de la page Paramètres  
 Utilisez la page **Paramètres** pour afficher les noms et valeurs des paramètres, ainsi que pour modifier ces valeurs.  
  
 Sélectionnez l’étendue des paramètres affichés sous les onglets **Paramètres** et **Gestionnaires de connexions** , dans la liste déroulante **Étendue** .  
  
 Vous trouverez ci-dessous la liste des options de l'onglet **Paramètres** .  
  
 **Conteneur**  
 Indique l'objet qui contient le paramètre.  
  
 **Nom**  
 Indique le nom du paramètre.  
  
 **Value**  
 Indique la valeur du paramètre. Cliquez sur le bouton de sélection pour modifier la valeur dans la boîte de dialogue **Définir la valeur du paramètre** .  
  
 Vous trouverez ci-dessous la liste des options de l'onglet **Gestionnaires de connexions** . Utilisez cet onglet pour modifier les valeurs des propriétés du Gestionnaire de connexions. Les paramètres sont automatiquement générés sur le serveur SSIS pour les propriétés.  
  
 **Conteneur**  
 Indique l'objet qui contient le gestionnaire de connexions.  
  
 **Nom**  
 Indique le nom du gestionnaire de connexions.  
  
 **Nom de la propriété**  
 Indique le nom de la propriété du gestionnaire de connexions.  
  
 **Value**  
 Indique la valeur affectée à la propriété du gestionnaire de connexions. Cliquez sur le bouton de sélection pour modifier la valeur dans la boîte de dialogue **Définir la valeur du paramètre** . Vous pouvez entrer une valeur littérale, mapper une variable d'environnement qui contient la valeur à utiliser, ou utiliser la valeur par défaut du package.  
  
##  <a name="references"></a> Définir les options de la page Références  
 Utilisez la page **Références** pour ajouter et supprimer des références aux environnements, ainsi que pour accéder aux propriétés des environnements.  
  
 Un environnement spécifie les valeurs d'exécution des packages contenus dans les projets que vous avez déployés sur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 **Environnement**  
 Indique l'environnement.  
  
 **Dossier d'environnement**  
 Indique le dossier qui contient l'environnement.  
  
 **Ouvrir**  
 Cliquez pour ouvrir la boîte de dialogue **Propriétés d’environnement** .  
  
 **Ajouter**  
 Cliquez pour ajouter une référence à un environnement. Dans la boîte de dialogue **Parcourir les environnements** , cliquez sur un environnement, puis cliquez sur **OK**.  
  
 Vous pouvez sélectionner un environnement contenu dans n'importe quel dossier de projet sous le nœud **SSISDB** .  
  
 **Supprimer**  
 Cliquez sur un environnement répertorié dans la zone **Références** , puis sur **Supprimer**.  
  
  
