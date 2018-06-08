---
title: Nouvelles fonctionnalités de l’interface graphique utilisateur de SSMA pour Oracle (OracleToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-oracle
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 62e2d30f-a73f-42d9-a6ab-3510a8198f4e
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: v-thobro
ms.openlocfilehash: 4fdbb359f9c8ebd9d1ecd4133b115c7700a6cda5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-gui-features-in-ssma-for-oracle-oracletosql"></a>Nouvelles fonctionnalités de l’interface graphique utilisateur de SSMA pour Oracle (OracleToSQL)
Ce chapitre décrit les nouvelles fonctionnalités de l’Interface utilisateur SSMA.  
  
## <a name="layouts"></a>Mises en page  
Cette fonctionnalité vous permet de choisir l’une des deux fenêtres prédéfinie à la sortie de mise en page ou créer votre propre disposition. Pour accéder au sous-menu mise en forme, dans le menu Affichage pointez aux mises en page. Vous pouvez choisir une des dispositions existantes, ajouter la nouvelle disposition ou gérer des dispositions.  
  
### <a name="add-current-layout"></a>Ajouter la disposition actuelle  
Pour enregistrer la disposition actuelle de windows, dans le menu Affichage, pointez sur mises en page, puis cliquez sur Ajouter la disposition actuelle.  
  
### <a name="choose-predefined-layout"></a>Choisir une mise en page prédéfinie  
Pour choisir une des dispositions prédéfinies, dans le menu Affichage, pointez aux mises en page, puis cliquez sur par défaut de mise en page ou sans explorateurs. Vous pouvez également utiliser les raccourcis Ctrl + Alt + 1 ou Ctrl + Alt + 2 pour les dispositions prédéfinies.  
  
### <a name="choose-user-defined-layout"></a>Choisissez défini par l’utilisateur de mise en page  
Pour choisir défini par l’utilisateur de mise en page, dans le menu Affichage, pointez sur mises en page, puis cliquez sur un de la mise en page définie par l’utilisateur. Vous pouvez également utiliser les raccourcis définis pour les mises en page.  
  
### <a name="manage-layouts"></a>Gérer des dispositions  
Pour ouvrir la boîte de dialogue Gérer les mises en page, dans le menu Affichage, pointez sur mises en page et cliquez sur Gérer les dispositions. Dans la boîte de dialogue Gérer les dispositions, vous trouverez une liste des configurations existantes sur le côté gauche de la boîte de dialogue. Vous pouvez sélectionner la mise en page pour modifier ses paramètres. Vous pouvez également modifier l’ordre de mises en page dans la liste ou supprimer la disposition à l’aide des boutons en haut de la liste. Sur le côté droit de la boîte de dialogue, vous pouvez modifier les paramètres de disposition suivants :  
  
-   Nom de la mise en page  
  
-   Synchronisation des métadonnées des explorateurs  
  
-   Visibilité et la largeur des source et cible les explorateurs de métadonnées  
  
-   Visibilité de la source ou cible windows et leur taille  
  
-   Visibilité et la hauteur de fenêtres auxiliaires  
  
## <a name="bookmarks"></a>Signets  
Cette fonctionnalité vous permet de définir un ou plusieurs signets dans la source ou le code cible, rapide trouvé un signet à l’aide de raccourcis, gérer les signets avec une boîte de dialogue convivial.  
  
### <a name="toggle-bookmark"></a>Basculer le signet  
Vous pouvez définir/supprimer un signet comme suit :  
  
-   Utilisez le bouton Basculer le signet par-dessus la fenêtre SQL source ou cible  
  
-   Cliquez sur la zone grise située à gauche de la fenêtre SQL  
  
-   Utilisez Ctrl + Maj +&lt;0.9&gt; pour définir le signet numéroté  
  
### <a name="bookmark-navigation"></a>Navigation de signet  
Vous pouvez parcourir les signets, par le biais de plusieurs manières :  
  
-   Utilisez les boutons signet suivant, le signet précédent en haut de la fenêtre SQL  
  
-   Utilisez Ctrl +&lt;0.9&gt; pour trouver le signet numéroté  
  
-   Utilisez les boutons Atteindre ou afficher la Source dans la boîte de dialogue Gérer les signets  
  
### <a name="removing-bookmark"></a>Suppression de signet  
Vous pouvez supprimer un signet comme suit :  
  
-   Utilisez le bouton Effacer en haut de la fenêtre SQL pour supprimer tous les signets dans le document actif  
  
-   Utilisez les boutons Supprimer ou supprimer tout dans la boîte de dialogue Gérer les signets  
  
### <a name="manage-bookmarks"></a>Gérer les signets  
Pour ouvrir la boîte de dialogue Gérer les signets, dans le menu Edition, cliquez sur Gérer les signets. Dans la boîte de dialogue, vous verrez une liste des signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.  
  
## <a name="object-history"></a>Historique de l’objet  
L’historique de l’interface graphique utilisateur objet vous permet des avantages suivants lorsque vous accédez à objets :  
  
-   Vous pouvez utiliser les boutons Précédent et suivant pour naviguer dans les objets que vous avez déjà visitées  
  
-   Lorsque vous sauvegardez à l’objet, vous sauvegarder sur le même onglet que vous avez laissées  
  
-   Lorsque vous à l’objet et l’onglet est SQL, vous sauvegardez à la même position de curseur que vous avez laissées  
  
## <a name="advanced-search-capabilities"></a>Fonctionnalités de recherche avancées  
Fonctionnalités de recherche avancées fournissent les fonctionnalités de recherche puissantes et flexibles et permettent de trouver la déclaration de l’objet, obtenir des informations sur l’objet, effectuer une recherche rapide, effectuer des objets avancée recherche dans les catégories à l’aide de modèles, etc.  
  
### <a name="get-quick-information"></a>Obtenir des informations rapides  
Vous pouvez obtenir rapidement des informations sur l’objet à la position de curseur comme suit :  
  
-   Cliquez sur le bouton Info Express en haut de la fenêtre SQL  
  
-   Sélectionnez Info express dans le menu contextuel  
  
-   Appuyez sur Ctrl + Maj + espace  
  
### <a name="find-declaration"></a>Trouver de déclaration  
Vous pouvez accéder à la déclaration de l’objet à la position du curseur comme suit :  
  
-   Cliquez sur le bouton Atteindre la déclaration au-dessus de la fenêtre SQL  
  
-   Sélectionnez Atteindre la déclaration dans le menu contextuel  
  
-   Appuyez sur F12  
  
### <a name="quick-search"></a>Recherche rapide  
Vous pouvez effectuer la recherche de texte rapide à l’aide des fonctionnalités suivantes :  
  
-   Vous pouvez démarrer la recherche à l’aide du raccourci Ctrl + F  
  
-   Vous pouvez répéter la dernière recherche vers l’avant à l’aide de F3  
  
-   Vous pouvez répéter la dernière recherche vers l’arrière à l’aide de MAJ + F3  
  
-   Vous pouvez rechercher l’occurrence suivante du mot à la position du curseur à l’aide de Ctrl + F3  
  
-   Vous trouverez l’occurrence précédente du mot à la position du curseur à l’aide de Ctrl + Maj + F3  
  
-   Vous pouvez également effectuer toutes ces actions avec les éléments de menu.  
  
### <a name="advanced-search"></a>Recherche avancée  
Pour ouvrir la boîte de dialogue Recherche avancée, sur le point de menu Modifier Rechercher, puis cliquez sur recherche avancée. Dans la boîte de dialogue, vous serez en mesure de trouver un objet à l’aide du modèle. En haut de la boîte de dialogue, vous pouvez rechercher des catégories de zone et d’objet.  
  
