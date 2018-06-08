---
title: Stocker des données chiffrées du serveur de rapports (Gestionnaire de configuration de SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 05/31/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], encryption
- credentials [Reporting Services]
- cryptography [Reporting Services]
- confidential reports [Reporting Services]
- encryption [Reporting Services]
- databases [Reporting Services], encryption
ms.assetid: ac0f4d4d-fc4b-4c62-a693-b86e712e75f2
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3feb813133578c1364be0642504e6b89b0653da4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ssrs-encryption-keys---store-encrypted-report-server-data"></a>Clés de chiffrement SSRS - Stocker des données chiffrées du serveur de rapports
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stocke des valeurs chiffrées dans la base de données du serveur de rapports et dans les fichiers de configuration. La plupart des valeurs chiffrées sont des informations d'identification utilisées pour accéder à des sources de données externes fournissant des données aux rapports. Cette rubrique indique quelles valeurs sont chiffrées et décrit la fonctionnalité de chiffrement utilisée dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], ainsi que les autres types de données confidentielles stockées qu'il convient de connaître.  
  
## <a name="encrypted-values"></a>Valeurs chiffrées  
 La liste suivante décrit les valeurs qui sont stockées dans une installation [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   Informations de connexion et informations d'identification utilisées par un serveur de rapports pour la connexion à une base de données de serveur de rapports qui stocke des données de serveur internes.  
  
     Ces valeurs sont spécifiées et chiffrées au cours de l'installation ou de la configuration du serveur de rapports. Vous pouvez mettre à jour les informations de connexion à tout moment par le biais de l’outil de configuration de Reporting Services ou de l’utilitaire **rsconfig** . Le chiffrement des paramètres de configuration s'effectue à l'aide de la clé au niveau machine de l'ordinateur local qui est disponible pour tous les utilisateurs. Les informations chiffrées de connexion au serveur de rapports sont stockées dans le fichier rsreportserver.config (aucun autre fichier de configuration ne possède de paramètres chiffrés). Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Informations d'identification stockées, utilisées par un serveur de rapports pour se connecter à des sources de données externes qui fournissent des données à un rapport.  
  
     Ces valeurs sont définies lorsque vous configurez les informations d'une source de données pour un rapport, puis elles sont stockées sous la forme de valeurs chiffrées dans une base de données du serveur de rapports. Le serveur de rapports utilise une clé symétrique pour chiffrer et déchiffrer ces données. Pour plus d’informations sur les informations d’identification stockées, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md) dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Un compte d'utilisateur sans assistance utilisé par le serveur de rapports pour se connecter à d'autres ordinateurs afin d'extraire des fichiers images externes ou des données externes utilisées dans un rapport.  
  
     Ce compte est utilisé lorsqu'une connexion à un ordinateur distant est requise et qu'aucune autre information d'identification n'est disponible pour établir la connexion. Ce compte est principalement utilisé pour gérer le traitement de rapports sans assistance qui n'utilise pas d'informations d'identification pour accéder à une source de données. Si vous créez des rapports basés sur des sources de données qui ne nécessitent pas ou n'utilisent pas d'informations d'identification lors de l'accès aux données, vous devez configurer ce compte pour permettre son utilisation par le serveur de rapports.  
  
     Ce compte se révèle indispensable dans certains cas et seul l’outil de configuration de Reporting Services ou l’utilitaire **rsconfig**peut créer un tel compte. Cette valeur est également stockée dans le fichier rsreportserver.config. Vous devez créer ce compte manuellement. Pour plus d’informations sur ce compte et sur son utilisation, consultez [Configurer le compte d’exécution sans assistance &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
-   Clé symétrique utilisée pour un chiffrement.  
  
     Cette valeur est créée pendant l'installation ou la configuration du serveur, elle est ensuite stockée sous forme de valeur chiffrée dans la base de données du serveur de rapports. Le service Windows Report Server utilise cette clé pour chiffrer et déchiffrer des données stockées dans la base de données du serveur de rapports.  
  
## <a name="encryption-functionality-in-reporting-services"></a>Fonctionnalité de chiffrement dans Reporting Services  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise des fonctions de chiffrement qui font partie du système d'exploitation Windows. Des chiffrements symétriques et asymétriques sont utilisés.  
  
 Les données de la base de données du serveur de rapports sont chiffrées à l'aide d'une clé symétrique. Il existe une clé symétrique pour chaque base de données de serveur de rapports. Cette clé symétrique est elle-même chiffrée à l'aide de la clé publique d'une paire de clés asymétriques générée par Windows. La clé privée est détenue par le compte du service Report Server Windows.  
  
 Dans un déploiement évolutif de serveur de rapports où plusieurs instances de serveur de rapports partagent la même base de données de serveur de rapports, une clé symétrique unique est employée par tous les nœuds de serveur de rapports. Chaque nœud doit avoir une copie de la clé symétrique partagée. Une copie de la clé symétrique est créée automatiquement pour chaque nœud lorsque le déploiement évolutif est configuré. Chaque nœud chiffre sa copie de la clé symétrique à l'aide de la clé publique d'une paire de clés spécifiques de son compte de service Windows. Pour en savoir plus sur la création de la clé symétrique pour les déploiements à instance unique et évolutifs, consultez [Initialiser un serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md).  
  
> [!NOTE]  
>  Lorsque vous changez le compte du service Report Server Windows, les clés asymétriques peuvent devenir non valides, ce qui affecte les opérations du serveur. Pour éviter ce problème, utilisez toujours l'outil de configuration de Reporting Services pour modifier les paramètres de compte de service. Lorsque vous utilisez l'outil de configuration, les clés sont mises à jour automatiquement. Pour plus d’informations, consultez [Configurer le compte de service Report Server &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
## <a name="other-sources-of-confidential-data"></a>Autres sources de données confidentielles  
 Un serveur de rapports stocke d'autres données non chiffrées, mais qui contiennent des informations sensibles que vous voulez protéger. Plus particulièrement, les instantanés d'historique de rapport et d'exécution de rapport contiennent des résultats de requêtes pouvant inclure des données destinées exclusivement à des utilisateurs autorisés. Si vous utilisez la fonction d'instantané pour des rapports qui contiennent des données confidentielles, sachez que les utilisateurs capables d'ouvrir des tables dans une base de données du serveur de rapports peuvent être en mesure d'afficher des parties d'un rapport stocké en inspectant le contenu de la table.  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne prend pas en charge la mise en cache ou l’historique de rapport pour les rapports basés sur l’identité de sécurité de l’utilisateur.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
