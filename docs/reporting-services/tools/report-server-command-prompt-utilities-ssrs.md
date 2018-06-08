---
title: Utilitaires d’invite de commandes du serveur de rapports (SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- rsconfig utility
- components [Reporting Services], command line utilities
- rs utility
- command prompt utilities [Reporting Services]
- rskeymgmt utility
ms.assetid: 68f2f9f4-f894-40ff-a71c-f9756bf4b68c
caps.latest.revision: 48
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 4f52c7216e42d3d0e233446de2521d403cafe610
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-server-command-prompt-utilities-ssrs"></a>Utilitaires d'invite de commandes du serveur de rapports (SSRS)
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend plusieurs utilitaires de ligne de commande qui permettent d’administrer un serveur de rapports. Ces utilitaires sont installés automatiquement lorsque vous installez un serveur de rapports.  
  
|Nom   |Fichier de commandes|Mode de déploiement pris en charge|Description|  
|----------|------------------|-------------------------------|-----------------|  
|Utilitaire RSS|rs.exe|Mode natif et mode SharePoint. La version [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] inclut la prise en charge du mode SharePoint.|[L’utilitaire rs.exe](../../reporting-services/tools/rs-exe-utility-ssrs.md) est un hôte de script que vous pouvez utiliser pour réaliser des opérations contenant des scripts. Par son intermédiaire, vous exécutez des scripts [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] pour publier des rapports, créer des éléments dans une base de données de serveur de rapports, copier des données entre des bases de données de serveurs de rapports, etc. Pour en savoir plus sur l’utilisation de scripts permettant d’administrer un serveur, consultez [Écrire des scripts pour les tâches d’administration et de déploiement](../../reporting-services/tools/script-deployment-and-administrative-tasks.md).|  
|Applets de commande Powershell||SharePoint uniquement|Pour obtenir la liste des applets de commande PowerShell, consultez [Applets de commande PowerShell pour le mode SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).|  
|rsconfig (utilitaire)|rsconfig.exe|Natif uniquement|[L’utilitaire rsconfig](../../reporting-services/tools/rsconfig-utility-ssrs.md) sert à configurer et à gérer une connexion de serveur de rapports à une base de données connexe, mais aussi à définir un compte d'utilisateur pour le traitement des rapports autonomes. Pour plus d’informations, consultez [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md). Pour en savoir plus sur la configuration de la connexion, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).|  
|Utilitaire Rskeymgmt|rskeymgmt.exe|Natif uniquement|[L’utilitaire rskeymgmt](../../reporting-services/tools/rskeymgmt-utility-ssrs.md) est un outil de gestion de clés de chiffrement. Utilisez-le pour sauvegarder, appliquer, recréer et supprimer des clés symétriques. Cet outil sert également à attacher une instance de serveur de rapports à une base de données de serveur de rapports partagée. Rskeymgmt est utile dans les opérations de récupération de base de données. Réutilisez une base de données dans une nouvelle installation en appliquant un exemplaire sauvegardé de clé symétrique. Si les clés ne peuvent pas être récupérées, cet outil permet de supprimer le contenu chiffré dont vous ne vous servez plus. Pour en savoir plus sur la gestion des clés et le stockage des données confidentielles, consultez [Stocker des données chiffrées du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md) et [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).|  
  
> [!NOTE]  
>  Si vous préférez utiliser un outil à interface utilisateur graphique, choisissez le Gestionnaire de configuration de Reporting Services au lieu de **rsconfig** et de **rskeymgmt**.  
  
## <a name="see-also"></a> Voir aussi  
 [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Outils de Reporting Services](../../reporting-services/tools/reporting-services-tools.md)   
 [Serveur de rapports Reporting Services &#40;mode natif&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)  
  
  
