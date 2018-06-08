---
title: Résoudre les problèmes de rendu des rapports Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 02/27/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1e0fb399-4c16-438a-92cb-db3e877896d0
caps.latest.revision: 4
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 1e067f6a03b839f9de233bc71eccbf6631bb599f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-reporting-services-report-rendering-issues"></a>Résoudre les problèmes de rendu des rapports Reporting Services
Une fois les données et les informations de mise en page du rapport combinées, le rapport compilé est envoyé à un convertisseur de rapport. Par exemple, lorsque vous affichez l'aperçu d'un rapport localement, vous utilisez le convertisseur HTML pour afficher le rapport compilé. Utilisez cette rubrique pour vous aider à résoudre les problèmes spécifiques au rendu de rapport.   
  
## <a name="why-do-i-have-extra-white-space-including-blank-pages-in-my-report"></a>Pourquoi des espaces blancs supplémentaires, y compris des pages vierges, apparaissent-ils dans mon rapport ?  
Les éléments de rapport sont automatiquement ajustés pendant le traitement de rapport pour conserver l'espace blanc qui est défini dans le cadre du rapport. L'espace blanc dans la vue de conception du rapport est conservé. Dans l'aire de conception du rapport, l'arrière-plan blanc représente l'espace blanc qui est conservé lorsqu'un rapport est affiché, exporté ou imprimé, en fonction du support cible.  
  
### <a name="white-space-and-page-breaks-interact-during-rendering"></a>L'espace blanc et les sauts de page interagissent au cours du rendu  
Lorsque vous affichez un rapport ou exportez le rapport dans un format de fichier, l'extension de rendu associée traite le rapport et l'enregistre au format de fichier spécifié. Chaque extension de rendu traite l'espace blanc dans un rapport selon des règles spécifiques. L'espace blanc est également affecté par les propriétés de mise en page, les sauts de page définis sur les éléments de rapport, la position relative des éléments de rapport placés dans le corps du rapport, la propriété KeepTogether pour certains éléments de rapport et si des éléments de rapport se trouvent ou non dans des conteneurs parents.   
  
Pour éliminer les pages supplémentaires dues à la largeur du rapport, faites glisser le bord de l'aire de conception du rapport pour l'aligner avec l'élément du rapport le plus à l'extérieur. Pour une mise en page de rapport de gauche à droite, faites glisser le bord droit à aligner avec l'élément du rapport le plus à l'extérieur. Pour en savoir plus, voir [Comportement de rendu (Générateur de rapports et SSRS)](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
### <a name="white-space-is-not-preserved-at-the-end-of-a-report"></a>L'espace blanc n'est pas conservé à la fin d'un rapport  
Reporting Services fournit une option vous permettant de conserver ou d'éliminer l'espace blanc à la fin d'un rapport.   
  
Pour conserver l'espace blanc à la fin d'un rapport, sélectionnez le rapport puis, dans le volet Propriétés, faites défiler la liste jusque ConsumeContainerWhitespace et tapez False.   
  
## <a name="why-do-my-reports-look-different-when-exported-to-different-formats"></a>Pourquoi mes rapports semblent-ils différents en cas d'exportation dans des formats différents ?  
Après avoir exécuté un rapport, vous pouvez l'exporter sous un autre format, tel que Excel, Word ou PDF. Des règles et limitations peuvent s'appliquer au format d'exportation choisi. Vous pouvez prendre en compte de nombreuses limitations lors de la création du rapport. Vous devrez peut-être utiliser une mise en page légèrement différente dans votre rapport, aligner soigneusement les éléments du rapport, restreindre les pieds de page du rapport à une seule ligne de texte, etc. Vous pouvez également utiliser la fonction globale intégrée RenderFormat pour appliquer de manière conditionnelle un modèle de mise en page de rapport selon le type de convertisseurs. Les autres fonctions globales intégrées peuvent vous aider à gérer la pagination dans le format exporté et nommer les onglets de la feuille de travail dans Excel. Pour en savoir plus, voir [Exportation de rapports (Générateur de rapports et SSRS)](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md) et [Références à des champs Globals et Users prédéfinis (Générateur de rapports et SSRS)](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).  
  
## <a name="how-can-i-view-all-my-report-data-on-one-page"></a>Comment afficher toutes les données de mon rapport sur une page ?  
Pour afficher de manière interactive les rapports ne contenant pas beaucoup de données, vous pouvez afficher toutes les données sur une page.   
  
Pour afficher toutes les données sur une page dans les convertisseurs de saut de page conditionnellele, dans les propriétés de rapport, attribuez à InteractiveHeight la valeur 0. Dans les convertisseurs de saut de page conditionnellele, les sauts de page existants sont ignorés.   
  
> [!NOTE]  
> Si un rapport ne comprend pas de sauts de page, le rapport entier doit être traité avant que vous ne puissiez afficher la première page.   
  
Pour en savoir plus sur les catégories de convertisseurs, voir [Comportement de rendu (Générateur de rapports et SSRS)](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
## <a name="reports-do-not-run-when-your-browser-is-configured-to-prompt-for-credentials"></a>Les rapports ne s'exécutent pas lorsque votre navigateur est configuré pour demander des informations d'identification  
L'affichage des rapports échoue et un message d'erreur apparaît si votre navigateur est configuré pour demander les informations d'identification et si votre source de données est configurée pour une authentification intégrée Windows. Cela se produit dans les conditions suivantes : votre source de données figure sur un autre ordinateur que le serveur de rapports, la source de données est configurée pour utiliser l'authentification Windows et le navigateur est configuré de façon à demander les informations d'identification. Voici quelques exemples de messages qui s'affichent.  
  
Lorsque la source de données est configurée pour un type de connexion Microsoft SQL Server :  
`An error has occurred during report processing.`  
`Cannot create a connection to data source 'localhost'.`  
`Login failed for user '(null)'. Reason: Not associated with a trusted SQL Server connection.`  
  
Lorsque la source de données est configurée pour un type de connexion de liste Microsoft SharePoint :  
`An error occurred during client rendering.`   
`An error has occurred during report processing.`   
`Query execution failed for dataset 'DataSet1'.`   
`The request failed with HTTP status 401: Unauthorized.`  
  
**Pour corriger ce problème :** modifiez la source de données de manière à utiliser les informations d’identification stockées à la place des informations d’identification Windows.  
  
**Ce problème s’applique à :** navigateurs configurés pour demander les informations d’identification.  
  
## <a name="see-also"></a> Voir aussi  
[Erreurs et événements (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
[Résoudre les problèmes de récupération des données avec des rapports Reporting Services](../../reporting-services/troubleshooting/troubleshoot-data-retrieval-issues-with-reporting-services-reports.md)  
[Résolution des problèmes d’abonnements et de remise de Reporting Services](../../reporting-services/troubleshooting/troubleshoot-reporting-services-subscriptions-and-delivery.md)  
  
  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect.md)]

