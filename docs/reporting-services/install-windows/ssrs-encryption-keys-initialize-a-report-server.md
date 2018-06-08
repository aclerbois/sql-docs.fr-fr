---
title: Initialiser un serveur de rapports (Gestionnaire de configuration de SSRS) | Microsoft Docs
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
- report servers [Reporting Services], initializing
- initialization process [Reporting Services]
- checking report server initializations
- scale-out deployments [Reporting Services]
- initializing report servers [Reporting Services]
- verifying report server initializations
ms.assetid: 861d4ec4-1085-412c-9a82-68869a77bd55
caps.latest.revision: 10
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: f51884df3e202153f3da1b75b63cd2155270eedc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ssrs-encryption-keys---initialize-a-report-server"></a>Clés de chiffrement SSRS - Initialiser un serveur de rapports
  Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], un serveur initialisé est un serveur qui peut chiffrer et déchiffrer les données d'une base de données de serveur de rapports. L'initialisation est une condition obligatoire pour le fonctionnement d'un serveur de rapports. Cette opération s'effectue lorsque le service Report Server est démarré pour la première fois. Elle s'accomplit également lorsque vous intégrez le serveur de rapports au déploiement existant ou lorsque, manuellement, vous recréez les clés dans le cadre d'un processus de récupération. Pour savoir comment les clés de chiffrement sont utilisées et pour quelles raisons, consultez [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md) et [Stocker des données chiffrées du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md).  
  
 Les clés de chiffrement sont en partie basées sur les informations de profil du service Report Server. Si vous modifiez l'identité de l'utilisateur servant à exécuter le service Report Server, vous devez mettre à jour les clés en conséquence. Si vous utilisez l'outil de configuration de Reporting Services pour changer l'identité, cette opération s'effectue automatiquement.  
  
 Si l’initialisation échoue pour une raison quelconque, le serveur de rapports retourne une erreur **RSReportServerNotActivated** en réponse à l’utilisateur et aux demandes de service. Dans ce cas, le problème se situe peut-être au niveau de la configuration du serveur ou du système. Pour plus d’informations, consultez [SSRS: Troubleshoot Issues and Errors with Reporting Services](http://social.technet.microsoft.com/wiki/contents/articles/1633.aspx) (Résoudre les problèmes et erreurs avec Reporting Services) (http://social.technet.microsoft.com/wiki/contents/articles/1633.aspx) dans la rubrique Wiki Technet.  
  
## <a name="overview-of-the-initialization-process"></a>Vue d'ensemble du processus d'initialisation  
 Le processus d'initialisation crée et stocke une clé symétrique destinée au chiffrement. Cette clé symétrique est créée par les services de chiffrement de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, puis utilisée par le service Report Server pour chiffrer et déchiffrer des données. La clé symétrique est elle-même chiffrée avec une clé asymétrique.  
  
 La description du processus d'initialisation est détaillée comme suit :  
  
1.  Au démarrage initial, le service Report Server lit le fichier RSReportServer.config pour récupérer l'identificateur d'installation et les informations de connexion à la base de données.  
  
2.  Le service Report Server fait la demande d'une clé publique auprès des services de chiffrement. Windows crée une clé privée et une clé publique, puis envoie la clé publique au service Report Server.  
  
3.  Le service Report Server se connecte à la base de données du serveur de rapports pour y stocker les valeurs de l'identificateur d'installation et de la clé publique.  
  
4.  Le service Report Server rappelle les services de chiffrement, cette fois-ci pour demander une clé symétrique. Windows crée la clé symétrique requise.  
  
5.  Le service Report Server se connecte à nouveau à la base de données du serveur de rapports et ajoute la clé symétrique aux valeurs de la clé publique et de l'identificateur d'installation stockées au cours de l'étape 3. Avant de la stocker, le service Report Server utilise sa clé publique pour chiffrer la clé symétrique. Une fois cette nouvelle clé stockée, le serveur de rapports est considéré comme initialisé et opérationnel.  
  
## <a name="initializing-a-report-server-for-scale-out-deployment"></a>Initialisation d'un serveur de rapports pour un déploiement évolutif  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prend en charge le modèle de déploiement avec montée en puissance parallèle qui partage une seule base de données de serveur de rapports entre plusieurs instances de serveurs de rapports. Pour intégrer un déploiement évolutif, un serveur de rapports doit créer et stocker sa copie de la clé symétrique dans la base de données partagée. Bien qu'une seule clé symétrique soit utilisée par les serveurs utilisateurs de la base de données, chaque serveur de rapports possède un exemplaire de la clé. Chaque exemplaire est différent en soi puisqu’il est chiffré exclusivement à l’aide de la clé publique qu’il possède.  
  
 Les premières étapes assurant l'initialisation d'un serveur de rapports en vue d'un déploiement évolutif sont identiques aux trois premières étapes de l'initialisation d'une combinaison base de données et serveur unique.  
  
 La seule différence réside dans la façon dont le serveur de rapports obtient la clé symétrique au cours du processus d'initialisation pour le déploiement évolutif. Lorsque le premier serveur est initialisé, il obtient la clé symétrique de Windows et Lorsque le deuxième serveur est initialisé durant la configuration du déploiement avec montée en puissance parallèle, il obtient sa clé symétrique du service Report Server déjà initialisé. L'instance du premier serveur de rapports utilise la clé publique de la deuxième instance pour créer un exemplaire chiffré de la clé symétrique pour l'instance du deuxième serveur de rapports. La clé symétrique n'est jamais exposée sous forme de texte brut à un seul stade de ce processus.  
  
## <a name="how-to-initialize-a-report-server"></a>Initialisation d'un serveur de rapports  
  
-   Pour initialiser un serveur de rapports, utilisez l'outil de configuration de Reporting Services. L'opération s'effectue automatiquement lorsque vous créez et configurez la base de données du serveur de rapports. Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Pour initialiser un serveur de rapports en vue de l’intégrer à un déploiement évolutif, utilisez la page d’initialisation de l’outil de configuration de Reporting Services ou l’utilitaire **RSKeymgmt**. Pour obtenir des instructions détaillées, consultez [Configurer un déploiement avec montée en puissance parallèle de serveurs de rapports en mode natif &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
> [!NOTE]  
>  **RSKeymgmt** est une application console qui s’exécute à partir d’une ligne de commande sur un ordinateur hébergeant une instance de serveur de rapports déjà intégrée à un déploiement évolutif. Lorsque vous exécutez cet utilitaire, vous spécifiez des arguments pour sélectionner une instance de serveur de rapports distante à initialiser.  
  
 Un serveur de rapports n'est initialisé que dans la mesure où une correspondance entre l'identificateur d'installation et la clé publique existe. Si la correspondance est trouvée, une clé symétrique permettant le chiffrement réversible est créée. S'il n'existe aucune correspondance, le serveur de rapports est désactivé. Dans ce cas il peut s'avérer nécessaire d'appliquer une clé de sauvegarde ou de supprimer les données chiffrées si aucune clé de sauvegarde n'est disponible ou valide. Pour plus d’informations sur les clés de chiffrement utilisées par un serveur de rapports, consultez [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).  
  
> [!NOTE]  
>  Vous pouvez également utiliser le fournisseur WMI (Windows Management Instrumentation) de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour initialiser un serveur de rapports par programme. Pour plus d’informations, consultez [Accès au fournisseur WMI de Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md) dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="how-to-confirm-a-report-server-initialization"></a>Confirmation de l'initialisation d'un serveur de rapports  
 Pour confirmer l’initialisation d’un serveur de rapports, exécutez une commande ping pour le service web Report Server en tapant **http://\<nom_serveur>/reportserver** dans la fenêtre de commande. Si l’erreur **RSReportServerNotActivated** se produit, l’initialisation a échoué.  
  
## <a name="see-also"></a> Voir aussi
[Configurer et gérer des clés de chiffrement (Gestionnaire de configuration de SSRS)](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)
  
  
