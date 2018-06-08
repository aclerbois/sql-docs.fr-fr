---
title: Parties de rapports (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- "10543"
ms.assetid: 957f664c-8a7a-4532-b5a6-5f859c5840bd
caps.latest.revision: 12
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c6685cf99cb3a46b4b90d5e0126c4df05dcb9575
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-parts-report-builder-and-ssrs"></a>Publication de parties de rapports (Générateur de rapports et SSRS)
  Les éléments de rapport tels que les tableaux, matrices, graphiques et images peuvent être publiés en tant que *parties de rapports*. Les parties de rapports sont des éléments de rapports paginés qui ont été publiés séparément sur un serveur de rapports et qui peuvent être réutilisés dans d’autres rapports paginés. Les parties de rapports ont l'extension de fichier .rsc.  
  
 Les parties de rapports permettent désormais aux groupes de travail de tirer parti des différents rôles et forces des membres de leur équipe. Par exemple, si vous êtes chargé de la création de graphiques, vous pouvez enregistrer vos graphiques en tant que parties séparées que vous et vos collègues pouvez réutiliser dans d'autres rapports. Vous pouvez publier des parties de rapport sur un serveur de rapports ou un site SharePoint intégré à un serveur de rapports. Vous pouvez les réutiliser dans plusieurs rapports et les mettre à jour sur le serveur.  
  
 La partie de rapport que vous ajoutez à votre rapport gère une relation à l'instance de la partie de rapport sur le site ou serveur au moyen d'un ID unique. Après avoir ajouté des parties de rapport d'un site ou serveur à un rapport, vous pouvez les modifier, indépendamment de la partie de rapport d'origine sur le site ou serveur. Vous pouvez accepter les mises à jour que d'autres personnes ont apportées à la partie de rapport sur le site ou le serveur, et vous pouvez enregistrer à nouveau la partie de rapport modifiée sur le site ou le serveur, soit en ajoutant une nouvelle partie de rapport, soit en remplaçant la partie de rapport d'origine, si vous avez des autorisations suffisantes.  
  
##  <a name="ComponentWorkflow"></a> Cycle de vie d'une partie de rapport  
 ![rs_ComponentCreation](../../reporting-services/report-design/media/rs-componentcreation.gif "rs_ComponentCreation")  
  
1.  Une personne A crée un rapport avec un graphique qui dépend d'un dataset incorporé.  
  
2.  La personne A choisit de publier le graphique sur le serveur de rapports. Le Générateur de rapports affecte un ID unique au graphique publié. La personne A ne choisissant pas de partager le dataset, celui-ci reste incorporé dans le graphique.  
  
3.  Une personne B crée un rapport vide, recherche la bibliothèque de parties de rapport, trouve le graphique et l'ajoute au rapport. Le graphique fait maintenant partie du rapport de la personne B, avec le dataset incorporé. La personne B peut modifier les instances du graphique et du dataset qui sont dans le rapport. Cela n'aura aucun effet sur les instances du graphique et du dataset sur le serveur de rapports et ne rompra pas non plus la relation entre les instances dans le rapport et sur le serveur de rapports.  
  
     ![rs_componentupdate](../../reporting-services/report-design/media/rs-componentupdate.gif "rs_componentupdate")  
  
4.  Une personne C ajoute le graphique à un rapport et transforme ce graphique à barres dans le rapport en un graphique à secteurs.  
  
5.  La personne C a les autorisations de remplacer le graphique sur le serveur et le fait, en le republiant sur le serveur. La copie publiée du graphique sur le serveur est ainsi mise à jour. La personne C ne choisissant pas non plus de partager le dataset, celui-ci reste incorporé dans le graphique.  
  
6.  La personne B accepte le graphique mis à jour du serveur. Les modifications que la personne B avait apportées au graphique dans son rapport sont ainsi remplacées.  
  
  
##  <a name="PublishingComponents"></a> Publication de parties de rapport  
 Lorsque vous publiez une partie de rapport, le Générateur de rapports lui affecte un ID unique, qui est distinct du nom de partie de rapport. Le Générateur de rapports gère cet ID, quelles que soient les autres modifications que vous avez apportées à la partie de rapport. L'ID lie l'élément de rapport d'origine dans votre rapport à la partie de rapport. Lorsque d'autres auteurs de rapports réutilisent la partie de rapport, l'ID lie également la partie de rapport dans leur rapport à celle sur le serveur de rapports.  
  
 Voici les éléments de rapport que vous pouvez publier comme parties de rapport :  
  
-   Graphiques  
  
-   Jauges  
  
-   Images  
  
-   Cartes  
  
-   Paramètres  
  
-   Rectangles  
  
-   Tables  
  
-   Matrices  
  
-   Listes  
  
 Lorsque vous publiez un élément de rapport qui affiche des données, tel qu'une table, une matrice ou un graphique, le dataset duquel l'élément de rapport dépend est enregistré avec lui, comme un dataset qui est incorporé. Vous pouvez également enregistrer le dataset séparément, comme un dataset partagé que vous et d'autres pouvez utiliser comme base pour d'autres parties de rapport. Pour plus d’informations, consultez [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md).  
  
 Certaines parties de rapport peuvent contenir d'autres éléments de rapport. Par exemple, une table peut contenir un graphique, et un rectangle peut contenir une matrice et un graphique. Lorsque vous publiez un élément de rapport qui contient d'autres éléments de rapport, ils sont enregistrés en tant qu'unité. Les autres éléments de rapport sont enregistrés incorporés dans la partie de rapport de conteneur. Vous ne pouvez pas les mettre à jour séparément, et vous ne pouvez pas enregistrer les éléments dans le conteneur en tant que parties de rapport séparées.  
  
 Pour plus d’informations sur la publication de parties de rapports, consultez [Publier et republier des parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/publish-and-republish-report-parts-report-builder-and-ssrs.md).  
  
### <a name="modifying-report-part-metadata"></a>Modification des métadonnées de parties de rapport  
 Vous pouvez publier des parties de rapport avec les paramètres par défaut à un emplacement par défaut, ou vous pouvez enregistrer chaque partie de rapport à un emplacement différent et modifier les métadonnées, telles que le titre et la description.  
  
 Il est judicieux de donner un nom simple et une description à la partie de rapport lorsque vous la publiez pour aider les personnes à l'identifier lors de la recherche. Vous pourriez finir avec un grand nombre de parties de rapport portant des noms semblables sur votre site ou serveur. Envisagez d'utiliser des conventions d'affectation des noms pour illustrer les relations entre les parties de rapport et leurs éléments dépendants.  
  
 Pensez également à enregistrer les sources de données partagées, les datasets partagés et les parties de rapport qui en dépendent dans le même dossier.  
  
 Vous pouvez également modifier la description dans le volet Propriétés.  
  
  
##  <a name="ReusingComponents"></a> Réutilisation de parties de rapport  
 La façon la plus facile de créer un rapport est d'ajouter une partie de rapport existante, comme une table ou un graphique, à votre rapport à partir de la bibliothèque de parties de rapport. Après l'avoir ajoutée à votre rapport, vous pouvez la modifier comme vous le souhaitez ou accepter les mises à jour du serveur. La modification de l'élément de rapport dans votre rapport n'affectera pas l'instance de la partie de rapport publiée sur le site ou serveur et ne rompra pas non plus la relation entre l'instance dans le rapport et sur le site ou serveur. Si vous avez des autorisations suffisantes, vous pouvez enregistrer à nouveau la copie mise à jour sur le site ou serveur. Si quelqu'un d'autre modifie la copie sur le site ou serveur, vous pouvez décider de garder votre copie telle quelle ou la mettre à jour pour qu'elle soit identique à celle du site ou serveur.  
  
### <a name="searching-for-report-parts"></a>Recherche de parties de rapport  
 Vous recherchez des parties de rapport à ajouter à votre rapport dans la bibliothèque de parties de rapport. Vous pouvez filtrer les parties de rapport en fonction de leur nom ou d'une partie de celui-ci, de leur créateur, de la personne qui a apporté la dernière modification ou du moment de cette dernière modification, de l'emplacement de stockage ou de leur type. Par exemple, vous pourriez rechercher tous les graphiques créés la semaine dernière par l'un de vos collègues.  
  
 Vous pouvez afficher les résultats de la recherche sous la forme de miniatures ou d'une liste et les trier par nom, par dates de création et de modification, et par créateur. Pour plus d’informations, consultez [Rechercher des parties de rapports et définir un dossier par défaut &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/browse-for-report-parts-and-set-a-default-folder-report-builder-and-ssrs.md).  
  
### <a name="what-comes-with-a-report-part"></a>Éléments associés à une partie de rapport  
 Lorsque vous ajoutez une partie de rapport à votre rapport, vous ajoutez également tout ce qu'elle doit comporter pour fonctionner. Par exemple, tout objet qui affiche des données dépend d'un dataset, autrement dit, une requête et une connexion à une source de données. Elle peut également avoir un ou plusieurs paramètres. Tous les éléments dont elle dépend sont ses *dépendances*, et l’ensemble de ces éléments, ou les pointeurs vers ces derniers, sont inclus avec la partie de rapport du moment où vous l’ajoutez à votre rapport. Le dataset et les paramètres apparaissent dans le volet des données de rapport de votre rapport.  
  
 Le dataset pour la partie de rapport peut être incorporé dans la partie de rapport, ou il peut s'agir d'un dataset séparé et partagé vers lequel la partie de rapport pointe. S'il est incorporé dans la partie de rapport, vous êtes peut-être en mesure de le modifier. Dans le cas d'un dataset partagé, il s'agit d'un objet séparé pour lequel vous avez besoin d'autorisations. Pour plus d’informations sur les datasets incorporés et partagés, consultez [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md).  
  
### <a name="resolving-naming-conflicts"></a>Résolution de conflits d'attribution de noms  
 Lorsque vous ajoutez une partie de rapport, le Générateur de rapports résout tous les conflits de noms. Par exemple, si vous avez déjà un élément Chart1 dans votre rapport et que vous ajoutez une partie de rapport appelée Chart1, le Générateur de rapports renomme automatiquement la nouvelle partie de rapport Chart2. Si vous avez déjà un élément Dataset1 dans votre rapport et que vous ajoutez une partie de rapport qui fait référence à un dataset différent qui est également appelé Dataset1, le Générateur de rapports renomme le nouveau dataset Dataset2 et met à jour les références.  
  
### <a name="adding-more-than-one-report-part"></a>Ajout de plusieurs parties de rapport  
 Vous pouvez ajouter un nombre infini de parties de rapport à votre rapport. Toutefois, vous ne pouvez ajouter qu'une seule partie de rapport à la fois. Vous pouvez même ajouter plusieurs instances d'une partie de rapport au même rapport. Elles auront toutes des noms uniques, mais seront toutes des instances de la même partie de rapport sur le serveur et auront le même ID unique.  
  
 Lorsque vous ajoutez une autre partie de rapport qui utilise un dataset identique à un dataset déjà dans votre rapport, l'Assistant n'ajoute pas une autre version de ce dataset à votre rapport ; il redirige les références dans la partie de rapport vers le dataset existant. Pour plus d’informations, consultez [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md).  
  
  
##  <a name="UpdatingComponents"></a> Mise à jour de parties de rapport avec les modifications du serveur  
 Chaque fois que vous ouvrez un rapport, le Générateur de rapports vérifie si les instances de serveur des parties de rapport de ce rapport ont été mises à jour sur le serveur. Il recherche également des modifications dans les éléments dépendants des parties de rapport, tels que le dataset et les paramètres. Si des parties de rapport publiées ou leurs dépendances ont été mises à jour sur le serveur, une barre d'informations dans votre rapport affiche le nombre d'éléments mis à jour. Vous pouvez choisir d'afficher et d'accepter ou de rejeter les mises à jour ou de faire disparaître la barre d'informations. Si vous choisissez d'afficher les mises à jour, une miniature de la partie de rapport, de la personne qui l'a modifié en dernier et du moment de cette modification apparaît. Vous pouvez alors accepter certains ou l'ensemble des éléments mis à jour.  
  
> [!NOTE]  
>  Vous pouvez désactiver la barre d'informations, auquel cas vous ne serez plus informé en cas de modification d'une partie de rapport. Vous définissez cette option lorsque vous ajoutez la partie de rapport à votre rapport. Même si vous avez désactivé la barre d'informations, vous pouvez toujours rechercher des mises à jour. Pour plus d’informations, consultez [Vérifier la présence de mises à jour ou désactiver les mises à jour (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/9c69792d-d7c4-453b-ae2f-6d2d071d8606).  
  
 Le Générateur de rapports recherche des différences entre la date de la dernière mise à jour de la partie de rapport sur le serveur et la date de la dernière synchronisation de la partie de rapport avec le serveur. Il ne vérifie pas la date à laquelle vous avez modifié la partie de rapport dans votre rapport. Ainsi, la partie de rapport dans votre rapport et celle sur le serveur peuvent être assez différentes mais, lorsque le Générateur de rapports recherche des mises à jour, il n'en trouve pas.  
  
### <a name="accepting-updates"></a>Acceptation des mises à jour  
 Lorsque vous acceptez une mise à jour pour une partie de rapport, elle remplace complètement la copie de la partie de rapport déjà dans votre rapport. Vous ne pouvez pas combiner les fonctionnalités de la partie de rapport dans le rapport avec celles de la partie de rapport publiée sur le serveur. Toutefois, si vous avez modifié l'une des dépendances de la partie de rapport, telles qu'un dataset incorporé, le Générateur de rapports ne copie pas sur la dépendance déjà dans votre rapport. Il télécharge une nouvelle copie de la dépendance et met à jour la partie de rapport pour pointer vers la nouvelle copie.  
  
### <a name="reverting-to-a-previous-version-of-a-report-part"></a>Rétablissement d'une version précédente d'une partie de rapport  
 Si vous avez modifié une version d’une partie de rapport dans votre rapport et décidez de la remplacer par la version qui se trouve sur le serveur, vous ne pouvez pas pour cela utiliser la boîte de dialogue **Mise à jour** . La mise à jour concerne uniquement les parties de rapport qui ont changé sur le serveur depuis que vous les avez téléchargées.  
  
 Pour rétablir la version sur le serveur, il vous suffit de supprimer la version que vous avez dans votre rapport et de la rajouter.  
  
  
##  <a name="RepublishingComponents"></a> Mise à jour de parties de rapport déjà sur le serveur  
 Vous pouvez choisir de mettre à jour une partie de rapport existante sur le serveur ou de la publier comme une nouvelle partie de rapport sans remplacer la partie existante. Lorsque vous mettez à jour la partie de rapport sur le serveur, les copies de la partie de rapport dans d'autres rapports ne sont pas modifiées automatiquement. Si d'autres auteurs de rapports ont ajouté cette partie de rapport à un rapport, ils sont informés de la modification la prochaine fois qu'ils ouvrent ce rapport. Ils peuvent choisir d'accepter ou non vos modifications.  
  
 Si vous choisissez de la publier comme une nouvelle partie de rapport, le Générateur de rapports lui donne un nouvel ID unique et elle n'est plus liée à la partie de rapport d'origine.  
  
 Si le dataset est incorporé dans la partie de rapport, chaque fois que vous publiez cette dernière, le dataset figure dans la boîte de dialogue **Publier les parties de rapports** . Les datasets partagés ne sont pas affichés dans la boîte de dialogue **Publier les parties de rapports** .  
  
  
##  <a name="RptPartsRptDesigner"></a> Utilisation de parties de rapports dans le Concepteur de rapports  
 Les parties de rapports fonctionnent un peu différemment dans le Concepteur de rapports de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Dans le Concepteur de rapports, la publication est unidirectionnelle : vous pouvez publier une partie de rapport à partir du Concepteur de rapports, mais vous ne pouvez pas réutiliser une partie de rapport existante à partir de ce même Concepteur. Pour plus d’informations, consultez [Parties de rapports dans le Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md).  
  
##  <a name="HowTo"></a> Rubriques de procédures  
 [Publier et republier des parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/publish-and-republish-report-parts-report-builder-and-ssrs.md)  
  
 [Rechercher des parties de rapports et définir un dossier par défaut &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/browse-for-report-parts-and-set-a-default-folder-report-builder-and-ssrs.md)  
  
 [Vérifier la présence de mises à jour ou désactiver les mises à jour (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/9c69792d-d7c4-453b-ae2f-6d2d071d8606)  
  
## <a name="see-also"></a> Voir aussi  
 [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Résoudre les problèmes liés aux parties de rapports (Générateur de rapports et SSRS)](http://msdn.microsoft.com/en-us/d9fe1932-46e7-421b-a8a9-4c54d9576e94)   
 [Gestion de parties de rapport](../../reporting-services/report-design/managing-report-parts.md)  
  
  
