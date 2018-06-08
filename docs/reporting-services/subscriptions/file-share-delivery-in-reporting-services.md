---
title: Remise par partage de fichiers dans Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], file share delivery
- file share delivery [Reporting Services]
ms.assetid: 9f338dd3-f68a-4355-b9d7-9b25dacf3b5e
caps.latest.revision: 54
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: bad4004a0bd151256397df94c329ae11fca1955b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="file-share-delivery-in-reporting-services"></a>Remise par partage de fichiers dans Reporting Services
  SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend une extension de remise de partage de fichiers qui vous permet de remettre un rapport dans un dossier. Cette extension est disponible par défaut et elle ne nécessite aucune configuration supplémentaire. Pour que la remise de fichier réussisse, vous devez définir des autorisations d'accès en écriture sur le dossier partagé. Le compte qui requiert des autorisations d’écriture peut être soit des informations d’identification configurées dans l’abonnement, soit un **compte de partage de fichiers** configuré pour le serveur de rapports. Pour plus d’informations sur le compte de partage de fichiers, consultez [Paramètres d’abonnement et compte de partage de fichiers &#40;Gestionnaire de configuration&#41;](../../reporting-services/install-windows/subscription-settings-and-a-file-share-account-configuration-manager.md). En outre, les utilisateurs qui demandent l'accès aux rapports doivent avoir des autorisations de lecture sur le dossier partagé.  
  
 Pour distribuer un rapport dans un partage de fichiers, vous devez définir un abonnement standard ou piloté par les données. Pour savoir comment utiliser la remise par partage de fichiers pour un abonnement piloté par les données, consultez [Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md). Par ailleurs, le compte qui exécute des abonnements de partage de fichiers distants nécessite des droits pour ouvrir une session locale sur l'ordinateur [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode natif &#124; [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en mode SharePoint|  
  
 **Dans cette rubrique :**  
  
-   [Rapports de caractéristiques remis aux dossiers partagés](#bkmk_Characteristics)  
  
-   [Dossiers cibles](#bkmk_target_folders)  
  
-   [Formats de fichier](#bkmk_file_formats)  
  
-   [Options de fichier](#bkmk_file_options)  
  
##  <a name="bkmk_Characteristics"></a> Rapports de caractéristiques remis aux dossiers partagés  
  
-   Contrairement aux rapports qui sont hébergés et gérés par un serveur de rapports, les rapports qui sont remis dans un dossier partagé sont des fichiers statiques.  
  
-   Les fonctionnalités interactives définies pour le rapport **ne fonctionnent pas** pour les rapports qui sont stockés en tant que fichiers dans le système de fichiers. Les fonctionnalités d'interaction sont représentées comme des éléments statiques. Par exemple, si vous remettez un rapport de matrice, le fichier généré offre une vue de niveau supérieur du rapport ; vous ne pouvez pas étendre les lignes et les colonnes pour afficher les données associées.  
  
-   Si le rapport contient des graphiques, la présentation par défaut est utilisée. Si le rapport contient un lien vers un autre rapport, le lien apparaît sous forme de texte statique.  
  
-   Si vous voulez conserver les fonctionnalités interactives dans un rapport remis, utilisez la remise par courrier électronique à la place. Le message électronique contient un lien vers le rapport hébergé sur le serveur de rapports et les utilisateurs peuvent utiliser les fonctionnalités interactives. Pour plus d’informations, consultez [Remise par courrier électronique dans Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md).  
  
##  <a name="bkmk_target_folders"></a> Dossiers cibles  
 Lorsque vous définissez un abonnement qui utilise la remise dans un partage de fichiers, vous devez spécifier un dossier existant comme dossier cible. Le serveur de rapports ne crée pas de dossiers sur le système de fichiers. Le dossier que vous spécifiez doit être accessible via une connexion réseau.  
  
 Vérifiez que les utilisateurs qui **afficheront les rapports** dans le dossier partagé disposent d’une autorisation de lecture.  
  
 Lors de la spécification d'un dossier cible dans un abonnement, utilisez le format UNC (Uniform Naming Convention) qui inclut le nom réseau de l'ordinateur. N'incluez pas de barre oblique inverse à la fin du chemin d'accès au dossier. L'exemple suivant illustre un chemin UNC :  
  
```  
\\<servername>\reportarchive\operations\2014  
```  
  
 Lorsque vous créez le dossier, tenez compte des limites de connexion requises. Le serveur de rapports nécessite deux connexions, mais vous devez inclure suffisamment de connexions pour la prise en charge des utilisateurs complémentaires qui veulent ouvrir des rapports sur le dossier partagé.  
  
##  <a name="bkmk_file_formats"></a> Formats de fichier  
 Les rapports peuvent être restitués sous plusieurs formats de fichier, tels que HTML, DOCX ou Excel. Pour enregistrer le rapport dans un format de fichier spécifique, sélectionnez le format de rendu au moment de la création de l'abonnement. Par exemple, en choisissant **Excel** , vous enregistrez le rapport sous la forme d'un fichier [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] . Bien que vous puissiez choisir n'importe quel format de rendu pris en charge, certains formats sont mieux adaptés que d'autres.  
  
 Pour la remise par partage de fichiers, choisissez un format qui assure la remise du rapport dans un seul fichier, où toutes les images et le contenu associé sont inclus dans le rapport. Les formats appropriés sont les suivants : archive Web, PDF, TIFF et Excel. Évitez le format HTML 4.0. Si votre rapport comporte des images, le format HTML 4.0 ne permettra pas de les inclure dans le fichier.  
  
##  <a name="bkmk_file_options"></a> Options de fichier  
 Lorsque vous créez un abonnement de partage de fichiers, vous pouvez configurer le mode de création du nom de fichier et spécifier si le fichier doit remplacer les versions précédentes du rapport. Un nom de fichier complet comprend trois parties : un nom, une extension et du texte ou un nombre ajouté au fichier pour créer un nom de fichier unique.  
  
 **Nom de fichier :** le nom de fichier est basé sur le nom du rapport, mais vous pouvez indiquer un nom personnalisé dans l’abonnement. L'extension est facultative, mais si vous la spécifiez, le serveur de rapports créera une extension qui correspond au format de rendu.  
  
 **Remplacer :** vous pouvez spécifier des options de remplacement afin de réutiliser le même nom de fichier pour chaque remise de rapport ou pour créer un nouveau fichier. Pour remplacer le fichier, vous devez utiliser les mêmes nom et extension de fichier.  
  
 Pour créer des noms de fichiers uniques pour la remise de rapport, il existe une autre approche qui consiste à inclure un élément d'horodatage dans le nom de fichier. Pour ce faire, ajoutez la variable **@timestamp** au nom de fichier (par exemple, *CompanySales@timestamp*). Avec cette approche, le nom de fichier est unique par définition : il ne sera jamais remplacé.  
  
 L’illustration suivante est un exemple de paramètres de fichier correspondant à un abonnement configuré pour la remise de partage de fichiers.  
  
 ![abonnement pour partage de fichiers](../../reporting-services/subscriptions/media/ssrs-file-share-subscription.png "abonnement pour partage de fichiers")  
  
## <a name="see-also"></a> Voir aussi  
 [Créer et gérer des abonnements pour les serveurs de rapports en mode natif](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)   
 [Paramètres d’abonnement et compte de partage de fichiers &#40;Gestionnaire de configuration&#41;](../../reporting-services/install-windows/subscription-settings-and-a-file-share-account-configuration-manager.md)  
  
  
